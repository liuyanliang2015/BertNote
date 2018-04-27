# JVM内存模型
![JVM内存模型](https://github.com/liuyanliang2015/BertNote/blob/master/pics/JVM.png)

## 程序计数器
记录正在执行的虚拟机字节码指令的地址
## 虚拟机栈
每个 Java 方法在执行的同时会创建一个栈帧用于存储局部变量表、操作数栈、常量池引用等信息。每一个方法从调用直至执行完成的过程，就对应着一个栈帧在 Java 虚拟机栈中入栈和出栈的过程。
## 本地方法栈
本地方法不是用 Java 实现，对待这些方法需要特别处理。
## 堆
所有对象实例都在这里分配内存，是垃圾收集的主要区域（"GC 堆"），现代的垃圾收集器基本都是采用分代收集算法。
## 方法区
用于存放已被加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。
## 运行时常量池
运行时常量池是方法区的一部分。Class 文件中的常量池（编译器生成的各种字面量和符号引用）会在类加载后被放入这个区域。
## 直接内存
在 JDK 1.4 中新加入了 NIO 类，它可以使用 Native 函数库直接分配堆外内存，然后通过一个存储在 Java 堆里的 DirectByteBuffer 对象作为这块内存的引用进行操作。这样能在一些场景中显著提高性能，因为避免了在 Java 堆和 Native 堆中来回复制数据。

# JVM案例
![JVM内存模型案例](https://github.com/liuyanliang2015/BertNote/blob/master/pics/JVM-demo.png)

# linux查看内存情况
使用jmap -heap 线程id，查看堆内存配置 <br>
![linux查看堆配置](https://github.com/liuyanliang2015/BertNote/blob/master/pics/jmap-heap.png) <br>

使用jstat -gc 线程id  时间间隔，来查看GC情况 <br>
![linux查看GC情况](https://github.com/liuyanliang2015/BertNote/blob/master/pics/jstat-gc.png)




