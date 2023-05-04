# JVM常用配置参数

## 1. 内存相关参数

`-Xmx`: 设置JVM最大可用内存

- `-Xmx`: 设置JVM最大可用内存
  
  例如：`java -Xmx1g Main` 表示将JVM最大可用内存设置为1GB。

`-Xms`: 设置JVM初始内存大小

- `-Xms`: 设置JVM初始内存大小
  
  示例：`java -Xms512m -jar myApp.jar`
  
  说明：该命令将设置JVM初始内存大小为512MB，可以根据实际情况调整大小。

`-Xmn`: 设置年轻代大小

- `-Xmn`: 设置年轻代大小
  
  ```
  -Xmn512m
  ```
  
  表示将年轻代大小设置为512MB。

`-XX:NewRatio`: 设置新生代与老年代的比例
-XX:NewRatio: 设置新生代与老年代的比例

例如：-XX:NewRatio=2 表示新生代与老年代的比例为1:2。

`-XX:SurvivorRatio`: 设置Eden区与Survivor区的比例-XX:SurvivorRatio: 设置Eden区与Survivor区的比例

例如，将Eden区和Survivor区的比例设置为8:2：

```
java -XX:SurvivorRatio=8
```

## 2. 垃圾回收相关参数

`-XX:+UseSerialGC`: 使用串行垃圾回收器

`-XX:+UseParallelGC`: 使用并行垃圾回收器

`-XX:+UseConcMarkSweepGC`: 使用CMS垃圾回收器

`-XX:+UseG1GC`: 使用G1垃圾回收器

`-XX:MaxGCPauseMillis`: 设置最大垃圾回收停顿时间
-XX:MaxGCPauseMillis: 设置最大垃圾回收停顿时间

该参数用于设置最大垃圾回收停顿时间，单位为毫秒。当JVM进行垃圾回收时，会停止应用程序的执行，直到垃圾回收完成。这个停顿时间越长，垃圾回收的效果越好，但是也会导致应用程序的响应时间变长。因此，可以通过设置该参数来平衡垃圾回收效果和应用程序的响应时间。例如，可以将该参数设置为500毫秒：

```
java -XX:MaxGCPauseMillis=500 MyApp
```

`-XX:GCTimeRatio`: 设置垃圾回收时间占总时间的比例-XX:GCTimeRatio：设置垃圾回收时间占总时间的比例

实例：

```
-XX:GCTimeRatio=19
```

这个参数的含义是，垃圾回收时间占总时间的比例为1:19，即垃圾回收时间最多占用总时间的5%。这个参数的默认值为99，即垃圾回收时间最多占用总时间的1%。如果你的应用程序需要更高的吞吐量，可以将这个值调低，以减少垃圾回收时间。但是，如果你的应用程序需要更快的响应时间，可以将这个值调高，以增加垃圾回收时间。

## 3. 类加载相关参数

`-XX:+TraceClassLoading`: 跟踪类的加载信息

`-XX:+TraceClassUnloading`: 跟踪类的卸载信息

`-XX:+PrintClassHistogram`: 输出当前JVM中类的统计信息

`-XX:+PrintGCApplicationStoppedTime`: 输出GC暂停时间

`-XX:+PrintGCDetails`: 输出GC详细信息



### CMS：

-Xloggc:d:/gc-cms-%t.log -Xms50M -Xmx50M -XX:MetaspaceSize=256M -XX:MaxMetaspaceSize=256M -XX:+PrintGCDetails -XX:+PrintGCDateStamps    -XX:+PrintGCTimeStamps -XX:+PrintGCCause  -XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=10 -XX:GCLogFileSize=100M   -XX:+UseParNewGC -XX:+UseConcMarkSweepGC   

### G1:

-Xloggc:d:/gc-g1-%t.log -Xms50M -Xmx50M -XX:MetaspaceSize=256M -XX:MaxMetaspaceSize=256M -XX:+PrintGCDetails -XX:+PrintGCDateStamps  
 -XX:+PrintGCTimeStamps -XX:+PrintGCCause  -XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=10 -XX:GCLogFileSize=100M -XX:+UseG1GC 






# 多大的内存适合用G1回收

## 1. G1回收概述

G1回收是什么

