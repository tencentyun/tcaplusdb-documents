# 说明

TcaplusDB 本地 Docker 版是为用户提供的一个满足本地开发调试的版本，相比腾讯云 TcaplusDB 版，本地版可以方便用户在无法连通云环境的开发环境进行代码调试，同时在功能上和腾讯云版本无差别。此文档主要介绍用户如何在本地开发环境部署 TcaplusDB 本地 Docker 版环境。同时为方便用户使用本地 Docker 版本，提供一个 tcapluscli 的工具来通过命令行方式与 Docker 版本 TcaplusDB 交互，方便快速入门。

- Linux 系统部署参考本文描述
- Windows10 系统部署参考: [文章](https://github.com/tencentyun/tcaplusdb-documents/blob/main/docker/TcaplusDB%E6%9C%AC%E5%9C%B0Docker%E7%89%88%E9%83%A8%E7%BD%B2Windows10%E6%96%B9%E6%A1%88.md)

# 依赖

| 依赖组件              | 下载地址                                                                                                      |
| --------------------- | ------------------------------------------------------------------------------------------------------------- |
| TcaplusDB Docker 镜像 | [Download](https://tcaplus-tool-1302668961.cos.ap-shanghai.myqcloud.com/docker/tcaplusdb-local-3.51.1.tar.gz) |
| TcaplusDB CLI 工具    | [Download](https://tcaplus-tool-1302668961.cos.ap-shanghai.myqcloud.com/tcapluscli/tcapluscli.tgz)            |

# 部署前置

- **部署 docker 机器尽量用比较干净机器，即未部署 mysql,nginx,elasticsearch 之类的软件，TcaplusDB 镜像内会启动 mysql 相关进程，如果机器部署有相关组件，会造成 TcaplusDB 进程无法启动，端口冲突, 涉及冲突端口: 3306,80, 9999,31001, 9200,9300,13755-13777**

- **部署机器磁盘大小建议 100G，可用磁盘空间大于 50GB**

- **部署机器操作系统建议 Centos7,64-bit 系统**

# 部署

## 准备机器环境

如果是在腾讯云上测试，则需要准备申请一台 CVM 实例，规格在: 4C8G, 操作系统用户自己选定，支持 Debian8.2、CentOS7、Windows10 系统。CVM 实例主要用于跑 docker 镜像和调试程序。CVM 需要有一个外网 IP 用于 TcaplusDB 操作控制台访问。

如果是在用户本地开发环境，则需要准备一台开发测试机，规格在: 4C8G, 可用内存尽量足够大，避免影响 docker 镜像的正常运转。机器用于跑 docker 镜像和调试程序。开发测试机环境可以被本地办公环境访问，用于访问 TcaplusDB 操作控制台。

## 准备 Docker 环境

Docker 安装一般通过系统命令来安装，如 yum、apt-get 等。以 CentOS7、CentOS8 和 Debian8.2 举例,安装方式如下:

```
#CentOS7环境，CVM实例
yum install -y docker

#Debian8.2环境
apt-get update
apt-get install -y docker-ce containerd.io

#CentOS8环境，CVM实例，用dnf来安装
dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
dnf repolist -v
dnf list docker-ce --showduplicates | sort -r
dnf install docker-ce --nobest
```

注意：　在安装 Docker 过程中，注意镜像源的配置，可参考网上相关文档资料，配置成国内镜像源。

安装完 docker 之后，可以用 docker 命令检查是否安装 Ok, 如果安装成功，则执行如下命令启动 docker 服务:

```
systemctl start docker.service
```

## 系统库准备

docker 镜像一些进程依赖于 mdadm,　如果机器默认没有的话需要装下:

```
#centos
yum install -y mdadm

#debian
apt-get update
apt-get install -y mdadm
```

## TcaplusDB 镜像准备

### 导入 TcaplusDB 本地镜像

导入已经制作的 TcaplusDB 本地 Docker 镜像，镜像下载地址：[tcaplusdb-local-3.51.1.tar.gz](https://tcaplus-tool-1302668961.cos.ap-shanghai.myqcloud.com/docker/tcaplusdb-local-3.51.1.tar.gz)或从本地导入，以镜像文件名 tcaplusdb-local-3.51.1.tar.gz 举例，　导入方式如下:

```
#镜像导入
[root@VM-32-2-centos ~]# docker load　< tcapludb-local-3.51.1.tar.gz

#镜像查看
[root@VM-32-2-centos ~]# docker images
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
tcaplusdb-local             3.51.1              e40b1d2157a9        3 days ago          8.15GB
```

### 创建镜像容器

在导入镜像基础上，创建一个新的容器。容器创建支持几种模式用以满足不同场景:

- **场景 1**：Docker 部署在腾讯云 CVM 上，想在本机（办公机, win10 或 macos）通过 CVM 外网 IP 访问 CVM 的 TcaplusDB 容器。
- **场景 2**: Docker 部署在客户开发机上，想在本机（办公机, win10 或 macos）通过客户开发机内网 IP（同办公机在一个局域网内）访问开发机上的 TcaplusDB 容器。
- **场景 3**: Docker 部署在本机（办公机，win10 或 macos）通过本机访问 TcaplusDB 容器。

#### 场景 1－容器创建

场景 1 是要通过外网 IP 进行访问，假设客户申请的 CVM 机器外网 Ip 为 106.54.12.135，　同时需要暴露 TcaplusDB 相关端口，主要端口:

- **80**: 用于 TcaplusDB web 运维平台访问
- **9999**: TcaplusDB 访问主端口，用于写代码时配置
- **13755-13765**: TcaplusDB proxy 端口

创建命令如下:

```
#参数TCAPLUS_CONTAINER_PROXY_PUBLIC_IP指定暴露的proxy ip, 参数TCAPLUS_CONTAINER_OMS_PASSWORD指定web平台登录密码
docker run -itd --shm-size=3G --privileged -e TCAPLUS_CONTAINER_PROXY_PUBLIC_IP="cvm外网ip" -e TCAPLUS_CONTAINER_OMS_PASSWORD="***" -p 80:80 -p 9999:9999 -p 13755-13765:13755-13765 --name [容器名] [镜像REPOSITORY名]:[TAG]

#示例,CVM外网IP为：106.54.12.135，客户自行替换成自己的IP
docker run -itd --shm-size=3G --privileged -e TCAPLUS_CONTAINER_PROXY_PUBLIC_IP="106.54.12.135"  -e TCAPLUS_CONTAINER_OMS_PASSWORD="***"  -p 80:80 -p 9999:9999 -p 13755-13765:13755-13765 fn--name test tcaplusdb-local:3.51.1
```

### 场景 2－容器创建

场景 2 和场景 1 类似，把相关外网 IP 换成内网开发机 IP 即可, ip 获取方式可通过 ifconfig 命令查看 eth1/eth0 网卡或客户自己局域网所在网卡的 ip 为主。

```
#内网ip，网卡eth1或eth0所在ip, 如172.17.32.2，　参数TCAPLUS_CONTAINER_OMS_PASSWORD指定web平台登录密码
docker run -itd --shm-size=3G --privileged -e TCAPLUS_CONTAINER_PROXY_PUBLIC_IP="172.17.32.2"  -e TCAPLUS_CONTAINER_OMS_PASSWORD="***"  -p 80:80 -p 9999:9999 -p 13755-13765:13755-13765 fn--name test tcaplusdb-local:3.51.1
```

### 场景 3－容器创建

场景 3 如果客户机器是 windows10 机器，需要通过 wsl2 方式部署 docker, 具体部署方式参考：[文章](https://github.com/tencentyun/tcaplusdb-documents/blob/main/docker/TcaplusDB%E6%9C%AC%E5%9C%B0Docker%E7%89%88%E9%83%A8%E7%BD%B2Windows10%E6%96%B9%E6%A1%88.md)。
容器创建方式：

```
#不指定proxy ip，　参数TCAPLUS_CONTAINER_OMS_PASSWORD指定web平台登录密码
docker run -itd --shm-size=3G --privileged   -e TCAPLUS_CONTAINER_OMS_PASSWORD="***"  -p 80:80 -p 9999:9999 -p 13755-13765:13755-13765 fn--name test tcaplusdb-local:3.51.1

#指定proxy ip, 用于一些本机无法telnet容器IP端口的场景，如telnet 172.17.0.2 9999无法通，指定IP获取方式：win10下进wsl环境，CentOS7 distro，执行ifconfig获取的eth0 ip
docker run -itd --shm-size=3G --privileged -e TCAPLUS_CONTAINER_PROXY_PUBLIC_IP="192.168.53.2"  -e TCAPLUS_CONTAINER_OMS_PASSWORD="***"  -p 80:80 -p 9999:9999 -p 13755-13765:13755-13765 fn--name test tcaplusdb-local:3.51.1
```

### 场景 4-host 模式创建容器

上述容器创建方式通过默认的 bridge 来完成，同时还支持通过 host 模式创建。两种方式区别如下：

- **host 模式**: docker 容器网络与宿主机网络一致，确保当前部署 docker 的机器上无其它相关部署组件，如:mysql, es, nginx 等，会造成相关端口冲突
- **bridge 模式**: docker 容器的默认网络连接模式，如果业务是本机部署 docker 并在本机调试，不涉及跨机访问的话可以用此模式,　如下启动容器命令所示：

host 模式容器创建命令：

```
#参数TCAPLUS_CONTAINER_OMS_PASSWORD指定web平台登录密码
docker run -itd --net=host -e TCAPLUS_CONTAINER_OMS_PASSWORD="***"  --shm-size=3G --privileged --name test tcaplusdb-local:3.51.1
```

创建容器后，可查看是否创建 OK, 执行如下命令:

```
[root@VM-32-2-centos ~]# docker ps
CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS              PORTS               NAMES
4fa0a671cf9b        tcaplusdb-local   "sh /data/tcaplus/se…"   9 minutes ago       Up 9 minutes                            test
```

### 启动 TcaplusDB 进程

容器创建后，TcaplusDB 后台相关进程还未启动，需要进入容器内，等待 1-2min 等启动脚本在后台执行拉取相关进程：

```
#进入容器
docker exec -it ［容器id] /bin/bash

#等待1-2min, 可通过ps命令查看进程是否ok, ps -ef | grep tcaplus

tcaplus_docker 172.18.0.2 root-> ps -ef | grep tcaplus | grep "/data/tcaplus"
root         1     0  0 Oct28 ?        00:00:00 sh /data/tcaplus/setup.sh
root       856   853  0 Oct28 ?        00:00:00 /opt/lampp/bin/rotatelogs /data/tcaplus/oms/tcapoms/www/log/error/error_log_%Y%m%d 86400 480
root       862   853  0 Oct28 ?        00:00:00 /opt/lampp/bin/rotatelogs /data/tcaplus/oms/tcapoms/www/log/accesslogs/access_log_%Y%m%d 86400
root      1316     1  0 Oct28 ?        00:05:20 /data/tcaplus/tsf4g_release/apps/tconnd/bin/tconnd --id=1.1.3 --use-bus --bus-key=1688 --conf-file=../cfg/tconnd/tconnd.xml --tlogconf=../cfg/tconnd/tconnd_log.xml --daemon start
root      1320     1  0 Oct28 ?        00:00:41 /data/tcaplus/tsf4g_release/apps/tcenterd/bin/tcenterd --id=1.1.1 --use-bus --bus-key=1688 --tlogconf=../cfg/tcenterd/tcenterd_log.xml --conf-file=../cfg/tcenterd/tcenterd.xml -D start
root      1324     1  0 Oct28 ?        00:06:50 /data/tcaplus/tsf4g_release/apps/tcm/bin/tcmcenter --id=1.1.2 --use-bus --timer=10 --bus-key=1688 --bus-beat-gap=60 --conf-file=../cfg/tcm/tcmcenter.xml --tlogconf=../cfg/tcm/tcmcenter_log.xml --conf-format=3 -D start
root      1334     1  1 Oct28 ?        01:05:47 /data/tcaplus/tsf4g_release/apps/tagent/bin/tagent --id=1.2.1 --tlogconf=../cfg/tagent/tagent_log.xml --conf-file=../cfg/tagent/tagent.xml -D start
root      1497  1215  0 20:27 ?        00:00:00 grep /data/tcaplus
root      1779     1  0 Oct28 ?        00:00:11 /data/tcaplus/tcaplus_service/bin/tbusd --epoll-wait=1 --use-tsm --idle-sleep=1 --conf-file=../cfg/tbusd/tbusd.xml --tlogconf=../cfg/tbusd/tbusd_log.xml --no-bus --daemon --id=0.0.0.1 --business-id=0 -D start
root      1832     1  4 Oct28 ?        02:41:57 /data/tcaplus/tcaplus_service/bin/1_2_1_1/swiftd --use-tsm --idle-sleep=1 --conf-file=../../cfg/access_1_2_1_1/swift_tcaproxy.xml --conf-format=3 --tlogconf=../../cfg/access_1_2_1_1/tcaproxy_log.xml -D --id=1.2.1.1 --bus-key=2088 --business-id=0 -D start
root      1846     1  4 Oct28 ?        02:41:41 /data/tcaplus/tcaplus_service/bin/1_2_1_2/swiftd --use-tsm --idle-sleep=1 --conf-file=../../cfg/access_1_2_1_2/swift_tcaproxy.xml --conf-format=3 --tlogconf=../../cfg/access_1_2_1_2/tcaproxy_log.xml -D --id=1.2.1.2 --bus-key=2088 --business-id=0 -D start
root      1904     1 75 Oct28 ?        1-19:24:51 /data/tcaplus/tcaplus_service/bin/1_2_2_1/tcapsvr --use-tsm --idle-sleep=1 --nthread=35 --bus-check-gap=0 --threadChannelSize=20480000 --conf-file=../../cfg/storage_1_2_2_1/tcapsvr.xml --conf-format=3 --tlogconf=../../cfg/storage_1_2_2_1/tcapsvr_log.xml --daemon --id=1.2.2.1 --bus-key=2088 --business-id=0 -D start
root      1906     1 74 Oct28 ?        1-18:46:41 /data/tcaplus/tcaplus_service/bin/1_2_2_2/tcapsvr --use-tsm --idle-sleep=1 --nthread=35 --bus-check-gap=0 --threadChannelSize=20480000 --conf-file=../../cfg/storage_1_2_2_2/tcapsvr.xml --conf-format=3 --tlogconf=../../cfg/storage_1_2_2_2/tcapsvr_log.xml --daemon --id=1.2.2.2 --bus-key=2088 --business-id=0 -D start
root      2041     1  3 Oct28 ?        01:43:53 /data/tcaplus/tcaplus_service/bin/tcapdir/swiftd --use-tsm --idle-sleep=1 --conf-file=../../cfg/tcapdir/tcapdir_swift.xml --tlogconf=../../cfg/tcapdir/tcapdir_log.xml -D --id=1.0.4.1 --business-id=0 -D start
```

TcaplusDB 本地镜像提供了功能齐全的运维平台供用户通过 Web 浏览器方式来操控 TcaplusDB, 如表格组相关操作和表相关操作，为快速让用户使用本地环境，这里先屏蔽运维平台细节。
如果需要访问 Web 控制台，只要办公机到部署 docker 机器的 80 端口通即可，然后在浏览器直接通过 docker 部署机器 ip 访问, web 控制台登录账户和密码:

```
用户名: tcaplus
密码: xxx (用户在创建容器时通过参数TCAPLUS_CONTAINER_OMS_PASSWORD指定)
```

### 后端服务暴露端口说明

docker 容器暴露端口主要关注两类：

- 一个是默认的`80`，用于 tcaplusdb web 访问 (OMS 管控平台）;
- 另一个是后端服务端口：`9999和<13755-13780>`，用于 tcaplusdb 　 sdk 连接后端服务使用。需要确保本地开发测试机到 docker 部署机器的策略互通情况。13755-13780 主要是 tcaplusdb 的 proxy 端口，和表格组数量有关，默认一个表格组会创建两个端口，从 13755 开始递增。

## TcaplusDB 工具准备

### 部署环境

为方便用户快速上手本地环境，TcaplusDB 提供了一个集成封装好的工具，用于快速在本地 Docker 环境创建表格组、表。工具支持不同的 OS 平台，如`MacOS、Debian8.2、CentOS7、CentOS8、Windows10`。这里以 CentOS 平台举例，　工具下载地址: TcaplusDB 本地工具。解压包后，找到对应平台的二进制文件即可，如 tcapluscli-centos7-i386。包中也有详细的帮助说明文档以供用户参考使用。可将工具拷贝到对应的开发测试机或云主机目录，并重命名为 tcapluscli 方便执行。

```
[root@VM-32-2-centos ~]# tar zxvf tcapluscli.tgz
[root@VM-32-2-centos ~]# cd tcapluscli; ls
README.md         tcapluscli-debian8-amd64                 tcapluscli-win10.exe
tcapluscli-centos7-i386        tcapluscli-centos8-i386          tcapluscli-macos

#重命名为tcapluscli,方便使用，　以CentOS7平台为例
[root@VM-32-2-centos ~]#  cp tcapluscli-centos7-i386 tcapluscli

[root@VM-32-2-centos ~]#  chmod +x tcapluscli
```

###部署要求
工具在使用之前，需要提前设置下个访问密钥相关(`~/.tcaplusdb/credentials`)，由于本地 Docker 版不涉及云环境　（**云环境需要设置账户真实密钥信息**)，所以 SECRET_KEY 和 SECRET_ID 设置可随意设置：

```
#进入用户根目录，创建一个.tcaplusdb目录,如用户根目录是/root, 如果是非root用户操作，则在对应用户home目录下创建，如/home/testuser/
mkdir /root/.tcaplusdb    #非root用户, mkdir /home/testuser/.tcaplusdb
#进入目录
cd ~/.tcaplusdb
#创建credentials文件
touch credentials
```

将如下内容 copy 到~/.tcaplusdb/credentials 文件:

```
[default]
SECRET_ID = xxx
SECRET_KEY = xxx
```

部署好后，可以执行-h 获取帮助信息：

```
./tcapluscli -h

Usage:
  tcapluscli [command]

Available Commands:
  cluster     TcaplusDB cluster operations.
  config      Configure  the common information of TcaplusDB CLI
  help        Help about any command
　privilege   Control the access privilege of API [Docker Env]
  table       TcaplusDB table operations.
  tablegroup  TcaplusDB tablegroup operations.

Flags:
  -h, --help   help for tcapluscli

Use "tcapluscli [command] --help" for more information about a command.
```

## 工具授权

在使用工具操作 TcaplusDB CLI 工具之前，需要授权 IP 访问权限，授权方式：

```
#指定docker容器endpoint地址，端口默认用80, 指定要授权的业务id(2: tdr, 3: pb), #授权所有IP，允许访问docker本地镜像
#tdr授权
./tcapluscli privilege --endpoint-url=http://localhost --access-id=2 --allow-all-ip
#PB授权
./tcapluscli privilege --endpoint-url=http://localhost --access-id=3 --allow-all-ip
```
