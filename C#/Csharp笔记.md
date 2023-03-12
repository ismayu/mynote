###### [foreach]()循环

```c#
foreach
int[] array = new int []{1,2,3,4,5,6,7,8};
foreach(int element int array)
{
	System.Console.WriteLine(element);
}
```

###### Internal访问修饰符

允许一类将其成员变量和成员函数暴露给当前程序中的其他函数和对象。换句话说，带有 internal 访问修饰符的任何成员可以被定义在该成员所定义的应用程序内的任何类或方法访问。

###### Protected Internal 访问修饰符

Protected Internal 访问修饰符允许在本类,派生类或者包含该类的程序集中访问。这也被用于实现继承。

###### C#可空类型

```c#
int? i =3;
Nullable<int> i = new Nullable<int>(3);
```

?：单问号用于将int,double,bool等无法直接赋值为Null的数据类型进行null赋值，数据类型为Nullable

```c#
int i;//default->0
int? ii;default->null
```

??：双问号用于判断一个变量在为null时返回一个指定的值

```c#
double? num1 = null;
double? num2 = 3.14157;
double num3;
num3 = num1 ?? 5.34; //若num1为空值则返回5.34
num3 = num2 ?? 5.34;
```

```c#
num3:5.34
num3:3.14157
```

###### Array

多维数组：

```c#
int [,] a = new int [3,4]{
    {0,1,2,3},
    {4,5,6,7},
    {8,9,10,11}
};
```

交错数组：

```c#
int[][] scores = new int[2][](new int[]{92,93,94},new int[]{85,86,87,88});
```

参数数组：

params关键字，调用数组做形参的方法时，既可以传递数组实参，也可以传递一组数组元素

```c#
public int Addelements(params int[] arr){
//...
}
Addelements(1,2,3,4,5);
```

只能为一维数组使用params关键字，多维数据不能是使用；params关键字不是方法签名的一部分，若两个方法签名只有params这唯一区别，则编译器报错；不能为参数数据指定ref或out修饰符；参数数据必须是方法的最后一个参数，其后不允许出现参数。

常用属性：

| 属性名      | 描述                                   |
| ----------- | -------------------------------------- |
| IsFixedSize | 获取一个值该值指示数组是否有固定大小   |
| IsReadOnly  | 获取一个值，指示该数组是否只读         |
| Length      | 获取32位整数，表示为数组中所有元素总数 |
| LongLength  | 获取64位整数，表示为数组中所有元素总数 |
| Rank        | 获取数组的维度                         |

常用方法：

| 方法名                    | 描述                                                         |
| ------------------------- | ------------------------------------------------------------ |
| Clear                     | 根据数组类型设置数组中某个范围的元素为0,false或为null        |
| Copy(Array, Array, Int32) | 从第一个元素开始赋值某个范围的元素到另一个数组的第一个元素位置，长度由32位整数指定 |
| Copy(Array, Int32)        | 从当前数组中复制所有元素到一个指定的一维数组指定索引位置，索引由32位整数指定 |
| GetLength                 | 获取32位整数，表示指定维度的数组中的元素总数                 |
| GetLongLength             | 获取64位整数，表示指定维度的数组中的元素总数                 |
| GetLowerBound             | 获取数组中指定维度的下界                                     |
| GetType                   | 获取当前实例的类型，从对象(Object)继承                       |
| GetUpperBound             | 获取数组中指定维度的上界                                     |
| GetValue(Int32)           | 获取一维数组中指定位置的值，索引值由32位整数指定             |
| IndexOf(Array,Object)     | 搜索指定的对象，返回第一次出现的索引                         |
| Reverse(Array)            | 逆转整个数组中元素顺序                                       |
| SetValue(Object,Int32)    | 给数组指定索引位置设置值，索引值由32位整数指定               |
| Sort(Array)               | 使用数组的每个元素二等IComparable实现来排序整个数组的元素(仅支持一维数组) |
| ToString                  | 返回一个表示当前对象的字符串，从Object继承                   |

###### String

常用属性：

| 属性名 | 描述                                   |
| ------ | -------------------------------------- |
| Chars  | 从当前String对象获取Char对象的指定位置 |
| Length | 获取当前String对象字符数               |

常用方法：

| 方法名                                                       | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| public static int Compare(string strA, string strB)          | 比较指定的两个string对象，并返回一个表示它们在排列顺序中相对位置的整数(该方法区分大小写) |
| public static int Compare(string strA, string strB,bool ignoreCase) | 比较指定的两个string对象，并返回一个表示它们在排列顺序中相对位置的整数,ignoreCase为true则不区分大小写 |
| public static string Concat(string str(), string str1)       | 连接两个string对象                                           |
| public bool Contains(string value)                           | 返回一个表示指定string对象是否出现在字符串中的值             |
| public static string Copy(string str)                        | 创建一个与指定字符串有相同值的string对象                     |
| public static string CopyTo(int Index, char[]des,int desindex, int count) | 从string对象的指定位置复制指定数量的字符到字符数组中的指定位置 |
| public bool EndsWith(string value)                           | 判断string对象结尾是否匹配指定的字符串                       |
| public bool Equals(string value)                             | 判断当前的string对象是否与指定的string对象具有相同的值       |
| public static bool Equals(string a, string b)                | 判断指定的两个string对象是否具有相同的值                     |
| public static string Format(string format, Object arg0)      | 把指定字符串中一个或多个个事项替换为指定对象的字符串表示形式 |
| public int IndexOf(char value)                               | 返回指定Unicode字符在当前字符串中第一次出现的索引，从0开始   |
| public int IndexOf(string value)                             | 返回指定字符串从该实例这种指定字符位置开始搜索第一次出现的索引，从0开始 |
| public int IndexOfAny(char[] anyOf)                          | 返回某一个指定的 Unicode 字符数组中任意字符在该实例中第一次出现的索引，从0开始 |
| public int IndexOfAny(char[] anyOf, int startIndex)          | 返回某一个指定的 Unicode 字符数组中任意字符从该实例中指定字符位置开始搜索第一次出现的索引，从0开始 |
| public string Insert( int startIndex, string value )         | 返回一个新的字符串，其中，指定的字符串被插入在当前 string 对象的指定索引位置 |
| public static bool IsNullOrEmpty( string value )             | 指示指定的字符串是否为 null 或者是否为一个空的字符串         |
| public static string Join( string separator, string[] value ) | 连接一个字符串数组中的所有元素，使用指定的分隔符分隔每个元素 |
| public static string Join( string separator, string[] value, int startIndex, int count ) | 连接接一个字符串数组中的指定位置开始的指定元素，使用指定的分隔符分隔每个元素 |
| public int LastIndexOf( char value )                         | 返回指定 Unicode 字符在当前 string 对象中最后一次出现的索引位置，从 0 开始 |
| public int LastIndexOf( string value )                       | 返回指定字符串在当前 string 对象中最后一次出现的索引位置，索引从 0 开始 |
| public string Remove( int startIndex )                       | 移除当前实例中的所有字符，从指定位置开始，一直到最后一个位置为止，并返回字符串 |
| public string Remove( int startIndex, int count )            | 从当前字符串的指定位置开始移除指定数量的字符，并返回字符串。 |
| public string Replace( char oldChar, char newChar )          | 把当前 string 对象中，所有指定的 Unicode 字符替换为另一个指定的 Unicode 字符，并返回新的字符串 |
| public string Replace( string oldValue, string newValue )    | 把当前 string 对象中，所有指定的字符串替换为另一个指定的字符串，并返回新的字符串 |
| public string[] Split( params char[] separator )             | 返回一个字符串数组，包含当前的 string 对象中的子字符串，子字符串是使用指定的 Unicode 字符数组中的元素进行分隔 |
| public string[] Split( char[] separator, int count )         | 返回一个字符串数组，包含当前的 string 对象中的子字符串，子字符串是使用指定的 Unicode 字符数组中的元素进行分隔的。int 参数指定要返回的子字符串的最大数目 |
| public bool StartsWith( string value )                       | 判断字符串实例的开头是否匹配指定的字符串                     |
| public char[] ToCharArray()                                  | 返回一个带有当前 string 对象中所有字符的 Unicode 字符数组    |
| public char[] ToCharArray( int startIndex, int length )      | 返回一个带有当前 string 对象中所有字符的 Unicode 字符数组，从指定的索引开始，直到指定的长度为止 |
| public string ToLower()                                      | 把字符串转换为小写并返回                                     |
| public string ToUpper()                                      | 把字符串转换为大写并返回                                     |
| public string Trim()                                         | 移除当前 String 对象中的所有前导空白字符和后置空白字符       |

