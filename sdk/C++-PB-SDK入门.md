# TcaplusDB PB SDK for C++ Quickstart

# Table of Contents

- [TcaplusDB PB SDK for C++ Quickstart](#tcaplusdb-pb-sdk-for-c-quickstart)
- [Table of Contents](#table-of-contents)
- [1. PROTOBUF 说明](#1-protobuf-\xE8\xAF\xB4\xE6\x98\x8E)
- [2. 入门](#2-\xE5\x85\xA5\xE9\x97\xA8)
- [3. 约束限制](#3-\xE7\xBA\xA6\xE6\x9D\x9F\xE9\x99\x90\xE5\x88\xB6)
- [4. 环境要求](#4-\xE7\x8E\xAF\xE5\xA2\x83\xE8\xA6\x81\xE6\xB1\x82)
  - [4.1 SDK 部署环境要求](#41-sdk-\xE9\x83\xA8\xE7\xBD\xB2\xE7\x8E\xAF\xE5\xA2\x83\xE8\xA6\x81\xE6\xB1\x82)
  - [4.2 其它要求 (非必须)](#42-\xE5\x85\xB6\xE5\xAE\x83\xE8\xA6\x81\xE6\xB1\x82-\xE9\x9D\x9E\xE5\xBF\x85\xE9\xA1\xBB)
- [5. SDK 依赖安装](#5-sdk-\xE4\xBE\x9D\xE8\xB5\x96\xE5\xAE\x89\xE8\xA3\x85)
  - [5.1 资源准备](#51-\xE8\xB5\x84\xE6\xBA\x90\xE5\x87\x86\xE5\xA4\x87)
  - [5.2 系统依赖库安装](#52-\xE7\xB3\xBB\xE7\xBB\x9F\xE4\xBE\x9D\xE8\xB5\x96\xE5\xBA\x93\xE5\xAE\x89\xE8\xA3\x85)
  - [5.3 protobuf 安装](#53-protobuf-\xE5\xAE\x89\xE8\xA3\x85)
  - [5.4 SDK 依赖安装](#54-sdk-\xE4\xBE\x9D\xE8\xB5\x96\xE5\xAE\x89\xE8\xA3\x85)
- [6. TcaplusDB 资源准备](#6-tcaplusdb-\xE8\xB5\x84\xE6\xBA\x90\xE5\x87\x86\xE5\xA4\x87)
  - [6.1 TcaplusDB 表准备](#61-tcaplusdb-\xE8\xA1\xA8\xE5\x87\x86\xE5\xA4\x87)
    - [6.1.1 准备 PROTO 表示例文件](#611-\xE5\x87\x86\xE5\xA4\x87-proto-\xE8\xA1\xA8\xE7\xA4\xBA\xE4\xBE\x8B\xE6\x96\x87\xE4\xBB\xB6)
    - [6.1.2 TcaplusDB 表创建-腾讯云环境](#612-tcaplusdb-\xE8\xA1\xA8\xE5\x88\x9B\xE5\xBB\xBA-\xE8\x85\xBE\xE8\xAE\xAF\xE4\xBA\x91\xE7\x8E\xAF\xE5\xA2\x83)
      - [步骤 1-集群创建](#\xE6\xAD\xA5\xE9\xAA\xA4-1-\xE9\x9B\x86\xE7\xBE\xA4\xE5\x88\x9B\xE5\xBB\xBA)
      - [步骤 2-表格组创建](#\xE6\xAD\xA5\xE9\xAA\xA4-2-\xE8\xA1\xA8\xE6\xA0\xBC\xE7\xBB\x84\xE5\x88\x9B\xE5\xBB\xBA)
    - [步骤 3-表创建](#\xE6\xAD\xA5\xE9\xAA\xA4-3-\xE8\xA1\xA8\xE5\x88\x9B\xE5\xBB\xBA)
    - [TcaplusDB 表创建-Docker 环境](#tcaplusdb-\xE8\xA1\xA8\xE5\x88\x9B\xE5\xBB\xBA-docker-\xE7\x8E\xAF\xE5\xA2\x83)
      - [步骤 1-获取集群信息](#\xE6\xAD\xA5\xE9\xAA\xA4-1-\xE8\x8E\xB7\xE5\x8F\x96\xE9\x9B\x86\xE7\xBE\xA4\xE4\xBF\xA1\xE6\x81\xAF)
      - [步骤 2-获取表格组信息](#\xE6\xAD\xA5\xE9\xAA\xA4-2-\xE8\x8E\xB7\xE5\x8F\x96\xE8\xA1\xA8\xE6\xA0\xBC\xE7\xBB\x84\xE4\xBF\xA1\xE6\x81\xAF)
      - [步骤 3-创建表](#\xE6\xAD\xA5\xE9\xAA\xA4-3-\xE5\x88\x9B\xE5\xBB\xBA\xE8\xA1\xA8)
  - [6.2 示例代码](#62-\xE7\xA4\xBA\xE4\xBE\x8B\xE4\xBB\xA3\xE7\xA0\x81)
    - [步骤 1-SDK 下载](#\xE6\xAD\xA5\xE9\xAA\xA4-1-sdk-\xE4\xB8\x8B\xE8\xBD\xBD)
    - [步骤 2-集群连接信息获取](#\xE6\xAD\xA5\xE9\xAA\xA4-2-\xE9\x9B\x86\xE7\xBE\xA4\xE8\xBF\x9E\xE6\x8E\xA5\xE4\xBF\xA1\xE6\x81\xAF\xE8\x8E\xB7\xE5\x8F\x96)
    - [步骤 3-配置环境变量](#\xE6\xAD\xA5\xE9\xAA\xA4-3-\xE9\x85\x8D\xE7\xBD\xAE\xE7\x8E\xAF\xE5\xA2\x83\xE5\x8F\x98\xE9\x87\x8F)
    - [步骤 4-生成表接口定义代码](#\xE6\xAD\xA5\xE9\xAA\xA4-4-\xE7\x94\x9F\xE6\x88\x90\xE8\xA1\xA8\xE6\x8E\xA5\xE5\x8F\xA3\xE5\xAE\x9A\xE4\xB9\x89\xE4\xBB\xA3\xE7\xA0\x81)
    - [步骤 5-修改 Makefile](#\xE6\xAD\xA5\xE9\xAA\xA4-5-\xE4\xBF\xAE\xE6\x94\xB9-makefile)
    - [步骤 6-执行示例](#\xE6\xAD\xA5\xE9\xAA\xA4-6-\xE6\x89\xA7\xE8\xA1\x8C\xE7\xA4\xBA\xE4\xBE\x8B)
    - [步骤 7-查看插入数据](#\xE6\xAD\xA5\xE9\xAA\xA4-7-\xE6\x9F\xA5\xE7\x9C\x8B\xE6\x8F\x92\xE5\x85\xA5\xE6\x95\xB0\xE6\x8D\xAE)

# 1. PROTOBUF 说明

PB (PROTO) 表是基于 PROTOBUF 协议设计的 TcaplusDB 表，PROTOBUF 协议是 Google 开源的通用 RPC 通信协议，用于 TcaplusDB 存储数据的序列化、反序列化等操作，具体关于 PROTO 表的定义说明可参考章节：[PROTO 表定义](https://cloud.tencent.com/document/product/596/44406)。PROTO 表定义以 protobuf 格式来定义表结构，支持丰富的数据类型, 可参考 protobuf 支持的类型。

# 2. 入门

快速入手 PROTOBUF 协议表的开发涉及几个步骤，下面介绍如何基于 TcalusDB 的腾讯云环境或本地 Docker 版环境，快速上手基于 C++ 进行 PROTO 表的增删查改操作。所有操作均在申请的开发测试机或云主机进行。

# 3. 约束限制

| 编号 | 资源                                      | 限制    |
| ---- | ----------------------------------------- | ------- |
| 1    | 单表格组允许表格数                        | 256     |
| 2    | 单表大小                                  | 2.56PB  |
| 3    | Generic 表主键字段数                      | 8       |
| 4    | Generic 表字段数                          | 256     |
| 5    | List 表主键字段数                         | 7       |
| 6    | List 表字段数                             | 255     |
| 7    | List 表中元素个数,单 key 对应链表元素个数 | 10000   |
| 8    | 表名长度                                  | 32Bytes |
| 9    | 字段名长度                                | 32Bytes |
| 10   | 主键字段值大小                            | 1KB     |
| 11   | 非主键字段值大小                          | 10MB    |
| 12   | 单记录大小                                | 10MB    |
| 13   | 批量查询返回记录数                        | 1024    |

# 4. 环境要求

## 4.1 SDK 部署环境要求

- **机器配置**: 建议 2c4g, Centos7 64bit, 腾讯云 CVM
- **GCC 版本**: 4.8.5 以上
- **protobuf 版本**: 3.5.1 以上
- **TcaplusDB SDK 版本** : 3.46.0 以上

## 4.2 其它要求 (非必须)

TcaplusDB 除了腾讯云环境,也支持本地版环境.本地环境主要用于开发调试. 对于本地版环境,需要额外部署 docker 环境.
具体请参考资料：[TcaplusDB 入门-Docker 部署篇.md](https://github.com/tencentyun/tcaplusdb-documents/blob/main/docker/TcaplusDB%E5%85%A5%E9%97%A8-Docker%E9%83%A8%E7%BD%B2%E7%AF%87.md)。

如果是基于 Docker 本地版进行 SDK 调试, 可以参考上述链接文档进行相关操作. TcaplusDB 还提供了 tcapluscli 工具进行相关资源操作, 如资源创建,删除,查看. 具体可查阅:[tcapluscli 手册](https://github.com/tencentyun/tcaplusdb-documents/tree/main/tcapluscli).

# 5. SDK 依赖安装

## 5.1 资源准备

| 资源名称           | 版本   | 资源说明           | 下载地址                                                                                                                                    |
| ------------------ | ------ | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| TcaplusPbApi3.46.0 | 3.46.0 | PB SDK             | [下载](https://tcaplusdb-sdk-1301716906.cos.ap-shanghai.myqcloud.com/release/3-46/TcaplusPbApi3.46.0.199033.x86_64_release_20201210.tar.gz) |
| protobuf           | 3.5.1  | protobuf 库,protoc | [下载](https://tcaplus-tool-1302668961.cos.ap-shanghai.myqcloud.com/sdk/protobuf-cpp-3.5.1.tar.gz)                                          |

## 5.2 系统依赖库安装

SDK 依赖一些系统库,主要如下:

- autoconf
- automake
- libtool
- curl
- make
- g++
- unzip
- gcc-c++
- openssl
- openssl-devel
- zlib-devel

可通过 yum 一建安装:

```
yum install -y autoconf automake libtool curl make g++ unzip gcc-c++ openssl openssl-devel zlib-devel
```

依赖安装完后,有一些 lib 需要手动创建一下软链:

```
cd /usr/lib64

#查看当前libcrypto.so前缀库名
ls libcrypto.so.*

#以libcrypto.so.1.0.2k举例,将此so建软链
ln -s /usr/lib64/libcrypto.so.1.0.2k /usr/lib64/libcrypto.so
```

## 5.3 protobuf 安装

从上述下载好的源码包解压，具体源码编译安装步骤:

```
./configure --prefix=/usr/local/protobuf
make
make check
make install
```

注:如果编译过程有问题,一般是少系统库问题,可参阅网上资料解决.

安装完后, 可测试是否 ok. 进/usr/local/protocbuf/bin 目录,执行:

```
./protoc --version
# 显示版本号
libprotoc 3.5.0
```

## 5.4 SDK 依赖安装

参考 5.1 下载 TcaplusDB C++ PB SDK 并解压。

# 6. TcaplusDB 资源准备

## 6.1 TcaplusDB 表准备

### 6.1.1 准备 PROTO 表示例文件

这里以示例中的 table_test.proto 举例，表名: `tb_online`, 表类型: `GENERIC`。文件具体内容如下：

```
syntax = "proto3";

package myTcaplusTable;
//import tcaplusdb extensions
import "tcaplusservice.optionv1.proto";

message tb_online {
    //define primary key
    option(tcaplusservice.tcaplus_primary_key) = "openid,tconndid,timekey";
    //primary key fields
    int32 openid = 1; //QQ Uin
    int32 tconndid = 2;
    string timekey = 3;

    //non-primary key fields
    string gamesvrid = 4;
    int32 logintime = 5 ;
    repeated int64 lockid = 6;
    pay_info pay = 7;

    message pay_info {
        uint64 total_money = 1;
        uint64 pay_times = 2;
    }
    map<string, pay_info> projects = 8;
}
```

将上述文件内容保存为`table_test.proto`。

### 6.1.2 TcaplusDB 表创建-腾讯云环境

#### 步骤 1-集群创建

参考, [创建集群](https://cloud.tencent.com/document/product/596/38807)

#### 步骤 2-表格组创建

参考, [创建表格组](https://cloud.tencent.com/document/product/596/38809)

### 步骤 3-表创建

参考, [创建表](https://cloud.tencent.com/document/product/596/38808)

### TcaplusDB 表创建-Docker 环境

对于需要本地开发调试的用户,TcaplusDB 也提供 docker 环境进行示例操作.

#### 步骤 1-获取集群信息

查看 PROTO 集群(业务)信息. 对于 Docker 本地版，集群已经默认创建好一个供大家使用，所以不用再创建集群。对于 PROTO 集群，已经默认创建一个`pb_app`的业务，集群接入 ID (AccessID) 默认为`3`。

#### 步骤 2-获取表格组信息

查看表格组(游戏区)信息. 对于 Docker 本地版, 已经默认在`pb_app`集群(业务)下创建了一个表格组(游戏区), 默认 ID 为`1`. 如果用户想自行创建, 可参考[tcapluscli 手册](https://github.com/tencentyun/tcaplusdb-documents/tree/main/tcapluscli), 手册中有描述如何创建表格组.

#### 步骤 3-创建表

创建表. 针对上述示例 proto 表定义文件 table_test.proto. 用 tcapluscli 工具进行创建.

```
#查看表创建命令提示帮助
./tcapluscli table -h

#创建一个表, 指定endpoint-url, 表格组id: group-id, 表类型: PROTO, 表定义文件:table_test.proto, 放当前路径, 如果用户自行创建了表格组,则group-id替换为自己创建的id即可
./tcapluscli table create  create --endpoint-url=http://localhost --access-id=3 --group-id=1 --schema-type=PROTO --schema-file=table_test.proto
```

备注:如果表创建提示失败, 一般有几类原因:

- Error1: 集群 access-id 出错, 默认为 3, 填其它如果未创建有问题
- Error2: 表格组 group-id 出错, 默认为 1, 如果用户自己创建了表格组,这里需要替换为自己创建的表格组 id
- Error3: schema-type 出错, 业务默认为 PROTO, 如果填其它则报错
- Error4: schema-file 出错, 可能找不到 shcema file, 建议把 proto 文件放在和 tcapluscli 同目录下

## 6.2 示例代码

以 C++ 示例代码为例，介绍如何使用 PROTOBUF 接口进行 TcaplusDB 表数据操作，这里主要介绍 Generic 类型表操作。

### 步骤 1-SDK 下载

参考上述 SDK 下载部分. 解压 SDK 到机器对应目录.

```
cd /root
tar zxvf TcaplusPbApi3.46.0.199033.x86_64_release_20201210.tar.gz
```

### 步骤 2-集群连接信息获取

示例代码中涉及连接 TcalusDB 部分, 连接信息配置在 SDK 目录:

```
/root/TcaplusPbApi3.46.0.199033.x86_64_release_20201210/release/x86_64/examples/tcaplus/C++_common_for_pb2/common.h
```

修改配置信息如下:

```
/*********************测试例子前需要用户手动修改的地方BEGIN**************************************/
// 目标业务的tcapdir地址, 即集群地址, 获取方式, 腾讯云参考: https://cloud.tencent.com/document/product/596/31652, docker环境: ip (docerk机器ip):9999
static const char DIR_URL_ARRAY[][TCAPLUS_MAX_STRING_LENGTH] =
{
    "tcp://172.17.32.17:9999",
};

// 目标业务的tcapdir 地址个数, 默认1个
static const int32_t DIR_URL_COUNT = 1;
// 目标业务的表名
static const char * TABLE_NAME = "tb_online";
// 目标业务的App ID, 即集群接入ID, 腾讯云参考: https://cloud.tencent.com/document/product/596/31652, docker环境: 3 (默认proto业务)
static const int32_t APP_ID = 70;
// 目标业务的Zone ID, 表格组id, 腾讯云参考: https://cloud.tencent.com/document/product/596/31652, docker环境: 1 (默认1, 也可自行创建)
static const int32_t ZONE_ID = 1;
// 目标业务的业务密码, 腾讯云参考: https://cloud.tencent.com/document/product/596/31652, docker环境: 进web控制台,业务管理->业务维护->选中pb_app业务,查看密码
static const char * SIGNATURE = "Tcaplus@2020";
/*********************测试例子前需要用户手动修改的地方END**************************************/
```

### 步骤 3-配置环境变量

以 SDK 中如下目录示例举例:

```
#pb3协议, add操作
/root/TcaplusPbApi3.46.0.199033.x86_64_release_20201210/release/x86_64/examples/tcaplus/C++_pb3_coroutine_simpletable/SingleOperation/add
```

- **envcfg.env**: 配置 PROTOBUF_HOME 、TCAPLUS_HOME 、TSF4G_HOME 环境变量

```
export PROTOBUF_HOME=/usr/local/protobuf;
export TCAPLUS_HOME=/root/TcaplusPbApi3.46.0.199033.x86_64_release_20201210/release/x86_64/;
```

### 步骤 4-生成表接口定义代码

- **示例目录**

```
cd /root/TcaplusPbApi3.46.0.199033.x86_64_release_20201210/release/x86_64/examples/tcaplus/C++_pb3_coroutine_simpletable/SingleOperation/add
```

- **生成接口定义**

```
sh conv.sh
```

### 步骤 5-修改 Makefile

示例:

```
# ================================================================
#Makefile for tcaplus example
#
# Date:   2016-09-14
#
# Copyright (C) 2016 Architechure IRED TENCENT
#
# ================================================================

CPPFILE=$(wildcard *.cpp)
CCFILE=$(wildcard *.cc)

#GENERATE_FILE=$(shell ./conv.sh )

# ${PROTOBUF_HOME} change to ${PROTOBUF_HOME}/lib
LIBS += -L $(PROTOBUF_HOME)/lib -L$(TCAPLUS_HOME)/lib -Wl,-Bstatic -ltcaplusprotobufapi -lprotobuf -lscew -lexpat -Wl,-Bdynamic -lpthread -lz -ldl -lcrypto -lanl
# ${PROTOBUF_HOME} change to ${PROTOBUF_HOME}/include
INC =-I$(PROTOBUF_HOME)/include -I$(TCAPLUS_HOME)/include/tcaplus_pb_api/ -I../../../C++_common_for_pb2


.PHONY: all clean

all:
	g++ -o mytest $(CCFILE) $(CPPFILE) $(INC) ${LIBS}

clean:
	rm -f mytest     mytest.log*
```

### 步骤 6-执行示例

```
./mytest
```

### 步骤 7-查看插入数据

TcaplusDB 提供有 tcaplus_client 工具进行数据查看, 在 SDK 目录下有对应的`tcaplus_client`. 具体使用参考:[使用 client 工具访问数据](https://cloud.tencent.com/document/product/596/49674).

```
cd /root/TcaplusPbApi3.46.0.199033.x86_64_release_20201210/release/x86_64/bin
#demo connect, -a: access-id/appid, -z: tablegroup-id/zone-id, -s: password, -d: connect address
./tcaplus_client -a 3 -z 1 -s ABCDEFEFEF -d 172.17.12.1:9999

#demo select, specify primary key of table
select * from tb_online where openid=1 and tconndid=2 and timekey='12345';
```

备注: **tcaplus_client 和 tcapluscli 的区别是: tcaplus_client 主要是数据层的操作工具( 如数据的增删查改), tcapluscli 主要是资源层的操作(如表/表格组/集群的创建,删除,查询)**