- G1回收是什么：
  
  - G1回收是一种垃圾回收器，它是在Java 7中引入的。它的全称是Garbage-First，意为垃圾优先。相比于其他垃圾回收器，G1回收器更适合处理大内存应用程序，因为它可以将大内存分成多个小块进行回收，从而避免了Full GC带来的停顿时间过长的问题。同时，G1回收器还可以根据应用程序的实际情况，动态调整回收策略，以达到更好的性能表现。 
  
  示例使用表格语法输出：
  
  | G1回收是什么                                                                                                                                                                    |
  | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
  | G1回收是一种垃圾回收器，它是在Java 7中引入的。它的全称是Garbage-First，意为垃圾优先。相比于其他垃圾回收器，G1回收器更适合处理大内存应用程序，因为它可以将大内存分成多个小块进行回收，从而避免了Full GC带来的停顿时间过长的问题。同时，G1回收器还可以根据应用程序的实际情况，动态调整回收策略，以达到更好的性能表现。 |

G1回收的优点

- G1回收的优点：
  - 高效：G1回收器采用了多线程和并发的方式，能够在短时间内完成垃圾回收，减少了应用程序的停顿时间，提高了应用程序的响应速度。
  - 可预测性：G1回收器能够根据应用程序的需求来进行垃圾回收，可以在一定程度上预测垃圾回收的时间和停顿时间，避免了应用程序的突发停顿。
  - 空间利用率高：G1回收器能够对内存进行动态划分，将内存分成多个区域，每个区域都可以独立地进行垃圾回收，提高了内存的利用率。
  - 可以处理大内存：G1回收器能够处理大内存，可以处理多达数百GB的内存，适合于大型应用程序的垃圾回收。

G1回收的缺点- G1回收的缺点：

1. G1回收器在回收过程中需要扫描整个堆，因此在处理大型堆时，它的效率会降低。
2. 在进行Full GC时，G1回收器可能会导致较长的停顿时间，从而影响应用程序的响应性能。
3. G1回收器需要更多的CPU和内存资源，因此在较小的应用程序中不一定是最佳选择。

## 2. 内存大小与G1回收

小内存下的G1回收

- 小内存下的G1回收
  
  | 内存大小  | G1回收表现 |
  | ----- | ------ |
  | 1GB   | 表现良好   |
  | 512MB | 出现卡顿   |
  | 256MB | 报错退出   |

中等内存下的G1回收

- 中等内存下的G1回收：
  - 当内存大小为8GB时，可以将G1的最小堆大小设置为4GB，最大堆大小设置为6GB，启用G1回收器进行垃圾回收。
    
    ```
    -Xms4g
    -Xmx6g
    -XX:+UseG1GC
    ```
  - 当内存大小为16GB时，可以将G1的最小堆大小设置为8GB，最大堆大小设置为12GB，启用G1回收器进行垃圾回收。
    
    ```
    -Xms8g
    -Xmx12g
    -XX:+UseG1GC
    ```
  - 当内存大小为32GB时，可以将G1的最小堆大小设置为16GB，最大堆大小设置为24GB，启用G1回收器进行垃圾回收。
    
    ```
    -Xms16g
    -Xmx24g
    -XX:+UseG1GC
    ```

大内存下的G1回收- 在大内存下使用G1回收，可以通过以下参数进行配置：

- `-Xms`: 设置JVM启动时的最小堆内存大小
- `-Xmx`: 设置JVM启动时的最大堆内存大小
- `-XX:G1HeapRegionSize`: 设置G1回收器中每个内存区域的大小，建议根据实际内存大小进行调整
  - 一般来说，当JVM启动时的最大堆内存大小大于等于4GB时，建议使用G1回收器进行垃圾回收。例如：
    
    ```
    java -Xms4g -Xmx8g -XX:G1HeapRegionSize=32m -XX:+UseG1GC Main
    ```
  - 在大内存下使用G1回收，可以获得更好的性能和更短的停顿时间。同时，也需要根据实际情况进行调整，以获得最优的性能表现。

## 3. G1回收的最佳实践

内存大小的选择