###### strcut

结构不可定义析构函数，可以定义构造函数，但是无法定义无参数的构造函数(无参数的构造函数是自动定义的)
结构不可继承其他的类或者结构
结构也不能作为其他结构或类的基础结构
结构可实现一个或多个接口
结构成员不能为abstract, virtual, protected
当使用new创建一个结构对象时，会调用适当的构造函数来创建结构。结构也可以不使用new来实例化
若不使用new，只有在所有字段都被初始化之后字段才被赋值，对象才被使用

###### class

类的默认访问标识符是internal，成员的默认访问标识符为private
数据类型<data type>指定了变量的类型，返回类型<return type>指定了返回的方法返回的数据类型
析构函数不能继承或者重载
static函数在对象创建之前就已经存在

###### struct和class不同点：

- 类是引用类型，结构是值类型
- 结构不支持继承
- 结构不能声明默认的构造函数(无参数的构造函数)

- 结构的构造函数必须给所有的字段赋值，类的构造函数无限制

###### abstract

不能创建实例
不能在抽象类外部声明一个抽象方法(抽象方法必须在抽象类当中)
在类定义前放置关键字sealed，可以将类声明为密封类，当一个类声明为sealed时，将不能被继承，抽象类不能被声明为sealed

子类继承抽象类必须override抽象类中的抽象方法

```C#
abstract class Father{
    public abstract void print();
    public void add(int a, int b){
        Console.WriteLine("add: {0}",a + b);
    }
}
class Son:Father{
    public override void print(){
        Console.WriteLine("son print");
    }
}
```

重写方法必须加上override关键字

###### 运算符重载

- implicit：用于声明隐式的用户定义类型转换运算符，可以实现两个不同类的隐式转换。

- explicit：用于声明必须通过转换来调用用户自定义的类型转换运算符。

C#要求**成对重载比较运算符**，若重载==，则必须重载!=，否则编译错误，而且必须返回bool类型的值。

C#不允许重载=运算符，若重载+运算符则会使用+运算符来执行+=运算符操作。

###### using用法

- using指令：引入命名空间

  ```c#
  using System;
  using Namespace1.SubNameSpace;
  ```

- using static：指定无需指定类型名称即可访问其静态成员的类型

  ```c#
  using static System.Math;
  var a = PI;
  ```

- 起别名

  ```c#
  using Project = PC.MyCompany.Project;
  ```

- using语句：将实例与代码绑定

  ```c#
  using (Font font3 = new Font("Arial", 10.0f),
  			font4 = new Font("Arial", 10.0f))
  {
  	//using font3 and font4
  }
  ```

  代码段结束时，自动调用font3和font4的Dispose方法，释放实例。

###### 枚举(Enum)

```c#
enum <enum_name>{
	enumeration list
};
```

枚举中每个符号代表一个整数值，一个比它前面符号大的整数值，默认情况下，第一个枚举符号值为0，枚举不能继承或传递继承。

```C#
emun Day {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
static void Main(){
	int x = (int)Day.Sun;
	int y = (int)Day.Fri;
}
// x = 0
// y = 5
```

###### get和set访问器：获取和设置字段(属性)的值

1. get{}访问器，用于获取属性的值，需要在get语句最后return一个与属性数据类型兼容的值；若属性定义中省略了该访问器，则不能在其他类中获取私有类型的字段值，称只读属性。
2. set{}访问器，用于设置字段的值，需使用特殊的值value给字段赋值；若省略set访问器无法在其他类中给字段赋值，也为只读属性。

简化后属性设置代码：

```C#
public int Id{get; set;}
```

若要使用自动属性方式来表示只读属性，则省略set访问器：suoyin

```c#
public int Id{get;}=1;
```

这里相当于把Id属性设置成1，并且要以分号结束，在使用自动生成属性的方法不能省略get访问器。

如果不允许其他类访问属性值，则可以在 get 访问器前面加上访问修饰符 private：

```c#
public int Id{private get; set;}
```

Id属性的get访问器只能在当前类中使用

###### 索引器

```c#
struct IntBits
{
	private int bits;
	public IntBits(int initial) => bits = initial;
	
	public bool this[int index]
	{
		get => (bits & (1 << index)) != 0;
		set
		{
			if(value)
				bits |= (1 << index);
			else
				bits &= ~(1 << index);
        }
	}
	
}
```

​	定义索引器时，使用**this**关键字代替方法名，使用中括号代替圆括号，中括号中的变量充当索引，索引指定要访问那个元素。由于我们要将int变量作为二进制位的数组，因此索引器返回值为布尔值。索引器使用this替代方法名，所以一个结构/类中只允许定义一个索引器，**但索引器可以被重载。**

​	索引器中的value变量实际上是外界进行写访问是传入的变量的数值**拷贝**，他是隐式定义的，天赐找不到value的定义，和属性中的value有一样的性质。

```c#
struct IntBits
{
	//...
	public override string ToString()
	{
		return (Convert.ToString(bits, 2));//输出bits的二进制表示
	}
}
```

索引器使用：

```c#
int adapted = 0b0_01111110;
IntBits bits = new IntBits(adapted);
bool peak = bits[6];
bits[5] = true;
```

在接口中声明索引器：

```c#
interface IHowAreYou
{
	bool this [ int index ] {get; set;}//读写访问器实现交给类
}
```

​	在类中实现接口的索引器时，可将索引器声明为virtual，从而允许派生类重写访问器。

```c#
class IMFine :IHowAreYou
{
	//TODO
    public virtual bool tihs [int index]
    {
    	get {...}
    	set {...}
    }
}
```



###### 关键字var

