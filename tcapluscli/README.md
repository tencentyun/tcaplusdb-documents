# TcaplusDB Console Client工具
Table of Contents
=================

   * [TcaplusDB Console Client工具](#tcaplusdb-console-client\xE5\xB7\xA5\xE5\x85\xB7)
   * [1.前言](#1\xE5\x89\x8D\xE8\xA8\x80)
   * [2. 工具说明](#2-\xE5\xB7\xA5\xE5\x85\xB7\xE8\xAF\xB4\xE6\x98\x8E)
      * [2.1 工具下载](#21-\xE5\xB7\xA5\xE5\x85\xB7\xE4\xB8\x8B\xE8\xBD\xBD)
      * [2.2 编译工具包说明](#22-\xE7\xBC\x96\xE8\xAF\x91\xE5\xB7\xA5\xE5\x85\xB7\xE5\x8C\x85\xE8\xAF\xB4\xE6\x98\x8E)
      * [2.2 配置依赖](#22-\xE9\x85\x8D\xE7\xBD\xAE\xE4\xBE\x9D\xE8\xB5\x96)
      * [2.3 工具授权](#23-\xE5\xB7\xA5\xE5\x85\xB7\xE6\x8E\x88\xE6\x9D\x83)
   * [3. 命令](#3-\xE5\x91\xBD\xE4\xBB\xA4)
      * [3.1 Secret信息设置](#31-secret\xE4\xBF\xA1\xE6\x81\xAF\xE8\xAE\xBE\xE7\xBD\xAE)
      * [3.2 集群相关命令](#32-\xE9\x9B\x86\xE7\xBE\xA4\xE7\x9B\xB8\xE5\x85\xB3\xE5\x91\xBD\xE4\xBB\xA4)
         * [3.2.1 创建集群](#321-\xE5\x88\x9B\xE5\xBB\xBA\xE9\x9B\x86\xE7\xBE\xA4)
         * [3.2.2 删除集群](#322-\xE5\x88\xA0\xE9\x99\xA4\xE9\x9B\x86\xE7\xBE\xA4)
         * [3.2.3 查看集群信息](#323-\xE6\x9F\xA5\xE7\x9C\x8B\xE9\x9B\x86\xE7\xBE\xA4\xE4\xBF\xA1\xE6\x81\xAF)
      * [3.3 表格组相关命令](#33-\xE8\xA1\xA8\xE6\xA0\xBC\xE7\xBB\x84\xE7\x9B\xB8\xE5\x85\xB3\xE5\x91\xBD\xE4\xBB\xA4)
         * [3.3.1 创建表格组](#331-\xE5\x88\x9B\xE5\xBB\xBA\xE8\xA1\xA8\xE6\xA0\xBC\xE7\xBB\x84)
         * [3.3.2 删除表格组](#332-\xE5\x88\xA0\xE9\x99\xA4\xE8\xA1\xA8\xE6\xA0\xBC\xE7\xBB\x84)
         * [3.3.3 查看表格组信息](#333-\xE6\x9F\xA5\xE7\x9C\x8B\xE8\xA1\xA8\xE6\xA0\xBC\xE7\xBB\x84\xE4\xBF\xA1\xE6\x81\xAF)
      * [3.4 表相关命令](#34-\xE8\xA1\xA8\xE7\x9B\xB8\xE5\x85\xB3\xE5\x91\xBD\xE4\xBB\xA4)
         * [3.4.1 创建表](#341-\xE5\x88\x9B\xE5\xBB\xBA\xE8\xA1\xA8)
         * [3.4.2 删除表](#342-\xE5\x88\xA0\xE9\x99\xA4\xE8\xA1\xA8)
         * [3.4.3 清除表](#343-\xE6\xB8\x85\xE9\x99\xA4\xE8\xA1\xA8)
         * [3.4.4 获取表信息](#344-\xE8\x8E\xB7\xE5\x8F\x96\xE8\xA1\xA8\xE4\xBF\xA1\xE6\x81\xAF)
         * [3.4.5 恢复删除表](#345-\xE6\x81\xA2\xE5\xA4\x8D\xE5\x88\xA0\xE9\x99\xA4\xE8\xA1\xA8)
         * [3.4.6 表扩容](#346-\xE8\xA1\xA8\xE6\x89\xA9\xE5\xAE\xB9)
         * [3.4.7 批量创建表](#347-\xE6\x89\xB9\xE9\x87\x8F\xE5\x88\x9B\xE5\xBB\xBA\xE8\xA1\xA8)
         * [3.4.8 批量清理表](#348-\xE6\x89\xB9\xE9\x87\x8F\xE6\xB8\x85\xE7\x90\x86\xE8\xA1\xA8)
         * [3.4.9 批量扩容表](#349-\xE6\x89\xB9\xE9\x87\x8F\xE6\x89\xA9\xE5\xAE\xB9\xE8\xA1\xA8)
      * [3.5  授权相关命令](#35--\xE6\x8E\x88\xE6\x9D\x83\xE7\x9B\xB8\xE5\x85\xB3\xE5\x91\xBD\xE4\xBB\xA4)
   * [4. 总结](#4-\xE6\x80\xBB\xE7\xBB\x93)

# 1.前言
TcaplusDB提供了丰富的功能接口来满足不同用户使用TcaplusDB的需求。针对腾讯云控制台一些的操作，目前已将TcaplusDB控制台相关操作集成在TencentCloud SDK中，用户可通过腾讯云API-Explorer平台进行在线代码调试。为进一步满足用户更便利使用控制台相关操作的需求，针对现有的TencentCloud SDK中关于TcaplusDB控制台操作相关的API作进一步的整合，主要目的是提供一个通用工具来实现所有相关控制台的操作，同时又能满足TcaplusDB本地Docker环境相关表操作。

本工具支持腾讯云TcaplusDB控制台相关操作命令，也支持本地版相关操作命令，具体命令如下所示：

* 支持云控制台Secret安全信息设置(secret_id, secret_key)
* 支持云控制台表集群创建、删除、查询
* 支持云控制台表格组创建、删除、查询
* 支持本地Docker版表格组创建、删除、查询
* 支持云控制台表创建、删除、清理、查询、扩容、回收站恢复、批量创建、批量清理、批量扩容
* 支持本地Docker版表创建、删除、清理

# 2. 工具说明
## 2.1 工具下载
|工具名|下载地址|
|---|---|
|编译工具包|[Download](https://tcaplus-tool-1302668961.cos.ap-shanghai.myqcloud.com/tcapluscli/tcapluscli.tgz)|
|工具源码包|[Download](https://tcaplus-tool-1302668961.cos.ap-shanghai.myqcloud.com/tcapluscli/tcapluscli-src.tgz)|
## 2.2 编译工具包说明

工具使用Go-1.15.2编译。为方便用户快速使用工具，针对常见的几个平台已经编译好相关的工具，工具下载后可生命名为`tcapluscli`, 如下：

|操作系统|版本|平台|工具名称|
|---|---|---|---|
|MacOS|Darwin-17.7.0|x86_64|tcapluscli-macos|
|CentOS|7|x86_64|tcapluscli-centos7-i386|
|CentOS|8|x86_64|tcapluscli-centos8-i386|
|Debian|8.2|amd64|tcapluscli-debian8-amd64|
|Windows|10|x86_64|tcapluscli-win10|

其它平台如果需要编译，可通过工具源码包进行下载编译。

## 2.2 配置依赖
工具依赖一些前置配置项，配置包括腾讯云控制台SECRET信息和tcaplusdb的一些公共信息。
文件路径如下:
* __Linux__: /root/.tcaplusdb
* __MacOS__: : /Users/[username]/.tcaplusdb
* __Windows__: C:/Users/[username]/.tcaplusdb

|文件名|文件说明|
|---|---|
|credentials|腾讯云访问账号密钥信息，SECRET_KEY, SECRET_ID|
|config|TcaplusDB公共一些配置项|

credentials文件内容格式如下:
```
[default]
SECRET_ID = xxx
SECRET_KEY = xxx
```

__注意__：__对于使用TcaplusDB Docker本地版的用户，针对`credentials`的设置可以随机设置值，不用反映真实云上的密钥__

## 2.3 工具授权
在使用tcapluscli操作本地Docker版本时，需要授权IP访问Docker版本资源， 授权方式：
```
#以centos平台工具为例, 授权所有IP访问权限
./tcapluscli　  privilege --endpoint-url=http://localhost:5678 --allow-all-ip
```


# 3. 命令
使用工具时，可通过相关帮助提示来辅助命令执行，如下所示:
```
Usage:
  tcapluscli [command]

Available Commands:
  cluster     TcaplusDB cluster operations. [Cloud Env]
  config      Configure  the common information of TcaplusDB CLI
  help        Help about any command
  privilege   Control the access privilege of API [Docker Env]
  table       TcaplusDB table operations.
  tablegroup  TcaplusDB tablegroup operations. [Cloud | Docker]

Flags:
  -h, --help   help for tcapluscli

Use "tcapluscli [command] --help" for more information about a command.
```

## 3.1 Secret信息设置
针对现有设置的ccredentials信息支持重新修改，命令如下:
```
./tcapluscli config set SECRET_KEY　xxx
./tcapluscli config set SECRET_ID xxx
```
获取对应的信息，命令如下:
```
./tcapluscli config get SECRET_KEY
./tcapluscli config get SECRET_ID
```

## 3.2 集群相关命令
集群相关命令只支持腾讯云TcaplusDB版本，不支持本地Docker版环境。获取集群相关命令帮助信息可执行`tcapluscli cluster -h`，如下:
```
Provide baseic commands to operate TcaplusDB cluster. The basic commands includes: create, delete and describe

Usage:
  tcapluscli cluster [flags]
  tcapluscli cluster [command]

Examples:
  ./tcapluscli cluster [create | delete | describe ] [flags]

Available Commands:
  create      Create a TcaplusDB cluster.
  delete      Delete a TcaplusDB cluster. [Cloud Env]
  describe    Get cluster description information. [Cloud Env]

Flags:
  -h, --help   help for cluster

Use "tcapluscli cluster [command] --help" for more information about a command.
```

### 3.2.1 创建集群
执行`tcapluscli cluster create -h`查看相关集群创建信息
```
Create a TcaplusDB cluster. Only work for cloud environment but not docker environment.
Note: The network environment should be confirmed first, such as VPC and Subnet.

Usage:
  tcapluscli cluster create [flags]

Examples:
  ./tcapluscli cluster create [flags]

Flags:
  -n, --cluster-name string       required: cluster name
  -p, --cluster-password string   required:cluster password
  -h, --help                      help for create
  -t, --idl-type string           required: idl type of cluster: PROTO or TDR
  -r, --region string             required: region name of cluster (default "ap-shanghai")
  -S, --subnet-id string          required: subnet id of cluster
  -V, --vpc-id string             required: vpc id of cluster
  ```
  ### 3.2.2 删除集群
  执行`tcapluscli cluster delete -h`查看相关帮助信息
  ```
  Delete a TcaplusDB cluster . Only work for cloud environment but not docker environment.
Note: The network environment should be confirmed first, such as VPC and Subnet.

Usage:
  tcapluscli cluster delete [flags]

Examples:
  ./tcapluscli  cluster delete [flags]

Flags:
  -c, --cluster-id string   required: tcaplusdb cluster id
  -h, --help                help for delete
  -r, --region string       required:region name of table (default "ap-shanghai")
  ```

  ### 3.2.3 查看集群信息
  执行命令`tcapluscli cluster describe -h`可查看相关帮助信息，如下：
  ```
  Get cluster description information. Only work for cloud environment but not docker environment.
If the flag 'cluster-id' is empty, all of cluster infos will be returned. Otherwise the specified cluster info will be returned.

Usage:
  tcapluscli cluster describe [flags]

Examples:
  ./tcapluscli  cluster describe --region=ap-shanghai [--cluster-id=1234445]
  ./tcapluscli cluster describe --region=ap-shanghai --list

Flags:
  -c, --cluster-id string   optional: tcaplusdb cluster id
  -h, --help                help for describe
  -l, --list                list all cluster infos, if 'cluster-id' is empty, 'list' flag must be existed
  -r, --region string       required:region name of table (default "ap-shanghai")
  ```

  ## 3.3 表格组相关命令
  表格组相关命令支持腾讯云TcaplusDB版本和本地Docker版本。执行`tcapluscli tablegroup -h`可查看帮助信息，如下:
  ```
  Provide baseic commands to operate TcaplusDB tablegroup. The basic commands includes: create, delete and describe

Usage:
  tcapluscli tablegroup [flags]
  tcapluscli tablegroup [command]

Examples:
  ./tcapluscli tablegroup [create | delete | describe ] [flags]

Available Commands:
  create      Create a table group of TcaplusDB. [Cloud | Docker ]
  delete      Delete a table group. [Cloud | Docker]
  describe    Get table group description. [Cloud | Docker]

Flags:
  -h, --help   help for tablegroup

Use "tcapluscli tablegroup [command] --help" for more information about a command.
  ```
  ### 3.3.1 创建表格组
  执行  `tcapluscli tablegroup create -h`获取帮助信息，如下：
  ```
  Create a table group of TcaplusDB. Support creating table group in local docker environment.
Note: The table cluster should be created first before creating table group.  The local docker env requires some flags ( 'group-id', 'endpoint-url') and removes flags ( 'region', 'cluster-id' )

Usage:
  tcapluscli tablegroup create [flags]

Examples:
  Cloud Env: ./tcapluscli tablegroup create --region=ap-shanghai --cluster-id=12434 --group-name=tb_group_1
  Docker Env: ./tcapluscli tablegroup create --endpoint-url=http://localhost --access-id=[2|3] --group-name=zone_1 --group-id=1

Flags:
  -a, --access-id string      required in docker env: available value [2|3], 2: default cluster of TDR, 3: default cluster of PROTO.
  -c, --cluster-id string     required in cloud env: tcaplusdb cluster id
  -u, --endpoint-url string   required in local docker env : endpoint url  (ip:port) for local docker env
  -g, --group-id string       required in local docker env: tcaplusdb table group id
  -n, --group-name string     required: tablegroup name,group name format: mix of letter, digit, and _. Legal name: zone_5, illegal name: zone-5
  -h, --help                  help for create
  -r, --region string         required in cloud env: region name of cluster (default "ap-shanghai")
  ```

  ### 3.3.2 删除表格组
  执行`tcapluscli tablegroup delete -h`获取帮助信息，如下:
  ```
  Delete a table group.  Support deleting tablegroups of local docker env with --endpoint-url flag

Usage:
  tcapluscli tablegroup delete [flags]

Examples:
  Cloud Env: ./tcapluscli tablegroup delete --region=ap-shanghai --cluster-id=123455 --group-id=1
  Docker Env: ./tcapluscli tablegroup delete --endpoint-url=http://localhost --access-id=[2 | 3] --group-id=1

Flags:
  -a, --access-id string      required in docker env: available value [2|3], 2: default cluster of TDR, 3: default cluster of PROTO.
  -c, --cluster-id string     required in cloud env: tcaplusdb cluster id
  -u, --endpoint-url string   required in docker env: endpoint url of docker env
  -g, --group-id string       required: tcaplusdb table  group id
  -h, --help                  help for delete
  -r, --region string         required in cloud env: region name of table group (default "ap-shanghai")
  ```

  ### 3.3.3 查看表格组信息
  执行`tcapluscli tablegroup describe -h`获取帮助信息，如下:
  ```
  Get table group description.
If no flag 'group-id', all tablegroup infos of cluster will be returned, if flags with 'group-id', the specified table group will be returned.
Support get table group descriptions of local docker env with '--endpoint-url' flag

Usage:
  tcapluscli tablegroup describe [flags]

Examples:
  Cloud Env: ./tcapluscli tablegroup describe --region=ap-shanghai --cluster-id=123 [--group-id=1]
Cloud Env: ./tcapluscli tablegroup describe --region=ap-shanghai --cluster-id=123 --list
Docker Env: ./tcapluscli  tablegroup describe --endpoint-url=http://localhost --access-id=[2|3] --group-id=1
Docker Env: ./tcapluscli tablegroup describe --endpoint-url=http://localhost --access-id=[2|3] --list

Flags:
  -a, --access-id string      required in docker env: available value [2|3], 2: default cluster of TDR, 3: default cluster of PROTO.
  -c, --cluster-id string     required in cloud env: tcaplusdb cluster id
  -u, --endpoint-url string   required in docker env : the endpoint url (ip:port) for local docker env, exposed by docker as webservice api
  -g, --group-id string       optional: tcaplusdb table  group id
  -h, --help                  help for describe
  -l, --list                  list all tablegroup infos, if 'group-id' is empty, 'list' flag must be existed
  -r, --region string         required in cloud env: region name of table group (default "ap-shanghai")
  ```

  ## 3.4 表相关命令
  表相关命令支持腾讯云TcaplusDB版本，部分命令支持本地Docker版本，可执行`tcapluscli table -h`获取帮助信息，如下:
  ```
  Provide basic commands to operate TcaplusDB table. The basic commands includes: create, delete, recover and describe

Usage:
  tcapluscli table [commands] [flags]
  tcapluscli table [command]

Examples:
  ./tcapluscli table [create | delete | recover | describe] [flags]

Available Commands:
  batchclear  Batch clear specified table data. [Cloud Env]
  batchcreate Create multiple TcaplusDB tables. [Cloud Env]
  batchexpand Expand multiple tables' quotas. [Cloud Env]
  clear       Clear specified table data. [Cloud | Docker]
  create      Create a TcaplusDB table [Cloud | Docker]
  delete      Delete a TcaplusDB table. [Cloud | Docker]
  describe    Get table description information. [Cloud | Docker]
  expand      Expand table quotas. [Cloud Env]
  recover     Recover a TcaplusDB table from Recycle. [ Cloud Env]

Flags:
  -h, --help   help for table

Use "tcapluscli table [command] --help" for more information about a command.
  ```

### 3.4.1 创建表
执行`tcapluscli table create -h`获取帮助信息，支持Cloud和Docker环境，如下:
```
Create a TcaplusDB table. Support  creating table in local docker env with '--endpoint-url' flag
Note: The TcaplusDB cluster and table group should be created first.  See cluster and table group command

Usage:
  tcapluscli table create [flags]

Examples:
  Cloud Env: ./tcapluscli table create --region=ap-shanghai --cluster-id=1234 --group-id=1 --schema-type=PROTO --schema-file=game_players.proto
  Docker Env: ./tcapluscli table create --endpoint-url=http://localhost --access-id=[2|3] --group-id=1 --schema-type=TDR --schema-file=table_test.xml

Flags:
  -a, --access-id string      required in docker env: available value [2|3], 2: default cluster of TDR, 3: default cluster of PROTO.
  -c, --cluster-id string     required in cloud env: table cluster id
  -u, --endpoint-url string   required in local docker env: local env endpoint
  -g, --group-id string       required: table group id
  -h, --help                  help for create
  -R, --rcu int               optional: table reserved Read Capacity  Units (default 80)
  -r, --region string         required in cloud env:region name of table (default "ap-shanghai")
  -f, --schema-file string    required: table schema file with relative path, e.g:./cfg/test.proto
  -t, --schema-type string    required: table schema protocol : PROTO | TDR (default "PROTO")
  -v, --volume int            optional: table reserved volume (GB) (default 1)
  -W, --wcu int               optional: table reserved Write Capacity Units (default 26)
```

### 3.4.2 删除表
执行`tcapluscli table delete -h`获取帮助信息，支持Cloud和Docker环境，如下:
```
Delete a TcaplusDB table. The deleted table will be removed into Recycle of TcaplusDB Console. If you want to delete the table thoroughly, you can manually delete in the Recycle of TcaplusDB Console . Support deleting table of docker env with --endpoint-url flag

Usage:
  tcapluscli table delete [flags]

Examples:
  Cloud Env: ./tcapluscli table delete --cluster-id=123 --group-id=1 --region=ap-shanghai --table-instance-id=tcaplus-12f397 --table-name=test
  Docker Env: ./tcapluscli table delete --endpoint-url=http://localhost --access-id=[2|3] --group-id=1 --table-name=test

Flags:
  -a, --access-id string           required in docker env: available value [2|3], 2: default cluster of TDR, 3: default cluster of PROTO.
  -c, --cluster-id string          required in cloud env: table cluster id
  -u, --endpoint-url string        required in docker env: access endpoint url of docker env
  -g, --group-id string            required: table group id
  -h, --help                       help for delete
  -r, --region string              required in cloud env:region name of table (default "ap-shanghai")
  -i, --table-instance-id string   required in cloud env: tcaplusdb table instance id
  -n, --table-name string          required: tcaplusdb table name
```

### 3.4.3 清除表
执行`tcapluscli table clear -h`获取帮助信息，支持Cloud和Docker环境，如下:
```
Clear specfied table data.  Support clearing table data of local docker env with --endpoint-url flag.
Note: This operation is high dangerous, please be careful to execute this api to clear table data. Please double-check before operation

Usage:
  tcapluscli table clear [flags]

Examples:
  Cloud Env: ./tcapluscli table clear --region=ap-shanghai --cluster-id=1234 --group-id=1 --table-instance-id=tcpalus-123df12 --table-name=test
  Docker Env: ./tcapluscli table clear --endpoint-url=http://localhost --access-id=[2|3] --group-id=1 --table-name=test

Flags:
  -a, --access-id string           required in docker env: available value [2|3], 2: default cluster of TDR, 3: default cluster of PROTO.
  -c, --cluster-id string          required in cloud env: table cluster id
  -u, --endpoint-url string        required in docker env: access endpoint url of docker env
  -g, --group-id string            required: table group id
  -h, --help                       help for clear
  -r, --region string              required in cloud env: region name of table (default "ap-shanghai")
  -i, --table-instance-id string   required in cloud env: tcaplusdb table instance id
  -n, --table-name string          required: tcaplusdb table name
```

### 3.4.4 获取表信息
执行`tcapluscli table describe -h`获取帮助信息，支持Cloud环境和Docker环境，如下:
```
Get table description information. If table instance id and table name are empty, the 'list' flag must be indispensable  and list all table infos of table group. Support get table descriptions of local docker env with --endpoint-url flag.

Usage:
  tcapluscli table describe [flags]

Examples:
  Cloud Env: ./tcapluscli table describe --region=ap-shanghai --cluster-id=123 --group-id=1  --table-instance-id=tcaplus-12f397 --table-name=test
  Cloud Env: ./tcapluscli  table describe --region=ap-shanghai --cluster-id=123 --group-id=1 --list
  Docker Env: ./tcapluscli  table describe --endpoint-url=http://localhost --access-id=[2|3] --group-id=1 --list

Flags:
  -a, --access-id string           required in docker env: available value [2|3], 2: default cluster of TDR, 3: default cluster of PROTO.
  -c, --cluster-id string          required in cloud env: table cluster id
  -u, --endpoint-url string        required in docker env: endpoint url of docker env
  -g, --group-id string            required: table group id
  -h, --help                       help for describe
  -l, --list                       list all table infos
  -r, --region string              required in cloud env : region name of table (default "ap-shanghai")
  -i, --table-instance-id string   required in cloud env: tcaplusdb table instance id
  -n, --table-name string          required in cloud env: tcaplusdb table name
```

### 3.4.5 恢复删除表
从回收站恢复删除的表，只支持Cloud环境，docker没有回收站概念，执行`tcapluscli table recover -h`获取帮助信息，如下:
```
Recover a TcaplusDB table. The deleted table  was in Recycle, call RecoverRecycleTables api to recover the deleted tables to online.
Note: This command does't support docker env

Usage:
  tcapluscli table recover [flags]

Examples:
  ./tcapluscli  table recover --cluster-id=123 --group-id=1 --region=ap-shanghai --table-instance-id=tcaplus-12f397 --table-name=test

Flags:
  -c, --cluster-id string          required: table cluster id
  -g, --group-id string            required: table group id
  -h, --help                       help for recover
  -r, --region string              required:region name of table (default "ap-shanghai")
  -i, --table-instance-id string   required: tcaplusdb table instance id
  -n, --table-name string          required: tcaplusdb table name

```

### 3.4.6 表扩容
支持预设置容量、读写QPS扩容，暂支持Cloud环境，执行`tcapluscli table expand -h`获取帮助信息，如下:
```
Expand the specified table quotas, including volume, rcu and wcu. Expand as default value if volume, rcu and wcu are not presented.

Usage:
  tcapluscli table expand [flags]

Examples:
  ./tcapluscli  table expand --region=ap-shanghai --cluster-id=123 --group-id=1 --table-instance-id=tcaplus-12df --table-name=test --volume=2 --rcu=100 --wcu=100
  ./tcapluscli  table expand --region=ap-shanghai --cluster-id=123 --group-id=1 --table-instance-id=tcaplus-12df --table-name=test

Flags:
  -c, --cluster-id string          required: table cluster id
  -g, --group-id string            required: table group id
  -h, --help                       help for expand
  -R, --rcu int                    optional: table reserved Read Capacity  Units (default 80)
  -r, --region string              required:region name of table (default "ap-shanghai")
  -i, --table-instance-id string   required: table instance id
  -n, --table-name string          required: table name
  -v, --volume int                 optional: table reserved volume (GB) (default 1)
  -W, --wcu int                    optional: table reserved Write Capacity Units (default 26)
```

### 3.4.7 批量创建表
支持一次创建多个表，多个表定义文件放指定目录即可，执行`tcapluscli table batchcreate -h`获取帮助信息，如下:
```
Create multiple TcaplusDB tables.
Note: The TcaplusDB cluster and table group should be created first.  See cluster and table group command

Usage:
  tcapluscli table batchcreate [flags]

Examples:
  ./tcapluscli table batchcreate --region=ap-shanghai --cluster-id=1234 --group-id=1 --schema-type=PROTO --schema-filepath=./cfg/schema

Flags:
  -c, --cluster-id string        required: table cluster id
  -g, --group-id string          required: table group id
  -h, --help                     help for batchcreate
  -R, --rcu int                  optional: table reserved Read Capacity  Units (default 80)
  -r, --region string            required:region name of table (default "ap-shanghai")
  -d, --schema-filepath string   required: file path of table schema file e.g:./cfg/schema
  -t, --schema-type string       required: table schema protocol : PROTO | TDR (default "PROTO")
  -v, --volume int               optional: table reserved volume (GB) (default 1)
  -W, --wcu int                  optional: table reserved Write Capacity Units (default 26)
```

### 3.4.8 批量清理表
根据文件中指定的表相关信息进行批量清理，暂时只支持Cloud环境，　文件内容格式如下:
```
[TableInstanceId], [TableGroupId], [TableName]
```

执行`tcapluscli batchclear -h`获取帮助信息，如下:
```
Batch clear specfied table data. If you want to clear multiple tables, the table infos should be saved in a text file, the file content should contain three fields: TableInstanceId, TableGroupId, and TableName and use comma to separate them.
Note: This operation is high dangerous, please be careful to execute this api to clear table data. Please double-check before operation

Usage:
  tcapluscli table batchclear [flags]

Examples:
  Cloud Env: ./tcapluscli table clear --region=ap-shanghai --cluster-id=1234 --clear-file=./data/target_table.clear

Flags:
  -f, --clear-file string   required in cloud env: the clear file that contains target table infos need to be cleared.
  -c, --cluster-id string   required in cloud env: table cluster id
  -h, --help                help for batchclear
  -r, --region string       required in cloud env: region name of table (default "ap-shanghai")
```

### 3.4.9 批量扩容表
支持将多个需要扩容的表放在指定文件中，文件内容格式如下:
```
[table group id], [table instance id], [table name], [volume], [rcu], [wcu]
```
执行`tcapluscli table batchexpand -h`获取帮助信息。
```
Expand multiple tables' quotas. Save the target table infos into a file. The file format is as below:
[table group id], [table instance id], [table name], [volume], [rcu], [wcu].

Usage:
  tcapluscli table batchexpand [flags]

Examples:
  ./tcapluscli table batchexpand --region=ap-shanghai --cluster-id=123 --expand-file=expand_table.txt

Flags:
  -c, --cluster-id string    required: table cluster id
  -f, --expand-file string   required: expand file, save the target table quotas that need to be expanded
  -h, --help                 help for batchexpand
  -r, --region string        required:region name of table (default "ap-shanghai")
```

## 3.5  授权相关命令
命令权限管控，允许指定的源端ip访问docker环境，在所有命令执行之前需要进行相关授权，授权根据业务来授，通过指定access-id来区分，命令如下:
```
Control the access privilege of API. Allow specified ips to access docker environment . Support allowing all ips to access with allow-all-ip flag and specifying ips to accss with ip-list flag

Usage:
  tcapluscli privilege [flags]

Examples:
  ./tcapluscli privilege --endpoint-url=http://localhost --access-id=[2|3] --allow-all-ip
  ./tcapluscli privilege --endpoint-url=http://localhost --access-id=[2|3] --ip-list "1.1.1.1,1.1.1.2,1.1.1.3"

Flags:
  -a, --access-id string       required in docker env: available value [2|3], 2: default cluster of TDR, 3: default cluster of PROTO.
  -A, --allow-all-ip ip-list   required without  ip-list flag : allow all ip to access docker env , only work for docker but not  cloud
  -u, --endpoint-url string    required: endpoint url of docker env
  -h, --help                   help for privilege
  -l, --ip-list allow-all-ip   required without allow-all-ip flag: allow specified ips to access docker env, only work for docker but not  cloud (default "-1")
```

# 4. 总结
工具总体上满足用户操作TcaplusDB表的相关诉求，需要注意的是一些像清理类和删除类的操作务必谨慎处理，需要反复check。
