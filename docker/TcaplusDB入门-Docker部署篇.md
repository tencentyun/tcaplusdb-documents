[TOC]
# 说明
TcaplusDB本地Docker版是为用户提供的一个满足本地开发调试的版本，相比腾讯云TcaplusDB版，本地版可以方便用户在无法连通云环境的开发环境进行代码调试，同时在功能上和腾讯云版本无差别。此文档主要介绍用户如何在本地开发环境部署TcaplusDB本地Docker版环境。同时为方便用户使用本地Docker版本，提供一个tcapluscli的工具来通过命令行方式与Docker版本TcaplusDB交互，方便快速入门。
# 依赖
|依赖组件|下载地址|
|---|---|
|TcaplusDB Docker镜像|[Download](https://tcaplus-tool-1302668961.cos.ap-shanghai.myqcloud.com/docker/tcaplus_community_image.tar.gz)|
|TcaplusDB CLI工具|[Download](https://tcaplus-tool-1302668961.cos.ap-shanghai.myqcloud.com/tcapluscli/tcapluscli.tgz)|

# 部署
##  准备机器环境
如果是在腾讯云上测试，则需要准备申请一台CVM实例，规格在: 4C8G, 操作系统用户自己选定，可以是Centos7系统或Deiban 8系列以上的版本。CVM实例主要用于跑docker镜像和调试程序。CVM需要有一个外网IP用于TcaplusDB操作控制台访问。

如果是在用户本地开发环境，则需要准备一台开发测试机，规格在: 4C8G, 可用内存尽量足够大，避免影响docker镜像的正常运转。机器用于跑docker镜像和调试程序。开发测试机环境可以被本地办公环境访问，用于访问TcaplusDB操作控制台。

## 准备Docker环境
Docker安装一般通过系统命令来安装，如yum、apt-get等。以CentOS7、CentOS8和Debian8.2举例,安装方式如下:

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

注意：　在安装Docker过程中，注意镜像源的配置，可参考网上相关文档资料，配置成国内镜像源。

安装完docker之后，可以用docker命令检查是否安装Ok, 如果安装成功，则执行如下命令启动docker服务:
```
systemctl start docker.service
```
## 系统库准备
docker镜像一些进程依赖于mdadm,　如果机器默认没有的话需要装下:
```
#centos
yum install -y mdadm

#debian
apt-get update
apt-get install -y mdadm
```
## TcaplusDB镜像准备
### 导入TcaplusDB本地镜像
导入已经制作的TcaplusDB本地Docker镜像，镜像下载地址：[tcaplus_community_image.tar.gz](https://tcaplus-tool-1302668961.cos.ap-shanghai.myqcloud.com/docker/tcaplus_community_image.tar.gz)或从本地导入，以镜像文件名tcaplusdb_community_image.tar.gz举例，　导入方式如下:
```
#镜像导入
[root@VM-32-2-centos ~]# docker load　< tcaplusdb_community_image.tar.gz

#镜像查看
[root@VM-32-2-centos ~]# docker images
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
tcaplusrun_20201118_es_pb   latest              e40b1d2157a9        3 days ago          8.15GB
```

### 创建镜像容器
在导入镜像基础上，创建一个新的容器，容器创建命令如下:

```
#命令格式
docker run -itd --net=host --shm-size=3G --privileged --name [容器名] [镜像REPOSITORY名]

#示例：带net网络参数，指定host模式，创建成功后容器内的环境与宿主机网络环境保持一致，共用网卡, 解决跨机访问docker容器问题
docker run -itd --net=host --shm-size=3G --privileged    --name test tcaplusrun_20201118_es_pb
```

注意：

  * __host模式__: docker容器网络与宿主机网络一致，确保当前部署docker的机器上无其它相关部署组件，如:mysql, es, nginx等，会造成相关端口冲突
  * __bridge模式__: docker容器的默认网络连接模式，如果业务是本机部署docker并在本机调试，不涉及跨机访问的话可以用此模式,　如下启动容器命令所示：
  ```
  docker run -itd -p 80:80 -p 9999:9999 -p 13755-13765:13755-13765 --shm-size=3G --privileged --name test tcaplusrun_20201118_es_pb
  ```

创建容器后，可查看是否创建OK, 执行如下命令:
```
[root@VM-32-2-centos ~]# docker ps
CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS              PORTS               NAMES
4fa0a671cf9b        tcaplusrun_20201112_es_pb   "sh /data/tcaplus/se…"   9 minutes ago       Up 9 minutes                            test
```

### 启动TcaplusDB进程
容器创建后，TcaplusDB后台相关进程还未启动，需要进入容器内，等待2-3min等启动脚本在后台执行拉取相关进程：
```
#进入容器
docker exec -it ［容器id] /bin/bash

#等待2－3min, 可通过ps命令查看进程是否ok, ps -ef | grep tcaplusdb

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

TcaplusDB本地镜像提供了功能齐全的运维平台供用户通过Web浏览器方式来操控TcaplusDB, 如表格组相关操作和表相关操作，为快速让用户使用本地环境，这里先屏蔽运维平台细节。
如果需要访问Web控制台，只要办公机到部署docker机器的80端口通即可，然后在浏览器直接通过docker部署机器ip访问, web控制台登录账户和密码:
```
用户名: tcaplus
密码: tcaplus
```


### 后端服务暴露端口说明
docker容器暴露端口主要关注两类：
* 一个是默认的`80`，用于tcaplusdb web访问 (OMS管控平台）;
* 另一个是后端服务端口：`9999和<13755-13780>`，用于tcaplusdb　sdk连接后端服务使用。需要确保本地开发测试机到docker部署机器的策略互通情况。13755-13780主要是tcaplusdb的proxy端口，和表格组数量有关，默认一个表格组会创建两个端口，从13755开始递增。

## TcaplusDB工具准备
为方便用户快速上手本地环境，TcaplusDB提供了一个集成封装好的工具，用于快速在本地Docker环境创建表格组、表。工具支持不同的OS平台，如`MacOS、Debian8.2、CentOS7、CentOS8、Windows10`。这里以CentOS平台举例，　工具下载地址: TcaplusDB本地工具。解压包后，找到对应平台的二进制文件即可，如tcapluscli-centos7-i386。包中也有详细的帮助说明文档以供用户参考使用。可将工具拷贝到对应的开发测试机或云主机目录，并重命名为tcapluscli方便执行。

```
[root@VM-32-2-centos ~]# tar zxvf tcapluscli.tgz
[root@VM-32-2-centos ~]# cd tcapluscli; ls
README.md         tcapluscli-debian8-amd64                 tcapluscli-win10.exe
tcapluscli-centos7-i386        tcapluscli-centos8-i386          tcapluscli-macos

#重命名为tcapluscli,方便使用，　以CentOS7平台为例
[root@VM-32-2-centos ~]#  cp tcapluscli-centos7-i386 tcapluscli

[root@VM-32-2-centos ~]#  chmod +x tcapluscli
```

工具在使用之前，需要提前设置下个访问密钥相关(`~/.tcaplusdb/credentials`)，由于本地Docker版不涉及云环境　（__云环境需要设置账户真实密钥信息__)，所以SECRET_KEY和SECRET_ID设置可随意设置：
```
#进入用户根目录，创建一个.tcaplusdb目录,如用户根目录是/root
mkdir /root/.tcaplusdb
#进入目录
cd ~/.tcaplusdb
#创建credentials文件
touch credentials
```

将如下内容copy到~/.tcaplusdb/credentials文件:
```
[default]
SECRET_ID = xxx
SECRET_KEY = xxx
```

部署好后，可以执行-h获取帮助信息：
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
在使用工具操作TcaplusDB CLI工具之前，需要授权IP访问权限，授权方式：
```
#指定docker容器endpoint地址，端口默认用80, 指定要授权的业务id(2: tdr, 3: pb), #授权所有IP，允许访问docker本地镜像
#tdr授权
./tcapluscli privilege --endpoint-url=http://localhost --access-id=2 --allow-all-ip
#PB授权
./tcapluscli privilege --endpoint-url=http://localhost --access-id=3 --allow-all-ip
```


