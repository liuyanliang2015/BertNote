JVM: Java Virtual Machine即 Java虚拟机。

虚拟机：通过软件模拟的具有完整硬件系统功能的，运行在一个完全隔离环境中的完整计算机系统。

有哪些虚拟机：VMWare 、 Visual Box 、 JVM

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



## JVM的常见配置参数

JVM区域总体分两类，heap区和非heap区。 <br>
heap区又分为：  <br>
- Eden Space（伊甸园）、  <br>
- Survivor Space(幸存者区)、  <br>
- Old Gen（老年代）。 <br>

非heap区又分：  <br>
- Code Cache(代码缓存区)；  <br>
- Perm Gen（永久代）；  <br>
- Jvm Stack(java虚拟机栈)；  <br>
- Local Method Statck(本地方法栈)； <br>


### trace跟踪参数

-verbose:gc 

-XX:+printGC

可以打印GC的简要信息。

-XX:+PrintGCDetails
打印GC的详细信息
![GC打印信息](https://github.com/liuyanliang2015/BertNote/blob/master/pics/jvm-printGC.png)

-XX:+PrintGCTimeStamps
打印GC发生的时间戳

    [GC[DefNew: 4416K->0K(4928K), 0.0001897 secs] 4790K->374K(15872K), 0.0002232 secs] 
    [Times: user=0.00 sys=0.00, real=0.00 secs] 

-XX:+PrintHeapAtGC 每一次GC后，都打印堆信息。
![GC打印信息](https://github.com/liuyanliang2015/BertNote/blob/master/pics/jvm-demo4.png)


-XX:+TraceClassLoading 监控类的加载
-

重定向GC log的位置，以文件输出,帮助开发人员跟踪问题：<br>
-Xloggc:log/gc.log

### 案例：

      JAVA_OPTS="-server -Xms2048m -Xmx2048m  -XX:NewSize=512m -XX:MaxNewSize=512m 
      -XX:PermSize=512m  -XX:MaxPermSize=512m -Djava.awt.headless=true 
	  -verbose:gc -XX:+PrintHeapAtGC -Xloggc:/usr/local/tomcat_app/logs/gc.log "

![GC打印信息](https://github.com/liuyanliang2015/BertNote/blob/master/pics/GC-catalina.png)


![GC打印信息](https://github.com/liuyanliang2015/BertNote/blob/master/pics/GC-log.png)







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

### 3：生存还是死亡&垃圾回收器算法
在根搜索算法中，不可达的对象也并非“非死不可”，这时候它们暂时处于“缓刑”阶段，要真正宣告一个对象死亡，至少要经历两次标记过程。

####垃圾回收器算法：<br>
A：标记-清除算法<br>
首先标记出所有需要回收的对象，在标记完成后统一回收掉所有被标记的对象。<br>
两个缺点：一个是效率问题；一个是空间问题。

B：复制算法<br>
将内存按容量划分为大小相等的两块，每次只使用其中一块。当这一块的内存用完了，就将还存活的对象复制到另一块上面，然后再把已使用过的内存空间一次清理掉。<br>
现在的商业虚拟机都是采用这种算法来回收新生代，IBM的专门研究表明，新生代中的对象98%都是朝生夕死的，所以不需要哪找1：1的比例来划分内存空间，而是将内存分为一块较大的Eden空间和两块较小的Survivor空间，每次使用Eden和其中一块Survivor空间。当回收的时候，将Eden区和当前Survivor空间还存活的对象一次性地拷贝到另外一块Survivor空间。最后清理掉Eden和用过的Survivor空间。

C：标记-整理算法<br>
标记过程与标记-清除算法一样，但是后续步骤不是直接对可回收对象进行清理，而是将所有存活的对象都向一端移动，然后直接清理掉端边界以外的内存。

D：分代收集算法<br>
当前商业虚拟机的垃圾回收器都采用“分代收集”算法，根据对象的存活周期的不同将内存划分为几块。一般是把Java堆分成新生代和老年代，这样就可以根据每个年代的特点采用最适当的收集算法。<br>
在新生代，每次垃圾回收都会发现有大批对象死去，只有少量存活，那就使用复制算法，只需要付出少量存活对象的复制成本就可以完成收集。而年老代中因为对象存活率高、没有额外的空间对它就行分配，就必须使用“标记-清理”或者“标记-整理”算法来进行回收。


Java虚拟机规范中对垃圾回收器应该如何实现没有规定，因此不同的厂商、不同版本的虚拟机所采用的垃圾回收器算法都有很大的差别。例如：Serial回收器、ParNew回收器、Serial Old回收器、G1收集器等。<br>

这里简单说一下G1回收器：G1回收器是垃圾回收器理论进一步发展的产物。一是，G1收集器采用的是基于“标记-整理”算法实现的。二是它可以非常精确地控制停顿，既能让使用者明确指定在一个长度为M毫秒的时间片段内，消耗在垃圾回收器上的时间不得超过N毫秒。


### Full GC
![heap](https://github.com/liuyanliang2015/BertNote/blob/master/pics/stack-out.png) <br>

从年轻代空间（包括 Eden 和 Survivor 区域）回收内存被称为 Minor GC，对老年代GC称为Major GC,而Full GC是对整个堆来说的，在最近几个版本的JDK里默认包括了对永生带即方法区的回收（JDK8中无永生带了），出现Full GC的时候经常伴随至少一次的Minor GC,但非绝对的。Major GC的速度一般会比Minor GC慢10倍以上。下边看看有那种情况触发JVM进行Full GC及应对策略。

**A:System.gc()方法的调用** <br>
此方法的调用是建议JVM进行Full GC,虽然只是建议而非一定,但很多情况下它会触发 Full GC,从而增加Full GC的频率,也即增加了间歇性停顿的次数。强烈影响系建议能不使用此方法就别使用，让虚拟机自己去管理它的内存，可通过通过-XX:+ DisableExplicitGC来禁止RMI调用System.gc。

**B:老年代空间不足** <br>
老年代空间只有在新生代对象转入及创建为大对象、大数组时才会出现不足的现象，当执行Full GC后空间仍然不足，则抛出如下错误：<br>
java.lang.OutOfMemoryError: Java heap space <br>
为避免以上两种状况引起的Full GC，调优时应尽量做到让对象在Minor GC阶段被回收、让对象在新生代多存活一段时间及不要创建过大的对象及数组。<br>

**C：永生区(jdk1.8已废弃)空间不足**<br>
JVM规范中运行时数据区域中的方法区，在HotSpot虚拟机中又被习惯称为永生代或者永生区，Permanet Generation中存放的为一些class的信息、常量、静态变量等数据，当系统中要加载的类、反射的类和调用的方法较多时，Permanet Generation可能会被占满，在未配置为采用CMS GC的情况下也会执行Full GC。如果经过Full GC仍然回收不了，那么JVM会抛出如下错误信息：<br>
java.lang.OutOfMemoryError: PermGen space <br>
为避免Perm Gen占满造成Full GC现象，可采用的方法为增大Perm Gen空间或转为使用CMS GC。<br>

**D：堆中分配很大的对象**<br>
所谓大对象，是指需要大量连续内存空间的java对象，例如很长的数组，此种对象会直接进入老年代，而老年代虽然有很大的剩余空间，但是无法找到足够大的连续空间来分配给当前对象，此种情况就会触发JVM进行Full GC。


## 类生命周期
![类生命周期](https://github.com/liuyanliang2015/BertNote/blob/master/pics/class-load.png)

有且只有下列五种情况必须对类进行初始化（加载、验证、准备都会随着发生）：

A：遇到 new、getstatic、putstatic、invokestatic 这四条字节码指令时，如果类没有进行过初始化，则必须先触发其初始化。最常见的生成这 4 条指令的场景是：使用 new 关键字实例化对象的时候；读取或设置一个类的静态字段（被 final 修饰、已在编译期把结果放入常量池的静态字段除外）的时候；以及调用一个类的静态方法的时候。

B：使用 java.lang.reflect 包的方法对类进行反射调用的时候，如果类没有进行初始化，则需要先触发其初始化。

C：当初始化一个类的时候，如果发现其父类还没有进行过初始化，则需要先触发其父类的初始化。

D：当虚拟机启动时，用户需要指定一个要执行的主类（包含 main() 方法的那个类），虚拟机会先初始化这个主类；

E：当使用 JDK.7 的动态语言支持时，如果一个 java.lang.invoke.MethodHandle 实例最后的解析结果为 REF_getStatic, REF_putStatic, REF_invokeStatic 的方法句柄，并且这个方法句柄所对应的类没有进行过初始化，则需要先触发其初始化；

### 1：加载

加载是类加载的一个阶段，加载过程完成三件事：<br>
通过一个类的全限定名来获取定义此类的二进制字节流。<br>
将这个字节流所代表的静态存储结构转化为方法区的运行时存储结构。<br>
在内存中生成一个代表这个类的 Class 对象，作为方法区这个类的各种数据的访问入口。<br>

### 2:验证
确保 Class 文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身的安全：

A：文件格式验证
验证字节流是否符合 Class 文件格式的规范，并且能被当前版本的虚拟机处理。

B：元数据验证
对字节码描述的信息进行语义分析，以保证其描述的信息符合 Java 语言规范的要求。

C：字节码验证
通过数据流和控制流分析，确保程序语义是合法、符合逻辑的。

D：符号引用验证
发生在虚拟机将符号引用转换为直接引用的时候，对类自身以外（常量池中的各种符号引用）的信息进行匹配性校验。

### 3：准备

类变量是被 static 修饰的变量，准备阶段为类变量分配内存并设置初始值，使用的是方法区的内存。

实例变量不会在这阶段分配内存，它将会在对象实例化时随着对象一起分配在 Java 堆中。

初始值一般为 0 值，例如下面的类变量 value 被初始化为 0 而不是 123
    
    public static int value = 123;

如果类变量是常量，那么会按照表达式来进行初始化，而不是赋值为 0。

    public static final int value = 123;

### 4：解析

将常量池的符号引用替换为直接引用的过程。

### 5：初始化

初始化阶段才真正开始执行类中的定义的 Java 程序代码。初始化阶段即虚拟机执行类构造器 <clinit>() 方法的过程。


## 类加载器分类

A：启动类加载器（Bootstrap ClassLoader）<br>
此类加载器负责将存放在 <JAVA_HOME>\lib 目录中的，或者被 -Xbootclasspath 参数所指定的路径中的，并且是虚拟机识别的（仅按照文件名识别，如 rt.jar，名字不符合的类库即使放在 lib 目录中也不会被加载）类库加载到虚拟机内存中。 启动类加载器无法被 Java 程序直接引用，用户在编写自定义类加载器时，如果需要把加载请求委派给启动类加载器，直接使用 null 代替即可

B:扩展类加载器（Extension ClassLoader）<br>
这个类加载器是由 ExtClassLoader（sun.misc.Launcher$ExtClassLoader）实现的。它负责将 <JAVA_HOME>/lib/ext 或者被 java.ext.dir 系统变量所指定路径中的所有类库加载到内存中，开发者可以直接使用扩展类加载器。

C:应用程序类加载器（Application ClassLoader）<br>
这个类加载器是由 AppClassLoader（sun.misc.Launcher$AppClassLoader）实现的。由于这个类加载器是 ClassLoader 中的 getSystemClassLoader() 方法的返回值，因此一般称为系统类加载器。它负责加载用户类路径（ClassPath）上所指定的类库，开发者可以直接使用这个类加载器，如果应用程序中没有自定义过自己的类加载器，一般情况下这个就是程序中默认的类加载器。


### 双亲委派机制

![双亲委派模型](https://github.com/liuyanliang2015/BertNote/blob/master/pics/class-load2.png)

#### 双亲委派机制原理：<br>
如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载，而是把这个请求委派给父类加载器，每一个层次的加载器都是如此，依次递归。因此所有的加载请求最终都应该传送到顶层的启动类加载器中，只有当父加载器反馈自己无法完成此加载请求（它搜索范围中没有找到所需类）时，子加载器才会尝试自己加载。


#### 双亲委派机制的优点：<br>
每一个类都只会被加载一次，避免了重复加载<br>
每一个类都会被尽可能的加载（从引导类加载器往下，每个加载器都可能会根据优先次序尝试加载它）<br>
有效避免了某些恶意类的加载（比如自定义了Java。lang.Object类，一般而言在双亲委派模型下会加载系统的Object类而不是自定义的Object类）<br>






## 五：虚拟机性能监控与故障处理工具
Java开发人员可能都知道JDK的bin目录下面的“java.exe和Javac.exe”这两个命令行工具，但并非所有程序员都了解过JDK的bin目录下其他的命令行程序的作用。<br>

我们会发现bin下面的程序大小体积基本都稳定在16KB左右(JDK1.7),这是因为这些命令行工具大多数都是jdk\lib\tools.jar类库的一层包装而已，它们的主要功能代码是在tools类库中实现的。<bin>

当程序部署到生产环境后，无论是直接接触物理服务器还是远程telnet到服务器都是受限制的，借助tools.jar类库中的接口，我们可以直接在应用程序中实现功能强大的监控分析功能。

### jps虚拟机进程状况工具

    [root@localhost ~]# jps -l
	26814 org.apache.catalina.startup.Bootstrap
	1349 org.apache.catalina.startup.Bootstrap
	8901 com.aliyun.tianji.cloudmonitor.Application
	14503 sun.tools.jps.Jps

功能类似：ps <br>
![ps命令](https://github.com/liuyanliang2015/BertNote/blob/master/pics/ps.png)

jps -q :只输出进程id <br>
jps -m :输出虚拟机进程启动时，传递给主类main()函数的参数 <br>
jps -l :输出主类的全名，如果进程执行的是jar包，输出jar路径 <br>
jps -v :输出虚拟机进程启动时的JVM参数

### jstat虚拟机统计信息监视工具
用于监视虚拟机各种运行状态信息的命令行工具。它可以显示本地或者远程虚拟机进程中的类装载、内存、垃圾回收集、JIT编译等等运行数据。<br>
命令格式： jstat option vmid interval count

    jstat -gc 15943 3000 10


每隔3000毫秒查询一次进程15943垃圾回收集的情况，一共查询10次。<br>
![jstat命令行工具](https://github.com/liuyanliang2015/BertNote/blob/master/pics/jstat.png)

option表示用户希望查询的虚拟机信息，主要由3类：类加载、垃圾回收集、运行期编译。监视选项有很多，列举一些：<br>

-class  :监视类装载、卸载数量、总空间以及装载所消耗的时间。<br>
-gc     :监视java堆状况，包括eden区，2个survivor区、老年代、永久代等容量，已用空间、GC时间等。<br>
-gcnew  :监视java新生代GC的状况。<br>
-gcold :监视java老年代的GC状况。<br>
-compiler:监视JIT编译器编译过的方法、耗时等信息。<br>

### jmap内存映像工具
jmap命令用于生成堆转存储快照(dump文件，可以用eclipse插件打开并分析)，还可以查询finalize执行队列，Java堆和永久代的详细信息，如空间使用率、当前用的是哪种收集器等等。<br>

jmap -dump :生成java堆转存储快照
jmap -heap :显示java堆详细信息，使用哪种收集器，参数配置，分代状况等等。

![jmap命令行工具](https://github.com/liuyanliang2015/BertNote/blob/master/pics/jmap-heap1.png)<br>
![jmap命令行工具](https://github.com/liuyanliang2015/BertNote/blob/master/pics/jmap-heap2.png)

### jconsole：java监视与管理控制台
在命令行执行jconsole，就可以打开控制台：<br>
![jconsole命令行工具](https://github.com/liuyanliang2015/BertNote/blob/master/pics/jconsole.png)<br>
![jconsole命令行工具](https://github.com/liuyanliang2015/BertNote/blob/master/pics/jconsole2.png)<br>
![jconsole命令行工具](https://github.com/liuyanliang2015/BertNote/blob/master/pics/jconsole3.png)<br>



## 线程安全的实现方法
A：不可变<br>
在java语言里，不可变的对象一定是线程安全的，这种安全性是最简单最纯粹的，例如final。


B：互斥同步<br>
Java里面，最基本的互斥同步手段就是synchronized关键字。<br>
我们还可以使用java.util.concurrent包中的重入锁(ReentrantLock)来实现同步。