var可以理解为匿名类型，可以认为它是一个声明变量的占位符，用于声明变量，无法确定数据类型时使用。

1. 必须在定义时初始化，即var str = "abcd",而不是var str; str = "abcd";
2. 一旦初始化完成，就不能再给变量赋与初始化类型不符的值了；
3. var是局部变量
4. 使用var定义变量和object不同，它在效率上和使用强类型方式定义变量完全一样

###### 预处理指令

| 指令       | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| #define    | 用于定义一系列成为符号的字符                                 |
| #undef     | 取消定义符号                                                 |
| #if        | 用于测试符号是否为真                                         |
| #else      | 创建复合条件指令，与#if一起使用                              |
| #elif      | 创建复合条件指令                                             |
| #endif     | 一个条件指令的结束                                           |
| #line      | 可以让您修改编译器的行数以及（可选地）输出错误和警告的文件名 |
| #error     | 它允许从代码的指定位置生成一个错误                           |
| #warning   | 它允许从代码的指定位置生成一级警告                           |
| #region    | 它可以让您在使用 Visual Studio Code Editor 的大纲特性时，指定一个可展开或折叠的代码块 |
| #endregion | 它标识着 #region 块的结束                                    |
| #pragma    | 可以抑制或还原指定的编译警告                                 |

###### 异常处理

//TODO

###### 文件I/O

| I/O类          | 描述                             |
| -------------- | -------------------------------- |
| BinaryReader   | 从二进制流读取原始数据           |
| BinaryWriter   | 以二进制格式写入原始数据         |
| BufferedStream | 字节流的临时存储                 |
| Directory      | 有助于操作目录结构               |
| DirectoryInfo  | 用于对目录执行操作               |
| DriveInfo      | 提供驱动器信息                   |
| File           | 有助于处理文件                   |
| FileInfo       | 用于对文件执行操作               |
| FileStream     | 用于文件中任何位置的读写         |
| MemoryStream   | 用于随机访问存储在内存中的数据流 |
| Path           | 对路径信息执行操作               |
| StreamReader   | 用于从字节流中读取字符           |
| StreamWriter   | 用于向一个流中写入字符           |
| StringReader   | 用于读取字符串缓冲区             |
| StringWriter   | 用于写入字符串缓冲区             |

FileStream类：https://www.runoob.com/csharp/csharp-file-io.html

文本文件读写StreamReader类和StreamWriter类：https://www.runoob.com/csharp/csharp-text-files.html

二进制文件读写BinaryReader类和BinaryWriter类：https://www.runoob.com/csharp/csharp-binary-files.html

Windows文件系统操作DirectoryInfo类和FileInfo类：https://www.runoob.com/csharp/csharp-windows-file-system.html

> ​	完毕”

```c#
public chass Shape
{
	public Shape()
	{
		Draw();
	}
	public virtual void Draw()
	{
		System.Console.WriteLine("基类");
	}
}
class Cirle : Shape
{
	public Cirle(){}
	public override void Draw()
	{
		System.Console.WriteLine("圆");
	}
}

Shape shapes = new Cirle();

//长方形
```

```C++
class Shape
{
public:
	Shape()
	{
		Draw();
	}
	virtual void Draw()
	{
		cout << "基类" << endl;
	}
}
class Cirle : public Shape
{
public:
	Cirle() {}
	virtual void Draw()
	{
		cout << "圆" << endl;
	}
}

Shape *shape = new Cirle();

//基类
```

###### 脆弱的基类(new 修饰符)

new修饰符，它在基类面前隐藏了派生类重新声明的成员。这时 不会调用派生的最远的成员而是搜索继承链，找到使用new修饰符的那个成员之前的成员，然后调用该成员。若继承链仅包含两个类么就会使用基类的成员，感觉像是派生类没有重写那个成员

```c#
public class BaseClass
{
	public void Display()
	{
		Console.WriteLine("Base");
	}
}
public class DerivedClass : BaseClass
{
	public virtual void Display()
	{
		Console.WriteLine("Derived");
	}
}
public class SubDerivedClass : DerivedClass
{
	public override void Display()
	{
		Console.WriteLine("SubDerived");
	}
}
public class SuperSubDerivedClass : SubDerivedClass
{
	public new void Display()
	{
		Console.WriteLine("SuperSubDerived");
	}
}
public static void Main()
{
	SuperSubDerivedClass super = new SuperSubDerivedClass();
	SubDerivedClass sub = super;
	DerivedClass der = super;
	BaseClass base = super;
	
	super.Display();
	sub.Display();
	der.Display();
	base.Display();
}
/*
SuperSubDerived    //无派生类，所以没有重载
SubDerived         //重写基类虚成员派生的最远的成员， super.Display()使用new进行隐藏了
SubDerived         //同上
Base               //没有重新声明任何基类成员并且非虚，直接调用
*/
```

###### 接口

接口名称使用Pascal大小写风格，以“I”作为前缀

如果接口InterfaceA和结构InterfaceB都声明了NumberofLegs无参方法。但他们的目的不同，InterfaceA的NumberOfLegs方法返回动物的腿的个数，InterfaceB的NumberOfLegs意在返回动物经过了几站，现在Horse类实现InterfaceA和InterfaceB接口，也就意味着他需要实现两个NumberOfLegs方法，如果按照隐式实现接口，那么编译器如何知道NumberOfLegs方法是InterfaceA声明的，还是InterfaceB声明的？

```c#
interface InterfaceA
{
	int NumberOfLegs();//返回动物腿的个数
}
interface InterfaceB
{
	int NumberOfLegs();//返回动物经过了几站
}
class Horse : InterfaceA, InterfaceB
{
	public int NumberOfLegs()
	{
		return 4;
	}
	//编译器如何知道这是哪个接口的实现
}
```

​	这样的代码无错误，再编译器看来虽然Horse类只实现了一个NumberOfLegs方法，但这一个方法实现了两个接口。

​	为了解决该问题，并区分哪个方法实现哪个接口应当显示实现接口，为此需要指出方法从属于哪个接口：

```c#
interface InterfaceA
{
	int NumberOfLegs();//返回动物腿的个数
}
interface InterfaceB
{
	int NumberOfLegs();//返回动物经过了几站
}
class Horse : InterfaceA,InterfaceB
{
	int InterfaceA.NumberOfLegs()
	{
		return 4;
	}
	//实现A接口，返回四条腿
	int InterfaceB.NumberOfLegs()
	{
		return 3;
	}
	//实现B接口，返回3站
}
```

​	我们可以发现，显式实现接口时，**无需使用访问限制符修饰方法**，这样意味着两个NumberOfLegs方法都是私有方法，当我们在外部直接调用这两个NumberOfLegs方法结果如何呢？

```c#
Horse horse = new Horse();
int legs = horse.NumberOflegs();//无法编译
```

​	此时我们需要通过 **接口引用类**的方式来调用NumberOfLegs方法，这种方式可以区分调用的是哪个方法，同时还有其他优势。

```c#
Horse horse = new Horse();
InterfaceA iMyA = horse;//合法
InterfaceB iMyB = horse;//合法
```

​	为了调用InterfaceA接口和InterfaceB接口中不同的NumberOfLegs方法，我们需要使用**引用了Horse对象的接口变量来调用相应方法。**

