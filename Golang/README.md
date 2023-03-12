#### 目录结构



#### `go build`

- 普通包：执行`go build`之后，不会产生任何文件；如果需要在`$GOPATH/pkg`生成相应文件，执行`go install`。

- main包：执行`go build`之后，在当前目录生成一个可执行文件。若在`$GOPATH/bin`生成相应文件，执行`go install`或者使用`go build -o path/filename.exe`。

- 若该项目文件夹下有多个文件，只想编译某个文件可以指定文件名编译，`go build filename.go`，`go build`会默认编译当前目录下所有的`go`文件。

- `go build`可以指定编译生成的文件名，`go build -o filename.exe`，非`main`包则为`package`名，`main`包为`main`。

- `go build`会忽略目录下以`_`或`.`开头的`go`文件。

- 若源代码针对不同的操作系统有不同的处理，那么可以根据操作系统后缀来命名文件。例如:
```go
array_linux.go array_darwin.go array_windows.go array_freebsd.go
```
​	`go build`会选择性的编译以系统名结尾的文件。

- 参数介绍

  -o 指定输出的文件名，可以带路径，`go build -o a/b/c`

  -i 安装相应的包，编译+ `go install`

  -a 更新全部已经是最新的包，标准包不适用

  -n 把需要执行的编译命令打印出来，但是不执行，这样很容易知道底层如何运行的

  -p n 指定可以并行运行的编译数目，默认是CPU数目

  -race 开启编译的时候自动检测数据竞争的情况，仅支持64位机器

  -v 打印出正在编译的包名

  -work 打印出编译时候产生临时文件夹的名称，并且如果已经存在的话就不要删除了

  -x 和-n结构类似，但是会执行

  -ccflags 'arg list' 传递参数给5c,6c,8c调用

  -compiler name 指定相应的编译器，gccgo还是gc

  -gccgoflags 'arg list' 传递参数给gccgo编译连接调用

  -gcflags 'arg lisst' 传递参数给5g,6g,8g调用

  -ldflags 'flag list' 传递参数给5l,6l,8l调用

  -tags 'tag list' 设置编译的时候可以适配的tag

#### `go clean`

-   这个命令用来移除当前源码包和关联源码包里面编译生成的文件。包括：

```go
obj/  		    旧的object目录，由Makefiles遗留
_test/ 			旧的test目录，由Makefiles遗留
_testmain.go	旧的gotest目录，由Makefiles遗留
test.out		旧的test目录，由Makefiles遗留
build.out		旧的test目录，由Makefiles遗留
*.[568ao]		object文件，由Makefiles遗留

DIR(.exe)		由go build产生
DIR.test(.exe) 	 由go test -C产生
MAINFILE(.exe)   由go build MAINFILE.go产生
*.so			由SWIG产生
```

- 参数介绍

  -i 清楚关联的安装的包和可运行文件，也就是通过`go install`安装的文件

  -n 把需要执行的清除命令打印出来但不执行，可以知道底层如何运行的

  -r 循环的清除import中引入的包

  -x 打印出来执行的详细命令，是 -n打印执行版本

#### `go fmt`

go工具提供的代码格式化工具，执行`go fmt filename.go`会将代码修改成标准格式。

- 参数介绍
  -l 显示需要格式化的文件

  -w 把改写后的内容直接写入到文件，而不是作为结果打印

  -r 添加形如“a[b:len(a)] -> a[b:]”的重写规则，方便我们做批量替换

  -s 简化文件中的代码

  -d 显示格式化前后的diff而不是写入文件，默认false

  -e 打印所有语法错误到标准输出

  -cpuprofile 支持调试模式，写入享用的cpufile到指定的文件

#### `go get`

这个命令用来动态获取远程代码包，目前支持BItBycket, GitHub, Google Code和Launchpad，这条命令内部实际上分成了两个操作：第一步下载源码包，第二部执行`go install`。下载源码包的go工具会自动根据不同的域名调用不同的源码工具。

- 参数介绍

  -d 只下载不安装

  -f 只有在你包含了-u参数才有效，不让-u去验证import中的每一个都已经获取了，对于本地fork包特别有用

  -fix 在获取源码之后先运行fix，然后再去做其他事情

  -t 同时也下载需要为运行测试所需要的包

  -u 强制使用网络去更新包和依赖包

  -v 显示执行的命令

#### `go install`

这个命令内部分成了两个操作，第一步生成结果文件(可执行文件或者.a包)，第二步会将编译好的结果移动到`$GOPATH/pkg`或者`$GOPATH/bin`。

#### `go test`

执行这个命令，会自动读取源码目录下面名为`*_test.go`的文件，生成并运行测试用的可执行文件。输出的信息类似：

