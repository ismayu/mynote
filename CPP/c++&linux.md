##### **C++**

C 标准库的`errno.h` 头文件定义了整数变量 errno，它是通过系统调用设置的，在错误事件中的某些库函数表明了什么发生了错误。该宏扩展为类型为 int 的可更改的左值，因此它可以被一个程序读取和修改。在程序启动时，errno 设置为零

###### C/C++内存分配管理：

`C++`中的`new`只是对`malloc`进行了一层封装，`malloc`的具体实现可以看`glibc`的`malloc`源码，然后调用system call，最终会接触操作系统的内存管理模块，所以最终还是需要了解操作系统的堆管理（简单堆管理参阅《深入理解计算机系统》第九章：虚拟存储器，`linux`中内存管理具体实现可以阅读`linux`源码），需要特别了解内存池的实现（可以阅读`sgi stl`源码）还有伙伴系统与`slab`...

###### C++反射：

工厂模式以及C++17了解一下

###### C++异常机制：

网上有很多讲C++异常机制的博客

###### C++智能指针：

可以按照接口要求自己实现一下四个简单的智能指针，其中`share_ptr`里的析构器其实是一个仿函数，然后可以自己想办法去实现一下function，any之类的模板类

###### C++四种类型转换：

[http://www.cplusplus.com/doc/tutorial/typecasting](https://link.jianshu.com?t=http%3A%2F%2Fwww.cplusplus.com%2Fdoc%2Ftutorial%2Ftypecasting)

###### C++ string实现原理：

其实实现与vector差不多，具体实现参阅sgi stl源码

###### C++ map、set实现原理：

封装了一颗红黑树，红黑树重要的就是旋转，还有平衡度（根据黑高证明）,具体实现参阅sgi stl源码

###### C++函数重载、覆盖、隐藏：

重载可以从汇编代码去看（根据参数类型去重命名函数名），覆盖可以去从虚函数表去分析，隐藏可以从作用域去理解

###### C++编译时多态与运行时多态：

类的多态（运行时多态）一定要看看《深度探索C++对象模型》这本书，stack overflow上有一个帖子深度讨论了类的多态、虚继承这些，讲到了构造析构过程中vptr的变化，然后可以自己去适当理解为什么虚函数的具体调用依赖于构造析构的当前进度（链接：https://stackoverflow.com/questions/6258559/what-is-the-vtt-for-a-class](https://link.jianshu.com?t=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F6258559%2Fwhat-is-the-vtt-for-a-class). ）。注意，函数模板不能局部特例化，不然就是模板重载，不得不多说一句，函数模板实例化后的函数与普通函数不在同一命名空间中（不是C++语言支持的namespace，是编译器所用的命名空间），所以能够出现具有相同名字相同参数类型的函数，实际上，从汇编代码去看，其实最终的名字还是不同的

###### C++为什么构造函数不能设置为虚函数：

vptr还没有被设置，会出现先有鸡还是先有蛋的矛盾

###### 动态链接库与静态链接库的区别

##### **设计模式**

###### 单例模式（static函数可以实现）

###### 策略模式（举例：share_ptr的析构器）

###### 简单工厂、工厂方法、抽象工厂模式

###### 装饰模式（闭包：C++ lambda、Python decorator了解一下）

###### 代理模式（举例：iterator有点代理模式的意思）

###### 原型模式（举例：实现boost库中的any时需要用到的clone方法）

###### 模板方法模式（《Effective C++》条款35：

考虑virtual函数以外的其他选择 有介绍，但是举的例子感觉不是很好，感觉最大的突出点是事前和事后，之后看了《大话设计模式》对模板方法的介绍，感觉它的最大特点应该是实现最大化代码复用）

###### 适配器模式（举例：STL中的容器适配器）

###### 迭代器模式（举例：iterator）

##### **算法**

《剑指offer》里面的算法最好能全部手写出来，一般面试的手撕算法几乎都来源于这本书

大数据处理：大数据top 100啊之类的问题很常见

大整数计算：转为字符串计算或者很强的可以自己用位运算实现一下...

如何设计好的散列函数

动态规划：了解一下动态规划基本概念以及有哪些常见算法

贪心算法：了解一下贪心算法基本概念以及有哪些常见算法

排序算法：快速排序、堆排序、归并排序、插值排序、冒泡排序、选择排序、计数排序（了解一下）、基数排序（了解一下），除了两个需要了解的，其他六个都需要快速写出来，而且知道它们的平均、最坏时间、空间复杂度

搜索算法：折半查找（一定要能快速写出来，还有其变种），二叉树查找，hash查找

数据结构-堆

数据结构-链表：数据结构-跳表、算法：二叉搜索树转为双向链表、链表回环、反转、复杂链表的复制...

数据结构-树：数据结构-AVL树、红黑树、B+树，算法：二叉树的前中后序遍历，还有《剑指offer》上的树算法

数据结构-图：dfs、bfs、dijkstra、floyd、prim、kruskal都要能够写出来

字符串算法：kmp、boyer-moore都要能够写出来、正则算法了解一下

手撕算法：手撕算法的代码量一般都不是很大，所以应该去权衡一下什么算法更容易被考到什么不容易被考到。除了《剑指offer》，还有很多跟程序鲁棒性和性能有关的手撕代码题：strcpy，实现单例模式...，还有一些题来源于leetcode，但是几乎都是easy或者medium程度的题

##### **操作系统/Linux 内核**

中断：介绍中断的作用，要是再能从硬件、汇编级别去介绍一下中断会发生什么就更棒了

信号：几乎所有讲操作系统的书都有介绍信号，linux信号参阅《Unix环境高级编程》第10章：信号，以及《深入理解linux内核》第11章：信号，需要了解可靠信号与不可靠信号的区别，还需要特别了解SIGCHLD、SIGCLD信号

进程与线程：轻量级进程、系统级线程、用户级线程、进程，这些可以读linux内核源码以及一些资料很容易理解，协程（Python中yield的效果以及它的具体实现（C源码）了解一下），可以去网上找找C++的协程库去读一读

linux常见的调度算法：知道这些算法的思想就行，读个linux源码就更棒

linux文件系统：《深入理解linux内核》第12章：虚拟文件系统 以及 第18章：Ext2和Ext3文件系统 以及 第16章：访问文件，可以让你深入了解linux文件系统

linux IPC：System V、Posix IPC熟悉一下，可以参阅《Unix环境高级编程 进程间通信》，以及清楚它们的作用与优缺点，当然还是推荐去阅读一下IPC的linux内核源码

如何实现linux的高并发，线程池的思想：github上有好多好多线程库，抓一个下来自己读一读，代码量不是很大，很推荐自己动手写一写

死锁产生的四个必要条件及如何预防

编写linux的并发测试程序：生产者消费者并发程序要求能够写得出来

惊群效应：举例子讲一下惊群现象，然后去了解一下如何避免。内核中存在两个惊群的例子，accept惊群与epoll惊群，linux内核经过改进避免了一些惊群，可以从内核源码去解释一下它是怎么做的

fork、vfork、clone的区别：这个真的只能读源码才能深入了解了，注意，phtread线程库就是使用的clone

僵尸进程：清楚一下概念以及它的危害，然后知道如何去避免僵尸进程

select、poll、epoll的区别：从内核源码去分析，select与poll实现差不多，读了一个源码差不多很快就能读懂第二个，epoll设计很独特也很有意思，赶快去读一读

linux内核伙伴系统、slab缓存（实现原理、与普通内存分配的区别以及优势）：简单介绍参阅《深入理解linux内核》第8章：内存管理，深入了解就去读linux内核源码...

**so shared object library**

```c++
#include <dlfcn.h>
void *dlopen(const char *filename, int flag);
其中flag有：RTLD_LAZY RTLD_NOW RTLD_GLOBAL，其含义分别为：
RTLD_LAZY:在dlopen返回前，对于动态库中存在的未定义的变量(如外部变量extern，也可以是函数)不执行解析，就是不解析这个变量的地址。
RTLD_NOW：与上面不同，他需要在dlopen返回前，解析出每个未定义变量的地址，如果解析不出来，在dlopen会返回NULL，错误为：
  : undefined symbol: xxxx......
RTLD_GLOBAL:它的含义是使得库中的解析的定义变量在随后的其它的链接库中变得可以使用。

char *dlerror(void);
void *dlsym(void *handle, const char *symbol);
int dlclose(void *handle);
handle = dlopen(DLL_FILE_NAME, RTLD_NOW);
func = dlsym(handle, "add");
dlclose(handle);
```

##### **导出函数**

linux下源文件中的所有函数都有一个默认的visibility属性，默认为public，即默认导出。如果要隐藏，则在GCC编译指令中加入 -fvisibility=hidden 参数，会将默认的public属性变为hidden。

隐藏函数导出后，所有的导出都隐藏，再在源码中，在需要导出的函数前添加 __attribute__ ((visibility(“default”))) ，使其仍按默认的public属性处理。

##### **so符号表**

两个符号表：“.symtab”和“.dynsym”。

.symtab：

包含大量的信息（包括全局符号global symbols）

.dynsym：

只保留“.symtab”中的全局符号

故“.dynsym”可看作“.symtab”的子集。故命令strip会去掉elf文件中“.symtab”，但不会去掉“.dynsym”。

##### **strip**

这和ELF有关，ELF文件包含allocable/non-allocable ELF section。

allocable：

ELF包含一些sections（如code/data）是在运行时需要的，这些section被称为allocable。

non-allocable：

其他一些sections仅仅是linker/debuger等工具需要但运行时不需要，被称为non-allocable。

当Linker构建ELF文件时，把allocable/non-allocable分开存放，当OS加载ELF时，仅仅allocable数据被映射到内存，non-allocable的数据仍静静地呆在文件中不被处理。所以strip命令的作用就是移除non-allocable sections。

查看符号表

未被strip的so库：

nm libbinder.so即可（默认查看.symtab符号表）。

被strip的so库：

由于.symtab符号表被移出，需要加上-D参数，如nm -Do libbinder.so。否则使用nm时提示no symbol。

**放置位置 & load/preload:**

共享库一般放在一些约定的目录下, 如/usr/lib/, /usr/local/lib, /lib/等. 这其实是遵循FHS的, 比如/usr/local/lib下放置的一般是用户开发的库.

在启动程序时, program loader(ld-linux.so.x)会找到并加载程序需要的共享库, loader查找的路径一般就是上述的几个目录, 这些目录在/etc/ld.so.conf文件中配置.

如果只想覆盖共享库的某几个函数, 保持其余函数不变, 则可以将共享库名字和函数名字输入到/etc/ld.so.preload中, 这里面定义的规则会覆盖标准规则.

 

**cache arrangement & ldconfig**

实际上, 在启动程序时再去搜寻所需的共享库不是高效做法, 所以loader使用了cache. ldconfig的作用就是读取文件/etc/ld.so.conf, 在各个库目录中, 对共享库设置合适的symbolic link(使得遵守命名约定), 然后写入某种数据到/etc/ld.so.cache, 这个文件再今后就被其他程序使用, 从而大幅提升了共享库的查找速度.

所以在每加入/移除一个共享库, 或者修改了/etc/ld.so.conf(即修改库目录)的时候, 最要运行ldconfig.

**elf**

elf的内容参考"elf & libelf, elftoolchain", 它是一种格式，也是一种规范, 可以用libelf写程序去操作它, 可以用objdump、nm和readelf去读取elf文件的内容.

**ltrace, ldd**

可以用来帮助确认某程序和某些共享库的关联关系是否正确.

ldd -- can print shared library dependencies

readelf -- print information of ELF file, it support both binary and shared library.

-s --symbols is used to display symbols in ELF

 

 

 