```c#
Horse horse = newe Horse();
InterfaceA iMyA = horse;
InterfaceB iMyB = horse;
int legsA = iMyA.NumberOfLegs();
int legsB = iMyB.NumberOfLegs();

```

​	编译器会通过接口变量的类型来决定调用类中的哪个NumberOfLegs方法，**我们需要创建接口变量并引用类才能通过该接口变量调用勒种实现的接口方法。**

```c#
int Find(InterfaceA myA)
{
	//TODO
}
```

​	在Find方法中我们可以获取任意实现了InterfaceA的实参，比如Horse类和Person类都实现了InterfaceA接口那么任意Horse类或者Person类变量都可以作为实参传入Find方法，这种传参方式**即扩展了传入参数的类型--实现接口的所有类型，又防止未实现接口的类型传入。**

​	可以使用is操作符验证对象是不是实现了接口的一个类的实例。

```c#
Horse horse = new Horse();
if(horse is InterfaceA)
{
	InterfaceA iMyA = horse;
}
```



###### 接口和抽象类比较

| 抽象类                                                       | 接口                                             |
| ------------------------------------------------------------ | ------------------------------------------------ |
| 不能直接实例化，只能通过实例化一个派生类                     | 不能直接实例化，只能通过实例化一个实现类型       |
| 派生类要么自己也是抽象的，要么必须实现所有抽象成员           | 实现类型必须实例化所有接口成员                   |
| 可添加额外的非抽象成员，他们可由所有派生类继承，而不会破坏版本兼容性 | 为接口添加额外的成员会破坏版本兼容性             |
| 可声明属性和字段                                             | 可声明属性但不能声明字段                         |
| 成员可以是实例，虚、抽象或静态的，而且非抽象成员可以提供默认实现供派生类使用 | 所有成员被自动看成抽象成员，因此不能包含任何实现 |
| 派生类只能从一个基类派生(单继承)                             | 实现类型可实现任意多的接口                       |

###### 装箱和拆箱

​	object变量可以引用任何引用类型的实例，此外object变量也能引用值类型的实例，

```c#
Circle myCircle = new Circle();
int salary = 10086;
object o = myCircle;//object变量引用引用类型实例
object o = salary;//objectn变量引用值类型实例
```

​	我们知道，引用类型的实例占用堆内存，使用object 变量引用引用类型实例是使object变量存储一个指向该对内存区域的地址，但是值类型的实例存储于栈上，使用object变量引用值类型，就意味着引用栈，然而所有的引用都必须引用堆上的对象，引用栈上的数据会严重损坏“运行时”的健壮性，所以是不允许的。

​	因此当在执行**第四行**时，实际上发生的事情是**运行时**在堆中分配一小块内存，然后salary的值会被复制到这块内存中，然后让o变量引用位于该堆上的拷贝。**这种将数据项从栈自动复制到堆的行为称为装箱**，因此修改salary的值，o变量所引用的堆上的值不变。

​	装箱只会在object变量引用值类型时发生。**使用强制类型转换可以实现拆箱**

```c#
int i = 10;
object o = i;
i = (int)o;//使用()的方式来实现类型转换
```

但是对于不合理的类型转换会抛出**InvalidCastException**异常

```c#
Circle = new Circle();
object o = c;
int i = (int)o;//exception
```

###### 泛型

​	

```c#
class Queue<T> //<>中是参数类型
{
	//TODO
}
```

实例：

```c#
class Queue<T>
{
	//TODO
	private T[] data;
	public Queue()
	{
		this.data = new T[DEFAULTQUEUESIZE];
	}
	public Queue(int size)
	{
		this.data = new T[size];
	}
	public void Enqueue(T item)
	{
		//TODO
	}
	public T Dequeue()
	{
		//TODO
		T queueItem = this.data[this.tail];
		//TODO
		return queueItem;
	}
}
```

```c#
Queue<int> intQueue = new Queue<int>();
Queue<Circle> circleQueue = new Circle<Circle>();
```

​	可以在<>填充类型即可生成对应的队列。

​	有时需要对传入泛型类的类型参数做一定限制，比如这个类型参数要实现某种接口才允许传入：

```c#
class Printable<T> where T : IPrintable
{
	//TODO
}
```

###### 泛型方法

```c#
static void Swap<T>(ref T first, ref T second)
{
	T temp = first;
	first = second;
	second = temp;
}
```

调用方式如下:

```c#
int a = 1, b = 2;
Swap<int>(ref a, ref b);
string s1 = "马", s2 = "玉";
Swap<string>(ref s1, ref s2);
```

###### 泛型接口

```c#
interface IWrapper<T>
{
	void SetData(T Data);
	T GetData();
}
class Wrapper<T> : IWrapper<T>
{
    private T storedData;
    void IWrapper<T>.SetData(T data)
    {
        this.storeData = data;
    }
    T IWrapper<T>.GetData()
    {
        return this.storeData;
    }
}
```

​	通过使用接口引用类的方式调用接口方法。

```c#
Wrapper<string> stringWrapper = new Wrapper<string>();
IWrapper<string> storedStromgWrapper = stringWrapper;
storedStringWrapper.SetData("hello");
Console.WriteLine($"{storedStringWrapper.GetData()}");
```

将上面第二行替换成这样：

```c#
IWrapper<object> storedObjectWrapper = stringWrapper;
```

理论上所有string都是object的，但是编译器会报"无法将类型"Wrapper<string>"隐式转换为"IWrapper<object>"，存在一个显式转换(是否缺少强制转换?)"。当我们尝试显式转换时：

```c#
IWrapper<object> storedObjectWrapper = (IWrapper<object>)stringWrapper;
```

但会抛出InvalidCastException异常，问题在于，虽然所有字符串都是对象，但是反过来不是所有对象都为字符串，如果上述代码合法，那么下列代码也会合法：

```c#
IWrapper<object> storeObjectWrapper = (IWrapper<object>)stringWriapper;
Circle myCircle = new Circle();
storeObjectWrapper.SetData(myCircle);//能传入 Circle 变量的原因在于接口的类型参数是 object
```

事实上，不应该用Circle变量对string变量进行赋值操作(SetData方法进行了写访问)，**这种操作是类型不安全的**允许类型IWrapper<object> storedObjectWrapper = stringWrapper会给程序带来隐患。

> 不能用 已指定类型参数的泛型接口引用来引用指定另一参数类型的泛型接口对象，即不能用IWrapper<A>引用来引用IWrapper<B>对象，A和B对象为两不同类型参数，哪怕B派生自A。

泛型类的具体实现实际上是不同的类，Wrapper<string>和Wrapper<object>应当理解成两个独立的类，它们仅是在内部结构上相同，所以接口也应有相同的性质，IWrapper<string>和IWrapper<object>是不同的接口，Wrapper<string>继承了IWrapper<string>接口，而IWrapper<string>和IWrapper<object>是两个不同的接口，所以Wrapper<string>和Wrapper<object>**不存在继承层次的关系**，所以**允许继承层次中较高层次的变量引用较低层次的对象这一接口引用类的基础不存在了。**

