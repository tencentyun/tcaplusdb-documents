# TcaplusDB 新特性说明手册

# Table of Contents

- [TcaplusDB 新特性说明手册](#tcaplusdb-\xE6\x96\xB0\xE7\x89\xB9\xE6\x80\xA7\xE8\xAF\xB4\xE6\x98\x8E\xE6\x89\x8B\xE5\x86\x8C)
- [1. 介绍](#1-\xE4\xBB\x8B\xE7\xBB\x8D)
- [2.前置工作](#2\xE5\x89\x8D\xE7\xBD\xAE\xE5\xB7\xA5\xE4\xBD\x9C)
  - [2.1 资源下载](#21-\xE8\xB5\x84\xE6\xBA\x90\xE4\xB8\x8B\xE8\xBD\xBD)
  - [2.2 示例表](#22-\xE7\xA4\xBA\xE4\xBE\x8B\xE8\xA1\xA8)
  - [2.3 示例接口说明](#23-\xE7\xA4\xBA\xE4\xBE\x8B\xE6\x8E\xA5\xE5\x8F\xA3\xE8\xAF\xB4\xE6\x98\x8E)
- [3. 新特性-条件过滤说明](#3-\xE6\x96\xB0\xE7\x89\xB9\xE6\x80\xA7-\xE6\x9D\xA1\xE4\xBB\xB6\xE8\xBF\x87\xE6\xBB\xA4\xE8\xAF\xB4\xE6\x98\x8E)
  - [3.1 条件过滤特性背景介绍](#31-\xE6\x9D\xA1\xE4\xBB\xB6\xE8\xBF\x87\xE6\xBB\xA4\xE7\x89\xB9\xE6\x80\xA7\xE8\x83\x8C\xE6\x99\xAF\xE4\xBB\x8B\xE7\xBB\x8D)
  - [3.2 条件过滤接口说明](#32-\xE6\x9D\xA1\xE4\xBB\xB6\xE8\xBF\x87\xE6\xBB\xA4\xE6\x8E\xA5\xE5\x8F\xA3\xE8\xAF\xB4\xE6\x98\x8E)
  - [3.3 条件过滤语法说明](#33-\xE6\x9D\xA1\xE4\xBB\xB6\xE8\xBF\x87\xE6\xBB\xA4\xE8\xAF\xAD\xE6\xB3\x95\xE8\xAF\xB4\xE6\x98\x8E)
  - [3.4 过滤条件的约束](#34-\xE8\xBF\x87\xE6\xBB\xA4\xE6\x9D\xA1\xE4\xBB\xB6\xE7\x9A\x84\xE7\xBA\xA6\xE6\x9D\x9F)
  - [3.5 性能优化建议](#35-\xE6\x80\xA7\xE8\x83\xBD\xE4\xBC\x98\xE5\x8C\x96\xE5\xBB\xBA\xE8\xAE\xAE)
- [4. 新特性-数组操作](#4-\xE6\x96\xB0\xE7\x89\xB9\xE6\x80\xA7-\xE6\x95\xB0\xE7\xBB\x84\xE6\x93\x8D\xE4\xBD\x9C)
  - [4.1 新特性背景介绍](#41-\xE6\x96\xB0\xE7\x89\xB9\xE6\x80\xA7\xE8\x83\x8C\xE6\x99\xAF\xE4\xBB\x8B\xE7\xBB\x8D)
  - [4.2 新特性接口介绍](#42-\xE6\x96\xB0\xE7\x89\xB9\xE6\x80\xA7\xE6\x8E\xA5\xE5\x8F\xA3\xE4\xBB\x8B\xE7\xBB\x8D)
  - [4.3 新特性使用示例](#43-\xE6\x96\xB0\xE7\x89\xB9\xE6\x80\xA7\xE4\xBD\xBF\xE7\x94\xA8\xE7\xA4\xBA\xE4\xBE\x8B)
    - [4.3.1 数组更新示例](#431-\xE6\x95\xB0\xE7\xBB\x84\xE6\x9B\xB4\xE6\x96\xB0\xE7\xA4\xBA\xE4\xBE\x8B)
    - [4.3.2 数组查询示例](#432-\xE6\x95\xB0\xE7\xBB\x84\xE6\x9F\xA5\xE8\xAF\xA2\xE7\xA4\xBA\xE4\xBE\x8B)
  - [4.4 新特性语法说明](#44-\xE6\x96\xB0\xE7\x89\xB9\xE6\x80\xA7\xE8\xAF\xAD\xE6\xB3\x95\xE8\xAF\xB4\xE6\x98\x8E)
  - [4.5 新特性约束](#45-\xE6\x96\xB0\xE7\x89\xB9\xE6\x80\xA7\xE7\xBA\xA6\xE6\x9D\x9F)
- [5. 示例代码](#5-\xE7\xA4\xBA\xE4\xBE\x8B\xE4\xBB\xA3\xE7\xA0\x81)
  - [5.1 示例代码说明](#51-\xE7\xA4\xBA\xE4\xBE\x8B\xE4\xBB\xA3\xE7\xA0\x81\xE8\xAF\xB4\xE6\x98\x8E)
  - [5.2 示例代码使用](#52-\xE7\xA4\xBA\xE4\xBE\x8B\xE4\xBB\xA3\xE7\xA0\x81\xE4\xBD\xBF\xE7\x94\xA8)
    - [5.2.1 代码编译](#521-\xE4\xBB\xA3\xE7\xA0\x81\xE7\xBC\x96\xE8\xAF\x91)
  - [5.3 示例代码说明](#53-\xE7\xA4\xBA\xE4\xBE\x8B\xE4\xBB\xA3\xE7\xA0\x81\xE8\xAF\xB4\xE6\x98\x8E)
    - [5.3.1 Generic 表示例](#531-generic-\xE8\xA1\xA8\xE7\xA4\xBA\xE4\xBE\x8B)
    - [5.3.2 List 表示例](#532-list-\xE8\xA1\xA8\xE7\xA4\xBA\xE4\xBE\x8B)
- [6. API 附录](#6-api-\xE9\x99\x84\xE5\xBD\x95)
  - [6.1 支持过滤条件的 API（Generic 表）](#61-\xE6\x94\xAF\xE6\x8C\x81\xE8\xBF\x87\xE6\xBB\xA4\xE6\x9D\xA1\xE4\xBB\xB6\xE7\x9A\x84-apigeneric-\xE8\xA1\xA8)
  - [6.2 支持过滤条件的 API（List 表）](#62-\xE6\x94\xAF\xE6\x8C\x81\xE8\xBF\x87\xE6\xBB\xA4\xE6\x9D\xA1\xE4\xBB\xB6\xE7\x9A\x84-apilist-\xE8\xA1\xA8)
  - [6.3 数组操作的 API（Generic 表）](#63-\xE6\x95\xB0\xE7\xBB\x84\xE6\x93\x8D\xE4\xBD\x9C\xE7\x9A\x84-apigeneric-\xE8\xA1\xA8)
  - [6.4 数组操作的 API（List 表）](#64-\xE6\x95\xB0\xE7\xBB\x84\xE6\x93\x8D\xE4\xBD\x9C\xE7\x9A\x84-apilist-\xE8\xA1\xA8)

# 1. 介绍

为了满足更多业务场景需要，提供更灵活的数据访问操作，TcaplusDB 新增了条件操作的特性，当前仅支持 protobuf 协议的表，该特性具有以下功能：

- **条件过滤**: 记录级别的条件过滤，只有满足条件，才对指定的（一个或多个）记录进行操作，包括对记录修改、删除或查询。
- **数组更新**: 只作用于数组类型的字段（即 protobuf 中的 repeated 字段），对数组元素进行增删改操作，同时可以用条件过滤某些元素。
- **数组查询**: 只作用于数组类型的字段（即 protobuf 中的 repeated 字段），查询返回满足过滤条件的或者某些下标范围内的数据，而不是完整的记录。

原则上，generic 表和 list 表都支持条件操作，其中 list 表支持**相同主键**对应多个记录，这些记录组成了 list，有 list index 进一步区分。

# 2.前置工作

## 2.1 资源下载

| 资源名称                                                 | 版本   | 下载地址                                                                                                                          | 说明                 |
| -------------------------------------------------------- | ------ | --------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| TcaplusPbApi3.52.0.203403.x86_64_release_20210304.tar.gz | 3.52.0 | [下载](https://tcaplus-tool-1302668961.cos.ap-shanghai.myqcloud.com/sdk/TcaplusPbApi3.52.0.203403.x86_64_release_20210304.tar.gz) | PB SDK 代码示例      |
| TSF4G_BASE-2.7.37.0a1db41b8_X86_64_Release.tar.gz        | 2.7.37 | [下载](https://tcaplus-tool-1302668961.cos.ap-shanghai.myqcloud.com/sdk/TSF4G_BASE-2.7.37.0a1db41b8_X86_64_Release.tar.gz)        | 示例编译依赖的系统包 |
| Protobuf                                                 | 3.5.1  | [下载](https://tcaplus-tool-1302668961.cos.ap-shanghai.myqcloud.com/sdk/protobuf-cpp-3.5.1.tar.gz)                                | protobuf 库          |

## 2.2 示例表

示例代码中，以 Generic 表和 List 表举例，示例表定义如下：

- **Generic 表**

```
syntax = "proto3";
package myTcaplusTable;
import "tcaplusservice.optionv1.proto";

message user {
    option(tcaplusservice.tcaplus_primary_key) = "id,name";
    option(tcaplusservice.tcaplus_index) = "name_index(name)";

    message mail {
        string title = 1;
        string content = 2;
    }

    int32 id = 1;
    string name = 2;
    int32 rank = 3;
    repeated int64 gameids = 4;
    repeated mail mailbox = 5;
}
```

- **List 表**

```
syntax = "proto3";
package myTcaplusTable;
import "tcaplusservice.optionv1.proto";

message list_user {
    option(tcaplusservice.tcaplus_primary_key) = "id,name";
    option(tcaplusservice.tcaplus_customattr) = "TableType=SORTLIST;ListNum=1024;SortField=rank";

    message mail {
        string title = 1;
        string content = 2;
    }

    int32 id = 1;
    string name = 2;
    int32 rank = 3;
    repeated int64 gameids = 4;
    repeated mail mailbox = 5;
}
```

## 2.3 示例接口说明

TcaplusDB 提供两类型的 protobuf 的 API，同步接口（ coroutine） 和异步接口（ async），目前都支持新特性条件操作。

- coroutine：协程方式调用的同步接口，见`tcaplus_coroutine_pb_api.h`。
- async：异步接口，见`tcaplus_async_pb_api.h`。

# 3. 新特性-条件过滤说明

支持记录级别的条件过滤，只有满足条件，才对指定的（一个或多个）记录进行操作，包括对记录修改、删除或查询等。

## 3.1 条件过滤特性背景介绍

在支持条件过滤之前，对于 generic 表，通过 key 查询或操作一个记录，若对应 key 不存在，则返回错误码 TXHDB_ERR_RECORD_NOT_EXIST，对于 list 则通过 key + index。

而条件则在这基础上再加一层过滤，对于 generic 表，在 key 对应存在基础上，必须条件满足才能查询或操作对应的记录，否则返回错误码 COMMON_ERR_EXPR_CONDITION_NOT_MATCHED。

key + condition，相当于 SQL 中的 `where` 语句，虽然能力比 SQL 还差不少，但已经提供一定程度的灵活能力。
特别是对于“读判断-然后写”的原子操作场景特别有用，条件过滤这个特性的初衷就是解决这个问题的。

例如，对于 user 表，若 gameids 数组不包含 101 这个元素，那么在该数组插入 101，那么应用代码会写成如下

```cpp
user u;
// 设置主键
u.set_id(1);
u.set_name("a");
int ret = api.Get(&u);
// ...
if (!Find(u.gameids(), 101)) // Find是应用实现的函数，在数组中找是否存在某元素
{
    u.add_gameids(101);
    ret = api.Set(&u);
    // ...
}
```

上述代码存在几个问题

- 应用端和服务端存在多处交互，先 Get 再 Set。
- 若 user 结构较大，那么这个流程中涉及的序列化、反序列带来不必要的开销较大。
- **最严重**的是，Get + Set 两次交互，这个逻辑不是原子的，若应用端存在多个这样流程并发，可能重复插入 101，这造成逻辑错误。由于 TcaplusDB 还不支持事务，应用端解决该问题比较繁琐。

针对上述老版本存在问题，通过条件过滤这一新特性可以解决一些交互多、非原子等问题，示例条件操作代码如下：

```cpp
user u;
// 设置主键
u.set_id(1);
u.set_name("a");
// 先判断gameids是否已经包含101，若不存在（条件不满足）再插入101
int ret = api.UpdateItem(&u, "PUSH gameids#[-1][$ = 101]", "gameids NOT CONTAINS($==101)");
if (ret == COMMON_ERR_EXPR_CONDITION_NOT_MATCHED) // 条件不满足，说明gameids已经存在101了
{
    // ...
}
```

上述代码，`"gameids NOT CONTAINS($==101)"`就是过滤条件，而`"PUSH gameids#[-1][$ = 101]"`是数组操作语句，后文介绍，这里是在 gameids 数组尾部插入 101。

## 3.2 条件过滤接口说明

TcaplusDB 提供的一些 protobuf API 新增了 `const std::string &condition` 入参，用于指定过滤条件，支持 condition 的接口详见后文的 API 附录。

一些接口的使用示例如下，更多的示例可见详细 example。

```cpp
// tcaplus_coroutine_pb_api.h
int Set(::google::protobuf::Message *msg, const std::string &operation = "", const std::string &condition = "");

int FieldInc(const std::set<std::string> &dottedpaths, ::google::protobuf::Message *msg, const std::string &operation = "", const std::string &condition = "");

int Traverse(::google::protobuf::Message *msg, const std::string &condition, TcaplusTraverseCallback *cb);

// example.cpp
user u;
// 设置主键
u.set_id(1);
u.set_name("a");
// 设置其他内容
// ...
// 当rank>100，才执行Set操作，若条件不满足会返回对应的错误码
int ret = api.Set(&u, "", "rank > 100");
if (ret == COMMON_ERR_EXPR_CONDITION_NOT_MATCHED) { ... } // 条件不满足的情况

// 设置递增的步长
u.set_rank(1);
// 当rank达到上限100之后，不在对rank递增，否则 +1
std::set<std::string> dottedpaths;
dottedpaths.insert("rank");
ret = api.FieldInc(dottedpaths, &u, "", "rank < 100");

// 遍历2021-01-01年之后修改过的记录，这里$.LastAccessTime是记录的内建属性，表示记录的最后更新时间
int ret = api.Traverse(&u, "$.LastAccessTime >= \"2021-01-01\"", &traverseCallback);
```

## 3.3 条件过滤语法说明

过滤条件是类 SQL 的 where 语句的语法，已支持以下几种过滤能力

- 比较，如 `rank > 1` ，比较符有 `>, >=, <, <=, ==, !=` 。
- 逻辑运算，如 `rank > 1 AND rank < 10`，运算符有 `AND, OR, NOT`。
- `CONTAINS` 和 `NOT CONTAINS`，即判断是否包含，如 `"mailbox CONTAINS(title == \"tcaplus\")"` 表示要求 mailbox 包含一个 title 等于"tcaplus"的元素。CONTAINS 括号中可以是更复杂的子条件。
- 引用内建属性`$.LastAccessTime`，该内建属性表示记录的最后更新时间，最小精度为秒，可用于和字符串表示的时间进行比较，如"2021"、"2021-01-01"或"2021-01-01 00:00:00"。
- 当前数组元素的引用`$`，如，`"gameids NOT CONTAINS($==101)"`。

完整语法如下

```
condition_expr ::=
      array CONTAINS '(' condition ')'
    | array NOT CONTAINS '(' condition ')'
    | condition

condition ::=
      operand
    | operand comparator operand
    | condition AND condition
    | condition OR condition
    | NOT condition

comparator ::=
      ==
    | <
    | >
    | <=
    | >=
    | !=

array ::=
    identifier

operand ::=
      identifier
    | number
    | string
    | $
    | $.LastAccessTime
```

- **语法说明**

  - **identifier**: 一个合法的标识名称，在这里是字段名或字段名路径，如`name`或`mail.title`。
  - **number**: 整型或浮点数，不支持大整数。
  - **string**: 双引号括起来的字符串。

- **比较**

  - 不同精度的整型或浮点型的数值都是可以相互比较的，这和 C++语言中是一致的，例如 int16 和 int32 比较，前者的类型会被提升之后再比较，整型和浮点比较，整型则会先被提升为浮点型。
  - int 和 uint 可比较，会先比较符号位。
  - 浮点型的等值会有精度偏差。
  - 字符串也是可比较，按照字母字典序，这和 C++中的 std::string 的比较行为是一致的。
  - 数值类型和字符串直接不可比较。

- **操作符优先级**

  - 条件表达式 condition 中，操作符优先级，从高到低为 `comparator NOT AND OR` ，例如 `"a==1 OR a>10 AND a<20"` ，会先计算 AND 的结果再计算 OR。
  - 当然可以使用括号来分隔条件表达式，例如 `"(a==1 OR a>10) AND a<20"` 则就先计算 OR。

## 3.4 过滤条件的约束

- 当前 API 的 condition 入参的文本最大长度（包括'\0'结束符）为 1024。
- 基本类型仅支持整型、浮点型、字符串类型，其他如枚举、二进制等类型不支持。
- 不支持嵌套数组。
- 条件表达式中必须引用字段，不支持`1==1`这种不引用任何字段的表达式。
- 不支持嵌套的 contains，如不支持 `"$.LastAccessTime > \"2020\" AND gameids CONTAINS($ == 101)"` ，而 `gameids CONTAINS($ == 101)"` 则合法。

## 3.5 性能优化建议

条件过滤的性能和 1）条件表达式、2）表的模式 有关，满足以下规则时，有针对性的性能优化（仅供参考，内部实现可能会调整）：

- 当条件表达式只用到 key 字段（包括主键和 index 字段）和记录的属性字段（如$.LastAccessTime），仅通过记录的主键或索引即可进行判断，无需从存储引擎读取全量数据
- 对于 SortList 表，当条件表达式只使用了 sort 字段，也有性能优化
- 对于 SortList 表，表定义的排序字段只有一个，且条件表达式是简单的二元比较（如`"field >= 1"`和`"field == 1"`等），有更好的性能优化，即使用二分查找

# 4. 新特性-数组操作

## 4.1 新特性背景介绍

目前 TcaplusDB 的 API（如 Set、ListReplace、Get、ListGet ）的操作粒度是完整记录，一方面较大的记录不宜频繁操作，另一方面缺乏灵活度。
而 FieldSet、FieldGet、FieldInc 的操作粒度则是一个字段，但接口能力单一同样缺乏灵活度，且不支持数组字段。

为了支持对数组的灵活操作，即 protobuf 中 repeated 字段，类似 Redis 中对 list、set 等数据结构的操作能力，TcaplusDB 提供对数组的命令式的操作。具备如下能力：

- PUSH 操作：在数组指定位置插入新的元素数据。
- SET 操作：修改数组指定位置的元素数据。
- POP 操作：删除数组中**某些下标范围**或者满足**某些条件**的元素。
- GET 查询：指定记录的 key，查询数组返回数组中**某些下标范围**或者满足**某些条件**的元素（即仅记录的局部数据）。

## 4.2 新特性接口介绍

支持的**PUSH、SET、POP**操作 使用 `UpdateItem` 接口，支持的**Get**使用 `Query` 接口，其中 generic 表的 coroutine 接口定义如下：

```cpp
// 入参/出参 msg：包含用户输入的key值，返回修改后的数据也填入到msg
// 入参 operation：数组操作语句，即PUSH、SET、POP
// 入参 condition：记录的过滤条件
int UpdateItem(::google::protobuf::Message *msg, const std::string &operation, const std::string &condition = "");

// 入参/出参 msg：包含用户输入的key值，返回查询的局部数据也填入到msg，注意，返回的msg会和输入的msg合并，相当于接口内部调用Message::MergeFrom
// 入参 operation：数组查询语句，即GET
// 入参 queryOption：查询选项，当前仅支持通过TCAPLUS_PB_API_QUERY_RETURN_ARRAY_INDEX指定是否需要返回查询数组元素的原始下标
// 入参 condition：记录的过滤条件
// 出参 vecArrayIndex：若指定TCAPLUS_PB_API_QUERY_RETURN_ARRAY_INDEX，返回查询数组元素的原始下标
int Query(::google::protobuf::Message *msg, const std::string &query, int queryOption, const std::string &condition, std::vector<int> &vecArrayIndex);
```

## 4.3 新特性使用示例

这里列举几个使用示例，数组操作的详细语法见下一小节，更多示例见 example。

### 4.3.1 数组更新示例

```cpp
user u;
// 设置主键
u.set_id(1);
u.set_name("a");

// 在mailbox数组尾部插入一个元素（同时对元素内的title等字段赋值），这里-1表示尾部的数组下标
int ret = api.UpdateItem(&u, "PUSH mailbox #[-1] [title = \"tcaplus\", content = \"...\"]");

// 当gameids数组不包含101时，在gameids数组头部插入一个元素101，这里0表示头部的数组下标，$表示当前操作的数组元素
// 注意，这里还多了第三个参数，即条件过滤，仅当条件满足时，才执行push操作
ret = api.UpdateItem(&u, "PUSH gameids #[0] [$ = 101]", "gameids NOT CONTAINS($ == 101)");

// 删除mailbox数组下标0 ~ 10范围内，且title不等于"tcaplus"的元素
ret = api.UpdateItem(&u, "POP mailbox #[0-10] [title != \"tcaplus\"]");

// 修改指定下标为1的元素
ret = api.UpdateItem(&u, "SET gameids #[1] [$ = 101]");
```

### 4.3.2 数组查询示例

```cpp
user u;
// 设置主键
u.set_id(1);
u.set_name("a");

// 假设服务端user.mailbox包含4个元素，为
// [ { "tcaplus", "..." }, { "not-tcaplus", "..." }, { "tcaplus", "..." }, { "not-tcaplus", "..." } ]

// 查询mailbox数组下标为0 ~ 2且title为"tcaplus"的元素
// 这里仅返回局部数据，即两个数组元素，并填入u中，也就是说u既作为入参（提供主键），也是出参（包含返回的局部数据）
// option设置TCAPLUS_PB_API_QUERY_RETURN_ARRAY_INDEX时，则也会返回这两个元素的下标，即0和2
int option = 0;
option |= TCAPLUS_PB_API_QUERY_RETURN_ARRAY_INDEX;
std::vector<int> vecArrayIndex;
ASSERT_EQ(0, u.mailbox_size());
int ret = api.Query(&u, "GET mailbox #[0-2] [title == \"tcaplus\"]", option, "", vecArrayIndex);
if (ret == 0) {
    ASSERT_EQ(2, u.mailbox_size());
    ASSERT_EQ(0, vecArrayIndex[0]);
    ASSERT_EQ(2, vecArrayIndex[1]);
}
```

## 4.4 新特性语法说明

PUSH 和 SET 语法一样，只有语义上的差异，前者在指定位置插入，后者在指定位置原地修改。

PUSH 和 SET 语法由 3 部分组成，即 `数组字段名称` + `下标` + `若干个赋值表达式`。其中当元素是组合类型，是各个字段的赋值，当元素是基本类型，使用`$`引用当前元素进行赋值。

POP 的语法由 3 部分组成，即 `数组字段名称` + `下标范围` + `嵌套的过滤条件`，后两个是可选的。
和 PUSH 不同，POP 可以某些下标范围而不仅仅是单个下标，这里过滤条件的语法和上一章节记录的过滤条件一样，区别是过来条件的上下文不一样，这里的是元素的数据，而另一个是整个记录的数据。

GET 的语法和 POP 一样。

完整的操作语法如下

```
push_expr ::=
    PUSH array # '[' index ']' '[' assign_expr [, assign_expr]* ']'

set_expr ::=
    SET array # '[' index ']' '[' assign_expr[, assign_expr]* ']'

pop_expr ::=
      POP array
    | POP array # '[' index_range ']'
    | POP array # '[' index_range ']' '[' condition ']'

get_expr ::=
      GET array
    | GET array # '[' index_range ']'
    | GET array # '[' index_range ']' '[' condition ']'

assign_expr ::=
      identifier = number | string
    | $ = number | string

index ::=
    integer

index_range ::=
      index [, index_range]*
    | index - index [, index_range]*
```

- **语法说明**

* **assign_expr**: 赋值表达式，若数组元素是组合类型，则是内部字段的赋值`title = \"tcaplus\", content = \"...\"`，若元素是基本类型，则使用`$`引用元素本身，如`$=101`。
* **index**: 数组的下标，其中 0 表示数组头部下标，在不知道数组大小情况下，使用-1 表示尾部的下标。
* **index_range**: 数组下标范围，如 "`0 - -1`" 表示所有的下标，也可以表示多个不连续的范围，如 "`0 - 8, -1`"。
* **condition**: 嵌套的过滤条件，语法和上一章节记录的条件过滤一致，区别在于，前者的语义上下文是整个记录的数据，这里的是数组元素中的数据。

- **PUSH/SET 的说明**

* 赋值表达式中，当前只支持对整型、浮点、字符串类型的字段赋值，且必须非 repeated 类型，若 repeated 字段本身是基本类型，则使用`$`引用的元素自身。
* 赋值表达式中，不同类型整型、浮点型可以相互赋值，即会进行类型强转，有可能出现截断等情况，类型转换的行为和 C++中一致。
* 对于 SET，若数组大小为 N，下标的有效范围是 0 ~ (N - 1)或-1。
* 对于 PUSH，若数组大小为 N，下标的有效范围是 0 ~ N 或-1。

- **POP 的说明**

* POP，删除某些范围或某些满足条件的元素。
* 基于 `删除不存在的元素不会报错` 的原则，pop 指定一个不存在的下标范围时，不会报错，例如数组大小为 10，`8-80`会删除最后 2 个元素。

## 4.5 新特性约束

- 当前 API 的 operation 或 query 入参的文本最大长度（包括'\0'结束符）为 1024。

# 5. 示例代码

## 5.1 示例代码说明

在 TcaplusPbApiXXX.tar.gz 的 API 包中有提供 example 代码：

Generic 表的 async 接口示例：

```
release/x86_64/examples/tcaplus/C++_pb3_asyncmode_simpletable/SingleOperation/condition_operation
├── condition_del.cpp
├── condition_fieldinc.cpp
├── condition_fieldset.cpp
├── condition_get.cpp
├── condition_indexget.cpp
├── condition_set.cpp
├── condition_traverse.cpp
├── conv.sh
├── envcfg.env
├── main.cpp
├── Makefile
├── operate_array.cpp
├── readme.txt
├── table_test.proto
├── tlogconf.xml
```

Generic 表的 coroutine 接口示例：

```
release/x86_64/examples/tcaplus/C++_pb3_coroutine_simpletable/SingleOperation/condition_operation
├── condition_del.cpp
├── condition_fieldinc.cpp
├── condition_fieldset.cpp
├── condition_get.cpp
├── condition_indexget.cpp
├── condition_set.cpp
├── condition_traverse.cpp
├── conv.sh
├── envcfg.env
├── main.cpp
├── Makefile
├── operate_array.cpp
├── readme.txt
├── table_test.proto
└── tlogconf.xml
```

List 表的 async 接口示例：

```
release/x86_64/examples/tcaplus/C++_pb3_asyncmode_simpletable/SingleOperation/list_condition_operation
├── condition_listbatchdel.cpp
├── condition_listdel.cpp
├── condition_listgetall.cpp
├── condition_listget.cpp
├── condition_listreplace.cpp
├── condition_listtraverse.cpp
├── conv.sh
├── envcfg.env
├── main.cpp
├── Makefile
├── operate_array.cpp
├── readme.txt
├── table_test.proto
└── tlogconf.xml
```

List 表的 coroutine 接口示例：

```
release/x86_64/examples/tcaplus/C++_pb3_coroutine_simpletable/SingleOperation/list_condition_operation
├── condition_listbatchdel.cpp
├── condition_listdel.cpp
├── condition_listgetall.cpp
├── condition_listget.cpp
├── condition_listreplace.cpp
├── condition_listtraverse.cpp
├── conv.sh
├── envcfg.env
├── main.cpp
├── Makefile
├── operate_array.cpp
├── readme.txt
├── table_test.proto
└── tlogconf.xml
```

## 5.2 示例代码使用

### 5.2.1 代码编译

- **步骤 1，下载资源**
  根据 2.1 章节的资源部分下载相关代码。
- **步骤 2, 依赖安装**

```
yum install -y autoconf automake libtool curl make g++ unzip gcc-c++ openssl openssl-devel zlib-devel
```

- **步骤 3，protobuf 安装**
  将下载的 protobuf 包解压，执行:

```
tar zxvf protobuf-cpp-3.5.1.tar.gz
cd protobuf-3.5.1
./configure --prefix=/usr/local/protobuf
make && make install

#查看安装是否ok
/usr/local/protobuf/bin/protoc --version
3.5.1
```

- **步骤 4, TSF4G 依赖安装**

```
tar zxvf TSF4G_BASE-2.7.37.0a1db41b8_X86_64_Release.tar.gz
mkdir /usr/local/tsf4g
mv ./TSF4G_BASE-2.7.37.0a1db41b8_X86_64_Release/release/x86_64/* /usr/local/tsf4g/
```

- **步骤 5, SDK 示例连接信息**

```
tar zxvf TcaplusPbApi3.52.0.203403.x86_64_release_20210304.tar.gz
cd TcaplusPbApi3.52.0.203403.x86_64_release_20210304/release/x86_64/examples/C++_common_for_pb2
# 编辑common.h
// 目标业务的tcapdir地址, 腾讯云页面地址
static const char DIR_URL_ARRAY[][TCAPLUS_MAX_STRING_LENGTH] =
{
    "tcp://172.17.32.17:9999"
};

// 目标业务的tcapdir 地址个数
static const int32_t DIR_URL_COUNT = 1;
// 目标业务的表名
static const char * TABLE_NAME = "user";
// 目标业务的App ID，集群接入ID
static const int32_t APP_ID = 70;
// 目标业务的Zone ID, 集群表格组id
static const int32_t ZONE_ID = 1;
// 目标业务的业务密码, 集群访问密码，腾讯云环境：腾讯云控制台集群信息页面查看，Docker环境：Web运维平台业务维护页面查看
static const char * SIGNATURE = "xxx";
```

- **步骤 6, 示例代码编译，以 Generic 表的同步接口示例中的 condition_operation 条件过滤举例**

```
#进示例目录
cd ./TcaplusPbApi3.52.0.203403.x86_64_release_20210304/release/x86_64/examples/tcaplus/C++_pb3_coroutine_simpletable/SingleOperation/condition_operation
```

```
#配置环境变量，打开文件envcfg.env，如下四个环境变量，根据实际部署目录来设置:

export TSF4G_HOME=/usr/local/tsf4g;

export PROTOBUF_HOME=/usr/local/protobuf;

export TCAPLUS_PB_HOME=/root/TcaplusPbApi3.52.0.203403.x86_64_release_20210304/release/x86_64;

export TCAPLUS_HOME=/root/TcaplusPbApi3.52.0.203403.x86_64_release_20210304/release/x86_64;

#配置完后，让环境变量生效
source envcfg.env
```

```
#配置conv.sh, 生成表定义接口代码
#将表定义文件通过protoc转换为h和cc文件, 如下所示
${PROTOBUF_HOME}/bin/protoc -I${PROTOBUF_HOME}/include/ -I${TCAPLUS_PB_HOME}/include/tcaplus_pb_api/ --cpp_out=./  --proto_path=./ table_test.proto
```

```
#配置Makefile, 主要是检查LIBS和INC配置，如下：
LIBS += -L $(PROTOBUF_HOME)/lib -L$(TCAPLUS_HOME)/lib -L$(TCAPLUS_PB_HOME)/lib -L$(TSF4G_HOME)/lib \
-Wl,-Bstatic -ltcaplusprotobufapi -ltsf4g_r -lprotobuf -lreadline -lncurses -lscew -lexpat -Wl,-Bdynamic -lpthread -lz -ldl -lcrypto -lanl

INC =-I$(PROTOBUF_HOME)/include -I$(TCAPLUS_PB_HOME)/include/tcaplus_pb_api/ -I../../../C++_common_for_pb2
```

```
#最后代码编译
source envcfg.env
sh conv.sh
make
```

如果代码编译有问题，请检查环境变量是否生效及表代码接口定义文件是否生成。

## 5.3 示例代码说明

示例代码分同步和异步代码:

```
#同步代码示例目录
TcaplusPbApi3.52.0.203403.x86_64_release_20210304/release/x86_64/examples/tcaplus/C++_pb3_coroutine_simpletable
#异步代码示例目录
TcaplusPbApi3.52.0.203403.x86_64_release_20210304/release/x86_64/examples/tcaplus/C++_pb3_asyncmode_simpletable
```

涉及条件和数组相关的示例在各自的子目录中：

- **condition_operation**: Generic 表条件操作相关的示例
- **list_condition_operation**: List 表条件操作相关的示例

### 5.3.1 Generic 表示例

| 示例代码文件           | 文件说明                                               |
| ---------------------- | ------------------------------------------------------ |
| operate_array.cpp      | 数组相关条件操作，包含 PUSH,POP,SET,GET 子操作示例     |
| condition_set.cpp      | 记录级条件插入或更新，满足条件的记录才会操作成功       |
| condition_get.cpp      | 记录级条件查询，满足条件的记录才会返回                 |
| condition_del.cpp      | 记录级条件删除，满足条件的记录才会删除                 |
| condition_fieldset.cpp | 记录级部分字段的插入或更新，满足条件的记录才会操作成功 |
| condition_fieldinc.cpp | 记录级部分字段自增，满足条件的记录才会自增成功         |
| condition_traverse.cpp | 记录级表过滤遍历，满足条件的记录才会返回               |
| condition_indexget.cpp | 记录级主键索引过滤，满足条件的记录才会返回             |

### 5.3.2 List 表示例

示例代码目录：`TcaplusPbApi3.52.0.203403.x86_64_release_20210304/release/x86_64/examples/tcaplus/C++_pb3_asyncmode_simpletable/SingleOperation/list_condition_operation`。
|示例代码文件|文件说明|
|---|---|
| operate_array.cpp|list 表数组条件过滤操作，包含 PUSH,POP,SET,GET|
|condition_listget.cpp|list 表记录级元素过滤操作，满足条件的 list 元素记录才会返回, 返回 1 个元素记录|
|condition_listdel.cpp|ist 表记录级元素删除操作，满足条件的 list 元素记录才会删除，删除 1 个元素记录|
|condition_listreplace.cpp|数 ist 表记录级元素替换操作，满足条件的 list 元素记录才会替换|
|condition_listgetall.cpp|ist 表记录级元素查询操作，满足条件的 list 元素记录才会返回，返回多个元素记录|
|condition_listbatchdel.cpp|ist 表记录级元素批量删除操作，满足条件的 list 元素记录才会删除，删除多个元素记录|
|condition_listtraverse.cpp|list 表记录级元素遍历操作，满足条件的 list 记录才会遍历返回|

# 6. API 附录

这里仅列出 coroutine 的 API，async 的类似。

## 6.1 支持过滤条件的 API（Generic 表）

```cpp
    /**
    *    @brief 根据用户输入的msg中的key值，获取msg消息的字段值，并填充到msg中。
    *
    *    @param [INOUT] msg   用户输入的key值，返回指定字段填到msg中
    *    @param [IN] condition 过滤条件
    *    @retval COMMON_ERR_EXPR_CONDITION_NOT_MATCHED  condition不满足
    *    @retval <0   失败，返回对应的错误码。
    *    @retval 0    成功。
    */
    int Get(::google::protobuf::Message *msg, const std::string &condition = "");

    struct IndexGetRequest
    {
        ::google::protobuf::Message* m_pMsg;
        std::string m_condition;
        // . . .
    };

    /**
    *    @brief 根据用户输入的req中的index名称，msg值，offset以及limit，通过索引获取多个记录的值填充到res中的vec结构中，并返回总记录数以及剩余记录数。
    *
    *    @param [INOUT] req   用户输入的req
    *    @param [INOUT] res   用户输入的res
    *    @retval <0   失败，返回对应的错误码。
    *    @retval 0    成功。
    */
    int Get(NS_TCAPLUS_PROTOBUF_API::IndexGetRequest& req, NS_TCAPLUS_PROTOBUF_API::IndexGetResponse *res);

    /**
    *    @brief 根据用户输入的msg中的key值，删除msg。
    *
    *    @param [IN] msg   数据记录msg，包含用户需要删除记录的key值
    *    @param [IN] condition 过滤条件，仅当该条件满足时才会执行Del操作。
    *    @retval TXHDB_ERR_RECORD_NOT_EXIST  记录不存在。
    *    @retval COMMON_ERR_EXPR_CONDITION_NOT_MATCHED  condition不满足，不会删除记录。
    *    @retval <0   失败，返回对应的错误码。
    *    @retval 0    成功。
    */
    int Del(::google::protobuf::Message *msg, const std::string &condition = "");

    /**
    *    @brief 根据用户输入的msg中的key，如果记录存在更新指定记录的值，否则插入指定记录。
    *           当记录不存在，传入的msg会作为新记录插入并返回成功，这种情况下condition会被忽略。
    *           可以通过记录的version进行区分记录是新插入还是修改之后的，version == 1 则表示记录是新插入的，version > 1的情况表示更新之后的
    *
    *    @param [INOUT] msg   用户输入的key值，以及需要设置的数据记录msg。
    *    @param [IN] operation 在Set操作执行成功的基础上，再执行的附加操作。operation是对数组的操作表达式，即PUSH或POP操作。
    *    @param [IN] condition 过滤条件，仅当该条件满足时才会执行Set操作。
    *    @retval COMMON_ERR_EXPR_CONDITION_NOT_MATCHED  condition不满足，不更新记录。
    *    @retval <0   失败，返回对应的错误码。
    *    @retval 0    成功。
    */
    int Set(::google::protobuf::Message *msg, const std::string &operation = "", const std::string &condition = "");

    /**
    *    @brief 根据用户输入的msg中的key值和values增量值，和dottedpaths指定的字段名称，增加msg指定字段的值。字段为数值型变量。
    *    @param [INOUT] msg   数据记录msg，包含用户输入的key值，返回增量字段的结果值更新到msg中
    *    @param [IN] dottedpaths 字段名称的点分嵌套字符串集
    *    @param [IN] operation 在FieldInc操作执行成功的基础上，再执行的附加操作。operation是对数组的操作表达式，即PUSH或POP操作
    *    @param [IN] condition 过滤条件，仅当该条件满足时才会执行修改操作。
    *    @retval TXHDB_ERR_RECORD_NOT_EXIST  记录不存在。
    *    @retval COMMON_ERR_EXPR_CONDITION_NOT_MATCHED  condition不满足，没有任何字段更新。
    *    @retval <0   失败，返回对应的错误码。表示没有任何字段更新。
    *    @retval 0    成功。全部字段更新成功。
    */
    int FieldInc(const std::set<std::string> &dottedpaths, ::google::protobuf::Message *msg,
                 const std::string &operation = "", const std::string &condition = "");

    /**
    *    @brief 根据用户输入的msg中的key值，和dottedpaths指定的字段名称，更新msg指定字段的值。服务端不存在的值会追加进去。
    *
    *    @param [IN] msg   数据记录msg，包含用户输入的key值，返回指定字段填到msg中
    *    @param [IN] dottedpaths 字段名称的点分嵌套字符串集
    *    @param [IN] operation 在FieldSet操作执行成功的基础上，再执行的附加操作。operation是对数组的操作表达式，即PUSH或POP操作
    *    @param [IN] condition 过滤条件，仅当该条件满足时才会执行FieldSet操作。
    *    @retval TXHDB_ERR_RECORD_NOT_EXIST  记录不存在。
    *    @retval COMMON_ERR_EXPR_CONDITION_NOT_MATCHED  condition不满足，没有任何字段更新。
    *    @retval <0   失败，返回对应的错误码。表示没有任何字段更新。
    *    @retval 0    成功。全部字段更新成功。
    */
    int FieldSet(const std::set<std::string> &dottedpaths, ::google::protobuf::Message *msg,
                 const std::string &operation = "", const std::string &condition = "");

    /**
    *    @brief 遍历表，消息会填充到msg中。
    *
    *    @param [INOUT] msg   返回指定字段填到msg中
    *    @param [IN] condition  过滤条件
    *    @param [INOUT] cb   回调函数
    *    @retval <0   失败，返回对应的错误码。
    *    @retval 0    遍历成功完成。
    */
    int Traverse(::google::protobuf::Message *msg, const std::string &condition, TcaplusTraverseCallback *cb);
```

## 6.2 支持过滤条件的 API（List 表）

```cpp
    /**
    *    @brief 根据用户输入req中的的msg中的key值，和dottedpaths指定的字段名称，获取List表中全部记录指定字段的值，并填充, 然后回调cb。
    *
    *    @param [INOUT] req   数据记录msg，包含用户输入的key值，返回指定字段填到msg中
    *    @param [OUT] cb 返回结果集回调
    *    @retval <0   失败，返回对应的错误码。
    *    @retval 0    成功。至少有一个字段查询成功才会返回0。
    */
    int ListGetAll(NS_TCAPLUS_PROTOBUF_API::ListGetAllRequest &req, TcaplusTraverseCallback *cb);

    /**
    *    @brief 根据用户输入req中的的msg中的key值，删除List表中全部记录指定字段的值，并填充到res中。
    *
    *    @param [INOUT] req   数据记录msg，包含用户输入的key值，返回指定字段填到msg中
    *    @param [OUT] res 返回结果集
    *    @retval SVR_ERR_FAIL_INVALID_INDEX  req.m_setElemIndexes传入的下标无效，不会有任何数据变更。
    *    @retval COMMON_ERR_EXPR_CONDITION_NOT_MATCHED  对于req.m_setElemIndexes中所有的下标，没有任何一个condition不满足，不会有任何数据变更。
    *    @retval 0    成功。至少有一个字段查询成功才会返回0。
    */
    int ListBatchDel(NS_TCAPLUS_PROTOBUF_API::ListBatchDeleteRequest &req, NS_TCAPLUS_PROTOBUF_API::ListBatchDeleteResponse *res);

    /**
    *    @brief 根据用户输入req中的的msg中的key值，在List表中指定位置更新记录，并填充到res中。
    *
    *    @param [INOUT] req   数据记录msg，包含用户输入的key值，返回指定字段填到msg中
    *    @param [OUT] res 返回结果
    *    @retval <0   失败，返回对应的错误码。
    *    @retval 0    成功。至少有一个字段查询成功才会返回0。
    */
    int ListReplace(NS_TCAPLUS_PROTOBUF_API::ListReplaceRequest &req, NS_TCAPLUS_PROTOBUF_API::ListReplaceResponse *res);

    /**
    *    @brief 根据用户输入req中的的msg中的key值，在List表中指定key的指定下标记录，并填充到res中。
    *
    *    @param [INOUT] req   数据记录msg，包含用户输入的key值，返回指定字段填到msg中
    *    @param [OUT] res 返回结果集
    *    @retval <0   失败，返回对应的错误码。
    *    @retval 0    成功。至少有一个字段查询成功才会返回0。
    */
    int ListGet(NS_TCAPLUS_PROTOBUF_API::ListGetRequest &req, NS_TCAPLUS_PROTOBUF_API::ListGetResponse *res);

    /**
    *    @brief 根据用户输入req中的的msg中的key值，删除List表中指定key的指定下标记录，并填充到res中。
    *
    *    @param [INOUT] req   数据记录msg，包含用户输入的key值，返回指定字段填到msg中
    *    @param [OUT] res 返回删除结果集
    *    @retval <0   失败，返回对应的错误码。
    *    @retval 0    成功。至少有一个字段查询成功才会返回0。
    */
    int ListDel(NS_TCAPLUS_PROTOBUF_API::ListDeleteRequest &req, NS_TCAPLUS_PROTOBUF_API::ListDeleteResponse *res);

    /**
    *    @brief 根据用户输入req中指定信息，遍历List表所有记录，并填充到cb中。
    *
    *    @param [INOUT] req   数据记录msg，包含用户输入的key值，返回指定字段填到msg中
    *    @param [OUT] cb 返回遍历结果集
    *    @retval <0   失败，返回对应的错误码。
    *    @retval 0    成功。至少有一个字段查询成功才会返回0。
    */
    int ListTraverse(NS_TCAPLUS_PROTOBUF_API::ListTraverseRequest &req, TcaplusTraverseCallback *cb);
```

## 6.3 数组操作的 API（Generic 表）

```cpp
    /**
    *    @brief 通过query描述的表达式在记录中进行查询，返回记录的局部数据内容，仅支持数组内的查询
    *
    *    @param [INOUT] msg     数据记录msg，作为输入时包含主键的值，作为输出时包含响应返回的数据
    *                           注意，返回的msg会和输入的msg合并，即接口内部调用Message::MergeFrom
    *    @param [IN] query      查询语句
    *    @param [IN] queryOption 查询选项设置，见NS_TCAPLUS_PROTOBUF_API::TcaplusQueryOption
    *    @param [IN] condition  记录级别的过滤条件，仅当该条件满足时才会执行query操作，== ""则不作过滤。
    *    @param [OUT] vecArrayIndex 当返回的结果是局部数组时，且query指定TCAPLUS_PB_API_QUERY_RETURN_ARRAY_INDEX，这里返回局部数组的元素下标
    *    @retval TXHDB_ERR_RECORD_NOT_EXIST  记录不存在。
    *    @retval COMMON_ERR_EXPR_CONDITION_NOT_MATCHED  condition不满足。
    *    @retval <0   失败，返回对应的错误码。表示没有任何字段更新。
    *    @retval 0    成功。全部字段更新成功。
    */
    int Query(::google::protobuf::Message *msg, const std::string &query, int queryOption,
        const std::string &condition, std::vector<int> &vecArrayIndex);

    /**
    *    @brief 通过operation描述的表达式执行对数组的操作
    *
    *    @param [INOUT] msg   数据记录msg，包含用户输入的key值，返回指定字段填到msg中
    *    @param [IN] operation 对数组的操作表达式，即PUSH或SET或POP操作
    *    @param [IN] condition 过滤条件，仅当该条件满足时才会执行UpdateItem操作。
    *    @retval TXHDB_ERR_RECORD_NOT_EXIST  记录不存在。
    *    @retval COMMON_ERR_EXPR_CONDITION_NOT_MATCHED  condition不满足，没有任何字段更新。
    *    @retval <0   失败，返回对应的错误码。表示没有任何字段更新。
    *    @retval 0    成功。全部字段更新成功。
    */
    int UpdateItem(::google::protobuf::Message *msg, const std::string &operation, const std::string &condition = "");
```

## 6.4 数组操作的 API（List 表）

```cpp
    /**
    *    @brief 通过operation描述的表达式执行对数组的操作
    *
    *    @param [IN]  req 操作请求，包含msg、操作表达式、条件过滤表达式
    *    @param [OUT] res 返回操作结果
    *    @retval <0   失败，返回对应的错误码。表示没有任何字段更新
    *    @retval 0    成功。全部字段更新成功。
    */
    int ListUpdateItem(NS_TCAPLUS_PROTOBUF_API::ListUpdateItemRequest &req, NS_TCAPLUS_PROTOBUF_API::ListUpdateItemResponse *res);

    /**
    *    @brief 通过query描述的表达式在记录中进行查询，返回记录的局部数据内容，仅支持数组内的查询
    *
    *    @param [INOUT] msg     数据记录msg，作为输入时包含主键的值，作为输出时包含响应返回的数据
    *    @param [IN]  req 查询请求，包含msg、查询表达式、条件过滤表达式
    *    @param [OUT] res 返回操作结果
    *    @retval TXHDB_ERR_RECORD_NOT_EXIST  记录不存在。
    *    @retval COMMON_ERR_EXPR_CONDITION_NOT_MATCHED  condition不满足。
    *    @retval <0   失败，返回对应的错误码。表示没有任何字段更新。
    *    @retval 0    成功。全部字段更新成功。
    */
    int ListQuery(NS_TCAPLUS_PROTOBUF_API::ListQueryRequest &req, NS_TCAPLUS_PROTOBUF_API::ListQueryResponse *res);
```
