# jvm优化

## 调优参数 
JVM 主要接受两类标志：布尔标志和附带参数的标志    
布尔标志: 
   -XX:+FlagName 表示开启， -XX:-FlagName 表示关闭    
附带参数的标志:
   -XX:FlagName=something
   
## 性能测试方法
### 原则1 测试真实应用
#### 微基准测试 
用来测量微小代码单元的性能
1. 调用同步方法的用时与调用异步方法的用时比较
2. 创建线程的代价与使用线程池的代价
3. 执行某种算法的耗时与替代实现的耗时
主意事项:
* 必须使用被测的结果
* 不要包括无关的操作
* 必须输入合理的参数
#### 宏基准测试

#### 介基准测试

### 原则2 理解批处理流逝时间、吞吐量和响应时间

#### 批处理流逝时间

热身

#### 吞吐量测试
* TPS 每秒实务数
* RPS 每秒请求数
* OPS 每秒处理数

#### 响应时间测试

### 原则3 用统计方法应对性能的变化

### 原则4 尽早频繁测试

## java 性能调优工具箱

### 操作系统的工具和分析

#### cpu 使用率
* 用户态时间
* 系统态时间
目的:
尽可能的让cpu使用率高   
注意cpu空闲的原因：
1. 应用被原语阻塞，直到锁释放才能继续执行
2. 应用在等待某些东西，例如数据库调用的响应
3. 应用的确无所事事
#### java与单cpu的使用率
* 应用类型（批处理）
* 接收请求
#### java与单cpu的使用率
通过不阻塞线程来提高cpu使用率

#### cpu 运行队列
运行队列反应的是机器上所有东西的运行情况

#### 磁盘使用率

#### 网络使用率


### java监控工具
* jcmd
它用来打印 Java 进程所涉及的基本类线程和VM信息    
jcmd process_id command optional_arguments    
* jconsole
提供jvm活动的图形化视图，包括线程的使用、类是使用和GC活动
* jhat
读取内存堆转储，有助于分析，事后使用的工具
* jmap
堆转储和其他jvm内存使用的信息
* jinfo
jvm的系统属性
* jstack
转储java进程的栈信息
* jstat
GC和类装载的活动信息
* jvisualvm
#### 基本VM的信息
* 运行时间
* 系统属性
* jvm版本
* jvm命令行
* jvm调优标志

#### 线程信息
* jstack
* jcmd process_id thread.print

#### 类信息
* jstat
* jconsole
#### 实时GC分析
#### 时候堆转储

### 性能分析工具
两种模式: 数据采样和数据探查
#### 采样分析器
#### 探查分析器
侵入性强、影响性能    
限制:   
小代码区域中的一些类和包
#### 阻塞方法和线程时间线
#### 本地分析器

### java 任务控制 jmc

#### java飞行记录器 jfr

#### jfr事件概览
* classloading
1. 装载和卸载的类的数量
2. 哪个类装载器装载的类，装载单个类需要的时间
* thread statistics
1. 创建和销毁的线程数
2. 被锁阻塞的线程（以及阻塞住它们的特定的锁）
* throwables
1. 应用所用的异常错误类
2. 有多少异常和错误被抛出了，以及他们生成时的栈跟踪信息
* tlab allocation
1. 堆分配的数量和本地线程分配缓冲的大小
2. 堆中分配的对象和分配它们的栈跟踪信息
* file and socket I/O
1. 执行io所用的时间
2. 每次读/写调用所调用的时间，读或写很长时间的特定文件或socket
* Monitor blocked
1. 等待管程的线程
2. 在特定管程上阻塞的线程以及它们阻塞的时间
* Code cache
1. 代码缓存的大小以及包含多少量
2. 从代码缓存中移走的java方法，代码缓存配置
* Code compilation
1. 哪个方法被编译，osr编译和编译的时长
2. 除了jfr其他工具也包括的信息，但这是从多个源头获取的统一信息
* Garbage collection
1. GC的时间 包括单个阶段，代的大小
2. 除了jfr其他工具也包括的信息，但这是从多个工具获取的统一信息
* Profiling
1. 探查和采样分析器
2. 不像真的性能分析器那样有很多信息，但jfr分析器提供了很好的更高层次的概要信息

#### 开启jfr
 -XX:+UnlockCommercialFeatures -XX:+flightRecorder  或jmc 两种方式
 1. 固定持续时间 
  适用于响应性分析
 2. 持续进行












