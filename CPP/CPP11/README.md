**nullptr**

nullptr 出现的目的是为了替代 NULL，nullptr是一个右值，只能用右值引用接收

考虑：当传入0的时候会调用哪一个函数

```C++
void foo(char *);
void foo(int);
// 会调用 
```

 实现

```C++
const class nullptr_t // 常量
{
public:
  template<class T>  //隐式转换 operator的功能之一，隐式类型转换，使nullptr可以转换成任意T
  inline operator T*() const
    { return 0; }
 
  template<class C, class T>  //隐式转换 是nullptr可以转换成C的成员指针T
  inline operator T C::*() const
    { return 0; }
private:
void operator&() const;
} nullptr = {};
```

 **C++11 无参数初始化风格**

```C++
m = new IntCell{}；
// C++11 可以这么做
vector<int> daysInMonth = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12 };
vector<int > vecList = {};
// 甚至还可以去掉等号
vector<int> daysInMonth { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12 };
```

 **shared_ptr**

```C++
std::shared_ptr<int>  p1(new int());
p1.use_count();
```

-  创建空的 shared_ptr 对象

  因为带有参数的 shared_ptr 构造函数是 explicit(禁止隐式构造，只能显示调用) 类型的，所以不能像这样std::shared_ptr<int> p1 = new int();隐式调用它构造函数。

  创建新的shared_ptr对象的最佳方法是使用std :: make_shared：

```C++
std::shared_ptr<int> p1 = std::make_shared<int>();
```

​		**std::make_shared 一次性为int对象和用于引用计数的数据都分配了内存，**而new操作符只是为int分配了内存。

-  分离关联的原始指针

  要使 shared_ptr 对象取消与相关指针的关联，可以使用reset()函数：

  不带参数的reset()：`p1.reset()`; 它将引用计数减少1，如果引用计数变为0，则删除指针。

  带参数的reset()：`p1.reset(new int(34))`; 在这种情况下，它将在内部指向新指针，因此其引用计数将再次变为1。

-  使用nullptr重置：

  p1 = nullptr;

  std::shared_ptr<int> p2(p1); 构造p2

- 缺少 ++, – – 和 [] 运算符


- 不要使用同一个原始指针构造 shared_ptr

  创建多个 shared_ptr 的正常方法是使用一个已存在的shared_ptr 进行创建，而不是使用同一个原始指针进行创建。

- 自定义删除器 Deleter 使用Lambda 表达式 / 函数对象作为删除器

  下面将讨论如何将自定义删除器与 std :: shared_ptr 一起使用。

  当 shared_ptr 对象超出范围时，将调用其析构函数。在其析构函数中，它将引用计数减1，如果引用计数的新值为0，则删除关联的原始指针。

  析构函数中删除内部原始指针，默认调用的是delete()函数。`delete Pointer`;

  有些时候在析构函数中，delete函数并不能满足我们的需求，可能还想加其他的处理。

  当 shared_ptr 对象指向数组 `std::shared_ptr<int> p3(new int[12])`; 像这样申请的数组，应该调用delete []释放内存，而shared_ptr析构函数中默认delete并不能满足需求。

  给shared_ptr添加自定义删除器,在上面在这种情况下，我们可以将回调函数传递给 shared_ptr 的构造函数，该构造函数将从其析构函数中调用以进行删除，即

  ```C++
  // 自定义删除器
  void deleter(Sample * x)
  {
      std::cout << "DELETER FUNCTION CALLED\n";
      delete[] x;
  }
  // 构造函数传递自定义删除器指针
  std::shared_ptr<Sample> p3(new Sample[12], deleter);
  ```

- 不要用栈中的指针构造 shared_ptr 对象

  shared_ptr 默认的构造函数中使用的是delete来删除关联的指针，所以构造的时候也必须使用new出来的堆空间的指针。

  

- shared_ptr如果放在容器中，记得调用容器的erase函数，才会释放内存

 **unique_ptr**

保存数组

```C++
unique_ptr<int[]> p(new int[5] {1, 2, 3, 4, 5});
p[0] = 0;  // 重载了operator[]
```

 **无法进行赋值和拷贝构造 只能进行移动构造，只接受右值**

```C++
unique_ptr<int> pInt(new int(5));
unique_ptr<int> pInt2 = std::move(pInt);  // 转移所有权，将pInt转换成右值，不转移的话不能赋值
```

 函数内返回

```C++
unique_ptr<int> clone(int p)
{
  unique_ptr<int> pInt(new int(p));
  return pInt;  // 返回unique_ptr
}
```

 **类型推导**

C++11 引入了 auto 和 decltype 这两个关键字实现了类型推导，让编译器来操心变量的类型。

 **auto**

注意：auto 不能用于函数传参，因此下面的做法是无法通过编译的（考虑重载的问题，我们应该使用模板）：

int add(auto x, auto y);

此外，auto 还不能用于推导数组类型：

```C++
#include <iostream>
int main() {
 auto i = 5;
 int arr[10] = {0};
 auto auto_arr = arr;
 auto auto_arr2[10] = arr;
 return 0;
}
```

**decltype**

decltype 关键字是为了解决 auto 关键字只能对变量进行类型推导的缺陷而出现的。它的用法和 sizeof 很相似：

decltype(表达式)

```C++
auto x = 1;
auto y = 2;
decltype(x+y)  z; 根据x和y 推导z的类型
```

从 C++14 开始是可以直接让普通函数具备返回值推导，因此下面的写法变得合法：

```C++
template<typename T, typename U>
auto add(T x, U y) {
  return x+y;
}
```

**区间迭代**

基于范围的 for 循环

