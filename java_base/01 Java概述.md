## Java起源
java之父：詹姆斯·高斯林（James Gosling）,开始在IBM工作，后来转至Sun公司。1990年，与Patrick，Naughton和Mike Sheridan等人合作“绿色计划”，后来发展一套语言叫做“Oak”，后改名为Java。<br>

![Java之父](https://github.com/liuyanliang2015/BertNote/blob/master/pics/java-father.png)

## Java版本
语言版本：<br>
现在市面上见得最多的就是JDK5、JDK6、JDK7、JDK8。<br>
JDK5是一个很重要的版本，出现了很多新特性。

平台版本：<br>
J2SE(Java 2 Platform Standard Edition)标准版<br>
是为开发普通桌面和商务应用程序提供的解决方案,该技术体系是其他两者的基础，可以完成一些桌面应用程序的开发

J2ME(Java 2 Platform Micro Edition)小型版<br>
是为开发电子消费产品和嵌入式设备提供的解决方案

J2EE(Java 2 Platform Enterprise Edition)企业版<br>
是为开发企业环境下的应用程序提供的一套解决方案,该技术体系中包含的技术如 Servlet、Jsp等，主要针对于Web应用程序开发

## Java特点
### A：安全性<br>
java通过所谓的沙箱安全模型保证了其安全性。安全沙箱基本组件：
- 类装载器
- class文件校验器
- 内置JVM的安全特性
- 安全管理器和Java API

[参考文章](https://blog.csdn.net/chdhust/article/details/42343473 "参考文章")

### B：多线程
Java通过继承Thread类或者是implements Runnable接口来实现多线程，并提供多线程之间的同步机制（关键字为synchronized）。

### C：简单性
Java丢弃了C++中很少使用的、很难理解的、令人迷惑的那些特性，如操作符重载、多继承、自动的强制类型转换。<br>
特别地，Java语言不使用指针，而是引用。<br>
并提供了自动的废料回收机制，使得程序员不必为内存管理而担忧。<br>

### D:健壮性
Java的强类型机制、异常处理、垃圾的自动收集等是Java程序健壮性的重要保证。对指针的丢弃是Java的明智选择。Java的安全检查机制使得Java更具健壮性。

### E：解释性
Java程序在Java平台上被编译为字节码格式，然后可以在实现这个Java平台的任何系统中运行。

### F：跨平台
Java可以“Write Once,Run Anywhere”,本质上就是因为将Java代码翻译成Java字节码，不同平台提供了对应的JVM来加载运行java程序。<br>

![Java跨平台](https://github.com/liuyanliang2015/BertNote/blob/master/pics/java-platform.png)

### G：面向对象


## JDK、JRE、JVM的关系

JDK(Java Development Kit) Java开发工具包<br>
JDK提供给开发人员使用，其中包含了开发工具，也包含了JRE，所以安装了JDK，就不用单独安装JRE了。


JRE(Java Runtime Environment) Java运行环境<br>
包括了Java虚拟机和Java所需要的核心类库等，如果想运行一个开发好的java程序，计算机只需要安装JRE即可。


使用JDK开发完成java程序，交给JRE去运行，JVM保证了跨平台特性。JDK > JRE > JVM


**JVM运行原理**：

Java Virtual Machine简称JVM，它是一种规范，是一种计算的仿真。我们知道java语言是跨平台的，而JVM是java跨平台的关键之所在。<br>

JVM上执行java字节码，执行时这些字节码可以解释成具体平台的机器码，因此java拥有“一次编译，处处运行”这一跨平台能力。<br>

![JVM结构](https://github.com/liuyanliang2015/BertNote/blob/master/pics/jvm-struc.png) <br>


![JVM原理](https://github.com/liuyanliang2015/BertNote/blob/master/pics/jvm-info.png)




