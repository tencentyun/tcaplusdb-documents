# TcaplusDB GO TDR SDK入门
# TDR说明
TDR(Tencent Data Representation), 腾讯自研的跨平台多语言数据表示协议，主要用于数据的序列化反序列化，以XML文件形式来定义表结构。具体关于TDR表的定义说明可参考: [TDR表定义](https://cloud.tencent.com/document/product/596/44407)。
#  入门
快速入手TDR协议表的开发涉及几个步骤，下面介绍如何基于TcalusDB本地Docker版环境，快速上手基于Golang进行TDR表的增删查改操作。所有操作均在申请的开发测试机或云主机进行。
## Docker环境准备
在开始示例代码演示之前，需要提前准备好TcaplusDB本地Docker环境及tcapluscli工具，具体请参考资料：[TcaplusDB入门-Docker部署篇.md](https://github.com/tencentyun/tcaplusdb-documents/blob/main/docker/TcaplusDB%E5%85%A5%E9%97%A8-Docker%E9%83%A8%E7%BD%B2%E7%AF%87.md)。
Docker部署好后，对于命令行工具需要授权所有IP访问Docker环境，授权方式:
```
#access-id指定业务id, 2: tdr业务，3: pb业务，这里是tdr业务所以默认为2
./tcapluscli privilege --endpoint-url=http://localhost --access-id=2 --allow-all-ip
```

## Go环境准备
GO SDK示例依赖GO环境的部署，对于Centos系统可以直接安装通过:
```
yum install golang
```
建议版本：`1.13`以上。


## TcaplusDB表准备
### 准备TDR表示例文件
这里以示例中的`service_info.xml`举例，表名: `service_info`, 表类型: `GENERIC`。文件具体内容如下：
这
```
<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
<metalib name="service_info" tagsetversion="1" version="1">
  <struct name="service_info" version="1" primarykey="gameid,envdata,name,expansion" splittablekey="gameid" >
    <entry name="gameid"           	type="string"     size="128" desc="gameapp id"/>
    <entry name="envdata"         	type="string"     size="1024" desc="env环境信息" />
    <entry name="name"             	type="string"     size="1024" desc="名字" />
    <entry name="expansion"      	type="string"     size="1024" desc="扩展字段" />
    <entry name="filterdata"        type="string"     size="1024" desc="过滤标签" />
    <entry name="updatetime"        type="uint64"     desc="最近更新时间，单位ms" />
    <entry name="inst_max_num"      type="uint64"     desc="最大实例个数" />
    <entry name="inst_min_num"      type="uint64"     desc="最小实例个数" />
    <entry name="routeinfo_len"     type="uint"   defaultvalue="0" desc="路由规则信息长度" />
    <entry name="routeinfo"         type="char"   count="1024" refer="routeinfo_len" desc="路由规则信息" />
    <index name="index_gameid_envdata_name" column="gameid,envdata,name" />
    <index name="index_gameid_envdata" column="gameid,envdata" />
    <index name="index_gameid_name" column="gameid,name" />
    <index name="index_gameid" column="gameid" />
  </struct>
</metalib>

```
将上述文件内容保存为`service_info.xml`。


### TcaplusDB集群准备
对于TcaplusDB,在创建表之前需要创建对应的表集群。对于Docker本地版，集群已经默认创建好一个供大家使用，所以不用再创建集群。对于TDR集群，已经默认创建一个`test_app`的业务，集群接入ID (AccessID) 默认为`2`。集群密码(AccessPassword)查看可打开TcaplusDB运维平台，打开方式：｀直接浏览器输入部署docker的机器ip即可，默认端口80`。默认登录方式：
```
用户名：　tcaplus
密码：　tcaplus
```
登录后，进入`业务管理->业务维护->选择业务名称，这里默认选pb_app业务,在对应行的右侧点击查看密码即可`。
### TcaplusDB表格组准备
TcaplusDB表在集群的基础上还依赖于表格组，相当于游戏里的逻辑分区，使用工具创建表格组命令如下：
```
# 查看表格组帮助命令
./tcapluscli tablegroup -h

#创建一个表格组，id指定为4，　endpoint-url为上面docker暴露的80端口，access-id为集群接入ID(业务ID，2表示TDR集群), 用于docker环境连接使用, group name由字母、数字和下划线组成
./tcapluscli tablegroup create --endpoint-url=http://localhost --access-id=2 --group-id=4　--group-name=zone_4
```

### TcaplusDB表创建
现在正式进入表创建环节，在上述表格组基础上创建一个TDR表，执行创建表命令，如下所示：
```
#查看表创建命令提示帮助
./tcapluscli table -h

#创建一个表, 指定endpoint-url, 表格组id: group-id, 表类型: PROTO, 表定义文件: game_players.proto, 放当前路径
./tcapluscli table create  create --endpoint-url=http://localhost --access-id=2 --group-id=4 --schema-type=TDR --schema-file=service_info.xml
```

# 示例代码
以Golang示例代码为例，介绍如何使用TDR接口进行TcaplusDB表数据操作，这里主要介绍Generic类型表操作。GO示例代码以`go mod`方式进行编译，GO版本以`1.15`举例.

## 示例代码下载
目前示例代码放在github路径，直接下载即可。
```
git clone https://github.com/tencentyun/tcaplusdb-go-examples.git
```
### 示例代码目录结构
```
[root@VM-48-13-centos tcaplusdb-go-examples]# tree
tdr
|-- async
|   |-- example.go
|   |-- logconf.xml
|   |-- Makefile
|   |-- README.md
|   |-- service_info
|   |   `-- service_info.go
|   `-- service_info.xml
|-- go.mod
|-- README.md
|-- sync
|   |-- example.go
|   |-- logconf.xml
|   |-- Makefile
|   |-- README.md
|   |-- service_info
|   |   `-- service_info.go
|   `-- service_info.xml
`-- tool
    |-- goimports
    |-- lib
    |   |-- core.py
    |   |-- core.pyc
    |   |-- go_cl.py
    |   |-- go_cl.pyc
    |   |-- go_proto.py
    |   |-- __init__.py
    |   |-- python_cl.py
    |   `-- python_proto.py
    `-- tdr.py

```
#### 示例代码目录结构说明
* __async__: 异步调用模式示例代码
* __sync__: 同步调用模式示例代码
* __tool__: 公共目录，放置一些工具，如tdr表定义xml文件生成tdr接口定义代码。

#### 日志配置文件说明
在sync和async目录下分别有一个logconf.xml, 用于日志相关配置。默认ERROR级别，可配置DEBUG | INFO | WARN等级别，如果是要压测，建议配置为ERROR级别，提高压测性能。


#### 公共代码说明
在tool目录下，存放一些公共工具，用于生成tdr接口定义代码:
* goimports: 保存的时候自动导入处理包
* lib: 依赖库目录
* tdr.py: 生成tdr接口定义代码的主要文件，会生成一个以表名命名的目录，使用方式:
```
python tdr.py service_info.xml
```

#### 表定义说明
在async和sync目录下存放相关表的接口定义文件和生成的接口定义代码。
|文件|文件说明|
|---|---|
|service_info.xml|示例TDR表定义文件|
|tcaplusservice.optionv1.proto|tcaplusdb pb协议定义文件|
|service_info|用tdr.py生成的表定义接口代码目录|
|gservice_info.go|生成的表定义接口代码|

#### 同步模式示例代码说明
在sync目录下存放同步调用模式接口代码。
|文件|文件说明|
|---|---|
|example.go| 示例代码主文件,包含CRUD接口|
|Makefile|编译文件，直接执行make可编译得到示例二进制文件|
|logconf.xml| 日志配置文件，默认级别ERROR, 如果需要更详细的，可开DEBUG,　如果压测的话用ERROR既可，避免性能损耗|

#### 异步模式示例代码说明
在async目录下存放异步调用模式接口代码。
|文件|文件说明|
|---|---|
|example.go| 示例代码主文件,包含CRUD接口|
|Makefile|编译文件，直接执行make可编译得到示例二进制文件|
|logconf.xml| 日志配置文件，默认级别ERROR, 如果需要更详细的，可开DEBUG,　如果压测的话用ERROR既可，避免性能损耗|

### 表接口代码生成
如果不想用示例代码中的示例表，参照service_info.xml定义好后，可以用如下命令生成：
```
cd tool
#生成表定义接口代码
python tdr.py my_table.xml
```

### 公共连接环境配置
打开example.go文件，并修改如下参数，改成业务方申请的tcaplusdb连接信息或本地部署的tcaplusdb信息;
```
　　//集群访问地址，本地docker版：配置docker部署机器IP, 端口默认:9999, 腾讯云线上环境配置为连接地址IP和端口
        DirUrl = "tcp://x.x.x.x:xxxx"
        //集群接入ID, 默认为3，本地docker版：直接填3，云上版本：根据实际集群接入ID填写
        AppId = 3
        //集群访问密码，本地docker版：参考集群准备阶段获取集群密码步骤，需要借助tcaplusdb web运维平台查看; 云上版本：根据实际申请集群详情页查看
        Signature = "xxxxx"
        //表格组ID，替换为自己创建的表格组ID
        ZoneId = 4
        //表名称
        TableName = "service_info"
```


### 编译代码
#### 同步模式编译
在示例代码目录，同步模式已经集成了`Makefile`, 方便用户用make方式进行编译。Makefile文件中把GO执行的一系列命令放在里面，用户无需再单独执行
```
#进示例代码目录
cd  tcaplusdb-go-examples/tdr/sync

#直接执行make
make

#生成example,可执行文件,直接执行即可进行相关操作
#体验CRUD接口, 输出所有接口的示例
./example


```
#### 异步模式编译
异步模式直接进async目录，类似同步模式编译，直接执行make就好。