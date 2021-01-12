# TcaplusDB RESTful API Go SDK

Table of Contents
=================

   * [TcaplusDB RESTful API Go SDK](#tcaplusdb-restful-api-go-sdk)
      * [1.SDK背景](#1sdk\xE8\x83\x8C\xE6\x99\xAF)
         * [约束限制](#\xE7\xBA\xA6\xE6\x9D\x9F\xE9\x99\x90\xE5\x88\xB6)
      * [2.使用准备](#2\xE4\xBD\xBF\xE7\x94\xA8\xE5\x87\x86\xE5\xA4\x87)
         * [2.1　TcaplusDB表创建](#21tcaplusdb\xE8\xA1\xA8\xE5\x88\x9B\xE5\xBB\xBA)
         * [2.2 CVM实例申请](#22-cvm\xE5\xAE\x9E\xE4\xBE\x8B\xE7\x94\xB3\xE8\xAF\xB7)
         * [2.3 Go环境安装](#23-go\xE7\x8E\xAF\xE5\xA2\x83\xE5\xAE\x89\xE8\xA3\x85)
         * [2.4 程序编译](#24-\xE7\xA8\x8B\xE5\xBA\x8F\xE7\xBC\x96\xE8\xAF\x91)
      * [3. 接口使用步骤](#3-\xE6\x8E\xA5\xE5\x8F\xA3\xE4\xBD\xBF\xE7\x94\xA8\xE6\xAD\xA5\xE9\xAA\xA4)
         * [3.1 表参数配置](#31-\xE8\xA1\xA8\xE5\x8F\x82\xE6\x95\xB0\xE9\x85\x8D\xE7\xBD\xAE)
         * [3.2 表连接创建](#32-\xE8\xA1\xA8\xE8\xBF\x9E\xE6\x8E\xA5\xE5\x88\x9B\xE5\xBB\xBA)
         * [3.3 表示例数据](#33-\xE8\xA1\xA8\xE7\xA4\xBA\xE4\xBE\x8B\xE6\x95\xB0\xE6\x8D\xAE)
      * [4. 接口列表](#4-\xE6\x8E\xA5\xE5\x8F\xA3\xE5\x88\x97\xE8\xA1\xA8)
         * [4.1 GetRecord 记录查询](#41-getrecord-\xE8\xAE\xB0\xE5\xBD\x95\xE6\x9F\xA5\xE8\xAF\xA2)
         * [4.2 AddRecord 插入记录](#42-addrecord-\xE6\x8F\x92\xE5\x85\xA5\xE8\xAE\xB0\xE5\xBD\x95)
         * [4.3 SetRecord 设置记录](#43-setrecord-\xE8\xAE\xBE\xE7\xBD\xAE\xE8\xAE\xB0\xE5\xBD\x95)
         * [4.4 DeleteRecord 删除记录](#44-deleterecord-\xE5\x88\xA0\xE9\x99\xA4\xE8\xAE\xB0\xE5\xBD\x95)
         * [4.5 FieldGetRecord 指定字段查询](#45-fieldgetrecord-\xE6\x8C\x87\xE5\xAE\x9A\xE5\xAD\x97\xE6\xAE\xB5\xE6\x9F\xA5\xE8\xAF\xA2)
         * [4.6 FieldSetRecord 指定字段设置](#46-fieldsetrecord-\xE6\x8C\x87\xE5\xAE\x9A\xE5\xAD\x97\xE6\xAE\xB5\xE8\xAE\xBE\xE7\xBD\xAE)
         * [4.7 FieldIncRecord 指定字段自增/自减](#47-fieldincrecord-\xE6\x8C\x87\xE5\xAE\x9A\xE5\xAD\x97\xE6\xAE\xB5\xE8\x87\xAA\xE5\xA2\x9E\xE8\x87\xAA\xE5\x87\x8F)
         * [4.8 PartkeyGetRecord 索引查询，仅指定索引键](#48-partkeygetrecord-\xE7\xB4\xA2\xE5\xBC\x95\xE6\x9F\xA5\xE8\xAF\xA2\xE4\xBB\x85\xE6\x8C\x87\xE5\xAE\x9A\xE7\xB4\xA2\xE5\xBC\x95\xE9\x94\xAE)


## 1.SDK背景
为满足用户通过Golang语言来操作TcaplusDB,基于RESTful API封装了关于TcaplusDB表操作的接口，涵盖增删查改场景。

### 约束限制

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

## 2.使用准备
### 2.1　TcaplusDB表创建
在腾讯云TcaplusDB控制台创建一个示例表，示例表定义如目录中`game_players.proto`所定义的,关于创建表的指引请参考[https://cloud.tencent.com/document/product/596/38807](https://cloud.tencent.com/document/product/596/38807)。

### 2.2 CVM实例申请
申请一台CVM实例来运行SDK示例程序，配置建议为4C-8G-100G, 该CVM需创建在TcaplusDB实例所在VPC网络中。

### 2.3 Go环境安装
安装Golang执行环境，安装命令如下:
```
yum install -y golang
```

### 2.4 程序编译
SDK示例程序通过make编译，在src目录下有Makefile文件，直接执行`make build`即可。编译好后，会生成一个可执行文件`example`。直接执行此文件即可演示所有示例接口。

## 3. 接口使用步骤

### 3.1 表参数配置
查看TcaplusDB控制台所创建表的相关信息，在示例中进行配置，如下所示:
```
//TcaplusDB RESTful API的连接参数
const (
        //服务接入点,表所在集群RESTful连接地址，默认端口80
        EndPoint = "http://172.17.0.12/"
        //应用接入id，表所在集群接入ID
        AccessID = 310
        //应用密码,表所在集群访问密码
        AccessPassword = "Tcaplus2020"
        //表格组ID
        TableGroupID = 1
        //表名称
        TableName = "game_players"
)
```
### 3.2 表连接创建
通过NewTcaplusClient指定EndPoint, AccessID, AccessPassword参数创建TcaplusRestClient的对象client
```
//通过指定EndPoint, AccessID, AccessPassword参数创建TcaplusClient的对象client
client, err := tcaplus_client.NewTcaplusClient(EndPoint, AccessID, AccessPassword)
if err != nil {
    fmt.Println(err.Error())
    return
}
```
### 3.3 表示例数据
插入数据为JSON格式，定义如下所示:
```
//AddRecord插入记录
//用户可将record定义成结构体/map/slice，需可转成json
record := map[string]interface{}{
    "player_id": 10805514,
    "player_name": "Calvin",
    "player_email": "calvin@test.com",
    "game_server_id": 10,
    "login_timestamp": []string{"2019-12-12 15:00:00"},
    "logout_timestamp": []string{"2019-12-12 16:00:00"},
    "is_online": false,
    "pay": map[string]interface{}{
        "pay_id": 10101,
        "amount": 1000,
        "method": 1,
    },
}
status, resp, err := client.AddRecord(record, tcaplus_client.RetAllLatestField, "userBuffer", TableGroupID, TableName)
if err != nil {
    fmt.Println(err.Error())
    return
}
```

## 4. 接口列表

### 4.1 GetRecord 记录查询
```
/**
	@brief  根据Key字段查询记录，可以根据selectFiled过滤出部分字段
	@param [IN] key             需要查询的记录的key字段信息，可以是结构体，map，slice，从而转成json，与proto中定义的类型保持一致
	@param [IN] selectFiled     需要查询的记录的字段列表，结构体嵌套则设为点分式；为nil表示查询全部字段
	@param [IN] groupID         表格组ID
	@param [IN] tableName       表名
	@retval(3) http响应码，http响应内容，错误信息
**/
func (c *TcaplusClient) GetRecord(key interface{}, selectFiled []string, groupID int, tableName string) (string, []byte, error)
```
### 4.2 AddRecord 插入记录
```
/**
	@brief  添加一条记录到表中，该记录若存在，则会报错
	@param [IN] record          需要添加的记录，可以是结构体，map，slice，从而转成json，与proto中定义的类型保持一致
	@param [IN] resultFlag      返回值标记位，可设置为以下值
                                        RetOnlySucOrFail：应答中仅包含请求成功或失败
                                        RetEqualReq：应答中包含与请求一致的值
                                        RetAllLatestField：应答中包含被修改的数据的所有字段最新值
                                        RetAllOldField：应答中包含记录被修改前的值
	@param [IN] userBuffer      用户自定义信息，在响应信息中原样返回，不关注则填""
	@param [IN] groupID         表格组ID
	@param [IN] tableName       表名
	@retval(3) http响应码，http响应内容，错误信息
**/
func (c *TcaplusClient) AddRecord(record interface{}, resultFlag int, userBuffer string, groupID int, tableName string) (string, []byte, error)

```
### 4.3 SetRecord 设置记录
```
/**
	@brief  更新/插入一条记录到表中，该记录若存在，则更新；不存在，则插入
	@param [IN] record          需要添加的记录，可以是结构体，map，slice，从而转成json，与proto中定义的类型保持一致
	@param [IN] resultFlag      返回值标记位，可设置为以下值
                                        RetOnlySucOrFail：应答中仅包含请求成功或失败
                                        RetEqualReq：应答中包含与请求一致的值
                                        RetAllLatestField：应答中包含被修改的数据的所有字段最新值
                                        RetAllOldField：应答中包含记录被修改前的值
	@param [IN] versionPolicy   记录的版本号校验策略，与version配合使用，用于乐观锁，不关注则设置为NoCheckDataVersionAutoIncrease
                                        CheckDataVersionAutoIncrease：检测记录版本号,只有当version与服务器端的版本号相同时，操作成功，记录版本号自增
                                        NoCheckDataVersionOverwrite：不检测记录版本号，强制把记录版本号version写入到服务器中
                                        NoCheckDataVersionAutoIncrease：不检测记录版本号，服务器端的版本号自增
	@param [IN] version         记录的版本号，用于版本号校验，不校验则设置为-1
	@param [IN] userBuffer      用户自定义信息，在响应信息中原样返回，不关注则填""
	@param [IN] groupID         表格组ID
	@param [IN] tableName       表名
	@retval(3) http响应码，http响应内容，错误信息
**/
func (c *TcaplusClient) SetRecord(record interface{}, resultFlag int, versionPolicy int, version int, userBuffer string, groupID int, tableName string) (string, []byte, error)

```
### 4.4 DeleteRecord 删除记录
```
/**
	@brief  删除一条记录；不存在，则报错
	@param [IN] record          需要删除的记录，包含key字段即可；可以是结构体，map，slice，从而转成json，与proto中定义的类型保持一致
	@param [IN] resultFlag      返回值标记位，可设置为以下值
                                        RetOnlySucOrFail：应答中仅包含请求成功或失败
                                        RetEqualReq：应答中包含与请求一致的值
                                        RetAllLatestField：应答中包含被修改的数据的所有字段最新值
                                        RetAllOldField：应答中包含记录被修改前的值
	@param [IN] userBuffer      用户自定义信息，在响应信息中原样返回，不关注则填""
	@param [IN] groupID         表格组ID
	@param [IN] tableName       表名
	@retval(3) http响应码，http响应内容，错误信息
**/
func (c *TcaplusClient) DeleteRecord(record interface{}, resultFlag int, userBuffer string, groupID int, tableName string) (string, []byte, error)

```
### 4.5 FieldGetRecord 指定字段查询
```
/**
	@brief  记录的部分字段查询，根据Key字段查询记录，根据selectFiled过滤字段内容
	@note   该接口与GetRecord的区别：GetRecord是查询的整条记录然后按selectFiled过滤；而FieldGetRecord是在svr端过滤，流量负载更低
	@param [IN] key             需要查询的记录的key字段信息，可以是结构体，map，slice，从而转成json，与proto中定义的类型保持一致
	@param [IN] selectFiled     需要查询的记录的字段列表，不能为空，结构体嵌套则设为点分式
	@param [IN] groupID         表格组ID
	@param [IN] tableName       表名
	@retval(3) http响应码，http响应内容，错误信息
**/
func (c *TcaplusClient) FieldGetRecord(key interface{}, selectFiled []string, groupID int, tableName string) (string, []byte, error)

```
### 4.6 FieldSetRecord 指定字段设置
```
/**
	@brief  更新一条记录，该记录若不存在则报错
	@param [IN] record          需要添加的记录，可以是结构体，map，slice，从而转成json，与proto中定义的类型保持一致
	@param [IN] setField        需要更新的字段列表，不能为空，结构体嵌套则设为点分式
	@param [IN] resultFlag      返回值标记位，可设置为以下值
                                        RetOnlySucOrFail：应答中仅包含请求成功或失败
                                        RetEqualReq：应答中包含与请求一致的值
	@param [IN] versionPolicy   记录的版本号校验策略，与version配合使用，用于乐观锁，不关注则设置为NoCheckDataVersionAutoIncrease
                                        CheckDataVersionAutoIncrease：检测记录版本号,只有当version与服务器端的版本号相同时，操作成功，记录版本号自增
                                        NoCheckDataVersionOverwrite：不检测记录版本号，强制把记录版本号version写入到服务器中
                                        NoCheckDataVersionAutoIncrease：不检测记录版本号，服务器端的版本号自增
	@param [IN] version         记录的版本号，用于版本号校验，不校验则设置为-1
	@param [IN] userBuffer      用户自定义信息，在响应信息中原样返回，不关注则填""
	@param [IN] groupID         表格组ID
	@param [IN] tableName       表名
	@retval(3) http响应码，http响应内容，错误信息
**/
func (c *TcaplusClient) FieldSetRecord(record interface{}, setField []string, resultFlag int, versionPolicy int, version int, userBuffer string, groupID int, tableName string) (string, []byte, error)

```
### 4.7 FieldIncRecord 指定字段自增/自减
```
/**
	@brief  对记录中的整型字段进行自增/自减，此命令字仅支持 int32, int64, uint32 和 uint64类型字段
	@param [IN] record          需要更新的的记录，记录中的字段值为正，则表示自增，累加该值；为负，则表示自减，累减该值
	@param [IN] resultFlag      返回值标记位，可设置为以下值
                                        RetOnlySucOrFail：应答中仅包含请求成功或失败
                                        RetEqualReq：应答中包含与请求一致的值
	@param [IN] versionPolicy   记录的版本号校验策略，与version配合使用，用于乐观锁，不关注则设置为NoCheckDataVersionAutoIncrease
                                        CheckDataVersionAutoIncrease：检测记录版本号,只有当version与服务器端的版本号相同时，操作成功，记录版本号自增
                                        NoCheckDataVersionOverwrite：不检测记录版本号，强制把记录版本号version写入到服务器中
                                        NoCheckDataVersionAutoIncrease：不检测记录版本号，服务器端的版本号自增
	@param [IN] version         记录的版本号，用于版本号校验，不校验则设置为-1
	@param [IN] userBuffer      用户自定义信息，在响应信息中原样返回，不关注则填""
	@param [IN] groupID         表格组ID
	@param [IN] tableName       表名
	@retval(3) http响应码，http响应内容，错误信息
**/
func (c *TcaplusClient) FieldIncRecord(record interface{}, resultFlag int, versionPolicy int, version int, userBuffer string, groupID int, tableName string) (string, []byte, error)

```
### 4.8 PartkeyGetRecord 索引查询，仅指定索引键
```
/**
	@brief  按索引进行批量查询
	@param [IN] key             需要查询的记录，仅需要索引包含的key字段，可以是结构体，map，slice，从而转成json，与proto中定义的类型保持一致
	@param [IN] indexName       索引名称
	@param [IN] selectFiled     需要查询的记录的字段列表，结构体嵌套则设为点分式；为nil表示查询全部字段
	@param [IN] limit           批量返回的记录上限，>0有效
	@param [IN] offset          批量返回的记录的偏移，>=0有效
	@param [IN] groupID         表格组ID
	@param [IN] tableName       表名
	@retval(3) http响应码，http响应内容，错误信息
**/
func (c *TcaplusClient) PartKeyGetRecord(key interface{}, indexName string, selectFiled []string, limit int, offset int, groupID int, tableName string) (string, []byte, error)

```

对于PartKeyGetRecord接口，1个请求返回的最大包大小为`256KB`, limit的设置依赖于单条记录大小。推荐设置策略：
* __单条记录小于256KB__：Limit 参考设置为256KB/[单条记录大小], 如记录大小为10KB，则limit推荐设置20-25左右。
* __单条记录大于等于256KB__: Limit设置为1, 即一次请求只返回一条记录。

对于设置Limit和Offset的场景，如果要根据索引键获取全量的数据，则需要依据响应包中返回的`TotalNum和RemainNum`标识来判断数据是否获取完全。
