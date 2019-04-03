# Java Thread中join的用法

## thread join

在JDk的API里对于join()方法是：

>join\
\
public final void join() throws InterruptedException Waits for this thread to die. Throws: InterruptedException  - if any thread has interrupted the current thread. The interrupted status of the current thread is cleared when this exception is thrown.

即join()的作用是：“等待该线程终止”，这里需要理解的就是该线程是指的主线程等待子线程的终止。也就是在子线程调用了join()方法后面的代码，只有等到子线程结束了才能执行。

### 简单示例

```Java

class XThread extends Thread {
    public XThread(String threadName) {
        super(threadName);
    }
    public void run() {
         String threadName = Thread.currentThread().getName();
         System.out.println(threadName + " start.");
         try {
             for (int i = 0; i < 5; i++) {
                 System.out.println(threadName + " loop at " + i);
                 Thread.sleep(1000);
             }
             System.out.println(threadName + " end.");
         } catch (Exception e) {
             System.out.println("Exception from " + threadName + ".run");
         }
    }
}

public class TestJoin {

    public static void main(String[] args) {
        String threadName = Thread.currentThread().getName();
        System.out.println(threadName + " start.");
        XThread xt1 = new XThread("xt1");
        XThread xt2 = new XThread("xt2");
        XThread xt3 = new XThread("xt3");
        try {

//            xt1.start();
//            xt1.join();
//
//            xt2.start();
//            xt2.join();
//
//            xt3.start();
//            xt3.join();

            // 执行结果
//            main start.
//            xt1 start.
//            xt1 loop at 0
//            xt1 loop at 1
//            xt1 loop at 2
//            xt1 loop at 3
//            xt1 loop at 4
//            xt1 end.
//            xt2 start.
//            xt2 loop at 0
//            xt2 loop at 1
//            xt2 loop at 2
//            xt2 loop at 3
//            xt2 loop at 4
//            xt2 end.
//            xt3 start.
//            xt3 loop at 0
//            xt3 loop at 1
//            xt3 loop at 2
//            xt3 loop at 3
//            xt3 loop at 4
//            xt3 end.
//            main do something
//            main end!


            xt1.start();
            xt2.start();
            xt3.start();

            xt1.join();
            xt2.join();
            xt3.join();

            // 执行结果
//            main start.
//            xt1 start.
//            xt1 loop at 0
//            xt3 start.
//            xt3 loop at 0
//            xt2 start.
//            xt2 loop at 0
//            xt2 loop at 1
//            xt3 loop at 1
//            xt1 loop at 1
//            xt1 loop at 2
//            xt2 loop at 2
//            xt3 loop at 2
//            xt1 loop at 3
//            xt2 loop at 3
//            xt3 loop at 3
//            xt1 loop at 4
//            xt3 loop at 4
//            xt2 loop at 4
//            xt1 end.
//            xt3 end.
//            xt2 end.
//            main do something
//            main end!

            System.out.println(threadName + " do something");
        } catch (Exception e) {
            System.out.println("Exception from main");
        }
        System.out.println(threadName + " end!");

    }

}

```

因为需要等待t线程执行结束，主线程才会执行t.join()后面的代码，所以下面示例中，xt1线程执行结束后，主线程才会执行xt2.start()，所以出现了xt1,xt2,xt3顺序执行的结果

```Java
//            xt1.start();
//            xt1.join();
//
//            xt2.start();
//            xt2.join();
//
//            xt3.start();
//            xt3.join();
```

下面的代码中，xt1,xt2,xt3启动之后才有的xt1.join()，所以三个线程是并行的。

```Java
            xt1.start();
            xt2.start();
            xt3.start();

            xt1.join();
            xt2.join();
            xt3.join();
```

## 从源码看join()方法

在AThread的run方法里，执行了bt.join();，进入看一下它的JDK源码：

```Java
public final void join() throws InterruptedException {
    join(0L);
}
```

然后进入join(0L)方法：

```Java
public final synchronized void join(long l)
    throws InterruptedException
{
    long l1 = System.currentTimeMillis();
    long l2 = 0L;
    if(l < 0L)
        throw new IllegalArgumentException("timeout value is negative");
    if(l == 0L)
        for(; isAlive(); wait(0L));
    else
        do
        {
            if(!isAlive())
                break;
            long l3 = l - l2;
            if(l3 <= 0L)
                break;
            wait(l3);
            l2 = System.currentTimeMillis() - l1;
        } while(true);
}
```

单纯从代码上看：

- 如果线程被生成了，但还未被启动，isAlive()将返回false，调用它的join()方法是没有作用的。将直接继续向下执行。
- 在TestJoin类中的main方法中，xt1.join()是判断xt1的active状态，如果xt1的isActive()方法返回false，在xt1.join(),这一点就不用阻塞了，可以继续向下进行了。从源码里看，wait方法中有参数，也就是不用唤醒谁，只是不再执行wait，向下继续执行而已。
- 在join()方法中，对于isAlive()和wait()方法的作用对象是个比较让人困惑的问题：

        isAlive()方法的签名是：public final native boolean isAlive()，也就是说isAlive()是判断当前线程的状态，也就是bt的状态。

wait()方法在jdk文档中的解释如下：

>Causes the current thread to wait until another thread invokes the notify() method or the notifyAll() method for this object. In other words, this method behaves exactly as if it simply performs the call wait(0).\
\
The current thread must own this object's monitor. The thread releases ownership of this monitor and waits until another thread notifies threads waiting on this object's monitor to wake up either through a call to the notify method or the notifyAll method. The thread then waits until it can re-obtain ownership of the monitor and resumes execution.

在这里，当前线程指的是main。
