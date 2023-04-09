# AQS

## 1. AQS概述

AQS是什么

- AQS是什么
  - AQS全称为AbstractQueuedSynchronizer，是Java并发包中用于实现同步器的基类。
  - 它提供了一种实现阻塞锁和一些其他同步器（如Semaphore、CountDownLatch等）的框架，使用它可以便捷地实现自己的同步器。

AQS的作用

- AQS的作用：
  - AQS（AbstractQueuedSynchronizer）是Java并发包中一个重要的同步工具类，它提供了一种实现阻塞锁和相关同步器（例如Semaphore、CountDownLatch等）的基础框架，为并发编程提供了强大的支持。
  - AQS的主要作用是管理同步状态和线程的排队，它通过内部的双向队列和一个状态变量来实现同步器的状态控制和线程的阻塞唤醒，具有高效、灵活、可扩展等特点。
  - AQS是Java并发包中的核心组件，它的设计和实现对于理解Java并发编程的原理和机制非常重要。在实际开发中，我们可以利用AQS提供的基础框架来实现自定义的同步器，从而满足不同的并发场景需求。

AQS的实现原理- AQS的实现原理

AQS（AbstractQueuedSynchronizer）是Java中用于实现锁和其他同步器的基础框架。它的实现原理是基于一个FIFO（先进先出）的双向队列，其中每个节点代表一个等待线程。AQS维护了一个volatile int state变量，用于表示同步状态，同时提供了一些基本的方法（如acquire、release等）供子类实现。AQS中最核心的方法是acquire和release方法，其中acquire方法会阻塞当前线程，直到获取同步状态成功，而release方法则会释放同步状态，并唤醒等待队列中的下一个线程。AQS的实现原理非常精妙，可以用来实现各种同步器，如ReentrantLock、CountDownLatch等。

## 2. AQS的使用

AQS的常用类

- ReentrantLock：可重入锁，支持公平和非公平两种竞争方式。
- ReentrantReadWriteLock：可重入读写锁，支持公平和非公平两种竞争方式。
- Semaphore：信号量，用于控制同时访问某个资源的线程数量。
- CountDownLatch：倒计时门闩，用于等待某个条件达成后再执行任务。
- CyclicBarrier：循环屏障，用于多个线程间相互等待，直到所有线程都到达某个状态后再执行任务。
- Condition：条件变量，用于实现复杂的线程间协作。

AQS的使用场景

- 实现一个线程安全的计数器：

```java
public class Counter {
    private final ReentrantLock lock = new ReentrantLock();
    private final Condition condition = lock.newCondition();
    private int count = 0;

    public void increment() throws InterruptedException {
        lock.lock();
        try {
            count++;
        } finally {
            lock.unlock();
        }
    }

    public void decrement() throws InterruptedException {
        lock.lock();
        try {
            count--;
            condition.signalAll();
        } finally {
            lock.unlock();
        }
    }

    public void awaitZero() throws InterruptedException {
        lock.lock();
        try {
            while (count != 0) {
                condition.await();
            }
        } finally {
            lock.unlock();
        }
    }
}
```

- 实现一个有界阻塞队列：

```java
public class BoundedBlockingQueue<T> {
    private final ReentrantLock lock = new ReentrantLock();
    private final Condition notFull = lock.newCondition();
    private final Condition notEmpty = lock.newCondition();
    private final int capacity;
    private final Queue<T> queue = new LinkedList<>();

    public BoundedBlockingQueue(int capacity) {
        this.capacity = capacity;
    }

    public void put(T element) throws InterruptedException {
        lock.lock();
        try {
            while (queue.size() == capacity) {
                notFull.await();
            }
            queue.offer(element);
            notEmpty.signalAll();
        } finally {
            lock.unlock();
        }
    }

    public T take() throws InterruptedException {
        lock.lock();
        try {
            while (queue.isEmpty()) {
                notEmpty.await();
            }
            T element = queue.poll();
            notFull.signalAll();
            return element;
        } finally {
            lock.unlock();
        }
    }
}
```

AQS的使用注意事项- AQS的使用注意事项：

- 在使用AQS时，需要注意加锁和解锁的顺序，否则可能会导致死锁。
- AQS的实现类应该尽可能地使用AQS提供的模板方法，而不是直接重写tryAcquire等方法。
- 如果需要使用AQS的共享模式，需要注意共享资源的状态，以及线程的竞争关系。
- 在使用AQS时，应该尽可能地避免使用无限循环等不可控制的行为，以免影响系统的稳定性。
- 在使用AQS时，应该考虑到性能问题，避免过多的锁竞争和线程阻塞，以免影响系统的响应性能。

