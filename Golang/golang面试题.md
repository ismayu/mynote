1.项目中用到的锁
2.介绍一下线程安全的共享内存方式
3.介绍一下goroutine
4.goroutine的自旋占用资源如何解决,gmp
5.介绍Linux系统信号
6.goroutine抢占时机,gc栈扫描
7.Gc触发时机
8.是否了解其他gc机制
9.内存管理方式
10.Channel分配在堆上还是在栈上？哪些对象分配在堆上？哪些对象分配在栈上？
11.代码效率分析，考虑局部性原理
12.多核CPU下，cache如何保持一致，不冲突
13.uint类型溢出
14.聊聊rune类型
15.介绍一下channel，有缓冲和无缓冲的区别
16.channel是否线程安全
17.介绍一下Mutex的实现,是悲观锁还是乐观锁
18.Mutex几种模式?
19.Muxtez可以做自旋锁?
20.介绍一下RWMutex
21.介绍一下大对象和小对象，为什么小对象多了会造成gc压力？
22.介绍项目中遇到的oop情况
23.介绍项目中遇到的坑
24.如果指定指令执行的顺序
25.什么是写屏障、混合写屏障，如何实现？
26.gc的stw是怎么回事
27.协程之间是怎么调度的
28.简单聊聊内存逃逸
29.为sync.WaitGroup中Wait函数支持 WaitTimeout 功能.
30.字符串转成byte数组，会发生内存拷贝吗？
31.http包的内存泄漏
32.Goroutine调度策略
33.对已经关闭的的chan进行读写，会怎么样？为什么？
34.实现阻塞读的并发安全Map
35.什么是goroutine leak？
36.data race问题怎么解决？能不能不加锁解决这个问题？
37.epoll原理
38.etcd怎么实现分布式锁?
39.滑动窗口的概念以及应用?
40.grpc内部原理是什么？
41.http2的特点是什么,与http1.1的对比。
42.time.Now有几次系统调用？如何优化
43.空struct{}是否使用过？会在什么情况下使用，举例说明一下。
44.聊聊runtime
45.介绍下你平时都是怎么调试bug以及性能问题的?
46.通过通信来共享内存，而不是通过共享内存而通信，怎么理解这句话，如何处理共享变量？
47.chan比mutex更轻么？还有更轻量的方法么？
48.什么时候用chan不如mutex效率高？