/ & 启用了引用

```c++
for(auto &i : arr) {  std::cout << i << std::endl; }
```

**初始化列表**

C++11 提供了统一的语法来初始化任意的对象，例如：

```C++
struct A {
  int a;
  float b;
};
struct B {
  B(int _a, float _b): a(_a), b(_b) {}
private:
  int a;
  float b;
};
A a {1, 1.1};  // 统一的初始化语法
B b {2, 2.2};
```

C++11 还把初始化列表的概念绑定到了类型上，并将其称之为 std::initializer_list，允许构造函数或其他函数像参数一样使用初始化列表，这就为类对象的初始化与普通数组和 POD 的初始化方法提供了统一的桥梁，例如：

```C++
#include <initializer_list>
class Magic {
public:
  Magic(std::initializer_list<int> list) {}
};
Magic magic = {1,2,3,4,5};
std::vector<int> v = {1, 2, 3, 4};
```

 **模板增强**

**外部模板**

传统 C++ 中，模板只有在使用时才会被编译器实例化。只要在每个编译单元（文件）中编译的代码中遇到了被完整定义的模板，都会实例化。这就产生了重复实例化而导致的编译时间的增加。并且，我们没有办法通知编译器不要触发模板实例化。

C++11 引入了外部模板，扩充了原来的强制编译器在特定位置实例化模板的语法，使得能够显式的告诉编译器何时进行模板的实例化：

template class std::vector<bool>;      // 强行实例化

extern template class std::vector<double>; // 不在该编译文件中实例化模板

**尖括号 “>”**

在传统 C++ 的编译器中，>>一律被当做右移运算符来进行处理。但实际上我们很容易就写出了嵌套模板的代码：

```c++
std::vector<std::vector<int>>  wow;
```

这在传统C++编译器下是不能够被编译的，而 C++11 开始，连续的右尖括号将变得合法，并且能够顺利通过编译。

**类型别名模板**

在传统 C++中，typedef 可以为类型定义一个新的名称，但是却没有办法为模板定义一个新的名称。因为，模板不是类型。例如：

```C++
template< typename T, typename U, int value>
class SuckType {
public:
  T a;
  U b;
  SuckType():a(value),b(value){}
};
template< typename U>
typedef SuckType<std::vector<int>, U, 1> NewType; // 不合法
```

C++11 使用 using 引入了下面这种形式的写法，并且同时支持对传统 typedef 相同的功效：

```c++
template <typename T>
using NewType = SuckType<int, T, 1>;  // 合法
```

