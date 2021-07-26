# TcaplusDB 本地 Docker 版部署 Windows10 方案

# 背景

为方便客户在开发过程中能在 Windows10 机器上直接部署 TcaplusDB 本地 Docker 版，介绍如何借助 Windows 已有的组件来完成整个部署过程。

# 前置

## 环境要求

| 依赖项                      | 说明                                                                                     |
| --------------------------- | ---------------------------------------------------------------------------------------- |
| Windows10                   | Build Version >=18362, 可通过在 Win10 的 CMD 终端执行`ver`命令查看                       |
| WSL2                        | Windows Subsystem for Linux 2, 微软部署 Linux 的方案，如果之前用的是 WSL1，可升级至 WSL2 |
| Linux Kernel Update Package | 用于 WSL2 的 Linux 内核组件的更新包                                                      |
| CentOS7 Distro              | Linux Kernel for WSL2, 跑 docker 服务的系统载体                                          |
| Docker-Desktop              | Windows 上 docker 管理平台，结合 WSL2 管理镜像和容器                                     |
| TcaplusDB 本地 Docker 镜像  | 用于部署 TcaplusDB, 最新版本: 3.53.1                                                     |

## 其它要求

部署依赖于 PowerShell 执行相关命令，特别注意：**以管理员身份**执行 Powershell，避免提示权限问题。

# 资源准备

