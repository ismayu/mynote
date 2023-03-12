###### 多线程创建

​	代码在不同线程的上下文中运行，需要使用`ThreadStart`或者`ParameterizedThreadStart`类型的一个委托来表示要执行的代码，给定用`ThreadStart`委托构造器创建的一个Thread实例，可调用`thread.Start`来启动线程。调用`Thread.Start()`s是告诉操作系统开始并发的执行一个新线程，然后主线程中的控制立即返回，来执行`Main()`方法中的for循环。两个线程独立运行，不会等待对方直到调用`Join()`。

```c#
using System;
using System.Thread;
class ThreadTest
{
    public const int Rep = 1000;
    public static void ThreadStartFun()
    {
        ThreadStart threadStart = DoWork;
        Thread thread = new Thread(threadStart);
        thread.Start();
        for(int count = 0; count < Rep; count++)
        {
            Console.Write('-');
        }
        thread.Join();
    }
    public static void DoWork()
    {
        for(int count = 0; count < Rep; count++)
        {
            Console.Write('+');
        }
    }
}
```

###### 线程管理

- `Join()`.调用Join()使一个线程等待另一个线程。它告诉操作系统暂停执行当前线程，直到另一个线程终止。`Join()`重载版本允许获取int或者`TimeSpan`作为参数，指定最多等待thread执行多长时间。
- `IsBackGround`，新线程默认为“前台”线程，操作系统将在进程的所有前台线程完成后终止进程，可将`thread.IsBackGroud`属性设为true，这样后台线程仍在运行，操作系统也允许进程终止(最好显示退出每个线程)。
- `Priority`，每个线程关联了优先级，可将`Priority`属性设为新的`ThreadPriority`枚举值，从而增大或减少线程的优先级。
- `ThreadState`，如果想知道一个线程是否还“活着”，还是已经完成了所有工作，可使用布尔属性`IsAlive`，更全面的线程状态可通过`ThreadState`属性访问。

###### 线程池处理

```c#
using System;
using System.Threading;
public class Program
{
	public const int Rep = 1000;
	public static void Main()
	{
		ThreadPool.QueueUserWorkItem(DoWork, '+');
		for(int count = 0; cout < Rep; count++)
		{
			Console.Write('-');
		}
		Thread.Sleep(1000);
	}
	public static void DoWork(object state)
	{
		for(int count = 0; count < Rep; count++)
		{
			Console.Write(state);
		}
	}
}
```







收入10000

7-7情况

10000-700=9300

9300+500=9800

你最中到手9800，公积金账户 自己700+公司700=1400



正常公司12-12情况

10000-1200=8800

你最终到手8800，公积金账户 自己1200+公司1200=2400



盛趣12-12

10000-1200=8800

8800-500=8300

你最终到手8300，公积金账户 自己1200+500+公司700=2400







