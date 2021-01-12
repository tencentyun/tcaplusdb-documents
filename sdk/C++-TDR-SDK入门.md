C++ TDR SDK Quickstart
Table of Contents
=================

   * [TDR说明](#tdr\xE8\xAF\xB4\xE6\x98\x8E)
   * [入门](#\xE5\x85\xA5\xE9\x97\xA8)
      * [Docker环境准备](#docker\xE7\x8E\xAF\xE5\xA2\x83\xE5\x87\x86\xE5\xA4\x87)
      * [TcaplusDB表准备](#tcaplusdb\xE8\xA1\xA8\xE5\x87\x86\xE5\xA4\x87)
         * [准备TDR表示例文件](#\xE5\x87\x86\xE5\xA4\x87tdr\xE8\xA1\xA8\xE7\xA4\xBA\xE4\xBE\x8B\xE6\x96\x87\xE4\xBB\xB6)
         * [TcaplusDB集群准备](#tcaplusdb\xE9\x9B\x86\xE7\xBE\xA4\xE5\x87\x86\xE5\xA4\x87)
         * [TcaplusDB表格组准备](#tcaplusdb\xE8\xA1\xA8\xE6\xA0\xBC\xE7\xBB\x84\xE5\x87\x86\xE5\xA4\x87)
         * [TcaplusDB表创建](#tcaplusdb\xE8\xA1\xA8\xE5\x88\x9B\xE5\xBB\xBA)
      * [示例代码](#\xE7\xA4\xBA\xE4\xBE\x8B\xE4\xBB\xA3\xE7\xA0\x81)
         * [SDK准备](#sdk\xE5\x87\x86\xE5\xA4\x87)
            * [TcaplusServiceApi部署方式](#tcaplusserviceapi\xE9\x83\xA8\xE7\xBD\xB2\xE6\x96\xB9\xE5\xBC\x8F)
            * [TSF4G部署方式](#tsf4g\xE9\x83\xA8\xE7\xBD\xB2\xE6\x96\xB9\xE5\xBC\x8F)
            * [环境变量设置](#\xE7\x8E\xAF\xE5\xA2\x83\xE5\x8F\x98\xE9\x87\x8F\xE8\xAE\xBE\xE7\xBD\xAE)
         * [编译环境准备](#\xE7\xBC\x96\xE8\xAF\x91\xE7\x8E\xAF\xE5\xA2\x83\xE5\x87\x86\xE5\xA4\x87)
         * [示例代码](#\xE7\xA4\xBA\xE4\xBE\x8B\xE4\xBB\xA3\xE7\xA0\x81-1)
         * [插入记录](#\xE6\x8F\x92\xE5\x85\xA5\xE8\xAE\xB0\xE5\xBD\x95)
         * [查询记录](#\xE6\x9F\xA5\xE8\xAF\xA2\xE8\xAE\xB0\xE5\xBD\x95)
         * [修改记录](#\xE4\xBF\xAE\xE6\x94\xB9\xE8\xAE\xB0\xE5\xBD\x95)
         * [删除记录](#\xE5\x88\xA0\xE9\x99\xA4\xE8\xAE\xB0\xE5\xBD\x95)
# TDR说明
TDR(Tencent Data Representation), 腾讯自研的跨平台多语言数据表示协议，主要用于数据的序列化反序列化，以XML文件形式来定义表结构。具体关于TDR表的定义说明可参考: [TDR表定义](https://cloud.tencent.com/document/product/596/44407)。

# 入门
快速入手TDR协议表的开发涉及几个步骤，下面介绍如何基于TcalusDB本地Docker版环境，快速上手基于C++进行TDR表的增删查改操作。所有操作均在申请的开发测试机或云主机进行。
## Docker环境准备
在开始示例代码演示之前，需要提前准备好TcaplusDB本地Docker环境及tcapluscli工具，具体请参考资料：[TcaplusDB入门-Docker部署篇.md](https://github.com/tencentyun/tcaplusdb-documents/blob/main/docker/TcaplusDB%E5%85%A5%E9%97%A8-Docker%E9%83%A8%E7%BD%B2%E7%AF%87.md)。
Docker部署好后，对于命令行工具需要授权所有IP访问Docker环境，授权方式:
```
./tcapluscli privilege --endpoint-url=http://localhost --allow-all-ip

# 约束限制
|编号|资源|限制|
|---|---|---|
|1|单表格组允许表格数|256|
|2|单表大小|2.56PB|
|3|Generic表主键字段数|8|
|4|Generic表字段数|256|
|5|List表主键字段数|7|
|6|List表字段数|255|
|7|List表中元素个数,单key对应链表元素个数|10000|
|8|表名长度|32Bytes|
|9|字段名长度|32Bytes|
|10|主键字段值大小|1KB|
|11|非主键字段值大小|10MB|
|12|单记录大小|10MB|
|13|批量查询返回记录数|1024|

```

## TcaplusDB表准备
### 准备TDR表示例文件
这里以table_test.xml举例，表名: PLAYERONLINECNT，　表类型: GENERIC。文件具体内容如下：
```
<?xml version="1.0" encoding="GBK" standalone="yes" ?>
<metalib name="tcaplus_tb" tagsetversion="1" version="1">
<struct name="PLAYERONLINECNT" version="1" primarykey="TimeStamp,GameSvrID" splittablekey="TimeStamp">
    <entry name="TimeStamp"         type="uint32"     desc="单位为分钟" />
    <entry name="GameSvrID"         type="string"     size="64" />
    <entry name="GameAppID"         type="string"     size="64" desc="gameapp id" />
    <entry name="OnlineCntIOS"      type="uint32"     defaultvalue="0" desc="ios在线人数" />
    <entry name="OnlineCntAndroid"  type="uint32"     defaultvalue="0" desc="android在线人数" />
    <entry name="BinaryLen"         type="smalluint" defaultvalue="1" desc="数据来源数据长度；长度为0时，忽略来源检查"/>
    <entry name="binary"            type="tinyint"     desc="二进制" count= "1000" refer="BinaryLen" />
    <entry name="binary2"           type="tinyint"     desc="二进制2" count= "1000" refer="BinaryLen" />
    <entry name="strstr"            type="string"     size="64" desc="字符串"/>
    <index name="index_id"  column="TimeStamp"/>
</struct>
</metalib>
```
### TcaplusDB集群准备
对于TcaplusDB,在创建表之前需要创建对应的表集群。对于Docker本地版，集群已经默认创建好一个供大家使用，所以不用再创建集群。
对于TDR集群，已经默认创建一个`test_app`的业务，集群接入ID (AccessID) 默认为`2`。集群密码(AccessPassword)查看可打开TcaplusDB运维平台，打开方式：｀直接浏览器输入部署docker的机器ip即可，默认端口80`。默认登录方式：
```
用户名：　tcaplus
密码：　tcaplus
```
登录后，进入`业务管理->业务维护->选择业务名称，这里默认选test_app业务,在对应行的右侧点击查看密码即可`。
### TcaplusDB表格组准备
TcaplusDB表在集群的基础上还依赖于表格组，相当于游戏里的逻辑分区，使用工具创建表格组命令如下：
```
#查看表格组帮助命令
./tcapluscli tablegroup -h

#创建一个表格组，id指定为4，　endpoint-url为上面docker暴露的80端口，access-id为集群访问id (业务id, tdr集群默认为2), 用于docker环境连接使用, group name由字母、数字和下划线组成
./tcapluscli tablegroup create --endpoint-url=http://localhost --access-id=2 --group-id=4　--group-name=zone_4
```

### TcaplusDB表创建
现在正式进入表创建环节，在上述表格组基础上创建一个TDR表，执行创建表命令，如下所示：
```
#查看表创建命令提示帮助
./tcapluscli table -h

#创建一个表, 指定endpoint-url, 表格组id: group-id, 表类型: TDR, 表定义文件: table_test.xml,放当前路径
./tcapluscli table create  create --endpoint-url=http://localhost --access-id=2 --group-id=4 --schema-type=TDR --schema-file=table_test.xml
```

## 示例代码
以C++示例代码为例，介绍如何使用TDR接口进行TcaplusDB表数据操作，这里主要介绍Generic类型表操作。示例代码包括两种模式：同步模式 和异步模式，这里主要介绍异步模式示例代码，同步模式代码可查看SDK中examples目录相关代码。

### SDK准备
这里以3.46.0版本为示例演示，需要下载两个组件：
|组件名|下载地址|用途|
|---|---|---|
|TcaplusServiceApi3.46.1.199000.x86_64_release_20201102|[Download](https://tcaplus-tool-1302668961.cos.ap-shanghai.myqcloud.com/sdk/TcaplusServiceApi3.46.1.199000.x86_64_release_20201102.tar.gz)|SDK API相关代码|
|TSF4G_BASE-2.7.37.0a1db41b8_X86_64_Release|[Download](https://tcaplus-tool-1302668961.cos.ap-shanghai.myqcloud.com/sdk/TSF4G_BASE-2.7.37.0a1db41b8_X86_64_Release.tar.gz)| tdr依赖包相关代码|

#### TcaplusServiceApi部署方式
TcaplusServiceApi3.46.1.199000.x86_64_release_20201102下载后，直接解压至目标机器相应目录即可，如:
```
/root/TcaplusServiceApi3.46.1.199000.x86_64_release_20201102
```
#### TSF4G部署方式
TSF4G_BASE-2.7.37.0a1db41b8_X86_64_Release下载后，解压后把子目录内的内容拷贝至/usr/local/tsf4g_release即可，如：
```
#创建目标部署目录
[root@VM-32-2-centos ~]# mkdir /usr/local/tsf4g_release

#进子目录
[root@VM-32-2-centos ~]# cd /root/TSF4G_BASE-2.7.37.0a1db41b8_X86_64_Release/release/x86_64


#拷贝至/usr/local/tsf4g_release
[root@VM-32-2-centos x86_64]# cp -rf * /usr/local/tsf4g_release
```

#### 环境变量设置
主要设置两个环境变量: TSF4G_HOME和TCAPLUS_HOME，　把下面内容添加至/root/.bashrc，然后执行source /root/.bashrc使其生效，如下所示:

```
export TSF4G_HOME=/usr/local/tsf4g_release/
export TCAPLUS_HOME=/root/TcaplusServiceApi3.46.1.199000.x86_64_release_20201102/release/x86_64
```

### 编译环境准备
|环境依赖|版本|说明|
|---|---|---|
|操作系统|CentOS7|x86体系|
|C++|C++98||
|GCC|4.8.5||
|gcc-c++|4.8.5|yum install -y gcc-c++|
|tsf4g|2.7.37|tdr相关依赖,参考上述TSF4G部署方式描述|

### 示例代码
示例代码目录在SDK目录下的examples子目录下，examples目录有很多不同类型的表示例，下面主要以C++_tdr1.0_asyncmode_generic_simpletable/SingleOperation子目录的来举例，具体子目录如下：
```
#SDK根目录
/root/TcaplusServiceApi3.46.1.199000.x86_64_release_20201102/release/x86_64

#SDK公共配置目录
/root/TcaplusServiceApi3.46.1.199000.x86_64_release_20201102/release/x86_64/examples/tcaplus/C++_common_for_tdr1.0

#SDK Examples目录
/root/TcaplusServiceApi3.46.1.199000.x86_64_release_20201102/release/x86_64/examples/tcaplus/C++_tdr1.0_asyncmode_generic_simpletable/SingleOperation
```

在编译具体示例代码之前，需要把本地版中关于公共参数配置的集群信息进行配置，配置目录在examples/tcaplus/C++_common_for_tdr1.0下，主要涉及如下：

```
#进入公共信息配置目录
cd TcaplusServiceApi3.46.0.198987.x86_64_release_20200925/release/x86_64/examples/tcaplus/C++_common_for_tdr1.0

#修改common.h，将相关集群信息改为本地版中集群相关信息

/*********************测试例子前需要用户手动修改的地方BEGIN**************************************/

// 目标业务的tcapdir地址,即集群访问地址，云上环境：参考云控制台集群详情页，docker环境：配置为docker部署ip
static const char DIR_URL_ARRAY[][TCAPLUS_MAX_STRING_LENGTH] =
{
    "tcp://0.0.0.0:9999"
};
// 目标业务的tcapdir 地址个数, 配置成：1, 对于云上版本和Docker版本默认地址就1个
static const int32_t DIR_URL_COUNT = 1;

// 目标业务的表名 PLAYERONLINECNT
static const char * TABLE_NAME = "PLAYERONLINECNT";

// 目标业务的App ID　(接入ID,Access ID), TcaplusDB本地版APP_ID默认为2，用户测试可以不用修改
static const int32_t APP_ID = 2;

// 目标业务的Zone ID (游戏区ID, Table Group ID), 参考上述章节创建的group id
static const int32_t ZONE_ID = 4;

// 目标业务的业务密码,访问TcaplusDB Web控制台在业务管理->业务维护-> 选择test_app业务，查看密码
static const char * SIGNATURE = "39859BC573A2E254";
/*********************测试例子前需要用户手动修改的地方END**************************************/
```

在examples/tcaplus/C++_tdr1.0_asyncmode_generic_simpletable/SingleOperation目录下每个操作子目录下的文件有几个:
|文件名|文件用途|
|---|---|
|conv.sh|依据表定义文件table_test.xml生成表定义接口文件table_test.h, table_test.cpp, 借助SDK根目录下bin目录下的tdr工具来生成|
|envcfg.env|环境变量设置，主要设置TCAPLUS_HOME和TSF4G_HOME环境变量，设置为SDK根目录，在编译之前执行source envcfg.env，使环境变量生效|
|main.cpp|示例代码主程序|
|Makefile|编译文件|
|readme.txt|编译操作提示说明|
|table_test.xml|示例表TDR定义文件|
|tlogconf.xml|日志配置文件，默认不需改动|

### 插入记录
示例代码目录：
```
/root/TcaplusServiceApi3.46.1.199000.x86_64_release_20201102/release/x86_64/examples/tcaplus/C++_tdr1.0_asyncmode_generic_simpletable/SingleOperation/insert
```

生成表定义接口文件，如下:

```
#生成table_test.h, table_test.cpp
sh conv.sh

#检查Makefile，重点是LIBS和INC两个变量，参考下面设置
LIBS += -L$(TSF4G_HOME)/lib -L$(TCAPLUS_HOME)/lib -Wl,-Bstatic -ltcaplusserviceapi -ltsf4g_r -lreadline -lncurses  -lscew -lexpat -Wl,-Bdynamic -lpthread -lanl
INC = -I$(TSF4G_HOME)/include -I$(TCAPLUS_HOME)/include/tcaplus_service -I../../../C++_common_for_tdr1.0

#编译
make

#执行编译好的二进制文件，插入记录,如果有问题，请联系TcaplusDB项目组相关人员协助定位问题
./mytest
```

### 查询记录
示例代码目录:
```
/root/TcaplusServiceApi3.46.1.199000.x86_64_release_20201102/release/x86_64/examples/tcaplus/C++_tdr1.0_asyncmode_generic_simpletable/SingleOperation/get
```
编译过程：
```
#生成table_test.h, table_test.cpp
sh conv.sh

#检查Makefile，重点是LIBS和INC两个变量
LIBS += -L$(TSF4G_HOME)/lib -L$(TCAPLUS_HOME)/lib -Wl,-Bstatic -ltcaplusserviceapi -ltsf4g_r -lreadline -lncurses  -lscew -lexpat -Wl,-Bdynamic -lpthread -lanl
INC = -I$(TSF4G_HOME)/include -I$(TCAPLUS_HOME)/include/tcaplus_service -I../../../C++_common_for_tdr1.0

#编译
make

#执行编译好的二进制文件，插入记录,如果有问题，请联系TcaplusDB项目组相关人员协助定位问题
./mytest


#编译 make #执行编译好的二进制文件，插入记录,如果有问题，请联系TcaplusDB项目组相关人员协助定位问题 ./mytest
```

### 修改记录
示例代码目录:
```
/root/TcaplusServiceApi3.46.1.199000.x86_64_release_20201102/release/x86_64/examples/tcaplus/C++_tdr1.0_asyncmode_generic_simpletable/SingleOperation/update
```
编译过程：
```
#生成table_test.h, table_test.cpp
sh conv.sh

#检查Makefile，重点是LIBS和INC两个变量
LIBS += -L$(TSF4G_HOME)/lib -L$(TCAPLUS_HOME)/lib -Wl,-Bstatic -ltcaplusserviceapi -ltsf4g_r -lreadline -lncurses  -lscew -lexpat -Wl,-Bdynamic -lpthread -lanl
INC = -I$(TSF4G_HOME)/include -I$(TCAPLUS_HOME)/include/tcaplus_service -I../../../C++_common_for_tdr1.0

#编译
make

#执行编译好的二进制文件，插入记录,如果有问题，请联系TcaplusDB项目组相关人员协助定位问题
./mytest

```

### 删除记录
示例代码目录：
```
/root/TcaplusServiceApi3.46.1.199000.x86_64_release_20201102/release/x86_64/examples/tcaplus/C++_tdr1.0_asyncmode_generic_simpletable/SingleOperation/delete
```
编译过程:
```
#生成table_test.h, table_test.cpp
sh conv.sh

#检查Makefile，重点是LIBS和INC两个变量
LIBS += -L$(TSF4G_HOME)/lib -L$(TCAPLUS_HOME)/lib -Wl,-Bstatic -ltcaplusserviceapi -ltsf4g_r -lreadline -lncurses  -lscew -lexpat -Wl,-Bdynamic -lpthread -lanl
INC = -I$(TSF4G_HOME)/include -I$(TCAPLUS_HOME)/include/tcaplus_service -I../../../C++_common_for_tdr1.0

#编译
make

#执行编译好的二进制文件，插入记录,如果有问题，请联系TcaplusDB项目组相关人员协助定位问题
./mytest

```
