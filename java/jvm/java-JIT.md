#  Just in Time  即时编译器
虚拟机的核心

## 概览
* 编译型语言
* 解释型语言 
可移植性，翻译重复

> 热点编译
jvm执行代码不会立即编译代码 1 如果代码只执行一次 那编译就是浪费体力 解释执行java字节码比先编译然后执行的速度快
2 jvm执行特定方法次数越多 使得jvm可以在编译代码时进行大量优化

## 调优入门 选择编译器类型(client、server或者二者同用)
* client 
c1
* server 
c2 

两者的区别在于编译代码时机的不同，client编译器开启编译比server编译器要早，意味着在代码执行的开始阶段，client编译器要比server编译器
要快，因为它的编译代码相比server编译器而言要多
为什么？  
代码先由client编译，随着代码预热，由server端编译器重新编译    

### 优化启动

### 优化批处理
分层编译是批处理任务合理的默认选择
### 优化长时间运行的应用
应该一直使用server编译器，最好配合分层编译

## java和jit编译器的版本
* 32 client  -client
* 63 server  -server
* 64 server  -d64

## 编译器中期调优
当代码缓存的大小超过限制之后，jvm就不能编译更多代码了，问题就会出现应用的大部分代码都是解释运行，这个问题在client以及分层编译实现
很常见，jvm就会发出警告， CodeCache is full,默认 32m-240m区间。目前没有什么机制能好的算出具体的缓存使用大小，基本都是增加1倍或者3倍

### 调优代码缓存
1. 代码缓存是一种有最大值的资源，它会影响jvm可运行的的编译代码总量
2. 分层编译很容易达到上限，应该监控代码缓存，必要时增加它的大小
#### 编译阀值
首先明白一点，编译是基于两种jvm计数器
1. 方法调用计数器
2. 方法中的循环回编计数器，可以看作循环完成执行的次数
> 循环完成执行
包括到达循环的末尾，也包括执行了像continue的分支语句
* 标准编译
* 栈上编译 osr  on stack replacement
jvm不等方法调用完成就会编译循环，如何循环的回边计数器超过阀值，那整个循环就可以被编译
结论:  
1. 当方法和循环执行次数达到某个阀值的时候，就会发生编译
2. 改变阀值会导致代码提早或推后编译
3. 由于计数器会随着时机而减少，以至于 温热的方法可能永远都达不到编译的阀值 特别是对于servcer编译器来说

#### 检测编译过程
代码编译的状态 attributes
* % 编译为osr
* s 方法是同步的
* !:方法有异常处理器
* b: 阻塞模式时发生的编译
* n: 为封装本地方法所发生的编译
可以使用jstat检测编译

## 高级编译器优化 

### 编译线程

### 内联

### 逃逸分析

## 逆优化
* mode not entrant
* made zombie
### 代码被丢弃
### 逆优化僵尸代码    
结论:    
1. 逆优化会使得编译器回到之前版本的编译代码
2. 先前的优化不再生效，才会发生逆优化
3. 代码逆优化时，会对性能产生小而短暂的影响，不过新编译的代码会尽快的热身
4. 分层编译时，如果之前代码由client编译器编译，而现在由server编译器优化，就会发生逆优化

## 分层编译级别
1. 0: 解释代码
2. 1:简单c1编译代码
3. 2:受限的c1编译代码
4. 3：完全c1编译代码
5. 4：c2编译代码
多数方法第一次编译级别是3    
结论：  
1. 分层编译可以在两种编译器和5种级别之间进行
2. 不建议人为更改级别
















