# 垃圾收集入门

## 主流垃圾回收收集器
* Serial收集器 
* Throughput(Parallel)收集器
* Concurrent(CMS)收集器
* G1收集器
cms 与g1 称为concurrent垃圾收集器，属于低停顿收集器，当然也有代价就是消耗更多的cpu

## 概述
垃圾收集主要分两部 1 查找不再使用的对象 2 以及如何释放这些对象的内存
### 分代垃圾收集器
* 新生代
1. eden
2. survivor   
   form 、to 
* 老年代
当新生代的内存填满时会发生minor gc,这种方式的好处有两个1新生代仅仅是堆的一部分，与处理整个堆相比，处理新生代的速度更快
意味着应用线程停顿的时间也越短(stw),但是停顿的次数会变多。2 源于新生代中对象的分配方式，对象分配到eden空间　垃圾收集时
eden空间的对象要么移走要么被回收,所有存活的对象要么被移动survivor空间要么被移动到老年代，这回自动产生了一次压缩整理，最终
老年代也会不断的变大，然后开始回收老年代，full gc产生，通常会导致应用长时间的停顿
考虑垃圾收集器 主要考虑单个请求与批量应用进行考量
### GC算法
1. Serial垃圾收集器
2. Throughput垃圾收集器
3. CMS收集器
4. G1垃圾收集器
### 选择GC算法
耗时、吞吐量、平均响应时间
* GC算法及批量任务
* GC算法和吞吐量测试
* GC算法及响应时间测试
* CMS收集器和G1收集器之间的抉择
## GC调优基础
###  调整堆的大小
堆的大小影响系统停顿的时间    
首个原则就是尽量不要把堆的大小设置大于机器的物理内存    
 虽然可以大于，但是GC会以数个数量级的停顿时间    
两个参数     
* 初始值 -xms
* 最大值 -xmx

###  代空间的调整
新生代过大，导致回收次数变少，full gc可能相应的变多
1. 新生代空间调整
-XX:NewRatio=N 设置新生代与老年代的空间占用比率 默认为2
-XX:NewSize=N  设置新生代初始化的值
-XX:MaxNewSize=N 设置新生代空间的最大大小
-XmnN   快捷方法 new & max    
计算公式:   
Initial Young Gen Size = Initial Heap Size / (1 + NewRatio)     
默认情况下 新生代占初始堆大小的33%
小结:   
1. 整个堆范围内，不同代的大小划分是由新生代所占用的空间控制的
2. 新生代的大小会随着整个堆的大小而增长，但这也是随着整个堆的空间比率波动变化的。

###  永久代与元空间的调整
Permgen 或Permanent Generation,java8 称之为metabace
> 类的元数据    
>永久代和元空间内保存的信息只对编译器或者jvm运行时有用     
永久代或者元空间的大小与程序所使用的类的数量成比率相关

###  控制并发
几乎所有的垃圾收集算法中基本的垃圾回收线程都依据机器上的cpu数目计算得出    
多个jvm运行同一个物理机上时，依据公式计算出的线程数会过高，必须进行减少

###  自适应调整
1. jvm在堆的内部如何调整新生代及老年代的百分比是由自适应调整机制控制的
2. 通常情况下我们应该开启它，因为垃圾回收算法依赖于调整后的代的大小来达到它停顿时间的性能指标
3. 对于已经精细调整过的堆，可以关闭自适应调整以提升性能

## 垃圾回收工具
开启GC日志  -verbose:gc 或者 -XX:+PrintGC    
 -XX:+PrintGCDetails  更详细的    
 -XX:+PrintGCTimeStamps  推荐  
 -XX:+PrintGCDateStamps 精确判断gc操作之间的时间      
 -Xloggc:filename 输出到某个文件   

#  概念
## 不再引用与不再需要
 前者是从虚拟机的角度看对象,后者是从人的角度上看程序
 
## 定义垃圾的标准是什么
根对象与某个对象之间还有引用路径

## java 对象的size byte为单位
java.lang.Object 8  
java.lang.Float 16  
java.lang.Double 16  
java.lang.Integer 16  
java.lang.Long 16  
java.math.BigInteger 56 (*)   
java.lang.BigDecimal 72 (*)   
java.lang.String 2*(Length) + 38 ± 2   
empty java.util.Vector 80  
object reference 4   
float array 4*(Length) + 14 ±    
## obj= null 有没有必要?
如果obj是局部变量那么这样写没有任何价值，因为一旦作用域结束，虚拟机会自动收集




