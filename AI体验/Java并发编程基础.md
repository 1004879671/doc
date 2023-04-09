## 1. Java并发编程基础

线程的概念

- 线程是程序执行的最小单位，一个进程可以包含多个线程。
- 线程可以并行执行，提高程序运行效率。
- 线程之间可以共享进程的资源，如内存、文件等。
- Java中的线程是通过Thread类来实现的。

线程的创建

- 使用Thread类创建线程
  
  ```java
  Thread thread = new Thread(() -> {
      // 线程执行的代码
  });
  thread.start();
  ```

- 实现Runnable接口创建线程
  
  ```java
  Runnable runnable = () -> {
      // 线程执行的代码
  };
  Thread thread = new Thread(runnable);
  thread.start();
  ```

- 实现Callable接口创建线程
  
  ```java
  Callable<String> callable = () -> {
      // 线程执行的代码
      return "result";
  };
  FutureTask<String> futureTask = new FutureTask<>(callable);
  Thread thread = new Thread(futureTask);
  thread.start();
  ```

- 使用线程池创建线程
  
  ```java
  ExecutorService executorService = Executors.newFixedThreadPool(10);
  executorService.execute(() -> {
      // 线程执行的代码
  });
  executorService.shutdown();
  ```

线程的状态转换

- 线程的状态转换示例：
  
  ```
  Thread thread = new Thread(() -> {
      System.out.println("线程开始执行");
      try {
          Thread.sleep(1000);
      } catch (InterruptedException e) {
          e.printStackTrace();
      }
      System.out.println("线程执行完毕");
  });
  
  System.out.println("线程状态：" + thread.getState()); // NEW
  
  thread.start();
  System.out.println("线程状态：" + thread.getState()); // RUNNABLE
  
  Thread.sleep(500);
  System.out.println("线程状态：" + thread.getState()); // TIMED_WAITING
  
  Thread.sleep(1000);
  System.out.println("线程状态：" + thread.getState()); // TERMINATED
  ```

线程的同步与互斥- 使用synchronized关键字实现同步：

```java
public synchronized void synchronizedMethod() {
    // 同步代码块
}
```

- 使用Lock接口实现同步：
  
  ```java
  Lock lock = new ReentrantLock();
  public void lockMethod() {
      lock.lock();
      try {
          // 同步代码块
      } finally {
          lock.unlock();
      }
  }
  ```

- 使用volatile关键字实现变量的可见性：
  
  ```java
  public class VolatileExample {
      private volatile boolean flag = false;
      public void write() {
          flag = true;
      }
      public void read() {
          while (!flag) {
              // 循环等待
          }
      }
  }
  ```

## 2. Java并发编程高级特性

线程池的概念

- 线程池的概念

Java中的线程池是一种线程使用模式，它可以优化线程的创建和销毁过程，提高程序的性能。线程池中包含一组线程，这些线程可以重复使用，线程的创建和销毁由线程池来管理。线程池会维护一个任务队列，当有新的任务时，线程池会从任务队列中取出一个任务并将其分配给空闲的线程来执行。线程池还可以控制并发线程数量，以避免过多的线程竞争资源导致程序性能下降。以下是线程池的一些常用参数：

    - corePoolSize：核心线程数，线程池中一直存在的线程数量。
    - maximumPoolSize：最大线程数，线程池中最多能存在的线程数量。
    - keepAliveTime：线程的存活时间，当线程池中的线程数量超过核心线程数时，多余的线程会在指定时间内被销毁。
    - workQueue：任务队列，用于存放等待执行的任务。
    - threadFactory：线程工厂，用于创建新的线程。
    - handler：拒绝策略，当任务队列已满且线程池中的线程数达到最大线程数时，用于处理新的任务。常用的拒绝策略有AbortPolicy、CallerRunsPolicy、DiscardOldestPolicy和DiscardPolicy。

线程池的实现与使用

- 线程池的实现与使用
  
  - 示例代码：
    
    ```java
    // 创建一个线程池，最大线程数为10，空闲线程存活时间为60秒
    ThreadPoolExecutor executor = new ThreadPoolExecutor(5, 10, 60, TimeUnit.SECONDS, new LinkedBlockingQueue<>());
    
    // 提交任务到线程池中
    executor.submit(() -> {
        // 执行任务的代码
    });
    
    // 关闭线程池
    executor.shutdown();
    ```

锁的概念

- 互斥锁
  
  ```java
  public class Mutex {
      private final static Lock lock = new ReentrantLock();
  
      public static void main(String[] args) {
          lock.lock();
          try {
              // do something
          } finally {
              lock.unlock();
          }
      }
  }
  ```