- 内存大小的选择：
  - 对于内存较小的应用，建议将G1的最小堆大小设置在4GB以下，以确保GC暂停时间不会太长。
  - 对于内存较大的应用，可以将G1的最小堆大小设置在8GB或更高，以获得更好的性能。
  - 可以通过设置G1的最大堆大小来控制GC的频率和暂停时间。如果需要更短的GC暂停时间，可以将最大堆大小设置得更小，但这可能会导致更频繁的GC。
  - 可以通过观察GC日志和内存使用情况来调整堆大小，以达到最佳的性能和稳定性。使用表格语法展示不同内存大小的建议设置：

| 应用内存大小     | G1最小堆大小 | G1最大堆大小 |
| ---------- | ------- | ------- |
| 小于4GB      | 2GB     | 4GB     |
| 4GB - 16GB | 4GB     | 8GB     |
| 大于16GB     | 8GB     | 16GB或更高 |

G1回收的参数设置

### 3. G1回收的最佳实践

- **G1回收的参数设置**
  
  | 参数名                               | 参数值     | 描述                          |
  | --------------------------------- | ------- | --------------------------- |
  | -XX:G1HeapRegionSize              | 1-32MB  | 设置G1堆区域大小，建议设置为1-8MB        |
  | -XX:MaxGCPauseMillis              | 200-500 | 设置最大垃圾回收停顿时间，建议设置为200-500ms |
  | -XX:G1NewSizePercent              | 5-10    | 设置新生代的大小占比，建议设置为5-10%       |
  | -XX:G1MaxNewSizePercent           | 60-80   | 设置新生代的最大大小占比，建议设置为60-80%    |
  | -XX:G1MixedGCLiveThresholdPercent | 65-85   | 设置混合垃圾回收的阈值，建议设置为65-85%     |
  
  上述参数是G1回收的常用参数，可以根据实际情况进行调整。

G1回收的监控与调优- 监控G1回收的日志信息，包括GC暂停时间、堆的使用情况等，以便及时调整参数。

- 使用可视化工具，如JVisualVM、Mission Control等，监控G1回收的实时情况。
- 调整G1回收的参数，如堆大小、分区数量、并发线程数等，以达到最佳的性能和内存利用率。
- 对于大内存应用，可以考虑使用表格方式来优化G1回收的效率，如将大对象放入单独的分区进行回收。

## 注意：以上大纲仅供参考，具体内容需要根据实际情况进行调整。





# 软引用、弱引用、虚引用java写法

## 1. 软引用

软引用是一种相对强一些的引用类型，可以让对象豁免一些垃圾回收。

在Java中，可以使用SoftReference类来实现软引用。

示例代码：

```java
Object obj = new Object();
SoftReference<Object> softRef = new SoftReference<>(obj);
obj = null; // 取消强引用
// 使用软引用获取对象
Object newObj = softRef.get();

```

## 2. 弱引用

弱引用是一种比软引用弱的引用类型，被弱引用关联的对象只能生存到下一次垃圾回收之前。
在Java中，可以使用WeakReference类来实现弱引用。
示例代码：

```java
Object obj = new Object();
WeakReference weakRef = new WeakReference<>(obj);
obj = null; // 取消强引用
// 使用弱引用获取对象
Object newObj = weakRef.get();
```



## 3. 虚引用

虚引用是最弱的一种引用类型，一个对象是否有虚引用关联，完全不会对其生存时间构成影响，也无法通过虚引用来获取一个对象实例。

在Java中，可以使用PhantomReference类来实现虚引用。

示例代码：

```java
Object obj = new Object();
ReferenceQueue<Object> refQueue = new ReferenceQueue<>();
PhantomReference<Object> phantomRef = new PhantomReference<>(obj, refQueue);
obj = null; // 取消强引用
// 使用虚引用获取对象
Object newObj = phantomRef.get();
```

## 软引用、弱引用使用案例

## 1. 软引用

### 1.1 使用场景

缓存数据

图片缓存

### 1.2 示例

使用SoftReference缓存图片

```java
SoftReference<Bitmap> bitmapRef = new SoftReference<>(bitmap);
```



## 2. 弱引用

### 2.1 使用场景

观察者模式
缓存数据


### 2.2 示例


使用WeakReference实现观察者模式

```java
public class Observer {
 private WeakReference<ObserverListener> listenerRef; 
     public void register(ObserverListener listener) {
         listenerRef = new WeakReference<>(listener);
     } 
     public void notify(String message) {
         if (listenerRef != null && listenerRef.get() != null) {
             listenerRef.get().onNotify(message);
         }
     }
}
```