###### 协变接口

​	假如我们如下定义IStoreWrapper<T>和IRetrieveWrapper<T>接口替代IWrapper<T>，并在Wrapper<T>中实现这些接口：

```c#
interface IStoreWrapper<T>
{
	void SetData(T Data);
}
interface IRetrieveWrapper<T>
{
	T GetData();
}
class Wrapper<T> : IStireWraper<T>, IRetrieveWrapper<T>
{
	private T storedData;
	void IStoreWrapper<T>.SetData(T Data)
	{
		this.storedData = data;
	}
	T IRetrieveWrapper<T>.GetData()
	{
		return this.storedData;
	}
}
```

```c#
Wrapper<string> stringWrapper = new Wrapper<string>();
IStoreWrapper<string> storedStringWrapper = stringWrapper;
IRetrieveWrapper<string> retrievedStringWrapper = stringWrapper;
storedStringWrapper.SetData("Hello");
Console.WriteLine($"{retrievedStringWrapper.GetData()}");
```

```c#
IRetrieveWrapper<object> retrievedStringWrapper = stringWrapper;
//对比上面第三行代码，此代码不合法
//不合法原因为无法实现隐式转换
```

虽然编译器会报错，C#编译器认为该语句不是类型安全的，但是这个认定有一些武断，因为认定的前提条件不存在了，IRetrieveWrapper<T>接口只允许使用GetData方法进行读访问，而不提供任何其他的写访问，因此上述代码不会导致类型不安全，**对泛型接口定义的方法，如果类型参数T仅在方法返回值中出现，就可以明确告诉编译器一些隐式转换是合法的，无需强制严格的类型安全性**：

```c#
interface IRtrieveWrapper<out T>//使用 out 修饰类型参数
{
	T GetData();
}
IRetrieveWrapper<object> retrievedStringWrapper = stringWrapper;//合法
```

​	在接口类型参数前面添加out关键字即可赋予其协变性。**协变性是指，只要存在从类型A到类型B的有效转换，或者说类型A派生自类型B，就可以将IRetrieveWrapper<A>对象赋给IRetrieveWrapper<B>引用**，对于上例stringWrapper是IRetrieveWrapper<string>的派生类Wrapper<T>的对象，因此操作是合法的。

> 特别强调的是，只有作为方法返回类型指定的类型参数才能使用out限定符，即当某个类型参数仅出现在方法的返回值中时，才能使用out限定符修饰，另外，协变性只适用于引用类型(值类型不支持继承层次关系)。

> 只有接口和委托类型能声明为协变量，不能为泛型类使用out修饰符。

###### 逆变接口

​	逆变性是指，**允许通过A类型(比如string)的引用来引用B类型(比如object)的对象，只要A从B派生，即允许我们用派生类引用基类对象。**

```c#
public interface IComparer<in T>//使用in修饰类型参数，使其具备逆变性
{
	int Compare(T x, T y);
}
```

​	实现该接口的类必须定义Compare方法，这个方法比较由T指定的那种类型的两个对象，Compare方法返回一个整数：x和y有相同的值，返回0，x小于y返回负值，x大于y返回正值。

```c#
class ObjectComparer : IComparer<object>
{
	int IComparer<object>.Compare(object x, object y)
	{
		//TODO;
	}
}
object x = ...;
object y = ...;
ObjiectComparer objectComparer = new ObjectComparer();
IComparer<object> objectComparator = objectComparer;
int result = objectComparator.Compare(x, y);
```

​	因为IComparer的参数有逆变性，如下代码是合法的：

```c#
IComparer<string> stringComparator = objectComparer;
```

​	这句代码似乎违反了类型安全行的规则，但是结合IComparer<T>接口干的事情，就能明白上述代码并无问题，Comparer方法的作用是对传入实参进行比较根据结果返回一个值。但是编译器怎么知道Compare方法有没有针对、依赖特定类型的操作呢？当我们用in关键字修饰接口的类型参数时，就相当于告诉编译器，程序员要么传递T作为参数类型，要么传递T的派生类。**使用in关键字保证程序员不能将参数T用做任何方法的返回类型。**

小总结：协变性：如果泛型接口中的方法返回string，他们也能返回object；逆变性：如果泛型接口的中的方法能获取object参数，他们也能获取string参数。

#### NEW

###### parse方法

```c#
static void Main()
{
	try
	{
		double a = double.Parse(Console.ReadLine());
		Console.WriteLine(a);
	}
	catch(Exception ex)
	{
		Console.WriteLine("Exception: {0}", ex.Message);
	}
}
```

###### 表达式主体方法(语法糖)

```c#
int addValues1(int left,int right) => left + right; //表达式主题方法
int addValues2(int left,int right)
{
	return left + right;
}
```

若表达式主体方法不返回值，则为void方法：

```c#
void showResult(int answer) => Console.WriteLine("Hello");
```

不能再表达式主体方法使用return语句，也无法实现类型转换；只有void类型的表达式主体方法可以使用包括输出语句等没有返回值的表达式作为方法主体。

###### 从方法返回多个值

通过(tuple)实现，元组是一些小的值的集合，在方法返回值类型处用元组替代类型即可：

```c#
(int, int) returnTwoValues(...)
{
	int val1, val2;
	return (val1, val2);
}
```

###### 嵌套方法

```c#
public long CalculateFactorial(string input)
{
	int inputValue = int.Parse(input);
	long factorial(int dataValue)
	{
		if(dataValue == 1)
		{
			return 1;
		}
		else 
		{
			return dataValue * factorial(dataValue - 1);
		}
    }		
	long factorialValue = factorial(intputValue);
	return factorialValue;
}
```

###### 使用具名参数

C#默认在每个实参在方法调用中的位置判断对于形参，同时c#还允许按名称指定参数，这样可以按照不同顺序传递实参，要将实参作为具名参数传递，必须输入参数，冒号，传递值：

```
void optMethod(int first, double second = 0.0, string third = "Hello")
{
	//TODO
}
optMethod(first : 90, second : 123.45, third : "Hello");
optMethod(first : 100, second : 100.01);

```

###### Convert.Tochar方法

将变量转化为char，Convert类包含Toint，Tolong,等多个方法，用于转换类型：

```c#
static void Main(stringp[] args)
{
	try
	{
		double a = double.Parse(Console.ReadLine());
		Console.WriteLine(a);
	}
	catch(Exception ex)
	{
		Console.WriteLine(ex.Message)
	}
}
```

```c#
static void Main(string[] args)
{
	try
	{
		double a = Convert.ToDouble(Console.ReadLine());
		Console.WriteLine(a);
	}
	catch(Exception ex)
	{
		Console.WriteLine(ex.Message);
	}
}
```



###### 命名规则

公共标识符以大写字母开头，此后每单词以大写字母开头。

非公共标识符以小写字母开头，此后每单词以大写开头。

类名以大写字母开头，所以私有构造器只能以大写字母开头。

不要声明仅大小写不同的两个公共成员。

以下划线开头命名为属性提供数据的私有字段。

