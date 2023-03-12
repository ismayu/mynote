```protobuf
protoc -I=. -I/usr/local/include -I=$(GOPATH)/src --go_out=.simple.proto
```

-I 指定了protoc的搜索import的proto的文件夹，在macOS中protobuf把一些扩展的proto放在了/usr/local/include对应文件夹中，一些三方的Go库放在了gopath对应的包下，所以这里都把它们加上了。对于这个简单的例子，实际是不需要的。

cpp_out生成C++代码，java_out生成Java代码，python_out生成Python代码，类似还有csharp_out，objc_out，ruby_out，php_out等参数。

生成的代码我们指定放在本地文件夹中(--go_out=.)。

#### proto3格式

##### 版本定义

```protobuf
syntax = "proto3"
```

##### 引入其他proto文件

```protobuf
import "other.proto";
import public "other2.proto"
import weak "other.proto"
```

比较少用的是public和weak关键字，默认情况下weak引入的文件不允许不存在(missing),只为了google内使用。public具有传递性，若你在文件中通过public引入第三方的proto文件，那么引入你这个文件同时也会引入第三方的proto。一般会忽略public和weak关键字。

##### pakage

定义proto的包名，报名可以避免对message类型之间的名字冲突，同名的Message可以通过package进行区分。在没有为特定语言定义option xxx_pakage的时候，它还可以用来生成特定语言的包名，比如Java package,go package。

```protobuf
package foo.bar;
```

##### option

option可以用在proto的scope中，或者message，enumerate，service的定义中。

可以是Protobuf定义的option，或者自定义的option。

option的定义格式是"option" optionName "=" constant ";" 

```protobuf
option java_package = "com.example.foo";
```

一些Protobuf定义的option:

java_package, java_multiple_files, java_outer_calssname,optimize_for, cc_enable_arenas, objc_class_prefix, deprecated

```protobuf
option(gogoproto.testgem_all) = true;
option(gogoproto.populate_all) = true;
option(gogoproto.benchgen_all) = true;

message NidRepPackedNative{
	repeated double Field1 = 1 [(gogoproto.nullable) = false, packed = true];
	repeated float Field2 = 2 [(gogoproto.nullable) = false, packed = true];
	repeated int32 Field3 = 3 [(gogoproto.nullable) = false, packed = true];
}
```

##### 普通字段

普通字段的格式为 field = ["repeated"] type fieldName "=" fieldNumber [ "[" fieldOptions "]" ] ";"

repeated允许字段重复，对于go语言来说，它会编译成数组(slice of type)类型的格式。

- 数字类型：double,float, int32, int64, uint32, uin64, sint32, sint64:存储长度可变的浮点数、整数、无符号整数和有符号整数
- 存储固定大小的数字类型：fixed32， fixed64， sfixed32， sfixed64：存储空间固定
- 布尔类型：bool
- 字符串：string
- bytes：字节数组
- messageType：消息类型
- enumType：枚举类型

字段名、消息名、枚举类型名、map名、服务名等名称首字母必须为字母类型，然后可以是字母、数字或者下划线_。[proto类型与各语言类型对应关系](https://developers.google.com/protocol-buffers/docs/proto3#scalar)

##### Oneof

若有一组字段，同时最多允许这一组中的一个字段出现就可以使用Oneof定义这一组字段，类似于Union，但是Oneof允许设置零各值。

在proto3中没有方法区分正常值还是设置了取得缺省值(比如int64类型字段，如果它的值是0，无法判断数据是否包含这个字段，因为0可能是数据中设置的值，也可能是这个字段的零值)，可以通过Oneof取得这个功能，Oneof可以判断字段是否被设置。

```protobuf
syntax = "proto3";
package abc;
message OneofMessage{
	oneof test_oneof{
		string name  = 4;
		int64  value = 9;
	}
}
```

##### map类型

map类型需要设置键和值的类型，格式是"map" "<" keyType "," type ">" mapName "=" fieldNumber [ "]" fieldOptions "]"。

```protobuf
map<int64, string> values = 1;
```

map字段不能同时使用repeated

##### Reserved

Reserved可以用来指明此message不使用某些字段，也就是忽略这些字段。可以通过字段编号范围或者字段名称指定保留的字段：

```protobuf
syntax = "proto3";
package abc;
mesage AllNormalypes{
reserved 2, 4 to 6;
reserved "field14", "field11";
double   field1  = 1;
// float field2  = 2;
int32 field3 = 3;
// int64 field4  = 4;
// uint32 field5 = 5;
// uint64 field6 = 6;
sint32   field7  = 7;
sint64   field8  = 8;
fixed32  field9  = 9;
fixed64  field10 = 10;
// sifxed32 field11 = 11;
sfixed64  field12 = 12;
bool      field13 = 13;
// string field14 = 14;
bytes     field15 =15;
}
```

声明保留的字段无法定义，编译错误。

##### 枚举类型

| id   | chinese | math | english |
| ---- | ------- | ---- | ------- |
| 1    | 70      | 80   | 50      |
| 2    | 70      | 80   | 70      |
| 3    | 70      | 80   | 90      |
| 4    | 70      | 80   | 66      |
| 5    | 60      | 50   | 40      |
| 6    | 55      | 70   | 80      |
| 7    | 60      | 50   | 90      |
| 8    | 80      | 90   | 100     |
| 9    | 77      | 60   | 60      |

##### Protocol Buffer API

addressbook.proto

```protobuf
package tutorial;

message Person {
  required string name = 1;
  required int32 id = 2;
  optional string email = 3;

  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }

  message PhoneNumber {
    required string number = 1;
    optional PhoneType type = 2 [default = HOME];
  }

  repeated PhoneNumber phone = 4;
}

message AddressBook {
  repeated Person person = 1;
}
```



```
//name
inline bool                 has_name()   const;
inline void                 clear_name();
inline const ::std::string& name()       const;
inline void set_name(const ::std::string& value);
inline void set_name(const char* value);
inline ::std::string* mutable_name();
//id
inline bool has_id() const;
inline void clear_id();
inline int32_t id() const;
inline void set_id(int32_t value);
```