```go
ok   archive/tar   0..11s
FAIL archive/zip   0.022s
ok   compress/gzip 0.033s
...
```

默认情况下无需参数，会将源码包下所有的test文件测试完毕。

- 参数介绍

  -bench regexp 执行相应的benchmarks，例如 -bench=.

  -cover 开启测试覆盖率

  -run regexp 之运行regexp匹配的函数，例如 -run=Array 那么执行包含Array开头的函数

  -v 显示测试的详细命令

#### `go`基础

1. 定义变量

   ```go
   //定义单个类型为"type"变量
   var variableName type
   //定义多个变量
   var vname1, vname2, vname3 type
   //定义多个变量并初始化
   var vname1, vname2, vname3 type = v1, v2, v3
   //简化版定义并初始化
   var vname1, vname2, vname3 = v1, v2, v3
   //编译器推导类型
   vname1, vname2, vname3 := v1, v2, v3
   ```

   `:=`限制在于在函数外部使用会无法通过编译，故一般用var方式来定义全局变量

   `_`是特殊的变量名，任何赋予它的值都会被丢弃

   ```go
   //将值35赋予b，同时丢弃34
   _, b := 34, 35
   ```
   
2. 常量
   在Go程序中，常量可定义为数值、布尔值或者字符串类型，语法如下
   
   ```go
   const constantName = value
   const Pi float32 = 3.1415926
   ```
   