## 3. AQS的扩展

AQS的扩展方式

- 基于AQS实现可重入锁ReentrantLock：
  
  ```
  ReentrantLock lock = new ReentrantLock();
  lock.lock();
  try {
      // do something
  } finally {
      lock.unlock();
  }
  ```

- 基于AQS实现读写锁ReentrantReadWriteLock：
  
  ```
  ReentrantReadWriteLock lock = new ReentrantReadWriteLock();
  lock.readLock().lock();
  try {
      // read something
  } finally {
      lock.readLock().unlock();
  }
  
  lock.writeLock().lock();
  try {
      // write something
  } finally {
      lock.writeLock().unlock();
  }
  ```

- 基于AQS实现信号量Semaphore：
  
  ```
  Semaphore semaphore = new Semaphore(10);
  semaphore.acquire();
  try {
      // do something
  } finally {
      semaphore.release();
  }
  ```

- 基于AQS实现倒计时器CountDownLatch：
  
  ```
  CountDownLatch latch = new CountDownLatch(3);
  new Thread(() -> {
      // do something
      latch.countDown();
  }).start();
  new Thread(() -> {
      // do something
      latch.countDown();
  }).start();
  new Thread(() -> {
      // do something
      latch.countDown();
  }).start();
  latch.await();
  // do something after all threads finish
  ```

- 基于AQS实现循环栅栏CyclicBarrier：
  
  ```
  CyclicBarrier barrier = new CyclicBarrier(3);
  new Thread(() -> {
      // do something
      barrier.await();
  }).start();
  new Thread(() -> {
      // do something
      barrier.await();
  }).start();
  new Thread(() -> {
      // do something
      barrier.await();
  }).start();
  // do something after all threads reach the barrier
  ```

- 基于AQS实现交替执行的锁Condition：
  
  ```
  Lock lock = new ReentrantLock();
  Condition condition = lock.newCondition();
  new Thread(() -> {
      lock.lock();
      try {
          // do something
          condition.signal();
      } finally {
          lock.unlock();
      }
  }).start();
  lock.lock();
  try {
      condition.await();
      // do something after the signal
  } finally {
      lock.unlock();
  }
  ```

- 基于AQS实现可中断的锁LockSupport：
  
  ```
  Thread thread = new Thread(() -> {
      LockSupport.park();
      // do something after being unparked
  });
  thread.start();
  LockSupport.unpark(thread);
  // do something before the thread is unparked
  ```

AQS扩展的实现原理

- AQS扩展的实现原理：

| 扩展                             | 实现原理                       |
| ------------------------------ | -------------------------- |
| ReentrantLock                  | 基于AQS的独占锁，支持重入             |
| ReentrantReadWriteLock         | 基于AQS的共享锁，支持重入             |
| Semaphore                      | 基于AQS的信号量，支持公平和非公平模式       |
| CountDownLatch                 | 基于AQS的倒计时器                 |
| CyclicBarrier                  | 基于AQS的屏障                   |
| Condition                      | 基于AQS的条件变量                 |
| FutureTask                     | 基于AQS的Future模式实现           |
| AbstractQueuedLongSynchronizer | AQS的64位实现，用于实现AtomicLong等类 |

AQS扩展的使用示例- AQS扩展的使用示例：

| 扩展类型          | 使用示例    |
| ------------- | ------- |
| ReentrantLock | ```java |

                ReentrantLock lock = new ReentrantLock();
                lock.lock();
                try {
                    // 执行业务逻辑
                } finally {
                    lock.unlock();
                }
            ``` |

  | Semaphore | ```java
                Semaphore semaphore = new Semaphore(2);
                try {
                    semaphore.acquire();
                    // 执行业务逻辑
                } finally {
                    semaphore.release();
                }
            ``` |
  | CountDownLatch | ```java
                CountDownLatch countDownLatch = new CountDownLatch(1);
                new Thread(() -> {
                    // 执行业务逻辑
                    countDownLatch.countDown();
                }).start();
                countDownLatch.await();
            ``` |





# 同步容器以及并发容器的使用案例

## 1. 同步容器的使用

### 1.1 ArrayList的同步

使用Collections.synchronizedList方法对ArrayList进行同步

实现线程安全的访问

### 1.2 HashMap的同步

使用Collections.synchronizedMap方法对HashMap进行同步

实现线程安全的访问

## 2. 并发容器的使用

### 2.1 ConcurrentHashMap的使用

