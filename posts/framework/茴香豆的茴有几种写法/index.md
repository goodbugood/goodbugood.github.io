# 茴香豆的茴有几种写法


茴香豆的茴的写法有以下几种，分别是茴，回，囘，囬。

## 单例有几种实现方法

8种，有一种懒汉单例使用了 DCL。

单例的指令重排。volatile 可以在单例双重检查中实现可见性和禁止指令重排序，从而保证安全性。

### 静态内部类，线程安全

### 饿汉，线程安全

## 创建一个线程有几种方式

```java
import org.junit.Test;

import java.util.List;
import java.util.concurrent.*;

public class ThreadTest {
    class ThreadA extends Thread {
        public ThreadA(String name) {
            super(name);
        }

        @Override
        public void run() {
            super.run();
            System.out.println("我是继承Thread的线程：" + this.getName());
        }
    }

    class ThreadB implements Runnable {
        @Override
        public void run() {
            System.out.println("我是实现Runnable接口的线程");
        }
    }

    class ThreadD implements Callable<List<Integer>> {
        @Override
        public List<Integer> call() throws Exception {
            System.out.println("我是实现Callable接口的线程");
            return null;
        }
    }

    /**
     * 测试线程的实现方式
     */
    @Test
    public void newThread() {
        // 方式一
        new ThreadA("线程A").start();
        // 方式二
        new Thread(new ThreadB()).start();
        // 方式三：
        Thread threadC = new Thread() {
            @Override
            public void run() {
                super.run();
                System.out.println("我是匿名线程：" + this.getName());
            }
        };
        threadC.setName("线程C");
        threadC.start();
        // 方式四：FutureTask 实现了 Runnable 接口
        ThreadD threadD = new ThreadD();
        FutureTask<List<Integer>> futureTask = new FutureTask<>(threadD);
        new Thread(futureTask).start();
        // 方式五：利用线程池，配置线程创建后的回调对象
        ExecutorService executorService = Executors.newFixedThreadPool(1);
        executorService.submit(new Runnable() {
            @Override
            public void run() {
                System.out.println("我是线程池的 Runnable 实现");
            }
        });
        // 方式六：也是线程池，不过配置的对象实现接口不同
        executorService.submit(new Callable<Object>() {
            @Override
            public Object call() throws Exception {
                System.out.println("我是线程池的 Callable 实现");
                return null;
            }
        });
        System.out.printf("主线程：%s%n", Thread.currentThread().getName());
    }
}
```

执行结果：

```sh
我是继承Thread的线程：线程A
我是实现Runnable接口的线程
我是匿名线程：线程C
我是实现Callable接口的线程
我是线程池的 Runnable 实现
主线程：main
我是线程池的 Callable 实现
```

### 继承java.lang.Thread类

### 实现Runnable接口的java.lang.Runnable#run方法

### Thread的匿名类

## Java有几种内部类

4种，成员，匿名，局部，静态。

## 获取 Class 类实例的方法有几种

3种

```java
/**
 * 获取 Class 类实例有几种方式：3种
 */
@Test
public void severalInstance() {
    // 方式一：类常量
    Class<ClassTest> classTestClass = ClassTest.class;
    // 方式二：getClass() 方法
    Class<? extends ClassTest> aClass = this.getClass();
    Class<?> classTest = null;
    try {
        // 方式三：forName
        classTest = Class.forName("shali.tdl.lang.ClassTest");
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
    // 方式四：类加载器
    ClassLoader classLoader = ClassTest.class.getClassLoader();
    Class<?> aClass1 = null;
    try {
        aClass1 = classLoader.loadClass("shali.tdl.lang.ClassTest");
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
    Assert.assertSame(aClass, classTestClass);
    Assert.assertSame(aClass, classTest);
    Assert.assertSame(aClass, aClass1);
}
```


