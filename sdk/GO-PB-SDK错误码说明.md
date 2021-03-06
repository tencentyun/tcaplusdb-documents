Table of Contents
=================

   * [1.错误码说明](#1\xE9\x94\x99\xE8\xAF\xAF\xE7\xA0\x81\xE8\xAF\xB4\xE6\x98\x8E)
   * [2. 业务错误码处理](#2-\xE4\xB8\x9A\xE5\x8A\xA1\xE9\x94\x99\xE8\xAF\xAF\xE7\xA0\x81\xE5\xA4\x84\xE7\x90\x86)
      * [2.1 引入错误码包](#21-\xE5\xBC\x95\xE5\x85\xA5\xE9\x94\x99\xE8\xAF\xAF\xE7\xA0\x81\xE5\x8C\x85)
      * [2.2  同步模式使用](#22--\xE5\x90\x8C\xE6\xAD\xA5\xE6\xA8\xA1\xE5\xBC\x8F\xE4\xBD\xBF\xE7\x94\xA8)
      * [2.3 异步模式使用](#23-\xE5\xBC\x82\xE6\xAD\xA5\xE6\xA8\xA1\xE5\xBC\x8F\xE4\xBD\xBF\xE7\x94\xA8)
   * [3.常见错误码](#3\xE5\xB8\xB8\xE8\xA7\x81\xE9\x94\x99\xE8\xAF\xAF\xE7\xA0\x81)

# 1.错误码说明
错误码处理分两种：
* 一种是普通系统操作的错误码，直接处理就行
* 另一种是TcaplusDB后端返回的业务错误码，这里重点关注

SDK所有的业务错误码描述均在源码目录`terror/error.go`中，用户可自行参考错误码描述，错误码命名基本能反映一些错误的一些原因，如果有疑惑可随时咨询TcaplusDB相关同学。
# 2. 业务错误码处理
下面介绍如何处理TcaplusDB后端返回的业务错误码。
## 2.1 引入错误码包
```
import "github.com/tencentyun/tcaplusdb-go-sdk/pb/terror"
```

## 2.2  同步模式使用
通过捕获接口返回错误信息，并解析`errCode`和`errMsg`来处理具体错误信息。如下示例：
```
//示例插入，假设record已经在表中存在
err := client.Insert(record)
//错误处理, 通过err.error()捕获错误信息
if err != nil {
    fmt.Printf("%v\n", err.Error())
}
//示例错误
errCode: -1293, errMsg: tcapsvr_fail_record_exist
```

## 2.3 异步模式使用
通过获取异步响应包的Result来判断错误码.　如下示例:
```
//示例插入, 假设record已经在表中存在
//示例错误处理
errCode := resp.GetResult()  //通过GetResult获取错误码
if errCode != terror.GEN_ERR_SUC {
                //返回错误码非成功错误码, 通过terror.GetErrMsg获取错误描述
                fmt.Printf("insert error:%s\n", terror.GetErrMsg(errCode))
                return
}
//示例错误信息
insert error: errCode:-1293, errMsg:tcapsvr_fail_record_exist
```

# 3.常见错误码
下面是一些常见的错误码列表：

|	编号	|	错误码	|	描述	|
| ---          |   ---          | ---          |
|	1	|	-1792	|	表处于只读模式，请检查RCU,	|
|	3	|	261	|	该记录不存在	|
|	4	|	-525	|	batchget操作请求超时,	|
|	5	|	-781	|	batchget,	|
|	6	|	-1037	|	系统繁忙，请联系管理员	|
|	7	|	-1293	|	记录已存在，请不要重复插入	|
|	8	|	-1549	|	访问的表字段不存在	|
|	9	|	-2061	|	表字段类型错误	|
|	10	|	-3085	|	SetFieldName操作指定了错误的字段	|
|	11	|	-3341	|	字段值大小超过其定义类型的限制	|
|	12	|	-4109	|	list数据类型元素下标超过范围	|
|	14	|	-4621	|	请求缺少主键字段或索引字段	|
|	15	|	-6157	|	list表元素个数超过定义范围,请设置元素淘汰	|
|	16	|	-6925	|	result_flag设置错误，请参考SDK中result_flag说明	|
|	17	|	-7949	|	请检查乐观锁，请求记录版本号与实际记录版本号不一致	|
|	18	|	-11277	|	操作表的方法不存在	|
|	19	|	-16141	|	PB表GetRecord操作失败，请联系管理员	|
|	20	|	-16397	|	PB表非主键字段值超过限定大小(256KB)	|
|	21	|	-16653	|	PB表FieldSetRecord操作失败，请联系管理员	|
|	22	|	-16909	|	PB表FieldIncRecord操作失败，请联系管理员	|
|	23	|	-275	|	主键字段个数超过限制，Generic表限制数为4,	|
|	24	|	-531	|	非主键字段个数超过限制，Generic表限制数为128,	|
|	25	|	-787	|	字段名称大小超过限制（32B）	|
|	26	|	-1043	|	字段值指超过限制（256KB）	|
|	27	|	-1555	|	字段值的数据类型与其定义类型不匹配	|
|	28	|	-5395	|	请求中缺少主键	|
|	29	|	-9235	|	index不存在	|
|	30	|	-12307	|	请求发送失败，网络过载，请联系管理员。	|
|	31	|	-12819	|	表不存在	|
|	32	|	-13843	|	后台网络异常，请求无法发送成功，如持续存在请联系管理员	|
|	33	|	-14099	|	插入的记录超过大小限制（1MB）	|
|	34	|	－6673	|	请求参数无主键	|
|	35	|	－6929	|	请求参数缺少主键字段	|