```c#
class CestLaVie
{
	public int HowAreYouToday();//公共标识符以大写字母开头
	public int IAmFine;//公共标识符以大写字母开头
	private int thankYou();//非公共标识符以小写字母开头
	private int andYou;//非公共标识符以小写字母开头
	
	private CestLaVie()//类名以大写字母开头，所以私有构造器只能以大写字母开头
	{
		Console.WriteLine("Bonjour");
	}
	private int are;//不要声明仅大小写不同的两个公共成员
	private int Are;
	private string _coordXOfYou = "Everywhere";//以下划线为开头命名为属性提供数据的私有字段
	private string _coordYOfYou = "But nowhere";
	public string X
	{
		get => this._coordXOfYou;
		set => this._coordXOfYou = value;
	}
}
```

###### 分部类

C#允许将类的源代码拆分到单独的文件中，这样，大型类的定义就可用较小的、更易管理的部分组织。使用**partial**关键字定义类不同的部分。

```c#
//whySoserious1.cs
partial class whySoSerious
{
	public whySoSerious()
	{
		Console.WriteLine("?");
	}
	public whySoSerious(int initialize)
	{
		Console.WriteLine($"!{initialize}");
	}
}
//whySoSerious2.cs
partial class whySoSerious
{
	private bool dove;
	private bool iHaveNoIdea;
	private void set()
	{
		this.dove = true;
		iHabeNoIdea = false;
	}
}
```

###### 解构器

​	解构器检查对象并提取它的字段的值。

```c#
class Point
{
	private int x,y;
	//...
	public void Deconstruct(out int x, out int y)
	{
		x = this.x;
		y = this.y;
	}
}
```

命名必须为Deconstruct；必须为void方法；必须获取多于零个参数，参数会被对象中的字段的值填充；参数用out修饰符标记。意味着向其赋值的行为会将值传回给调用者；方法主体的代码向参数赋值。

使用解构器的方式和调用返回一个元组的方法一样，需要创建元组并将对象赋给它。

```c#
Point origin = new Point();
//...
(int xVal, int yVla) = origin;
```

###### 可空类型

​	使用如下方式可在 **调用方法时**判断变量是否为空

```c#
variable?.Area();//使用?操作符
```

​	variable为空就不调用Area方法，否则就正常调用。

###### 使用ref和out参数

​	为参数(形参)添加ref前缀，**作用于参数(形参)的所有参数都会作用于原始实参**。传递实参时也需要添加ref前缀，明确告知开发人员实参可能改变，事实上为形参添加ref前缀后，编译器调用该方法是会生成代码**传递对实参的引用**

```c#
void temp(ref int wuhu)
{
	wuhu++;
}
static void Main()
{
	int arg = 1;
	temp(ref arg);
	Console.WriteLine(arg);
}
//2
```

​	out可以由方法初始化参数，所以需要传递未初始化的实参，需要用到out关键字，**作用于参数(形参)的所有操作都会作用于原始实参，且允许传递未被初始化的变量**

```c#
void temp(out int wuhu)
{
	wuhu = 1;//必须在方法内初始化 out 参数，否则无法编译
}
static void Main()
{
	int arg; // 未初始化
	temp(out arg);// 传递实参时也需要添加 out 前缀
	Console.WriteLine(arg);// arg 变量在 temp 内被初始化，此处输出 1
}
//1
```

###### 数据的安全转型

​	c#提供了两个相当有用的操作符，is和as操作符。

```c#
Example myExample = new Example();
object o = myExample;
if(o is myExample)//若转型是安全的
{
	Example newExample = (Example)o;//执行强制类型转换
}
```

​	is操作符用于验证对象类型是不是自己希望的，is是二元操作符，左操作符是对象引用，右操作符是类型名称，若左边引用的对象的类型是右边类型(或父类)，则is表达式返回true，反之返回false，保证了只有转型成功之后才能进行强制类型转换。

​	程序在执行强制类型转换时，会再进行一次是否转型成功的判断，这无疑导致了资源的浪费。为此我们可以在需要转型时使用 as 表达式。

```c#
Example myExample = new Example();
object o = myExample;
Example newExample = o as Example;
if(newExample != null)
{
	//TODO
}
```

​	as操作符运行时尝试将左侧所引用对象转换为右侧类型，若尝试成功就返回转换成功后的结果，否则返回null，并且null值会被赋给newExample变量as操作符只执行一次判断。

###### 集合类

List<T>集合类：用法和数据差不多，可用数组语法(中括号加索引)访问集合元素，但不能在集合初始化之后用这种语法添加新元素，使用List<T>能避免数组的以下几项限制：

- 为了改变数组大小，必须创建新数组，复制数组元素然后更新对原始数组的引用，使其引用新数组。
- 删除一个数组元素，之后所有元素都必须上移一位。
- 插入一个数组元素，必须使元素下移一位来腾出空位，但最后一个元素可能会丢失。

```c#
List<int> numbers = new List<int>();
foreach(int number in new int[12]{10,9,8,7,6,5,4,3,2,1})
{
	numbers.Add(number);
}
for(int i = 0; i < numbers.Count; i++)
{
	int number = numbers[i];
	Console.WriteLine(number);
}
```

Dictionary<TKey, TValue>集合类：在该类内部中维护两个数组来实现该功能。一个存储要从其映射的键，另一个存储映射到的值。分别称为键数组和值数组。在 Dictionary<TKey, TValue> 中插入键/值对时，将自动记录哪个键和哪个值关联，允许开发人员快速、简单地获取具有指定键的值。Dictionary<TKey, TValue> 类支持数组语法。

```c#
Dictionary<string, int> ages = new Dictionary<string, int>();
ages.Add("John", 23);
ages.Add("Diana", 24);
ages["James"] = 67;
ages["Francesca"] = 56;

foreach(KeyValuePair<string, int> element in ages)
{
	string name = element.Key;
	int age = element.Value;
	Console.WriteLine($"Name: {name}, Age: {age}"):
}
```

SortedList<TKey, TValue>集合类：该类的键数组是排好序的，当插入一个pair时，key会插入键数组的正确索引位置，保证数据处于排好序的状态，然后，值会插入值数组的相同索引位置。SortedList<TKey,TValue> 自动保证键和值的同步，即使在增加/删除了元素之后。这意味着可按任意顺序将键/值对插入一个 SortedList<TKey,TValue>，它们总是按照键的值来排序。

Queue<T>集合类：Queue<T>实现了先入先出队列，元素在队尾插入，从队头移除。入队用 Enqueue 方法，出队用 Dequeue 方法。

```c#
public T Peak();//返回队首元素，且不将其出队
public T[] ToArray();//将队列元素复制进新数组
public void CopyTo(T[] array, int arrayIndex);//从指定数据索引开始讲Queue<T>元素复制到现有一维Array中
public bool Contains(T item);//确定某元素是否在Queue<T>
public void Clear();//移除所有元素
```

Stack<T>集合类：Stack<T>实现了先入先出的栈，元素在顶部入栈，也从顶部出栈。

```c#
public void Push(T item);//入栈
public T Pop();//出栈
```

HashSet<T>集合类：数据项使用Add()方法插入HashSet<T>，用Remove方法删除，但HashSet<T>强大的是IntersectWith，UnionWith和ExceptWith方法，这些方法修改HashSet<T>集合来生成与另一个HashSet<T>相交、合并或不包含其数据项的新集合。