**参考网站：**[**https://blog.csd**

[**n.net/xp178171640/article/details/104527251/**](https://blog.csdn.net/xp178171640/article/details/104527251/)

**默认模板参数**

我们可能定义了一个加法函数：

```C++
template<typename T, typename U>
auto add(T x, U y) -> decltype(x+y) {
  return x+y
}
```

但在使用时发现，要使用 add，就必须每次都指定其模板参数的类型。 

在 C++11 中提供了一种便利，可以指定模板的默认参数：

```C++
template<typename T = int, typename U = int>
auto add(T x, U y) -> decltype(x+y) {
  return x+y;
}
```

 **构造函数**

委托构造

C++11 引入了委托构造的概念，这使得构造函数可以在同一个类中一个构造函数调用另一个构造函数，从而达到简化代码的目的：

```C++
class Base {
public:
  int value1;
  int value2;
  Base() {
	value1 = 1;
  }
  Base(int value) : Base() { // 委托 Base() 构造函数
    value2 = 2;
  }
};
```

继承构造

在继承体系中，如果派生类想要使用基类的构造函数，需要在构造函数中显式声明。 

假若基类拥有为数众多的不同版本的构造函数，这样，在派生类中得写很多对应的“透传”构造函数。如下：

```C++
struct A
{
 A(int i) {}
 A(double d,int i){}
 A(float f,int i,const char* c){}
 //...等等系列的构造函数版本
}；
struct B:A
{
 B(int i):A(i){}
 B(double d,int i):A(d,i){}
 B(folat f,int i,const char* c):A(f,i,e){}
 //......等等好多个和基类构造函数对应的构造函数
};
```

C++11的继承构造：

```C++
struct A
{
 A(int i) {}
 A(double d,int i){}
 A(float f,int i,const char* c){}
 //...等等系列的构造函数版本
}；
struct B:A
{
 using A::A;
 //关于基类各构造函数的继承一句话搞定
 //......
}
```

default 和delete（类似以private修饰构造函数）修饰构造函数

delete不可以修饰析构函数

```c++
class noncopyable 
{
protected: 
	constexpr noncopyable() = **default;
	~noncopyable() = **default;
	noncopyable(**const noncopyable &) = delete;
	noncopyable &operator= (**const noncopyable &) = delete;
};
```

 

**override和final**

作用于虚函数，更多的作用是：显式的标识是否应该多态继承或不应该 

**std::move()**  移动语义

能把左值强制转换为右值

```C++
Integer b(temp); //调用正常拷贝函数
Integer b(std::move(temp)); //调用右值引用的拷贝构造函数
```

 **Lambda 表达式**

Lambda 表达式，实际上就是提供了一个类似匿名函数的特性，而匿名函数则是在需要一个函数，但是又不想费力去命名一个函数的情况下去使用的。

Lambda 表达式的基本语法如下：

```C++
[ caputrue ] ( params ) opt -> ret { body; };
// capture是捕获列表； 
// params是参数表；(选填) 
// opt是函数选项；可以填mutable,exception,attribute（选填）
	// mutable说明lambda表达式体内的代码可以修改被捕获的变量，并且可以访问被捕获的对象的non-const方法。 
	// exception说明lambda表达式是否抛出异常以及何种异常。
	// attribute用来声明属性。 
// ret是返回值类型（拖尾返回类型）。(选填) 
// body是函数体。
```

捕获列表：lambda表达式的捕获列表精细控制了lambda表达式能够访问的外部变量，以及如何访问这些变量。

- []不捕获任何变量。 
- [&]捕获外部作用域中所有变量，并作为引用在函数体中使用（按引用捕获）。
- [=]捕获外部作用域中所有变量，并作为副本在函数体中使用(按值捕获)。**注意值捕获的前提是变量可以拷贝，且被捕获的变量在 lambda 表达式被创建时拷贝，而非调用时才拷贝。**如果希望lambda表达式在调用时能即时访问外部变量，我们应当使用引用方式捕获。

```C++
int a = 0;
auto f = [=] { return a; };
a+=1;
cout << f() << endl;    //输出0
int a = 0;
auto f = [&a] { return a; };
a+=1;
cout << f() <<endl;    //输出1
```

- [=,&foo]按值捕获外部作用域中所有变量，并按引用捕获foo变量。 
- [bar]按值捕获bar变量，同时不捕获其他变量。
- [this]捕获当前类中的this指针，让lambda表达式拥有和当前类成员函数同样的访问权限。如果已经使用了&或者=，就默认添加此选项。捕获this的目的是可以在lamda中使用当前类的成员函数和成员变量。

虽然按值捕获的变量值均复制一份存储在lambda表达式变量中，修改他们也并不会真正影响到外部，但我们却仍然无法修改它们。如果希望去修改按值捕获的外部变量，需要显示指明lambda表达式为mutable。被mutable修饰的lambda表达式就算没有参数也要写明参数列表。

原因：lambda表达式可以说是就地定义仿函数闭包的“语法糖”。**它的捕获列表捕获住的任何外部变量，最终会变为闭包类型的成员变量**。按照C++标准，lambda表达式的operator()默认是const的，一个const成员函数是无法修改成员变量的值的。而mutable的作用，就在于取消operator()的const。

```C++
int a = 0;
auto f1 = [=] { return a++; };        //error
auto f2 = [=] () mutable { return a++; };    //OK
```

lambda表达式的大致原理：每当你定义一个lambda表达式后，编译器会自动生成一个匿名类（这个类重载了()运算符），我们称为闭包类型（closure type）。那么在运行时，这个lambda表达式就会返回一个匿名的闭包实例，是一个右值。所以，我们上面的lambda表达式的结果就是一个个闭包。**对于复制传值捕捉方式，类中会相应添加对应类型的非静态数据成员。在运行时，会用复制的值初始化这些成员变量，从而生成闭包。**对于引用捕获方式，无论是否标记mutable，都可以在lambda表达式中修改捕获的值。

lambda表达式是不能被赋值的：

```C++
auto a = [] { cout << "A" << endl; };
auto b = [] { cout << "B" << endl; };
a = b;  // 非法，lambda无法赋值禁用了=号
auto c = a;  // 合法，生成一个副本 使用赋值构造函数
```

闭包类型禁用了赋值操作符，但是没有禁用复制构造函数，所以你仍然可以用一个lambda表达式去初始化另外一个lambda表达式而产生副本。

默认引用捕获所有变量，你有很大可能会出现悬挂引用（Dangling references），因为引用捕获不会延长引用的变量的生命周期：

**可能会延长变量周期的情况**：使用临时变量来初始化引用

```C++
std::function<int(int)> add_x(int x)
{   
  return [&](int a) { return x + a; };
}
```

上面函数返回了一个lambda表达式，参数x仅是一个临时变量，函数add_x调用后就被销毁了，但是返回的lambda表达式却引用了该变量，当调用这个表达式时，引用的是一个垃圾值，会产生没有意义的结果。**上面这种情况，使用默认传值方式可以避免悬挂引用问题。**

但是采用默认值捕获所有变量仍然有风险，看下面的例子：

```C++
class Filter
{
public:
  Filter(int divisorVal):
    divisor{divisorVal}  {}
  std::function<bool(int)> getFilter() 
  {
    return [=](int value) {return value % divisor == 0; };
  }
private:
  int divisor;
};
```

这个类中有一个成员方法，可以返回一个lambda表达式，这个表达式使用了类的数据成员divisor。而且采用默认值方式捕捉所有变量。你可能认为这个lambda表达式也捕捉了divisor的一份副本，但是实际上并没有。因为数据成员divisor对lambda表达式并不可见，你可以用下面的代码验证：

 // 类的方法，下面无法编译，因为divisor并不在lambda捕捉的范围

```C++
std::function<bool(int)> getFilter() 
{
  return [divisor](int value) {return value % divisor == 0; };
}
```

原代码中，lambda表达式实际上捕捉的是this指针的副本，所以原来的代码等价于：

```C++
std::function<bool(int)> getFilter() 
{
  return [this](int value) {return value % this->divisor == 0; };
}
```

尽管还是以值方式捕获，但是捕获的是指针，其实相当于以引用的方式捕获了当前类对象，所以lambda表达式的闭包与一个类对象绑定在一起了，这很危险，因为你仍然有可能在类对象析构后使用这个lambda表达式，那么类似“悬挂引用”的问题也会产生。所以，采用默认值捕捉所有变量仍然是不安全的，主要是由于指针变量的复制，实际上还是按引用传值。

[&this]是不允许的，[this]永远按照值捕获，捕获this后可以调用类的成员变量

lambda表达式可以赋值给对应类型的函数指针。但是使用函数指针并不是那么方便。所以STL定义在< functional >头文件提供了一个多态的函数对象封装std::function，其类似于函数指针。它可以绑定任何类函数对象，只要参数与返回类型相同。如下面的返回一个bool且接收两个int的函数包装器：

```C++
std::function<bool(int, int)> wrapper = [](int x, int y) { return x < y; };
```

lambda表达式一个更重要的应用是其可以用于函数的参数，通过这种方式可以实现回调函数。

最常用的是在STL算法中，比如你要统计一个数组中满足特定条件的元素数量，通过lambda表达式给出条件，传递给count_if函数：

```C++
int value = 3;
vector<int> v {1, 3, 5, 2, 6, 10};
int count = std::count_if(v.beigin(), v.end(), [value](int x) { return x > value; });
```

再比如你想生成斐波那契数列，然后保存在数组中，此时你可以使用generate函数，并辅助lambda表达式：

```C++
vector<int> v(10);
int a = 0;
int b = 1;
std::generate(v.begin(), v.end(), [&a, &b] { int value = b; b = b + a; a = value; return value; });
// 此时v {1, 1, 2, 3, 5, 8, 13, 21, 34, 55}
```

当需要遍历容器并对每个元素进行操作时：

```C++
std::vector<int> v = { 1, 2, 3, 4, 5, 6 };
int even_count = 0;
for_each(v.begin(), v.end(), [&even_count](int val){
  if(!(val & 1)){
    ++even_count;
  }
});
std::cout << "The number of even is " << even_count << std::endl;
```

大部分STL算法，可以非常灵活地搭配lambda表达式来实现想要的效果。

 **using取代typedef：**

```C++
typedef double db; //c99
using db = double; //c++11

typedef void(*function)(int, int)；//c99，函数指针类型定义
using function = void(*)(int, int)；//c++11，函数指针类型定义

using kvpairs = std::map<std::string, std::string>;
using CompareOperator = std::function<int (kvpairs &, kvpairs &)>;
using query_record = std::tuple<time_t, std::string>;

template<class T> 
using twins = std::pair<T, T>；
//更广泛的还可以用于模板
```

**字符串和数值类型的转换**

以前的atoi、itoa等等成为历史

to_string：itoa成为历史

stoi、stol、stoul、stoll、stoull、stof、stod、stold：atoX成为历史

 **std::ref和std::cref**

分别对应变量的引用和const引用，主要用于作为c++11函数式编程时传递的参数

 **std::chrono时间相关**

比以前的时间方便了许多：

```C++
std::chrono::duration<double> duration//时间间隔
std::this_thread::sleep_for(duration);//sleep
LOG(INFO) << "duration is " << duration.count() << std::endl;
std::chrono::microseconds //微秒
std::chrono::seconds //秒
end = std::chrono::system_clock::now();//获取当前时间
```

 **原子变量**

`std::atomic<XXX>`

用于多线程资源互斥操作，属c++11重大提升，多线程原子操作简单了许多

事实上基于c++11实现的无锁队列，让boost::lockfree无锁队列也将成为历史

 **2、容器**

2.1、tuple & 花括号初始化

元组的出现，和python拉齐了，c++也实现了函数可以多个返回值

```C++
using result = std::tuple<int, char, double>;
result res {1,'a'，1.0};
return res;
return {2, 'b',100.0};
std::vector<int> v{1, 2, 3, 4, 5}；

using res_tp = std::tuple<bool, char, int, float, double>;
res_tp res(true, 'b', 11, 1.1, 100.1);
LOG(INFO) << "res.bool: " << std::get<0>(res);
LOG(INFO) << "res.char: " << std::get<1>(res);
LOG(INFO) << "res.int: " << std::get<2>(res);
LOG(INFO) << "res.float: " << std::get<3>(res);
LOG(INFO) << "res.double: " << std::get<4>(res);
```

以上都是合法的，尤其对vector，简单的测试程序再不需要一行行的push_back了！

2.2、hash正式进入stl

unordered_map、unordered_set、unordered_multimap、unordered_multiset。extstl扩展方式的使用hash成为历史。

2.3、emplace：

作用于容器，区别于push、insert等，如push_back是在容器尾部追加一个容器类型对象，emplace_back是构造1个新对象并追加在容器尾部

对于标准类型没有变化，如std::vector<int>，push_back和emplace_back效果一样

如自定义类型class A，A的构造函数接收一个int型参数，

那么对于push_back需要是：

```c++
std::vector<A> vec；
A a(10);
vec.push_back(a);
```

 对于emplace_back则是：

```C++
std::vector<A> vec；
vec.emplace_back(10);
```

改进点是什么？改进点：**避免无用临时变量**。比如上面例子中的那个a变量

2.4、shrink_to_fit() 尝试减少容量

```C++
<int> v{1, 2, 3, 4, 5}; 
v.push_back(1); 
std::cout << "before shrink_to_fit: " << v.capacity() << std::endl; 
v.shrink_to_fit(); 
std::cout << "after shrink_to_fit: " << v.capacity() << std::endl; 
// 可以试一试，减少了很多，有一定价值
```

**请求容量降低到和size匹配，但是这个仅是个请求是否执行由编译器决定**

[**https://blog.csdn.net/qq844352155/article/details/38685907**](https://blog.csdn.net/qq844352155/article/details/38685907)

**shrink_to_fit 和swap的区别都是利用拷贝构造函数来交换空间**

**容器**

**std::array**

std::array 保存在栈内存中，相比堆内存中的 std::vector，我们能够灵活的访问这里面的元素，从而获得更高的性能。

std::array 会在编译时创建一个固定大小的数组，std::array 不能够被隐式的转换成指针，使用 std::array只需指定其类型和大小即可

 **正则表达式**

```C++
int main() {
  std::string fnames[] = {"foo.txt", "bar.txt", "test", "a0.txt", "AAA.txt"};
  // 在 C++ 中 `\` 会被作为字符串内的转义符，为使 `\.` 作为正则表达式传递进去生效，需要对 `\` 进行二次转义，从而有 `\\.`
  std::regex txt_regex("[a-z]+\\.txt");
  for (const auto &fname: fnames)
    std::cout << fname << ": " << std::regex_match(fname, txt_regex) << std::endl;
}
```

**语言级线程支持**

std::thread 

std::mutex/std::unique_lock

std::unique_lock 与std::lock_guard都能实现自动加锁与解锁功能，但是std::unique_lock要比std::lock_guard更灵活，但是更灵活的代价是占用空间相对更大一点且相对更慢一点。

std::unique_lock

(1) 默认构造函数

新创建的 unique_lock 对象不管理任何 Mutex 对象。

(2) locking 初始化

新创建的 unique_lock 对象管理 Mutex 对象 m，并尝试调用 m.lock() 对 Mutex 对象进行上锁，如果此时另外某个 unique_lock 对象已经管理了该 Mutex 对象 m，则当前线程将会被阻塞。

(3) try-locking 初始化

新创建的 unique_lock 对象管理 Mutex 对象 m，并尝试调用 m.try_lock() 对 Mutex 对象进行上锁，但如果上锁不成功，并不会阻塞当前线程。

(4) deferred 初始化

新创建的 unique_lock 对象管理 Mutex 对象 m，但是在初始化的时候并不锁住 Mutex 对象。 m 应该是一个没有当前线程锁住的 Mutex 对象。

(5) adopting 初始化

新创建的 unique_lock 对象管理 Mutex 对象 m， m 应该是一个已经被当前线程锁住的 Mutex 对象。(并且当前新创建的 unique_lock 对象拥有对锁(Lock)的所有权)。

(6) locking 一段时间(duration)

新创建的 unique_lock 对象管理 Mutex 对象 m，并试图通过调用 m.try_lock_for(rel_time) 来锁住 Mutex 对象一段时间(rel_time)。

(7) locking 直到某个时间点(time point)

新创建的 unique_lock 对象管理 Mutex 对象m，并试图通过调用 m.try_lock_until(abs_time) 来在某个时间点(abs_time)之前锁住 Mutex 对象。

(8) 拷贝构造 [被禁用]

unique_lock对象不能被拷贝构造。

(9) 移动(move)构造

新创建的 unique_lock 对象获得了由 x 所管理的 Mutex 对象的所有权(包括当前 Mutex 的状态)。调用 move 构造之后， x 对象如同通过默认构造函数所创建的，就不再管理任何 Mutex 对象了。

综上所述，由 (2) 和 (5) 创建的 unique_lock 对象通常拥有 Mutex 对象的锁。而通过 (1) 和 (4) 创建的则不会拥有锁。通过 (3)，(6) 和 (7) 创建的 unique_lock 对象，则在 lock 成功时获得锁。

std::future/std::packaged_task 

```c++
#include <iostream>
#include <future>
int main()
{
  std::promise<int> dataPromise;                 //建立一个承诺
  std::future<int> dataFuture = dataPromise.get_future();     //每个承诺的等待者
  std::thread thPromise([&dataPromise]
  {
    std::cout << "Enter thPromise" << std::endl;
    std::this_thread::sleep_for(std::chrono::seconds(2));
    dataPromise.set_value(10);                 //承诺兑现
  });
  std::thread thFuture([&dataFuture]
  {
    std::cout << dataFuture.get() << std::endl;         //得到承诺成果
  });
  thPromise.join();
  thFuture.join();
  return 0;
}
```

```C++
#include <iostream>
#include <future>
int main()
{
  std::packaged_task<int()> dataTask([]()                  //建立任务
  {
    std::this_thread::sleep_for(std::chrono::seconds(2));
    return 20;
  });
  auto dataFuture = dataTask.get_future();                 //任务的等待者，等待任务结束
  std::thread thTask([&dataTask]()
  {
    dataTask();                              //创建线程执行任务
  });
  std::thread thFuture([&dataFuture]()
  {
    std::cout << dataFuture.get() << std::endl;              //得到任务结果
  });
  thTask.join();
  thFuture.join();
  return 0;
}
```

std::condition_variable // 条件变量

代码编译需要使用 -pthread 选项

 **std::function&std::bind**

**1. 可调用对象**

可调用对象有一下几种定义：

是一个函数指针，参考 C++ 函数指针和函数类型；

是一个具有operator()成员函数的类的对象；

可被转换成函数指针的类对象；

一个类成员函数指针；

C++中可调用对象的虽然都有一个比较统一的操作形式，但是定义方法五花八门，这样就导致使用统一的方式保存可调用对象或者传递可调用对象时，会十分繁琐。C++11中提供了std::function和std::bind统一了可调用对象的各种操作。

不同类型可能具有相同的调用形式，如：

```C++
// 普通函数
int add(int a, int b){return a+b;} 
// lambda表达式
auto mod = [](int a, int b){ return a % b;}
// 函数对象类
struct divide
{
  int operator()(int denominator, int divisor)
  {
  	return denominator/divisor;
  }
};
```

上述三种可调用对象虽然类型不同，但是共享了一种调用形式：

`int(int ,int)`

std::function就可以将上述类型保存起来，如下：

```C++
std::function<int(int ,int)> a = add; 
std::function<int(int ,int)> b = mod ; 
std::function<int(int ,int)> c = divide(); 
```

**2. std::function**

std::function 是一个可调用对象包装器，是一个类模板，可以容纳**除了类成员函数指针**之外的所有可调用对象，它可以用统一的方式处理函数、函数对象、函数指针，并允许保存和延迟它们的执行。

定义格式：std::function<函数类型>。

std::function可以取代函数指针的作用，因为它可以延迟函数的执行，特别适合作为回调函数使用。它比普通函数指针更加的灵活和便利。

**3. std::bind**

可将std::bind函数看作一个通用的函数适配器，它接受一个可调用对象，生成一个新的可调用对象来“适应”原对象的参数列表。

std::bind将可调用对象与其参数一起进行绑定，绑定后的结果可以使用std::function保存。std::bind主要有以下两个作用：

**将可调用对象和其参数绑定成一个仿函数；**

只绑定部分参数，减少可调用对象传入的参数。

**3.1 std::bind绑定普通函数**

```C++
double my_divide (double x, double y) {return x/y;}
auto fn_half = std::bind (my_divide,_1,2); 
std::cout << fn_half(10) << '\n';            // 5
```

bind的第一个参数是函数名，普通函数做实参时，会隐式转换成函数指针。因此`std::bind (my_divide,_1,2)`等价于`std::bind (&my_divide,_1,2)；`

`_1`表示占位符，位于<functional>中，`std::placeholders::_1；`

**3.2 std::bind绑定一个成员函数**

```C++
struct Foo {
  void print_sum(int n1, int n2)
  {
    std::cout << n1+n2 << '\n';
  }
  int data = 10;
};
int main() 
{
  Foo foo;
  auto f = std::bind(&Foo::print_sum, &foo, 95, std::placeholders::_1);
  f(5); // 100
}
```

**bind绑定类成员函数时，第一个参数表示对象的成员函数的指针，第二个参数表示对象的地址。**

必须显示的指定&Foo::print_sum，因为编译器不会将对象的成员函数隐式转换成函数指针，所以必须在Foo::print_sum前添加&；

使用对象成员函数的指针时，必须要知道该指针属于哪个对象，因此第二个参数为对象的地址 &foo；

```C++
// Bind_std_function.cpp : 定义控制台应用程序的入口点。
#include "stdafx.h"
#include <iostream>
#include <functional>
#include <random>
#include <memory>
//学习bind的用法
void f(int n1, int n2, int n3, const int & n4, int n5)
{
  std::cout << n1 << ' ' << n2 << ' ' << n3 << ' ' << n4 << ' ' << n5 << "\n";
}
int g(int n1)
{
  return n1 + 100;
}
struct Foo {
  Foo() = default;
  Foo(const Foo & a)
  {
    data = a.data;
    std::cout << "复制构造" << std::endl;
  }
  void print_sum(int n1, int n2)
  {
    std::cout << n1 + n2 << '\n';
  }
  int data = 10;
};
```

 

```c++
//////////////////////////////////////////////////////////////////////////
//std::bind的不同的placeholders个数证明调用函数体时需要传入的参数量及位置
//std::bind的时候目标的函数的参数的顺序与bind的时候的顺序的一一对应的
//////////////////////////////////////////////////////////////////////////
int _tmain(int argc, _TCHAR* argv[])
{
  int n = 7;
  auto f1 = std::bind(f, std::placeholders::_2, std::placeholders::_1, 43, std::cref(n), n);
  //第一位置 目标函数（f）的第一个参数 是调用时传的第二个参数
  //第二位置 目标函数（f）的第二个参数 是调用时传的第一个参数
  //第三位置 目标函数（f）的第三个参数 是43
  //第四位置 目标函数（f）的第四个参数 是n的const ref传递
  //第五位置 目标函数（f）的第五个参数 是n
  n = 10;
  f1(1, 2);//相当于f(2,1,43,10,7);

  using namespace std::placeholders;
  auto f2 = std::bind(f, _3, std::bind(g, _3), _3, 4, 5);
  //第一位置 目标函数f的第一个参数 是调用时传的第三个参数
  //第二位置 目标函数f的第二个参数 是调用时传的g(第三个参数)
  //第三位置 目标函数f的第三个参数 是调用时传的第三个参数
  //第四位置 目标函数f的第四个参数 是4
  //第五位置 目标函数f的第五个参数 是5
  //由此可见，调用时的第一个参数和第二个参数是没有用的。调用时你把第一个或第二个参数传多少都是没有用的
  f2(1000, 2000, 55);//f(55,g(55),55,4,5);
 
  // common use case: binding a RNG with a distribution
  std::default_random_engine e;
  std::uniform_int_distribution<> d(0, 10);
  std::cout << d(e) << std::endl;//生成一个随机数

  std::function<int()> rnd = std::bind(d, e);//rnd就相当于d(e)
  for (int n = 0; n < 10; ++n)
    std::cout << rnd() << ' ';
  std::cout << '\n';

  //绑定类成员函数用对象的指针
  Foo foo;
  auto f3 = std::bind(&Foo::print_sum, &foo, 95, _1);
  f3(5);

  // 绑定类成员变量
  std::cout << "测试绑定类成员" << std::endl;
  auto f4 = std::bind(&Foo::data, _1);
  std::cout << f4(foo) << '\n';
  //std::cout << f4(&foo) << '\n';//尝试传入类对象指针编译不通过//vs2017通过
  std::cout << f4(std::cref(foo)) << '\n';//引用包装传递
 
  //测试发现vs2013不支持Foo的智能指针做为f4的参数
  system("pause");
  return 0;
}
```

 

**3.3 绑定一个引用参数**

默认情况下，bind的那些不是占位符的参数被拷贝到bind返回的可调用对象中。但是，与lambda类似，有时对有些绑定的参数希望以引用的方式传递，或是要绑定参数的类型无法拷贝。

```c++
#include <iostream>
#include <functional>
#include <vector>
#include <algorithm>
#include <sstream>
using namespace std::placeholders;
using namespace std;

ostream & print(ostream &os, const string& s, char c)
{
  os << s << c;
  return os;
}
int main()
{
  vector<string> words{"helo", "world", "this", "is", "C++11"};
  ostringstream os;
  char c = ' ';
  for_each(words.begin(), words.end(), 
          [&os, c](const string & s){os << s << c;} );
  cout << os.str() << endl;
 
  ostringstream os1;
  // ostream不能拷贝，若希望传递给bind一个对象，
  // 而不拷贝它，就必须使用标准库提供的ref函数
  for_each(words.begin(), words.end(),
          bind(print, ref(os1), _1, c));
  cout << os1.str() << endl;
}
```

 

**4. 指向成员函数的指针**

通过下面的例子，熟悉一下指向成员函数的指针的定义方法。

```C++
#include <iostream>
struct Foo {
  int value;
  void f() { std::cout << "f(" << this->value << ")\n"; }
  void g() { std::cout << "g(" << this->value << ")\n"; }
};
void apply(Foo* foo1, Foo* foo2, void (Foo::*fun)()) {
  (foo1->*fun)(); // call fun on the object foo1
  (foo2->*fun)(); // call fun on the object foo2
}
int main() {
  Foo foo1{1};
  Foo foo2{2};
  apply(&foo1, &foo2, &Foo::f);
  apply(&foo1, &foo2, &Foo::g);
}
```

成员函数指针的定义：`void (Foo::*fun)()`，调用是传递的实参: `&Foo::f`；

fun为类成员函数指针，所以调用是要通过解引用的方式获取成员函数*fun,即`(foo1->*fun)()`;



**右值引用** 实现移动构造（拷贝构造函数传入右值引用，利用左值引用可以接受匿名变量，左值，右值等），避免深度拷贝

```C++
int& b = 1; //编译错误! 1是右值，不能够使用左值引用
int&& a = 1; //实质上就是将不具名(匿名)变量取了个别名
```

左值和右值的区分标准在于能否获取地址。

最早的c++中，左值的定义表示的是可以获取地址的表达式，它能出现在赋值语句的左边，对该表达式进行赋值。但是修饰符const的出现使得可以声明如下的标识符，它可以取得地址，但是没办法对其进行赋值：

`const int& i = 10;`

右值表示无法获取地址的对象，有常量值、函数返回值、Lambda表达式等。无法获取地址，但不表示其不可改变，当定义了右值的右值引用时就可以更改右值：

右值引用关联到右值时，右值被存储到特定位置，右值引用指向该特定位置，也就是说，右值虽然无法获取地址，但是右值引用是可以获取地址的，该地址表示临时对象的存储位置。语法如下：

```C++
int && iii = 10;
const int & i = 10; // 与右值引用一样 

int&& iii = 10;

int& ii = iii;   //ii等于10，对ii的改变同样会作用到iii

Int &&iii = move(ii) // 移动语义， 此后对ii 除了销毁和复制，不能做任何其他操作
```

 

第一个问题就是临时对象非必要的昂贵的拷贝操作，

第二个问题是在模板函数中如何按照参数的实际类型进行转发。

通过引入右值引用，很好的解决了这两个问题，改进了程序性能

 

在C++11中所有的值必属于左值、将亡值、纯右值三者之一。比如，非引用返回的临时变量、运算表达式产生的临时变量、原始字面量和lambda表达式等都是纯右值。而**将亡值**是C++11新增的、与右值引用相关的表达式，比如，将要被移动的对象、T&&函数返回值、std::move返回值和转换为T&&的类型的转换函数的返回值等

```C++
T&& k = getVar(); //产生的临时变量将与k一样生命周期
```

通过右值引用的声明，右值又“重获新生”，其生命周期与右值引用类型变量的生命周期一样长，只要该变量还活着，该右值临时量将会一直存活下去

 

`T&& t`在发生自动类型推断的时候，它是未定的引用类型（universal references），如果被一个左值初始化，它就是一个左值；如果它被一个右值初始化，它就是一个右值，它是左值还是右值取决于它的初始化。

 

**引用折叠**

所有的右值引用叠加到右值引用上仍然还是一个右值引用；

所有的其他引用类型之间的叠加都将变成左值引用。

 

一个带有堆内存的类，必须提供一个深拷贝拷贝构造函数，因为默认的拷贝构造函数是浅拷贝，会发生“指针悬挂”的问题。如果不提供深拷贝的拷贝构造函数，上面的测试代码将会发生错误（编译选项-fno-elide-constructors），内部的m_ptr将会被删除两次，一次是临时右值析构的时候删除一次，第二次外面构造的a对象释放时删除一次，而这两个对象的m_ptr是同一个指针，这就是所谓的指针悬挂问题

 

```C++
class A
{
public:
  A() :m_ptr(new int(0)){}
  A(const A& a):m_ptr(new int(*a.m_ptr)) //深拷贝的拷贝构造函数
  {
    cout << "copy construct" << endl;
  }
  A(A&& a) :m_ptr(a.m_ptr)
  {
    a.m_ptr = nullptr;
    cout << "move construct" << endl;
  }
  ~A(){ delete m_ptr;}
private:
  int* m_ptr;
};
int main(){
  A a = Get(false); 
}
```

我们提供移动构造函数的同时也会提供一个拷贝构造函数，以防止移动不成功的时候还能拷贝构造，使我们的代码更安全

https://www.cnblogs.com/likaiming/p/9029908.html

　C++11引入了完美转发：在函数模板中，完全依照模板的参数的类型（即保持参数的左值、右值特征），将参数传递给函数模板中调用的另外一个函数。C++11中的std::forward正是做这个事情的，他会按照参数的实际类型进行转发。看下面的例子：



```C++
void processValue(int& a){ cout << "lvalue" << endl; }
void processValue(int&& a){ cout << "rvalue" << endl; }
template <typename T>
void forwardValue(T&& val)
{
  processValue(std::forward<T>(val)); //照参数本来的类型进行转发。
}
void Testdelcl()
{
  int i = 0;
  forwardValue(i); //传入左值 
  forwardValue(0);//传入右值 
}
// 输出：
// lvaue 
// rvalue
```

 

　　右值引用T&&是一个universal references，可以接受左值或者右值，正是这个特性让他适合作为一个参数的路由，然后再通过std::forward按照参数的实际类型去匹配对应的重载函数，最终实现完美转发。

　　我们可以结合完美转发和移动语义来实现一个泛型的工厂函数，这个工厂函数可以创建所有类型的对象。具体实现如下：

```C++
template<typename… Args>
T* Instance(Args&&… args)
{
  return new T(std::forward<Args >(args)…);
}
```

　　这个工厂函数的参数是右值引用类型，内部使用std::forward按照参数的实际类型进行转发，如果参数的实际类型是右值，那么创建的时候会自动匹配移动构造，如果是左值则会匹配拷贝构造。

 **闭包**

闭包函数：声明在一个函数中的函数，叫做闭包函数。

闭包：内部函数总是可以访问其所在的外部函数中声明的参数和变量，即使在其外部函数被返回（寿命终结）了之后。

 **C++11 获取随机数新方式**

```c++
int min,max; //定义上下边界
default_random_engine e; //创建引擎
uniform_int_distribution<unsigned> u(min,max); //创建取值范围
int randNum=u(e); //获取伪随机数
```

 **Atomic**

​    C++11给我们带来的Atomic一系列原子操作类，它们提供的方法能保证具有原子性。这些方法是不可再分的，获取这些变量的值时，永远获得修改前的值或修改后的值，不会获得修改过程中的中间数值。

​     这些类都禁用了拷贝构造函数，原因是原子读和原子写是2个独立原子操作，无法保证2个独立的操作加在一起仍然保证原子性。

​     这些类中，最简单的是atomic_flag（其实和atomic<bool>相似），它只有test_and_set()和clear()方法。其中，test_and_set会检查变量的值是否为false，如果为false则把值改为true。

 

​    除了atomic_flag外，其他类型可以通过atomic<T>获得。atomic<T>提供了常见且容易理解的方法：

store: store是原子写操作

load: load则是对应的原子读操作

exchange: exchange允许2个数值进行交换，并保证整个过程是原子的。

compare_exchange_weak/compare_exchange_strong:

​	compare_exchange_weak和compare_exchange_strong则是著名的CAS（compare and set）。参数会要求在这里传入期待的数值和新的数值。它们对比变量的值和期待的值是否一致，如果是，则替换为用户指定的一个新的数值。如果不是，则将变量的值和期待的值交换。

​    weak版本的CAS允许偶然出乎意料的返回（比如在字段值和期待值一样的时候却返回了false），不过在一些循环算法中，这是可以接受的。通常它比起strong有更高的性能。





https://blog.csdn.net/starlee/article/details/2897970

现在对上面代码中所用到的一些相关背景知识进行一下介绍。

  GCHandle结构提供从非托管内存访问托管对象的方法。

  GCHandle.Alloc方法(Object)为指定的对象分配Normal句柄。它保护对象不被垃圾回收。当不再需要GCHandle时，必须通过Free将其释放。Normal句柄类型表示不透明句柄，这意味着无法通过此句柄解析固定对象的地址。可以使用此类型跟踪对象，并防止它被垃圾回收器回收。当非托管客户端持有对托管对象的唯一引用（从垃圾回收器检测不到该引用）时，此枚举成员很有用。

  上面的代码中，在类CPerson的构造函数中用GCHandle为C#类Person的对象分配一个句柄，并将该句柄转换为void指针存放在成员变量中，以保证这个对象不会被垃圾回收器回收。然后在类CPerson的析构函数中释放这个句柄，将C#类Person的对象的回收权交给系统。

  Marshal类提供了一个方法集，这些方法用于分配非托管内存、复制非托管内存块、将托管类型转换为非托管类型，此外还提供了在与非托管代码交互时使用的其他杂项方法。

  Marshal..::.StringToHGlobalUni方法向非托管内存复制托管String的内容。StringToHGlobalUni对于自定义封送处理或者在混合托管和非托管代码时很有用。由于该方法分配字符串所需的非托管内存，因此应始终通过调用FreeHGlobal释放内存。

  （更多关于上面介绍的背景知识可以搜索MSDN。说实在的MSDN真是一个宝库！VS能在Windows平台开发中取得绝大多数的份额，除了因为它和Windows都是微软开发的之外，MSDN的完备性功不可没！）

  通过上面的方法，就把一个C#编写的类Person用托管C++给封装成了一个C++可以使用的CPerson类。我们可以在C++的工程中像使用一般的C++类一样使用类CPerson。比如下面的代码