实现高效的并发访问

使用ConcurrentHashMap实现线程安全的Map

### 2.2 CopyOnWriteArrayList的使用

实现高效的并发读取

使用CopyOnWriteArrayList实现线程安全的List





# CopyOnWriteArrayList

## 1. 概述

介绍CopyOnWriteArrayList的定义和特点- CopyOnWriteArrayList是Java集合框架中的一种线程安全的List实现。

- 它的特点是在对集合进行修改操作时，会先将原有集合进行复制，然后对复制出来的集合进行修改，完成修改后再将原有集合指向新的集合，这样可以保证对原有集合的读取操作不受修改操作的影响。
- 由于CopyOnWriteArrayList在修改时需要复制集合，因此修改操作的性能较低，适用于读多写少的场景。

## 2. 实现原理

分析CopyOnWriteArrayList的实现原理

- 实现原理
  
  分析CopyOnWriteArrayList的实现原理：
  
  ```java
  public class CopyOnWriteArrayList<E> implements List<E>, RandomAccess, Cloneable, java.io.Serializable {
      // 内部维护的数组
      private transient volatile Object[] array;
      // 构造方法
      public CopyOnWriteArrayList() {
          setArray(new Object[0]);
      }
      // 获取元素方法
      public E get(int index) {
          return (E) get(getArray(), index);
      }
      // 修改元素方法
      public E set(int index, E element) {
          final ReentrantLock lock = this.lock;
          lock.lock();
          try {
              Object[] elements = getArray();
              E oldValue = (E) elements[index];
              if (oldValue != element) {
                  int len = elements.length;
                  Object[] newElements = Arrays.copyOf(elements, len);
                  newElements[index] = element;
                  setArray(newElements);
              }
              return oldValue;
          } finally {
              lock.unlock();
          }
      }
      // 添加元素方法
      public boolean add(E e) {
          final ReentrantLock lock = this.lock;
          lock.lock();
          try {
              Object[] elements = getArray();
              int len = elements.length;
              Object[] newElements = Arrays.copyOf(elements, len + 1);
              newElements[len] = e;
              setArray(newElements);
              return true;
          } finally {
              lock.unlock();
          }
      }
      // 移除元素方法
      public E remove(int index) {
          final ReentrantLock lock = this.lock;
          lock.lock();
          try {
              Object[] elements = getArray();
              int len = elements.length;
              E oldValue = (E) elements[index];
              Object[] newElements = new Object[len - 1];
              System.arraycopy(elements, 0, newElements, 0, index);
              System.arraycopy(elements, index + 1, newElements, index, len - index - 1);
              setArray(newElements);
              return oldValue;
          } finally {
              lock.unlock();
          }
      }
      // 获取数组方法
      final Object[] getArray() {
          return array;
      }
      // 设置数组方法
      final void setArray(Object[] a) {
          array = a;
      }
  }
  ```
  
  CopyOnWriteArrayList的实现原理是，在每次修改操作时，都会创建一个新的数组，并将原数组的元素复制到新数组中，然后修改新数组中的元素。这样就保证了在并发修改时，不会影响到其他线程的读操作，因为读操作仍然是访问的原数组。同时，由于每次修改都会创建新的数组，所以写操作不会影响到其他线程的写操作。这种实现方式虽然会占用更多的内存空间，但是可以保证线程安全。

描述写时复制的过程- 在创建新的CopyOnWriteArrayList时，会先复制原始数组，这样就可以在不影响原始数组的情况下进行修改操作。

- 当进行修改操作时，会先将原始数组复制一份，然后在新的数组上进行修改操作。
- 修改完成后，将新的数组赋值给原始数组，这样就完成了写时复制的过程。

实例：

```java
CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();
list.add("apple");
list.add("banana");
list.add("orange");

// 进行修改操作
list.set(1, "grape");

// 实际上是先复制原始数组，然后在新的数组上进行修改操作
// 修改完成后，将新的数组赋值给原始数组，完成写时复制的过程
```

表格实例：

| 原始数组                          | 新的数组                         |
| ----------------------------- | ---------------------------- |
| ["apple", "banana", "orange"] | ["apple", "grape", "orange"] |

## 3. 适用场景

分析CopyOnWriteArrayList的适用场景

- 适用场景：
  - 当读操作远远多于写操作时，CopyOnWriteArrayList的性能比ArrayList更好。
  - 当需要遍历集合的同时，集合中的元素可能被修改时，使用CopyOnWriteArrayList可以避免ConcurrentModificationException异常。
  - 当需要对集合进行高效的并发读写时，CopyOnWriteArrayList是一个不错的选择。
  - 当集合中的元素数量不是很大时，CopyOnWriteArrayList的性能表现更为优秀。