- 读写锁
  
  ```java
  public class ReadWriteLockDemo {
      private final static ReentrantReadWriteLock readWriteLock = new ReentrantReadWriteLock();
  
      public static void main(String[] args) {
          new Thread(() -> read(), "读线程1").start();
          new Thread(() -> read(), "读线程2").start();
          new Thread(() -> write(), "写线程1").start();
          new Thread(() -> write(), "写线程2").start();
      }
  
      public static void read() {
          readWriteLock.readLock().lock();
          try {
              // do something
          } finally {
              readWriteLock.readLock().unlock();
          }
      }
  
      public static void write() {
          readWriteLock.writeLock().lock();
          try {
              // do something
          } finally {
              readWriteLock.writeLock().unlock();
          }
      }
  }
  ```

- 重入锁
  
  ```java
  public class ReentrantLockDemo {
      private final static ReentrantLock lock = new ReentrantLock();
  
      public static void main(String[] args) {
          new Thread(() -> test(), "线程1").start();
          new Thread(() -> test(), "线程2").start();
      }
  
      public static void test() {
          lock.lock();
          try {
              // do something
          } finally {
              lock.unlock();
          }
      }
  }
  ```

- 信号量
  
  ```java
  public class SemaphoreDemo {
      private final static Semaphore semaphore = new Semaphore(3);
  
      public static void main(String[] args) {
          new Thread(() -> test(), "线程1").start();
          new Thread(() -> test(), "线程2").start();
          new Thread(() -> test(), "线程3").start();
          new Thread(() -> test(), "线程4").start();
      }
  
      public static void test() {
          try {
              semaphore.acquire();
              // do something
          } catch (InterruptedException e) {
              e.printStackTrace();
          } finally {
              semaphore.release();
          }
      }
  }
  ```

锁的种类

- synchronized关键字
  
  ```java
  public synchronized void method() {
      // 代码块
  }
  ```

- ReentrantLock
  
  ```java
  private ReentrantLock lock = new ReentrantLock();
  
  public void method() {
      lock.lock();
      try {
          // 代码块
      } finally {
          lock.unlock();
      }
  }
  ```

- ReadWriteLock
  
  ```java
  private ReadWriteLock lock = new ReentrantReadWriteLock();
  
  public void readMethod() {
      lock.readLock().lock();
      try {
          // 读操作代码块
      } finally {
          lock.readLock().unlock();
      }
  }
  
  public void writeMethod() {
      lock.writeLock().lock();
      try {
          // 写操作代码块
      } finally {
          lock.writeLock().unlock();
      }
  }
  ```

- StampedLock
  
  ```java
  private StampedLock lock = new StampedLock();
  
  public void readMethod() {
      long stamp = lock.tryOptimisticRead();
      // 读操作代码块
      if (!lock.validate(stamp)) {
          stamp = lock.readLock();
          try {
              // 读操作代码块
          } finally {
              lock.unlockRead(stamp);
          }
      }
  }
  
  public void writeMethod() {
      long stamp = lock.writeLock();
      try {
          // 写操作代码块
      } finally {
          lock.unlockWrite(stamp);
      }
  }
  ```

- Condition
  
  ```java
  private Lock lock = new ReentrantLock();
  private Condition condition = lock.newCondition();
  
  public void method() {
      lock.lock();
      try {
          while (condition不满足) {
              condition.await();
          }
          // 代码块
      } finally {
          lock.unlock();
      }
  }
  
  public void otherMethod() {
      lock.lock();
      try {
          // 修改condition
          condition.signalAll();
      } finally {
          lock.unlock();
      }
  }
  ```

锁的使用场景- synchronized关键字的使用场景：

```java
class Counter {
    private int count = 0;
    public synchronized void increment() {
        count++;
    }
    public synchronized int getCount() {
        return count;
    }
}
```

- ReentrantLock的使用场景：
  
  ```java
  class Counter {
      private int count = 0;
      private ReentrantLock lock = new ReentrantLock();
      public void increment() {
          lock.lock();
          try {
              count++;
          } finally {
              lock.unlock();
          }
      }
      public int getCount() {
          lock.lock();
          try {
              return count;
          } finally {
              lock.unlock();
          }
      }
  }
  ```

- ReadWriteLock的使用场景：
  
  ```java
  class Counter {
      private int count = 0;
      private ReadWriteLock lock = new ReentrantReadWriteLock();
      public void increment() {
          lock.writeLock().lock();
          try {
              count++;
          } finally {
              lock.writeLock().unlock();
          }
      }
      public int getCount() {
          lock.readLock().lock();
          try {
              return count;
          } finally {
              lock.readLock().unlock();
          }
      }
  }
  ```

- StampedLock的使用场景：
  
  ```java
  class Point {
      private double x, y;
      private final StampedLock sl = new StampedLock();
      void move(double deltaX, double deltaY) {
          long stamp = sl.writeLock();
          try {
              x += deltaX;
              y += deltaY;
          } finally {
              sl.unlockWrite(stamp);
          }
      }
      double distanceFromOrigin() {
          long stamp = sl.tryOptimisticRead();
          double currentX = x, currentY = y;
          if (!sl.validate(stamp)) {
              stamp = sl.readLock();
              try {
                  currentX = x;
                  currentY = y;
              } finally {
                  sl.unlockRead(stamp);
              }
          }
          return Math.sqrt(currentX * currentX + currentY * currentY);
      }
  }
  ```