3. 内置基础类型

  ```go
  //Boolean 布尔值的类型为bool，值是true或false,默认值false
  var IsActive bool
  var enable, disabled = true, false
  func test(){
  	var available bool
  	valid := false
  	available = true
  }
  ```

  ```go
  //数值类型 int和uint，两种类型长度相同，但具体长度取决于不同编译器的实现，GO也有直接定义好位数的类型:rune, int8, int16, int32, int64和byte, uin8, uint16, uint32, uint64其中rune是int32别称byte是uint8别称
  //浮点数类型有float32和float64两种，默认为float
  ```

  ```go
  //字符串 go使用utf-8字符集编码，字符串使用""或者``括起来定义，类型为string
  var frenchHello string 
  var emptyString string
  func test(){
  	no, yes, maybe := "no", "yes", "maybe"
  	japaneseHello := "Konichiwa"
  	frenchHellp = "Bonjour"
  }
  //在Go中字符串是不可变的，如下编译时会报错
  var s string = "helllo"
  s[0] = 'c' // error
  //可以使用如下方式修改
  s := "hello"
  c := []byte(s)  //将字符串转化为[]byte类型
  c[0] = 'c'
  s2 := string(c) //再转换回string类型
  fmt.Printf("%s\n", s2)
  //Go中可以使用+操作符连接两个字符串
  s := "hello"
  m := " world"
  a := s + m
  fmt.Printf("%s\n", a)
  //修改字符串也可以写为
  s := "hello"
  s = "c" + s[1:] //字符串不能更改，但可以进行切片操作
  fmt.Printf("%s\n", s)
  //声明纯字符串(所有字符不会进行转义)
  m := `hello
               world`
  ```

  ```go
  //错误类型 Go内置有一个`error`类型，专门处理错误信息，Go的`package`里面还有专门一个包`errors`来处理错误:
  err := errors.New("emit macho dwarf: elf head corrupted")
  if err != {
  	fmt.Print(err)
  }
  ```

  类型存储：

  ![img](https://github.com/astaxie/build-web-application-with-golang/raw/master/zh/images/2.2.basic.png?raw=true)

4. 一些技巧

   ```go
   //可以使用分组的方式进行声明
   import "fmt"
   improt "os"
   const i = 10
   const Pi = 3.14
   const name = "Ma"
   var i int
   var pi float32
   var name string
   
   import(
   	"fmt"
   	"os"
   )
   const(
   	i = 10
   	Pi = 3.14
   	name = "Ma"
   )
   var (
   	i int
   	pi float32
   	name stinr
   )
   ```

5. `iota`枚举

   Go中一个关键字`iota`，这个关键字用来声明`enum`时候才用，默认开始值为0，const中每增加一行加1：

   ```go
   package main
   import(
   	"fmt"
   )
   const (
   	x = iota
   	y = iota
   	z = iota
   	w         //常量声明省略值时，默认和之前值的字面相同 此时 w == 3
   )
   const v = iota //每遇到一个const关键字，iota就会重置 此时 v == 0
   const (
   	h, i, j = iota, iota, iota //h,i,j在同一行值相同
   )
   ```

6. 设计规则

   - 大写字母开头的变量是可导出的，也就是其他包可以读取的，是公有变量;小写字母开头的就是不可导出的，是私有变量。
   - 大写字母开头的函数也是一样，相当于`class`带`public`关键字的公有函数；小写字母开头的就是有`private`关键词的私有函数。

7. array， slice， map

   ```go
   //array是数组，定义方式如下
   var arr [n]type
   //[n]type n为数组长度，type为元素类型，通过[]来进行读取或赋值
   var arr [10]int
   arr[0] = 12
   arr[1] = 13
   //由于长度也是数组的一部分，因此[3]int和[4]int是不同类型，数组之间的赋值是值得赋值，当一个数组作为参数传入函数的时候，传入的其实是该数组的副本，而不是它的指针，如果使用指针，那么就需要使用后面介绍的`slice`类型。
   //数组也可以使用:=声明
   a := [3]int{1, 2, 3}
   b := [10]int{1, 2, 3}//其他默认为0
   c := [...]int{1, 2, 3}//省略长度，Go会根据元素个数来计算长度
   //二维数组
   doubleArray := [2][4]int{[4]int{1,2,3,4}, [4]int{5,6,7,8}}
   //简化
   doubleArray := [2][4]int{{1,2,3,4}, {5,6,7,8}}
   easyArray := [2][4]int{{1, 2, 3, 4}, {5, 6, 7, 8}}
   ```

   ```go
   //slice 在初始定义数组时，并不确定需要多大的数组，因此需要“动态数组”，再GO中这种数据结构叫slice
   //slice并不是真正意义上的动态数组，而是一个引用类型，slice总是指向一个底层array，声明也可以想array一样只是不需要长度
   var fslice []int
   slice := []byte {'a', 'b', 'c', 'd'}
   //slice可以从一个数组或一个存在的slice中再次声明，slice通过array[i:j]中获取，i时开始位置，j时结束位置，不包含array[j],长度为j-i
   ```

   - `slice`默认开始位置为0，`ar[:n]`等价于`ar[0:n]`
   - `slice`的第二个序列默认是数组的长度，`ar[n:]`等价于`ar[n:len(ar)]`
   - 如果从一个数组直接获取`slice`可以直接`ar[:]`默认第一个序列为0，第二个为数组长度等价于`ar[0:len(ar)]`

   `slice`是引用类型，当引用改变其中元素的值时，其他的所有引用都会改变该值，从概念上说`slice`是一个结构体，结构体包含三个元素：一个指针，指向数组中`slice`开始的指定位置；长度，即`slice`长度；最大长度，也就是`slice`开始位置到数组的最后位置的长度

   `slice`内置函数：`len`获取`slice`长度；`cap`获取`slice`的最大容量；`aooend`向`slice`里面追加一个或者多个元素然后返回和`slice`一样类型的`slice`；`copy`函数从源`slice`的`src`中复制元素到目标`dst`，并且返回复制元素的个数

   ```go
   //map 它的格式为map[keyType]valueType
   var numbers map[string]int
   numbers = make(map[string]int)
   numbers["one"] = 1
   numbers["ten"] = 10
   numbers["three"] = 3
   ```

   `map`注意的问题：

   - `map`是无序的，每次打印出来的`map`会不一样，它不能通过`index`获取，必须通过`key`获取
   - `map`长度是不固定的，也就是和`slice`一样，也是引用类型
   - 内置的`len`函数也适用于`map`，返回`map`拥有的`key`的数量
   - **`map`和其他基本类型不同，它不是`thread-safe`，在多个`go-routine`存取时，必须使用`mutex lock`机制**

   ```go
   //初始化一个map
   rating := mao[string]float32{"C":5, "GO":4.5, "Python":4.5, "C++":2}
   csharpRating, ok := rating["C#"]
   if ok{
   	fmt.Println("yes")
   }else{
   	fmt.Println("no")
   }
   delete(rating, "C")
   ```

8. make、new操作

   `make`用于内建类型(`map`, `slice`, `channel`)的内存分配，`new`用于各种类型的内存分配，返回了一个指针，指向新分配的类型`T`的零值。

   内建函数`make(T,args)`与`new(T)`有着不同的功能，`make`只能创建`slice`、`map`和`channel`，并且返回一个有初始值(非零值)的`T`类型，而不是`*T`，本质上说 这三个类型有所不同的原因是指向数据结构的引用在使用前必须初始化。例如`slice`是一个包含只想数据(内部array)的指针、长度和容量的三项描述符；在这些项目被初始化之前，`slice`为`nil`，对于这三种类型来说，`make`初始化了内部的数据结构，填充适当的值。

   > `new`返回指针
   >
   > `make`返回初始化之后的(非零)值

#### 流程控制

```go
//for配合range可以用于读取`slice`和`map`的数据：
for k, v := range map{
	fmt.Println("map's key:", k)
	fmt.Println("map's val:", v)
}
//Go支持多值返回，对于声明而未调用的变量，编译器会报错，这种情况下使用_来丢弃不需要的返回值
for _, v := range map{
	fmt.Println("map's val:", v)
}
```

```go
//Go支持变参，接受变参的函数有着不定数量的参数
//定义函数使其接收变参
func myfunc(arg ...int){}
//arg是一个int的slice
for _, n := range arg{
    fmt.Println("%d", n)
}
```

**Go语言中`channel`,`slice`,`map`这三种类型的实现机制类似指针，所以可以直接传递，不需要取地址后传递指针(若函数更改`slice`的长度，则仍需要取地址传递指针)。**

```go
//defer,当函数执行到最后时，这些defer语句会按照逆序执行，最后该函数返回，如果有多个defer，defer遵循先进后出原则
for i := 0; i < 5; i++{
	defer fmt.Printf("%d ", i)
}
// 4 3 2 1 0
```

1. import

   Go的import支持以下两种方式来加载自己写的模块：

   ```go
   //相对路径
   import "./model" //当前文件同一目录的model目录，但是不建议这种方式来import
   //绝对路径
   import "sharturl/model" //加载gopath/src/shorturl/model模块
   ```

   - 点操作

     ```go
     import(
     	. "fmt"
     )
     //点操作的含义就是这个包导入之后在你调用这个包的函数时，你可以省略前缀的包名，fmt.Println("hello")->Println("hello")
     ```

   - 别名操作

     ```go
     import(
     	f "fmt"
     )
     //别名操作使调用包函数时前缀变成了指定的前缀，即 f.Println("hello")
     ```

   - _操作

     ```go
     import(
     	"database/sql"
     	_ "github.com/ziutek/mymysql/godrv"
     )
     //_操作其实是引入该包，而不直接使用包里面的函数，而是调用了该包里面的init函数
     ```

##### `struct`类型

```go
type person struct{
	name string
	age int
}
var P person
P.name = "ma"
P.age = "18"
fmt.Printf("%s", P.name)
//还有如下几种声明方式
P := person{"Tom", 20}
P := person{age:24, name:"Tom")
P := new(person)
```

```go
// 匿名字段
package main
import "fmt"
type Human struct {
    name string
    age int
    weight int
}
type Student struct{
    Human //默认Student包含了Human的所有字段
    speciality string
}

func main(){
    mark := Student{Human{"Mark", 25, 100}, "Computer Science"}
    fmt.Println("name: ", mark.name)
    fmt.Println("age: ", mark.age)
    ...
}
//Student组合了Human和string基本类型
//student还能访问Human这个字段作为字段名
mark.Human = Human("Marcus", 55 ,130)
mark.Human.age -= 1
//也可以将内置类型作为匿名字段
type Skills []string
type Human struct {
    name string
    age int
    weight int
}
type Student struct{
    Human
    Skills
    int
    speciality string
}
jane := Student{Human:Human{"Jane", 35, 100}, speciality:"Biology"}
jane.Skills = []string{"anatomy"}
jane.int = 3
//冲突问题，当student包含字段phone，human也包含字段phone，这是访问student.phone是student的字段而不是human的字段，如果想访问human的字段则需要显式调用，student.human.phone
```

#### 面向对象

##### 指针作为`receiver`

```go
type Data struct {
    x int
}

func (self Data) ValueTest() { // func ValueTest(self Data);
    fmt.Printf("Value: %p\n", &self)
}
func (self *Data) PointerTest() { // func PointerTest(self *Data);
    fmt.Printf("Pointer: %p\n", self)
}
func main() {
    d := Data{}
    p := &d
    fmt.Printf("Data: %p\n", p)
    d.ValueTest()   // ValueTest(d)
    d.PointerTest() // PointerTest(&d)
    p.ValueTest()   // ValueTest(*p)
    p.PointerTest() // PointerTest(p)
}
//output
Data    : 0x1167c0ec
Value   : 0x1167c108
Pointer : 0x1167c0ec
Value   : 0x1167c10c
Pointer : 0x1167c0ec
```

> 如果一个method的receiver是`*T`，那么可以在一个`T`类型的实例变量`V`上面调用这个method，而不需要`&V`去调用这个method(**变量`b`调用`func(self *Data) PointerTest()`方法不需要对`b`取地址之后调用**)
>
> 如果一个method的receiver是`T`，那么可以在一个`*T`类型的实例变量`P`上面调用这个method，而不需要`*P`去调用这个method(**变量`p`调用`func (self Data) ValueTest()`方法不需要对`p`解引用之后调用**)

##### `method`继承

如果一个匿名字段实现了一个method，那么包含这个匿名字段的struct也能调用该method

```go
package main
import "fmt"
type Human struct {
	name string
	age int 
	phone string
}
type Student struct {
	Human
	school string
}
type Employee struct {
 	Human
 	company string 
}
func (h *Human) SayHi(){
	fmt.Printf("Hi, %s, %s", h.name, h.phone)
}
func main(){
	mark := Student{Human{"Mark", 25, "222-222'2222"}, "MIT"}
	sam := Student{Human{"Sam", 45, "123-123-1234"}, "Golang Inc"}
	mark.SayHi()
	sam.SayHi()
}
```

##### `method`重写

当`Employee`也要实现自己的`SayHi`，可以定义一个`Employee`的method，重写匿名字段的方法

```go
package main
import	"fmt"
type Human struct{
	name string
	phone string
}

type Student struct{
	Human
	school string
}

type Employee struct{
	Human
	company string
}

func(h *Human) SayHi(){
	fmt.Printf("Human func, name:%s, phone:%s", h.name, h.phone)
}

func(e *Employee) SayHi(){
	fmt.Printf("Employee func, name:%s, phone:%s, company:%s", e.name, e.phone, e.company)
}

func main(){
	ma := Student{Human{"ma", "123-456"}, "xicai"}
	yu := Employee{Human{"yu", "111-234"}, "daxue"}
	
	ma.SayHi()
	yu.SayHi()
}
```

##### 反射

一般使用reflect包，使用reflect包分为三步：要去反射的是一个类型的值(这些值都实现了空interface)，首先需要把它转化为reflect对象（`reflect.Type`或`reflect.Value`，根据不同的情况调用不同的函数）。

```go
t := reflect.TypeOf(i) //获得类型的元数据，通过t可以获取到类型定义里面的所有元素
v := reflect.ValueOf(i) // 获取实际的值，通过v可以获取到存储在里面的值，还可以去改变值
```

获取反射值能返回相应的类型和数值

```go
var x float64 = 3.4
v := reflect.ValueOf(x)
fmt.Println("type:", v.Type())
fmt.Println("kind is float64:", v.kind() == reflect.Float64)
fmt.Println("value:", v.Float())
```

反射的字段必须是可修改的

```go
//error
var x float64 = 3.4
v := reflect.ValueOf(x)
v.SetFloat(7.1)
//如果修改相应的值，必须这样写
var x float64 = 3.4
p := reflect.ValueOf(&x)
v := p.Elem()
v.SetFloat(7.1)
```

#### 并发

##### `goroutine`

`goroutine`是通过Go的`runtime`管理的有一个线程管理器，`goroutine`通过`go`关键字实现了。

```go
package main
import(
	"fmt"
	"runtime"
)
func say(s string){
	for i:=0; i<5; i++{
		runtime.Gosched()
		fmt.Println(s)
	}
}
func main(){
	go say("world")
	say("hello")
}
```

##### `channels`

`goroutine`运行在相同的地址空间，因此访问共享内存必须做好同步，Go提供了一个很好的通信机制`channel`，`channel`可以和双向管道类比，通过它发送或者接收值，这些值只能是特定的类型：`channel`类型。

定义一个`channel`时，也需要定义发送到`channel`的值的类型，**注意，必须使用`make`创建`channel`**。

```go
ci := make(chan int)
cs := make(chan string)
cf := make(chan interface{})
```

`channel`通过操作符`<-`来接收和发送数据

```go
ch <- v //发送v到channel ch
v := <-ch //从ch接收数据并赋值给v
```

##### `Buffered Channerls`

上面的是默认的非缓存类型`channel`，不过Go也允许指定`channel`的缓冲大小

```go
//ch := make(chan type, value)
ch := make(chan bool, 4)
```



```go
//当value=0是，channel是无缓冲阻塞读写的，当value>0时，channel时有缓冲、非阻塞的，直到写满value个元素才阻塞写入
package main
import "fmt"
func main(){
	c := make(chan int, 2)//修改为1，修改为三
	c <- 1
	c <- 2
	fmt.Println(<-c)
	fmt.Println(<-c)
}
```

##### `Range`和`Close`

多次读取`channel`很不方便，可以像操作`slice`或者`map`一样操作`channel`

```go
package main
import (
	"fmt"
)
func fibonacci(n int, c chan int){
	x, y := 1, 1
	for i:=0; i<n; i++{
		c <- x
		x, y = x, x+y
	}
	close(c)
}
func main(){
	c := make(chan int, 10)
	go fibonacci(cap(c), c)
	for i:= range c{
		fmt.Println(i)
	}
}
```

`for i := range c`能够不断的读取`channel`里面的数据，直到该`channel`被显示的关闭。关闭`channel`之后就无法发送任何数据了，可以通过`v, ok := <- ch`测试`channel`是否被关闭，如果`ok`返回`false`，那么说明channel已经没有任何数据并且已经被关闭。

> 在生产者处关闭channel，而不是消费的地方去关闭它，这样容易引起panic

##### `Select`

如果存在多个`channel`的时候，Go提供了一个关键字`select`，通过`select`可以监听`channel`的数据流动。

`select`默认阻塞，只有当监听的`channel`中有发送或接收可以进行时才会运行，当多个channel都准备好的时候，`select`是随机的选择一个执行的。

```go
 package main
 
 import(
	 "fmt"
 )

 func fibonacci(c, quit chan int){
	 x, y := 1, 1
	 for{
		 select{
		 case c <- x: //将x发送给c
			x, y = y, x+y
		 case <- quit: //从quit接收数据
			fmt.Println("quit")
			return
		 }
	 }
 }

 func main()  {
	c := make(chan int)
	quit := make(chan int)
	go func(){
		for i:=0; i<10; i++{
			fmt.Println(<-c) //从c接收数据
		}
		quit <- 0 //将0发送给quit
	}()
	fibonacci(c, quit)
}
```

在 `select`中还有 `default`语法，`select`类似于switch功能，`default`就是当监听的 `channel`都没有准备好的时候默认执行的(此时不再阻塞等待`channel`)

```go
select {
	case i:= <-c
		// use i
	default:
		//不再阻塞等待
}
```

##### 超时

利用`select`设置超时，避免`goroutine`长时间阻塞

```go
func main(){
	c := make(chan int)
	o := make(chan bool)
	go func(){
		for{
			select {
				case v:= <- c:
					println(v)
				case <- time.After(5 * time.Second):
					println("timeout")
					o <- true
					break
			}
		}
    }()
    <- o //从o接收数据
}
```

##### `runtime goroutine`

`runtime`包处理`goroutine`函数：

- `Goexit`：退出当前执行的`goroutine`，但是defer函数还是会执行
- `Gosched`：让出当前`goroutine`的执行权限，调度器安排其他任务运行，并在下次某个时候从该位置恢复执行
- `NumCPU`：返回CPU核数量
- `NumGoroutine`：返回正在执行和排队任务总数
- `GOMAXPROCES`：设置可以并行计算CPU核数最大值，并返回之前的值

#### `go Http`包详解

##### `Conn`的`goroutine`

`go`为了实现高并发和高性能，使用`goroutines`来处理Conn的读写事件，每个请求都能保持独立，不会阻塞。

```go
c, err := srv.NewConn(rw)
if err != nil{
	continue
}
go c.serve()
//每次请求都会创建一个Conn，里面保存了该次请求的信息，然后传递给对应的handler，该handler中可以读取相应的header信息，保证每次请求的独立性
```

##### `ServeMux`的自定义

`conn.serve`内部调用了`http`包默认的路由器，通过路由器把本次的请求的信息传递给后端的处理函数，路由器实现如下：

```go
type ServeMux struct{
	mu sync.RWMutex // 涉及到并发处理，需要锁机制
	m map[string]muxEntry //路由规则，一个string对应一个mux实体，string就是注册的路由表达式
	hosts bool // 是否在任意的规则中带有host信息
}
```

```go
//muxEntry
type muxEntry struct {
	explicit bool    //是否精准匹配
	h        Handler //这个路由表达式对应那个handler
	pattern  string  //匹配字符串
}
```

```go
//Handler
type Handler interface{
	ServeHTTP(ResponseWriter, *Request) //路由实现器
}
```

`Handler`是一个接口，但是之前的`sayhelloName`函数并没有实现`ServeHTTP`这个接口，为什么能添加呢？原来在`http`包里面还定义了一个类型`HandlerFunc`,我们定义的函数`sayhelloName`就是`HandlerFunc`调用之后的结果，这个类型实现了`ServeHTTP`这个接口，即调用了`HanderFunc(f)`强制转换f成为`HandlerFunc`类型，这样`ServeHTTP`方法。

```go
type HandlerFunc func(ResponseWriter, *Request)
// ServeHTTP calls f(w, r)
func (f HandlerFunc) ServeHTTP(w ResponseWriter, r *Request){
	f(w, r)
}
```

路由器存储好相应的路由规则，具体请求的分发如下：

```go
func (mux *ServeMux) ServeHTTP(w ResponseWriter, r *Request){
	if r.RequestURI == "*"{
		w.Header().set("Connection", "close")
		w.WriteHeader(StatusBadRequest)
		return
	}
	h, _ := mux.Handler(r)
	h.ServerHTTP(w, r)
}
//路由器接收请求之后"*"关闭连接，不然调用mux.Handler(r)返回对应设置的路由处理Handler，执行h.ServeHTTP(w,r)也就是调用对应路由的handler的ServeHTTP接口.
```

```go
//mux.Handler(r)
func (mux *ServeMux) Handler(r *Request) (h Handler, pattern string){
	if r.Method != "CONNECT"{
		if p:= cleanPath(r.URL.Path); p != r.URL.Path{
			_, pattern = mux.handler(r.Host, p)
			return RedirectHandler(p, StatusMovePermanently), pattern
		}
	}
	return mux.handler(r.Host, r.URL.Path)
}
func (mux *ServeMux) handler(host, path string)(h Handler, pattern string){
	mux.mu.RLock()
	defer mux.mu.RUnlock()
	if mux.hosts{
		h, pattern = mux.match(host + path)
	}
	if h == nil{
		h, pattern = mux.match(path)
	}
	if h == nil{
		h, pattern = NotFoundHandler(), ""
	}
	return 
}
```

根据用户请求的URL和路由器里面存储的map去匹配的，当匹配到之后返回存储的handler，调用这个handler的`ServeHTTP`接口就可以执行到相应的函数了。

##### 执行流程

- 首先调用`Http.HandleFunc`

  1. 调用`DefaultServeMux`的`HandleFunc`
  2. 调用`DefaultServeMux`的`Handle`
  3. 往`defaultServeMux`的`map[string]muxEntry`中增加对应的`handler`和路由规则

- 其次调用`http.ListenAndServe(":9090", nil)`

  1. 实例化`Server`

  2. 调用`Server`的`ListenAndServe()`

  3. 调用`net.Listen("tcp", addr)`监听端口

  4. 启动`for`循环，在循环中`Accept`请求

  5. 对每个请求实例化一个`Conn`，并且开启一个`goroutine`为这个请求进行服务`go c.serve()`

  6. 读取每个请求的内容`w, err := c.readRequest()`

  7. 判断`handler`是否为空，如果没有设置`handler`，`handler`默认值为`DefaultServeMux`

  8. 调用`handler`的`ServeHTTP`

  9. 在这个例子中进入到`DefaultServeMux.ServeHttp`

  10. 根据`request`选择`handler`，并且进入到这个`handler`的`ServeHTTP`

  11. 选择`handler`

      判断是否有路由能满足`request`（循环遍历`ServeMux`的`muxEntry`）

      如果有路由满足，调用这个路由`handler`的`ServeHTTP`

      如果没有路由满足，调用`NotFoundHandler`的`ServeHTTP`

#### `Socket`编程

##### `TCP Socket`

在`Go`语言中的`net`包有一个类型`TCPConn`，这个类型可以用作客户端和服务器端交互的通道，主要有两个函数

```go
func (c *TCPConn) Write(b []byte) (int, error)
func (c *TCPConn) Read(b []byte) (int, error)
```

`TCPConn`可以在客户端和服务器端来读写数据，我们还需要直到一个`TCPAddr`类型，表示一个`TCP`的地址信息，定义如下：

```go
type TCPAddr struct {
	IP IP
	Port int
	Zone string //IPv6 scoped addressing zone
}
```

在`Go`中通过`ResolveTCPAddr`获取一个`TCPAddr`

```go
func ResolveTCPAddr(net, addr string) (*TCPAddr, os.Error)
//net参数是"tcp4"、"tcp6"、"tcp"中的任意一个
//addr表示域名或者ip地址
```

##### `TCP client`

`Go`通过`net`包中的`DialTCP`函数来建立TCP连接，并且返回一个`TCPConn`类型的对象，当连接建立时服务端也创建一个同类型的对象，此时客户端和服务端通过各自拥有的`TCPConn`对象来进行数据交换。一般而言，客户端用过`TCPConn`对象将请求信息发送到服务器端，读取服务器端相应的信息。服务器端读取并解析来自客户端的请求，并返回应答信息，这个连接只有当任一端关闭了连接之后才失效， 建立连接的函数定义如下：

```go
func DialTCP(network string, laddr, raddr *TCPAddr)(*TCPConn, err)
//network参数是"tcp4"、"tcp6"、"tcp"中的任意一个
//laddr表示本机地址，一般为nil
//raddr表示远程的服务地址
```

##### `TCP Server`

通过`net`包创建服务器端程序，在服务器端我们需要绑定服务到指定的非激活端口，并监听此端口。

```go
func ListenTCP(network string, laddr *TCPAddr) (*TCPlistener, error)
func (l *TCPListener) Accept() (Conn, error)
```



##### 控制TCP连接

```go
func DialTimeout(net, addr string, timeout time.Duration) (Conn, error)
```

```go
//设置连接的超时时间，客户端和服务器端都适用，当超过设置的时间，连接自动关闭
func (c *TCPConn) SetReadDeadline(t time.Time) error
func (c *TCPConn) SetWriteDeadline(t time.Time) error
//用来设置写入/读取一个连接的超时时间，当超时时间到达，连接自动关闭
func (c *TCPConn) SetKeepAlive(keepalive bool) os.Error
```

设置`KeepAlive`属性，操作系统在`tcp`上没有数据和`ACK`时，会间隔发送`keeplive`包，操作系统可以以此来判断一个`tcp`连接是否已经断开，在windows上默认2个小时没有收到数据和`keepalive`包的时候认为`tcp`连接已经断开。

##### `UDP Socket`

`Go`语言中处理`UDP Socket`和`TCP Socket`不同的地方就是在服务器端处理多个客户端请求数据包的方式不同，`UDP`缺少了对客户端连接请求的`Accpet`函数，其他基本一样。

```go
func ResolveUDPAddr(net, addr string)(*UDPAddr, os.Error)
func DialUDP(net string, laddr, raddr *UDPAddr)(c *UDPConn, err os.Error)
func LitenUDP(net string, laddr *UDPAddr)(c *UDPConn, err os.Error)
func (c *UDPConn) ReadFromUDP (b []byte)(n int, addr *UDPAddr, err os.Error)
func (c *UDPConn) WriteToUDP (b []byte, addr *UDPAddr)(n int, err os.Error)
```

#### `Go RPC`

`Go`标准包中已经提供了对`RPC`的支持，而且支持三个级别的`RPC：TCP、HTTP、JSONRPC`。但是`Go`的`RPC`是独一无二的`RPC`，和传统的`RPC`不同，他支持`Go`开发的服务器和客户端之间的交互，在内部他们使用了`Gob`来编码。

```go
//正确的RPC函数格式如下
func (t *T) MethodName(argType T1, "replyType" *T2) error
//函数必须是导出的(首字母大写)
//必须有两个导出类型的参数
//第一个参数是接收的参数，第二个参数是返回给客户端的参数，且必须为指针类型的
//函数返回值为error
```

T、T1和T2类型必须能够被`encoding/gob`包编解码

#### `GDB`调试

编译`Go`程序需要注意以下几点

1. 传递参数`-ldflags` "-s" 忽略`debug`的打印信息
2. 传递`-gcflags` "-N -I"参数，可以忽略Go内部的优化(聚合变量和函数等优化)

#### `Go`测试用例

`Go`语言中自带一个轻量级的测试框架`testing`和自带的`go test`来实现单元测试和性能测试，`testing`框架和其他语言中的测试框架类似。

> //建议安装`gotests`插件自动生成测试文件代码：
> `go get -u -v github.com/cweill/gotests/..`

由于`go test`命令只能在一个相应的目录下执行所有的文件，所以新建一个项目目录`gotest`，在目录下创建两个文件`gotest.go`和`gotest_test.go`

`gotest_test.go`是单元测试文件，必须符合如下规则

- 文件名必须是`_test.go`结尾的，这样执行`go test`的时候才会执行到相应的代码
- 必须`import testing`这包
- 所有测试用例函数必须是`Test`开头
- 测试用例会按照源代码中写的顺序依次执行
- 测试函数`TestXxxx()`的参数是`testing.T`，我们可以使用该类型记录错误或者测试状态
- 测试格式：`func TestXxx(t *testing.T), Xxx`部分可以为任意的字母数字的组合，但是首字母不能是小写的[a-z]
- 函数中通过调用`testing.T`的`Error`，`Errorf`，`FailNow`， `Fatal`， `FatalIf`方法，说明测试不通过，调用`Log`方法来记录测试信息。

```go
go test -v gotest.go gotest_test.go
```

##### 压力测试

压力测试用例必须遵循如下规则：

```go
func BenchmarkXXX(b *testing.B){ ... }
//XXX是任意字母数字组合但首字母不能为小写字母
```

- `go test`不会默认执行压力测试的函数，执行压力测试需要带上参数`test.bench`，语法`-test.bench="test_name_regex"`,例如`go test -test.bench=".*"`表示测试全部的压力测试函数
- 在压力测试用例中，记得在循环体内使用`testing.B.N`使测试正常运行
- 文件名也必须以`_test.go`结尾

```go
go test gotest.go webbench_test.go -test.bench=".*"
```

