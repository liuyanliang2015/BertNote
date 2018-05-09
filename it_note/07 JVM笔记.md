## 一：为什么学习JVM？
在绝大多数情况下，提升硬件效能无法等比例地提升程序的性能和并发能力，有时甚至可能对程序的性能没有任何改善。这里面有java虚拟机的原因：为了达到为所有硬件提供一致的虚拟平台的作用，牺牲可一些硬件相关的性能特性。更总要的是人为原因：开发人员如果不了解虚拟机的一些技术特性的运行原理，就无法写出最适合虚拟机运行和可自优化的代码。

## 二：Java语言概述
> 1995年，Oak语言改名为java，并发布了Java1.0版本。从第一个Java版本诞生到现在(2018)已经有23年的历史了。

> JDK从1.5版本开始，在官方的正式文档与宣传资料中已经不再使用类似“JDK 1.5”的名称，只有在程序员内部使用的开发版本号(java -version)才继续沿用1.5 、 1.6 、1.7的版本号。公开版本号则改为JDK5、JDK6、JDK7的命名方式。

>从jdk1.2以来，Java在语法层面上的变化一直很小。但JDK1.5在语法易用性上做出了非常大的改进。自动装箱、泛型、动态注解、枚举、可变长参数、循环遍历等语法特性都是在1.5加入的。


###Java作为一种面向对象的编程语言，有两个显著的特征： <br>
1:Write Once,Run Anywhere！ 编写一次，到处运行，能够非常容易地获得跨平台能力。Java的跨平台特性与Java虚拟机的存在密不可分，可在不同的环境中运行。比如说Windows平台和Linux平台都有相应的JDK，安装好JDK后也就有了Java语言的运行环境。其实Java语言本身与其他的编程语言没有特别大的差异，并不是说Java语言可以跨平台，而是在不同的平台都有可以让Java语言运行的环境而已，所以才有了Java一次编译，到处运行这样的效果。<br>

2:GC垃圾回收机制。Java 通过垃圾收集器（Garbage Collector）回收分配内存，大部分情况下，程序员不需要自己操心内存的分配和回收。


### JDK与JRE
JDK：Java程序设计语言、Java虚拟机、Java API类库，这三部分统称为JDK（Java Development Kit）,是支持Java开发的最小环境。<br>

JRE：Java API类库中的Java SE API子集和Java虚拟机两部分统称为JRE(Java Runtime Environment)，是支持java运行的标准环境。

## 三：Java内存区域与内存溢出异常

>对于Java程序员来说，在虚拟机的自动内存管理机制的帮助下，不再需要为每一个new操作去写配对的delete/free代码，而且不容易出现内存泄漏和内存溢出问题，看起来由虚拟机管理内存一切都很美好。

>不过，也正是因为这样，一旦出现内初泄漏和内存溢出方面的问题，如果不了解虚拟机是怎样管理内存的，那排查错误将会是一项异常艰难的工作。