```c#
HashSet<string> employees = new HashSet<string>(new string[] {"Fred", "Bert", "Harry", "John"});
HashSet<string> customers = new HashSet<string>(new string[] {"John", "Sid", "Harry", "Diana"});
employees.Add("James");
customers.Add("Francesca");
Console.WriteLine("Employees: ");
foreach(string name in employees)
{
	Console.WriteLine(name);
}
Console.WriteLine("Customers: ");
foreach(string name in employees)
{
	Console.WriteLine(name);
}
Console.WriteLine("Customers who are also employees: ");
customers.IntersectWith(employees);
foreach(string name in customers)
{
	Console.WriteLine(name);
}
//customers.IntersectWith(employees);语句会生成两集合的交集并用这个并集覆盖customers集合
```

###### 集合初始化器

```c#
List<int> numbers = new List<int>(){10,9,8,7,6,5,4,3,2,1};
int[] pins = new int[4]{4,3,9,6};
```

​	集合使用这种初始化方法时，c#编译器内部会将其转换为一系列对Add()方法的调用，因此，**只有支持Add方法的集合才能这样写**，所以Queue<T>和Stack<T>不可以这样写。

```
Dictionary<string, int> ages = new Dictionary<string, int>()
{
	["John"] = 51;
	["Diana"] = 20;
	["Jame"] = 23;
	["Francesca"] = 21;
};
Dictionary<string, int> ages = new Dictionary<string, int>();
{
	{"John", 51},
	{"diana", 20},
	{"Jame", 23},
	{"Francesca", 21}
};
```

###### Find方法、谓词和Lambda表达式

​	面向字典的集合(也就是Dictionary<TKey, TValue>, SortedDictionary<TKey, TValue>, SortedList<TKey, TValue>)允许根据键来快速查找值，对于List<T>和LinkedList<T>等支持无键随机访问的集合，他们无法通过数组语法来查找项，所以提供了Find方法，Find方法的实参时代表搜索条件的谓词。

​	**谓词就是一个方法，也就是说Find方法的参数是另一个方法**， 作为谓词的方法检查集合的每一项、并返回Boolean值指出该项是否匹配，Find方法根据谓词返回的Boolean值返回第一个匹配项。另外FindLast则返回倒数第一个匹配项，FindAll返回一个包含所有匹配项的集合。

​	谓词最好用Lambda表达式指定，简单来说，Lambda表达式是能够返回方法的表达式。

```c#
ReturnType MethodName(parametersList)
{
	//MainBody
}
```

​	Lambda表达式的基本构成是：参数列表，方法主体

```c#
(ParamtersList) => { MainBody }
//=>是告诉编译器这是Lambda表达式，这与在表达式主体方法中的含义不同，方法主体的大括号在一些特殊情况下也能够省去
```

​	Lambda表达式没有方法名，也没有方法名，也没有返回值类型，编译器会根据上下文信息判断它的返回值类型。

​	返回到Find方法，在Find方法中，谓词依次处理集合中的每一项，谓词的主体**必须检查项**，根据是否匹配返回true或false，示例如下：

```c#
struct Person
{
	public int ID{ get; set; }
	public string Name{ get; set; }
	public int Age { get; set; }
}

List<Person> personnel = new List<Persion>()
{
	new Person() { ID = 1, Name = "John", Age = 51},
	new Person() { ID = 2, Name = "Sid", Age = 28},
	new Person() { ID = 3, Name = "Fred", Age = 34},
	new Person() { ID = 4, Name = "Paul", Age = 22}
};

Person match = personnel.Find( (Person p) => {return p.ID == 3;} );
Console.WriteLine($"ID: {match.ID}, Name: {match.Name}, Age	: {match.Age}");
```

​	Find方法的实参时谓词，我们用`Lambda表达式(Person  p) => { return p.ID == 3;}`充当了谓词，这个`Lambda表达式`的功能是判断传入的Person类型变量p的ID是否为3， 返回一个Boolean值。

​	调用 Find 方法时，实参 `(Person p) => {return p.ID == 3;}·`是实际干活儿的 Lambda 表达式，**它包含以下元素：**

- 圆括号的参数列表，即使Lambda表达式不获取任何参数，也要提供一对圆括号，对于Find方法，谓词要针对集合中的每一项执行操作，集合中的每一项都会作为参数传入Lambda表达式。
- =>操作符，他告诉编译器这是一个Lambda表达式
- 方法主体，编译器根据主题得知，Lambda表达式要返回一个Boolean值，另外方法主体可以包含多个语句，只需用分号结束语句即可。

Lambda表达式的参数列表的括号，方法主体的大括号在一些特殊情况下也能够省去，当Lambda表达式主体只有一个表达式，大括号和分号就可省去，如果参数列表也只有一个参数，那么圆括号也可以省去，最后我们甚至可以省略参数的类型，根据以上原则，可以得到如下Lambda表达式：

```c#
Person match = personnel.Find( p => p.ID ==3);
//p => p.ID == 3 等价于 (Person p) => { return p.ID == 3;}
```

```c#
//简化规则的Lambda表达式
x => x * x
x => { return x * x;}
(int x) => x / 2
() => Console.WriteLine("Hello World")
(x, y) => { x++; return x / y;}
(ref int x, int y) { x++; return x / y;}
```

​	任何`Lambda表达式`都可以转化为委托类型，`Lambda表达式`可以转换的委托类型由其参数和返回值的类型定义，若`Lambda表达式`不返回值，则可以将其转换为`Action委托类型`之一；否则可将其转换为`Func委托类型`之一。例如，有两个参数且不返回值的Lambda表达式可转换为Action<T1, T2>委托，有一个参数且不返回值的Lambda表达式可转换为Func<T, TResult>委托。

```c#
Func<int, int> square = x => x * x;
Console.WriteLine(square(5));
```

- ​	仅当Lambda只有一个输入参数时，括号才是可选的，否则括号是必须的，当参数个数为0或2,3,4....都需要括号。
- 编写Lambda时，通常不必为输入参数指定类型，因为编译器可以根据Lambda主体、参数类型以及C#语言规范中描述的其他因素来推断类型。Lambda类型推断的规则如下：
  1. Lambda包含的参数数量必须与委托类型包含的参数数量相同。
  2. Lambda中的每个输入参数必须能够隐式转换为其对应的委托参数。
  3. Lambda的返回值(如果有)必须能够隐式转换为委托的返回类型，

###### 枚举集合和枚举器

​	`foreach`语句极大简化了需要编写的代码，但它的使用也是有条件的，这个条件就是：`foreach`只可用于遍历**可枚举集合。**

​	可枚举集合就是实现了`System.Collections.IEnumerable`接口的集合，C#所有数组都是`System.Array`的实例，而`System.Array`类是实现了`IEnumerate`接口的类，所有所有数组都支持`foreach`语句。

​	`IEnumerate`接口只包含`GetEnumerator`方法，因此我们只需要实现这个方法，就能完后才能为接口`IEnumerable`的实现，`GetEnumerator`方法的返回值是`IEnumerator`，为何有一个返回接口的方法？其实返回值`IEnumerator`表示的是需要返回一个**实现了`IEnumerator`接口的对象**。