| 资源名                      | 下载地址                                                                                                  |
| --------------------------- | --------------------------------------------------------------------------------------------------------- |
| Linux Kernel Update Package | [下载](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)                          |
| CentOs7 Distro              | [下载](https://github.com/wsldl-pg/CentWSL/releases/download/7.0.1907.3/CentOS7.zip)                      |
| Docker-Desktop              | [下载](https://hub.docker.com/editions/community/docker-ce-desktop-windows/)                              |
| TcaplusDB 本地 Docker 版    | [下载](https://tcaplus-tool-1302668961.cos.ap-shanghai.myqcloud.com/docker/tcaplusdb-local-3.53.1.tar.gz) |

# WSL2 部署

WSL2 具体资料可以参考[微软官网介绍](https://docs.microsoft.com/en-us/windows/wsl/install-win10)。WSL2 部署可参考[官网](https://docs.microsoft.com/en-us/windows/wsl/install-win10)。下面介绍下具体流程。
如果用户已经有 WSL1 的环境，请跳转至**"WSL1 升级 WSL2 部分"**

## 步骤 1，允许 WSL2 组件

可通过命令来操作，以**管理员打开 PowerShell**, 并运行如下命令：

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

## 步骤 2, 允许虚拟化特性

WSL2 依赖于虚拟化特性，同时依赖 Hyper-v 特性，需要允许打开

- **允许虚拟化特性**

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

- **允许 Hyper-v 特性**

```
dism.exe  /online /enable-feature /featurename:Microsoft-Hyper-V  /all /norestart

#这个很关键，需要执行，如果未执行，会报BIOS问题
bcdedit /set hypervisorlaunchtype auto
```

## 步骤 3，重启电脑

上面设置完虚拟化特性后，需要重启下电脑，使其生效。

## 步骤 4, 安装 Linux 内核更新包

根据上述下载的 Linux Kernel Update Package，直接双击安装即可。

## 步骤 5，设置 WSL2 支持低版本 CentOS 镜像, 如 Centos6

在**C:\Users\<Username>**目录，如：C:\Users\JasonLiu, 新建一个文件**.wslconfig**,添加如下内容:

```
[wsl2]
kernelCommandLine = vsyscall=emulate
```

##步骤 6，设置默认的 WSL 版本号
以管理员身份打开 PowerShell,执行如下命令：

```
wsl --set-default-version 2
```

如果设置成功，则没啥问题。

# WSL1 升级 WSL2

如果用户本身已经开启 WSL 功能，并且用的是 WSL1。则升级类似，除了**步骤 1**不执行，其它按上面**步骤 2、步骤 3、步骤 4、步骤 5、步骤 6**设置即可。对于已有的 Linux Distro, 可以通过修改 Distro 的版本为 WSL2,如现有的 Distro 名称为 CentOS7,修改方式如下:

```
wsl --set-version CentOS7 2
```

# CentOS7 Distro 部署

TcaplusDB 镜像对 CentOS7 系列适配较好，可以部署一个 CentOS7 的内核。根据上述**资源准备**阶段下载的 CentOS7 Distro。下载到指定目录后解压，右键以**管理员**身份安装即可。
把 Centos7 设置为默认的 Distro, 以管理员身份打开 PowerShell, 执行:

```
wsl --set-default CentOS7
```

查看当前 Distro 的状态:

```
wsl -l -v
```

# Docker Desktop 部署

根据资源准备阶段下载的 Docker-Desktop, 直接点击安装即可。安装过程会提示是否允许 WSL2, 直接勾选即可。具体可参考[官方部署文档](https://docs.docker.com/docker-for-windows/wsl/)。

- 如果安装过程中未勾选 wsl2, 可以在安装完后，打开 Docker-Desktop 的"Settings"，在"General" 处看 "Use the WSL2 based engine"是否勾选。
- 可以在 Docker-Desktop 的"Settings"页面的"Resources"处，查看当前已经安装的 Distro, 会自动关联已安装的所有 Distros, 除了关联默认的 Distro, 可以选择关联其它的 Distro.

Docker Desktop 安装好后，通过 wsl -l -v 可查看状态:

```
PS C:\Windows\system32> wsl -l -v
  NAME                   STATE           VERSION
* CentOS7                Running         2
  docker-desktop         Running         2
  docker-desktop-data    Running         2
```

从上面信息来看，默认的 Distro 是 CentOS7, 同时会有两个默认的 Distro: `docker-desktop`, `docker-desktop-data`。

# TcaplusDB 镜像部署

## 步骤 1，准备镜像

参考资源准备阶段下载的镜像,名称: `tcaplusdb-local-3.53.1.tar.gz`

## 步骤 2，导入镜像

确认当前 WSL2 的默认 Linux 是 CentOS7, 如果不是，参照上面**"CentOS7 Distro 部署"**。以管理员身份打开 PowerShell, 执行:

```
docker load -i tcaplusdb-local-3.53.1.tar.gz
```

导入完成后，查看镜像是否 ok:

```
PS C:\Windows\system32> docker images
REPOSITORY        TAG       IMAGE ID       CREATED       SIZE
tcaplusdb-local   3.53.1    d06cce065bc1   2 hours ago   6.95GB
```

## 步骤 3，创建容器

创建容器支持两种：

- 一种是默认的容器创建，命令如下：

```
#参数TCAPLUS_CONTAINER_OMS_PASSWORD用于指定web平台登录密码，保证安全
docker run -itd --privileged  -p 8080:80 -p 13755-13765:13755-13765 -p 9999:9999  -e TCAPLUS_CONTAINER_OMS_PASSWORD="***"  ----shm-size=3G --name test tcaplusdb-local:3.53.1
```

- 另一种是指定 IP，用于一些本机无法访问 docker 容器内 IP 的场景，如在 win10 　 cmd 下 telnet 172.17.0.2 9999 无法通时可用此方式创建容器

```
#TCAPLUS_CONTAINER_PROXY_PUBLIC_IP是指定proxy的ip, 获取方式：进CentOS7 distro环境，用ifconfig 获取eth0的ip,
docker run -itd --privileged  -e TCAPLUS_CONTAINER_PROXY_PUBLIC_IP="192.168.53.2" -e TCAPLUS_CONTAINER_OMS_PASSWORD="***" -p 8080:80 -p 13755-13765:13755-13765 -p 9999:9999 --shm-size=3G --name test tcaplusdb-local:3.53.1
```

上述容器创建命令还将容器内的端口暴露到宿主机：

- **80**: Web 运维平台的端口，暴露为 8080，可直接在浏览器通过 localhost:8080 访问
- **13755-13765**: tcaplusdb proxy 的端口
- **9999**: tcaplusdb 目录服务地址端口，是业务连接时所用的端口

查看创建的容器:

```
[root@ballenwen-PC4 e]# docker ps
CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS        PORTS                                                        NAMES
4a6ce2b1811c   tcaplusdb-local:3.53.1   "sh /data/tcaplus/tc…"   2 seconds ago   Up 1 second   0.0.0.0:13755-13765->13755-13765/tcp, 0.0.0.0:8080->80/tcp   test
```

容器创建后，进入容器，等 20-60s，tcaplus 进程会自动启动，可以通过执行：

```
tcaplus_docker 172.17.0.2 root> ps -ef | grep tcaplus
root         1     0  0 21:32 pts/0    00:00:00 sh /data/tcaplus/tcaplus_run.sh
root       883     1  0 21:32 ?        00:00:00 /data/tcaplus/tsf4g_release/apps/tconnd/bin/tconnd --id=1.1.3 --use-bus --bus-key=1688 --conf-file=../cfg/tconnd/tconnd.xml --tlogconf=../cfg/tconnd/tconnd_log.xml --daemon start
root       899     1  0 21:32 ?        00:00:00 /data/tcaplus/tsf4g_release/apps/tcenterd/bin/tcenterd --id=1.1.1 --use-bus --bus-key=1688 --tlogconf=../cfg/tcenterd/tcenterd_log.xml --conf-file=../cfg/tcenterd/tcenterd.xml -D start
root       923     1  2 21:32 ?        00:00:01 /data/tcaplus/tsf4g_release/apps/tcm/bin/tcmcenter --id=1.1.2 --use-bus --timer=10 --bus-key=1688 --bus-beat-gap=60 --conf-file=../cfg/tcm/tcmcenter.xml --tlogconf=../cfg/tcm/tcmcenter_log.xml --conf-format=3 -D start
root       940     1  3 21:32 ?        00:00:02 /data/tcaplus/tsf4g_release/apps/tagent/bin/tagent --id=1.2.1 --tlogconf=../cfg/tagent/tagent_log.xml --conf-file=../cfg/tagent/tagent.xml -D start
root       962     1  0 21:32 pts/0    00:00:00 /data/tcaplus/tcaplus_service/bin/tbusd --epoll-wait=1 --use-tsm --idle-sleep=1 --conf-file=../cfg/tbusd/tbusd.xml --tlogconf=../cfg/tbusd/tbusd_log.xml --no-bus --daemon --id=0.0.0.1 --business-id=0 -D start
root      1320  1318  0 21:32 ?        00:00:00 /opt/lampp/bin/rotatelogs /data/tcaplus/oms/tcapoms/www/log/error/error_log_%Y%m%d 86400 480
root      1324  1318  0 21:32 ?        00:00:00 /opt/lampp/bin/rotatelogs /data/tcaplus/oms/tcapoms/www/log/accesslogs/access_log_%Y%m%d 86400
tcaplus   1712  1318  0 21:32 ?        00:00:00 /opt/lampp/bin/httpd -k start -E /opt/lampp/logs/error_log -DSSL -DPHP
tcaplus   1713  1318  0 21:32 ?        00:00:00 /opt/lampp/bin/httpd -k start -E /opt/lampp/logs/error_log -DSSL -DPHP
tcaplus   1714  1318  0 21:32 ?        00:00:00 /opt/lampp/bin/httpd -k start -E /opt/lampp/logs/error_log -DSSL -DPHP
tcaplus   1715  1318  0 21:32 ?        00:00:00 /opt/lampp/bin/httpd -k start -E /opt/lampp/logs/error_log -DSSL -DPHP
tcaplus   1716  1318  0 21:32 ?        00:00:00 /opt/lampp/bin/httpd -k start -E /opt/lampp/logs/error_log -DSSL -DPHP
tcaplus   1717  1318  0 21:32 ?        00:00:00 /opt/lampp/bin/httpd -k start -E /opt/lampp/logs/error_log -DSSL -DPHP
tcaplus   1718  1318  0 21:32 ?        00:00:00 /opt/lampp/bin/httpd -k start -E /opt/lampp/logs/error_log -DSSL -DPHP
tcaplus   1719  1318  0 21:32 ?        00:00:00 /opt/lampp/bin/httpd -k start -E /opt/lampp/logs/error_log -DSSL -DPHP
tcaplus   1720  1318  0 21:32 ?        00:00:00 /opt/lampp/bin/httpd -k start -E /opt/lampp/logs/error_log -DSSL -DPHP
root      1895     1  5 21:32 ?        00:00:02 /data/tcaplus/tcaplus_service/bin/1_2_1_1/swiftd --use-tsm --idle-sleep=1 --conf-file=../../cfg/access_1_2_1_1/swift_tcaproxy.xml --conf-format=3 --tlogconf=../../cfg/access_1_2_1_1/tcaproxy_log.xml -D --id=1.2.1.1 --bus-key=2088 --business-id=0 -D start
root      1898     1  4 21:32 ?        00:00:02 /data/tcaplus/tcaplus_service/bin/1_3_1_1/swiftd --use-tsm --idle-sleep=1 --conf-file=../../cfg/access_1_3_1_1/swift_tcaproxy.xml --conf-format=3 --tlogconf=../../cfg/access_1_3_1_1/tcaproxy_log.xml -D --id=1.3.1.1 --bus-key=2088 --business-id=0 -D start
root      1914     1 31 21:32 ?        00:00:16 /data/tcaplus/tcaplus_service/bin/1_1_2_1/tcapsvr --use-tsm --idle-sleep=1 --nthread=8 --bus-check-gap=0 --threadChannelSize=20480000 --conf-file=../../cfg/storage_1_1_2_1/tcapsvr.xml --conf-format=3 --tlogconf=../../cfg/storage_1_1_2_1/tcapsvr_log.xml --daemon --id=1.1.2.1 --bus-key=2088 --business-id=0 -D start
root      1931     1 30 21:32 ?        00:00:15 /data/tcaplus/tcaplus_service/bin/1_1_2_2/tcapsvr --use-tsm --idle-sleep=1 --nthread=8 --bus-check-gap=0 --threadChannelSize=20480000 --conf-file=../../cfg/storage_1_1_2_2/tcapsvr.xml --conf-format=3 --tlogconf=../../cfg/storage_1_1_2_2/tcapsvr_log.xml --daemon --id=1.1.2.2 --bus-key=2088 --business-id=0 -D start
root      1938     1  4 21:32 ?        00:00:02 /data/tcaplus/tcaplus_service/bin/1_3_8_1/swiftd --use-tsm --idle-sleep=1 --conf-file=../../cfg/tcaprestproxy_1_3_8_1/swift_tcaprestproxy.xml --conf-format=3 --tlogconf=../../cfg/tcaprestproxy_1_3_8_1/tcaprestproxy_log.xml -D --id=1.3.8.1 --business-id=0 -D start
root      1944     1  4 21:32 ?        00:00:02 /data/tcaplus/tcaplus_service/bin/tcapdir/swiftd --use-tsm --idle-sleep=1 --conf-file=../../cfg/tcapdir/tcapdir_swift.xml --tlogconf=../../cfg/tcapdir/tcapdir_log.xml -D --id=1.0.4.1 --business-id=0 -D start
root      2000     1 29 21:32 ?        00:00:14 /data/tcaplus/tcaplus_service/bin/1_2_5_1/tcapdb --use-tsm --idle-sleep=1 --nthread=8 --threadChannelSize=10000000 --timer=3000 --conf-file=../../cfg/tcapdb_1_2_5_1/tcapdb_cfg.xml --conf-format=3 --tlogconf=../../cfg/tcapdb_1_2_5_1/tcapdb_log.xml -D --id=1.2.5.1 --bus-key=2088 --business-id=0 -D start
root      2253  2140  0 21:33 pts/1    00:00:00 grep tcaplus
```

容器如果创建失败，几个原因检测下:

- **机器资源检查**: 磁盘可用在 50G 以上，可用内存:4-6G
- **系统参数检查**: C:\Users\<username>\.wslconfig 是否创建，参考上述**步骤 5**
- **查看容器创建日志**: docker logs <container name>

## 步骤 4，查看创建是否 ok

通过浏览器访问 localhost:8080 即可，账号为`tcaplus`, 密码通过在创建容器时指定参数 TCAPLUS_CONTAINER_OMS_PASSWORD 来指定密码, 可登录平台后在账号管理处修改密码。如果访问正常，即表示容器创建 ok, 同时进"运维平台"=>"集群状态", 查看集群进程状态是否 ok,看是否有停止状态的进程。

# 常见报错

## Issue1, BIOS 问题

问题描述：

```
Please enable the Virtual Machine Platform Windows feature and ensure virtualization is enabled in the BIOS
```

问题解决: 允许 Hyper-v, 并用 bcdedit 设置虚拟特性自动注册，重设下 BIOS 配置

```
dism.exe  /online /enable-feature /featurename:Microsoft-Hyper-V  /all /norestart

bcdedit /set hypervisorlaunchtype auto
```

## Issue2, 容器创建失败问题

问题描述: 容器创建后自动退出，查看资料是发现 WSL2 对于 Centos6.x 的镜像支持需要额外配置参数。
问题解决:

```
#在用户目录下创建一个文件
c:\users\<username>\.wslconfig

#在文件中填充以下内容
[wsl2]
kernelCommandLine = vsyscall=emulate
```