具体分析过程可以参考：[JVM内存分析](https://github.com/liuyanliang2015/BertNote/blob/master/itNote/02%20JVM%E5%86%85%E5%AD%98%E5%88%86%E6%9E%90.md "JVM内存分析")

### 对象访问

    Object obj = new Object();
“Object obj”这部分语义将会反映到Java栈的本地变量表中，作为一个reference(引用)类型数据出现。访问堆中对象的具体位置的主流方式有：使用句柄和直接指针。<br>

“new Object()”这部分语义将会反应到java堆中，形成一块存储Object类型所有实例数据值的结构化内存，另外Java堆中还必须包含能查找次对象数据(父类、实现的接口、方法等)的地址信息，这些类型数据存储在方法区中。

### 模拟内存溢出
#### 堆溢出

    public class TestOutOfMemory {
	public static void main(String[] args) {
		List<String> list = new ArrayList<String>();
		while(true){
			list.add("abcdefg");
		}
	}

	}


![JVM内存溢出](https://github.com/liuyanliang2015/BertNote/blob/master/pics/heap-out.png)

    Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
	at java.util.Arrays.copyOf(Arrays.java:2245)
	at java.util.Arrays.copyOf(Arrays.java:2219)
	at java.util.ArrayList.grow(ArrayList.java:242)
	at java.util.ArrayList.ensureExplicitCapacity(ArrayList.java:216)
	at java.util.ArrayList.ensureCapacityInternal(ArrayList.java:208)
	at java.util.ArrayList.add(ArrayList.java:440)
	at com.test.TestOutOfMemory.main(TestOutOfMemory.java:10)



#### 栈溢出

    /**
	 * A:使用-Xss参数减少栈内存的容量，结果抛出java.lang.StackOverflowError，异常出现的栈深度相应缩小
	 * B:定义大量的本地变量，增加此方法帧中本地变量表的长度，结果StackOverflowError异常出现的栈深度相应缩小
	 * -Xss256k  stackLength:2488
	 * -Xss128k  stackLength:1005
	 */
	public class TestOutOfMemory2 {
		private int stackLength = 1;
		public void stackLeak(){
			stackLength ++;
			stackLeak();
		}
		public static void main(String[] args) {
			TestOutOfMemory2 t = new TestOutOfMemory2();
			try {
				t.stackLeak();
			} catch (Throwable e) {
				System.out.println("stackLength:"+t.stackLength);
				//e.printStackTrace();
				throw e;
			}
		
	}


![JVM内存溢出](https://github.com/liuyanliang2015/BertNote/blob/master/pics/stack-out.png)


栈溢出抛出StackOverflowError错误，出现此种情况是因为方法运行的时候栈的深度超过了虚拟机容许的最大深度所致。对于栈内存溢出，根据《Java 虚拟机规范》中文版：如果线程请求的栈容量超过栈允许的最大容量的话，Java 虚拟机将抛出一个StackOverflow异常；如果Java虚拟机栈可以动态扩展，并且扩展的动作已经尝试过，但是无法申请到足够的内存去完成扩展，或者在新建立线程的时候没有足够的内存去创建对应的虚拟机栈，那么Java虚拟机将抛出一个OutOfMemory 异常。例子中递归调用了构造函数，故导致StackOverflowError

### 运行时常量池异常

如果要向运行时常量池中添加内容，最简单的做法就是使用String.intern()这个Native方法。该方法的作用是：如果常量池中已经包含一个等于此String对象的字符串，则返回代表池中这个字符串的String对象；否则，将此String对象包含的字符串添加到常量池，并且返回此String对象的引用。

因为常量池在方法区中，我们可以通过-XX:PermSize和-XX:MaxPermSize限制方法区的大小，从而间接限制其中常量池的容量。

## 四：垃圾回收器以及内存分配策略

### 1：如何判断堆中的对象，是否“存活”

**A：引用计数算法**<br>
给对象添加一个引用计数器，每当一个地方引用它的时候，计数器值+1；当引用失效时，计数器值-1；任何时刻计数器都为0的对象就是不可能再使用的。<br>
其实，实际情况没有上面描述的那么简单。这种算法很难解决对象之间相互引用的问题。<br>
**JVM用的不是引用计数器算法。**

**B：根搜索算法**<br>
通过一系列名为“GC Roots”的对象作为起点，从这些节点开始向下搜索，搜索所有走过的路径称为引用链(Reference Chain),当一个对象到GC Roots没有任何引用链相连时，则证明此对象是不可用的。<br>

**在Java语言里，可以作为GC Roots的对象：**<br>
虚拟机栈中引用的对象；<br>
方法区中的类静态属性引用的对象；<br>
方法区中的常量引用对象；<br>
本地方法栈中JNI(Native方法)的引用对象。<br>

### 2：应用的概念
Java1.2之后，Java对应用的概念进行扩充，将引用分为四种：强引用、软引用、弱引用、虚引用。这四种引用强度依次逐渐减弱。<br>

**A：强引用**：代码中普遍存在，类似“Object obj = new Object()”,只要强引用存在，垃圾回收器永远不会回收被引用的对象。

**B：软引用**：一些还有用，但并非必须的对象。对于软引用关联着的对象，在系统将要发生内存溢出异常之前，将会把这些对象列进回收范围之内进行第二次回收。在JDK1.2之后，提供了SoftReference类来实现软引用。


**C：弱引用**：弱引用也是来描述非必须对象的，但是强度比软引用更弱一些，被弱引用关联的对象只能生存到下一次垃圾回收器发生之前。在JDK1.2之后，提供了WeakReference类来实现弱引用。


**D：虚引用**：最弱的一种引用关系。一个对象是否有虚引用的存在，完全不会对其生存时间构成影响，也无法通过虚引用来取得一个对象的实例。为一个对象设置虚引用的唯一目的就是希望在这个对象被收集器回收时，收到一个系统通知。在JDK1.2之后，提供了PhantomReference类来实现虚引用。

### 3：生存还是死亡
在根搜索算法中，不可达的对象也并非“非死不可”，这时候它们暂时处于“缓刑”阶段，要真正宣告一个对象死亡，至少要经历两次标记过程。