​	一个实现了`IEnumerator`接口的对象被称为枚举器对象，因此方法`GetEnumerator`正如其名，返回一个枚举器。`IEnumerator`接口制定了以下属性和方法：

```c#
object Current{ get; }
bool MoveNext();
void Reset();
```

​	枚举器实现以上代码段声明的方法和属性，可以将<u>枚举器想象成实现了`IEnumerable`接口的对象中的元素的指针</u>，放在List<T>类中，指针最开始指向第一个元素之前的位置，可以理解位置为-1，调用`MoveNext`方法就可以将指针移动至列表中的下一项，如果可以实际移动到下一个项，`MoveNext`返回true，否则返回false，可用Current属性访问当前指向的那一项；`Reset`方法可以使指针会到列表第一项之前的位置(-1)。

​	<u>实现了`IEnumerable`接口的类通过集合的`GetEnumerator`方法创建枚举器，然后反复调用`MoveNext`方法，并获取`Current`属性的值，就可以每次在该集合中移动一个元素的位置，`foreach`就是做这样的事情。</u>所以，为了创建自己的可枚举集合类，就必须实现`IEnumerable`接口，并提供`IEnumerator`接口的实现，由`GetEnumerator`方法返回。

​	稍微想一下，就会发现`IEnumberator`接口的`Current`属性具有非类型安全的行为，因为他返回的为`object`类型而非具体类型，.NET Framework提供了`IEnumerator<T>`接口，该接口也有`Current`属性，但返回的是一个T。

```c#
using System;
using System.Collections;

public class Person
{
	public Person(string fName, string LName)
	{
		this.firstName = fName;
		this.lastName = lName;
	}
	public string firstName;
	public string lastName;
}
public class People : IEnumerable
{
	private Person[] _people;
	public People(Personp[] pArray)
	{
		_people = new Person[pArray.Length];
		for(int i = 0; i< pArray.Length; i++)
		{
			_people[i] = pArray[i];
		}
	}
	IEnumerator IEnumerable.GetEnumerator()
	{
		return (IEnumerator)GetEnumerator();
	}
	public PeopleEnum GetEnumerator()
	{
		return new PeopleEnum(_people);
	}
}
public class PeopleEnum : IEnumerator
{
    public Person[] _people;
    int position = -1;
    public PeopleEnum(Personp[] list)
    {
        _people = list;
    }
    public bool MoveNext()
    {
        position = -1;
    }
    public Person Current
    {
        get
        {
            try
            {
                return _people[position];
            }
            catch(IndexOutOfRangeException)
            {
                throw new InvalidOperationException();
            }
        }
    }
    object Ienumerator.Current
    {
        get
        {
            return Current;
        }
    }
}
class App
{
    static void Main()
    {
        Personp[] peopleArray = new Person[3]
        {
            new Person("John", "Smith"),
            new Person("Jim", "Johnson"),
            new Person("Sue", "Rabon"),
        };
        People peopleList = new People(peopleArray);
        foreach(Person p in peopleList)
        {
            Console.WriteLine(p.firstName + " " + p.lastName);
        }
	}
}
```



###### 迭代器实现枚举器

###### 委托和事件

委托：委托实际上是一个类，一个密封类，委托是对方法的引用。声明委托的操作如下：

```c#
delegate void myDelegate();
//关键字   返回值类型   委托名称 参数列表
```

和类的方式一样还需创建实例。

```c#
//声明委托
delegate void myDelegate();
//声明一个准备被委托引用的方法
private void myMethod() {...};
...
//这种方法创建委托实例并初始化，参数值必须是方法，方法签名要和委托签名匹配
myDelegate del = new myDelegate(this.myMethod);
```

调用委托的方式和调用方法的方式一样：

```c#
myDelegate del;
//初始化del以及执行其他操作
del();
```

事件：正如使用属性能在封装字段的同时使得字段能被外界有限制地访问，事件有类似的功能：对委托进行封装。

事件的声明：

```c#
delegate void myDelegate();
class myClass
{
	public event myDelegate bellRang;
	public void Connect()
	{
		//将goCanteen方法加入订阅列表
		bellRang += goCanteen;
	}
	public void goCanteen()
	{
		//去食堂
	}
}
```

​	在Connect方法中用+=操作符使得`goCanteen`方法订阅了事件`bellRang`(取消订阅为-=)。有了事件和事件的订阅者，还需触发事件即调用事件。

```c#
delegate void myDelegate();
class XiaoMing
{
	//中午铃响事件
	public event myDelegate bellRang;
	...
	private void RaiseEvent()
	{
		//事件不为空就触发事件
		if (this.bellRang != null)
		{
			this.bellRang();
		}
	}
}
```

#### Append

###### 终结器

  由于垃圾回收器负责回收所有内存管理工作，所以终结器不负责回收内存，主要任务是释放数据库连接和文件句柄这样的资源，同时也要避免在终结器中出现异常。

###### 迭代器

  迭代器提供了迭代器接口(也就是`IEnumerable<T>`和`IEnumerator<T>`接口的组合)的快捷实现。

```c#
using System;
using System.CollectionsGeneric;

public class BinaryTree<T> : IEnumerable<T>
{
	public BinaryTree(T value)
	{
		Value = value;
	}
	#regin IEnumerable<T>
	public IEnumerable<T> GetEnumerator()
	{
	    ///...
	}
	public T Value
	{ get; private set;}
	public Pair<BinaryTree<T>> SubItems
	{ get; private set;}
}
public struct Pair<T>
{
	public Pair(T first, T second) : this()
	{
		First = first;
		Second = second;
	}
	public T First { get; private set;}
	public T Second { get; private set;}
}
```

​    迭代器类似于函数，但它不是返回(return)一个值，而是生成(yield)一系列值。在`BinaryTree<T>`的情况下，迭代器生成的值的类型是提供给T的类型实参，假如使用的是IEnumerator的非泛型版本，生成的值就是object类型。

​    为了实现迭代器模式，需要维护一些内部状态，以便可以在枚举集合时跟踪记录当前位置，在`BinaryTree<T>`的情况下，要记录的就是树中那些元素已经被枚举，那些还有没被枚举，迭代器由编译器转化成”状态机“来跟踪当前位置，它还知道如何将自己移动到下一个位置。

​    迭代器每次遇到yield return语句都会生成一个值，控制立即返回到请求数据项的调用者，然后当调用者请求下一项时，会紧接上一个yield return语句之后执行。

```c#
using System;
using System.Collections.Generic;
public class CSharpBuiltInTypes: IEnumerable<string>
{
    public IEnumerator<string> GetEnumerator()
	{
        yield return "object";
        yield return "byte";
        yield return "uint";
        yield return "ulong";
        yield return "float";
        ///...
	}
    System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }
}
public class Program
{
    static void Main()
    {
        CSharpBuiltInTypes keywords = new CSharpBuiltInTypes();
        foreach(string keyword in keywords)
        {
            Console.WriteLine(keyword);
        }
    }
}
```