比较CopyOnWriteArrayList和ArrayList的区别- CopyOnWriteArrayList和ArrayList的区别：

|      | CopyOnWriteArrayList | ArrayList |
| ---- | -------------------- | --------- |
| 读操作  | 非常快                  | 快         |
| 写操作  | 较慢                   | 非常慢       |
| 适用场景 | 读多写少                 | 读写频繁      |

例如，当我们需要在多线程环境下进行读取操作远远多于写入操作时，可以使用CopyOnWriteArrayList来保证线程安全性和读取性能。

## 4. 优缺点

列出CopyOnWriteArrayList的优点和缺点

- 优点：
  - 线程安全：在读取时不需要加锁，写入时使用复制出来的新数组，保证写入操作不会影响到已有的读取操作，从而实现线程安全。
  - 迭代器弱一致性：CopyOnWriteArrayList的迭代器可以保证弱一致性，即迭代器遍历的是创建迭代器时的快照，不会抛出ConcurrentModificationException异常。
  - 适合读多写少的场景：由于写操作需要复制整个数组，所以适合读多写少的场景。
- 缺点：
  - 内存占用：由于每次写入操作都需要复制整个数组，所以会占用更多的内存空间。
  - 写操作的延迟：由于写操作需要复制整个数组，所以写操作的延迟比较大，不适合写入操作比较频繁的场景。

分析CopyOnWriteArrayList的使用场景和限制- 优点：

- 线程安全：CopyOnWriteArrayList是线程安全的，多个线程可以同时读取，而不需要额外的同步。
- 高效读取：由于CopyOnWriteArrayList在读取时不需要加锁，因此读取效率非常高。
- 可以避免ConcurrentModificationException：CopyOnWriteArrayList在写入时会创建新的数组，并在新数组中进行修改，因此不会抛出ConcurrentModificationException异常。

- 缺点：
  
  - 写入效率低：由于CopyOnWriteArrayList在写入时需要创建新的数组，因此写入效率较低。
  - 内存占用大：由于每次写入都需要创建新的数组，因此CopyOnWriteArrayList的内存占用比较大。

分析CopyOnWriteArrayList的使用场景和限制：

CopyOnWriteArrayList适合于读多写少的场景，比如缓存、事件监听器等。但是由于写入效率低、内存占用大，不适合频繁写入的场景。此外，由于CopyOnWriteArrayList在读取时不需要加锁，因此可以在高并发场景下提高读取效率。但是如果需要频繁修改数据，则不建议使用CopyOnWriteArrayList。

## 5. 示例代码

提供使用CopyOnWriteArrayList的示例代码

- 示例代码：

```java
import java.util.concurrent.CopyOnWriteArrayList;

public class Example {
    public static void main(String[] args) {
        CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();
        list.add("apple");
        list.add("banana");
        list.add("orange");

        // 在遍历过程中修改列表
        for (String s : list) {
            System.out.println(s);
            list.add("watermelon");
        }
    }
}
```

- 表格：

| 输出结果   |
| ------ |
| apple  |
| banana |
| orange |

分析示例代码的运行结果和效率- 分析示例代码的运行结果和效率：

```java
import java.util.Iterator;
import java.util.concurrent.CopyOnWriteArrayList;

public class CopyOnWriteArrayListExample {

    public static void main(String[] args) {
        CopyOnWriteArrayList<String> dataList = new CopyOnWriteArrayList<>();
        dataList.add("A");
        dataList.add("B");
        dataList.add("C");
        dataList.add("D");

        Iterator<String> iterator = dataList.iterator();
        while (iterator.hasNext()) {
            String value = iterator.next();
            System.out.println(value);
            if (value.equals("C")) {
                dataList.remove(value);
            }
        }
    }
}
```

上述代码的运行结果为：

```
A
B
C
D
```

可以看到，虽然我们在迭代过程中删除了元素"C"，但是输出的结果中仍然包含了"C"这个元素。这是因为CopyOnWriteArrayList在进行迭代的时候，是对原始数据的一个快照进行迭代的，所以在迭代过程中对原始数据进行修改是不会影响迭代结果的。

但是需要注意的是，由于CopyOnWriteArrayList在进行添加、删除等操作时需要对整个数组进行复制，所以在多线程环境下，可能会影响到系统的性能。因此，在并发量较大的情况下，建议使用其他线程安全的集合类。
