- [0. 目录](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/Java/JVM.md#0-%E7%9B%AE%E5%BD%95)
- [1. jvm杂七杂八基础](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/Java/JVM.md#1-jvm%E6%9D%82%E4%B8%83%E6%9D%82%E5%85%AB%E5%9F%BA%E7%A1%80)
- [2. jvm内存+垃圾回收](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/Java/JVM.md#2-jvm%E5%86%85%E5%AD%98%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6)
- [3. class文件结构+类加载机制](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/Java/JVM.md#3-class%E6%96%87%E4%BB%B6%E7%BB%93%E6%9E%84%E7%B1%BB%E5%8A%A0%E8%BD%BD%E6%9C%BA%E5%88%B6/)
- [4. 编译及优化](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/Java/JVM.md#4-%E7%BC%96%E8%AF%91%E5%8F%8A%E4%BC%98%E5%8C%96)


# 0. 目录

![目录](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_jvm/%E7%9B%AE%E5%BD%95.jpg)

# 1. jvm杂七杂八基础

![1](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_jvm/1.jpg)

# 2. jvm内存+垃圾回收
（jvm内存区域划分，hotspot对象，垃圾回收）

![2.1](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_jvm/2.1.jpg)

### Java中 堆 VS 栈 ：  
- 存储内容  

> 堆：存放对象实例及数组  
> 栈：支持虚拟机进行方法调用和方法执行，存储局部变量表、操作数栈、动态连接、方法出口等信息

- 内存管理  

> 堆：由编译器管理  
栈：由GC管理  

- 是否线程私有   

> 堆：线程共享  
栈：线程私有  

- 存取速度  

> 堆：存储速度慢，但可以动态分配内存  
栈：存储速度快，但存在栈中的数据大小和生命期是确定的，缺乏灵活性

- 空间地址增长方向  

> 堆： 向高地址扩展的数据结构，是不连续的内存区域  
栈：栈顶地址和栈的最大容量由系统预先规定，栈是向低地址扩展的数据结构，是一块连续的内存区域

- 内存碎片

> 堆：由空间碎片  
栈：无空间碎片


![2.2](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_jvm/2.2.jpg)

![2.3](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_jvm/2.3.jpg)

![2.4](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_jvm/2.4.jpg)

# 3. class文件结构+类加载机制

![3.1](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_jvm/3.1.jpg)

![3.2](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_jvm/3.2.jpg)

### 双亲委派模型的优势
&nbsp;&nbsp;&nbsp;&nbsp;层次关系可以避免类的重复加载，保障系统安全

# 4. 编译及优化
![4](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_jvm/4.jpg)

### 基于采样的热点探测
&nbsp;&nbsp;&nbsp;&nbsp;
周期性的检查各个线程的栈顶，如果发现某个或某些方法经常出现在栈顶，则这个方法是热点方法。

### 基于计数器的热点探测
&nbsp;&nbsp;&nbsp;&nbsp;
为每个方法建立计数器，统计方法的执行次数，如果执行次数超过一定的阈值，就认为它是“热点方法”。