## 3. Java并发编程实战

多线程的安全问题

- 多线程的安全问题
  
  - 示例1：线程安全的单例模式实现
    
    ```java
    public class Singleton {
        private static volatile Singleton instance;
        private Singleton() {}
        public static Singleton getInstance() {
            if (instance == null) {
                synchronized(Singleton.class) {
                    if (instance == null) {
                        instance = new Singleton();
                    }
                }
            }
            return instance;
        }
    }
    ```
  
  - 示例2：使用synchronized关键字同步方法实现线程安全
    
    ```java
    public class Counter {
        private int count;
        public synchronized void increment() {
            count++;
        }
        public synchronized int getCount() {
            return count;
        }
    }
    ```
  
  - 示例3：使用ReentrantLock类实现线程安全
    
    ```java
    public class Counter {
        private int count;
        private final Lock lock = new ReentrantLock();
        public void increment() {
            lock.lock();
            try {
                count++;
            } finally {
                lock.unlock();
            }
        }
        public int getCount() {
            lock.lock();
            try {
                return count;
            } finally {
                lock.unlock();
            }
        }
    }
    ```

多线程的性能优化

- 使用线程池来避免不必要的线程创建和销毁，提高线程的重用率。
- 使用ThreadLocal来避免多线程之间的变量共享，提高程序的性能。
- 使用volatile关键字来保证多线程之间变量的可见性，避免出现脏读的情况。
- 使用synchronized关键字来保证多线程之间的同步，避免出现竞态条件。
- 使用Lock接口来提供更加灵活的锁定机制，避免出现死锁的情况。
- 使用Atomic包提供的原子操作类来保证多线程之间的原子性，避免出现数据不一致的情况。
- 使用Concurrent包提供的高效并发容器来提高程序的并发性能。
- 使用Fork/Join框架来实现任务的并行执行，提高程序的并发性能。
- 使用并发编程的最佳实践来避免出现线程安全问题，提高程序的可靠性和稳定性。

表格：

| 多线程的性能优化     | 示例                                                                                                                                 |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| 线程池          | ExecutorService executorService = Executors.newFixedThreadPool(10);                                                                |
| ThreadLocal  | private static final ThreadLocal<SimpleDateFormat> dateFormat = ThreadLocal.withInitial(() -> new SimpleDateFormat("yyyy-MM-dd")); |
| volatile     | private volatile boolean flag = false;                                                                                             |
| synchronized | public synchronized void addCount() { count++; }                                                                                   |
| Lock         | private Lock lock = new ReentrantLock(); lock.lock(); try { // do something } finally { lock.unlock(); }                           |
| Atomic       | private AtomicInteger count = new AtomicInteger(0); count.incrementAndGet();                                                       |
| Concurrent   | ConcurrentMap<String, String> map = new ConcurrentHashMap<>(); map.put("key", "value");                                            |
| Fork/Join    | ForkJoinPool forkJoinPool = new ForkJoinPool(); forkJoinPool.invoke(new MyRecursiveAction());                                      |
| 最佳实践         | 避免使用共享变量、避免锁的粒度过大或过小、避免死锁、避免饥饿和活锁等。                                                                                                |

多线程的应用场景

- 实时性要求较高的场景：比如视频直播、在线游戏等。
- 资源共享的场景：比如数据库连接池、线程池等。
- 大数据处理的场景：比如数据分析、数据挖掘等。
- 并发编程框架的使用场景：比如Hadoop、Spark等。
- 表格：

| 场景          | 示例            |
| ----------- | ------------- |
| 实时性要求较高的场景  | 视频直播、在线游戏等    |
| 资源共享的场景     | 数据库连接池、线程池等   |
| 大数据处理的场景    | 数据分析、数据挖掘等    |
| 并发编程框架的使用场景 | Hadoop、Spark等 |

并发编程的最佳实践- 使用线程池来管理线程，避免频繁创建和销毁线程，提高效率。

- 使用volatile关键字来保证变量的可见性，避免出现线程安全问题。
- 使用synchronized关键字或者Lock接口来保证线程安全，避免出现多线程竞争问题。
- 使用Atomic包提供的原子操作类来保证线程安全，提高效率。
- 使用Concurrent包提供的并发容器来实现线程安全的集合操作，提高效率。
- 使用CountDownLatch、CyclicBarrier等同步工具来协调多个线程的执行，提高效率。
- 使用ThreadLocal来实现线程本地变量，避免出现线程安全问题。
- 避免使用过多的锁，避免出现死锁等问题。
- 尽量使用不可变对象或者线程安全的对象，避免出现线程安全问题。
- 对于IO操作等阻塞操作，可以使用异步IO或者NIO来提高效率。
