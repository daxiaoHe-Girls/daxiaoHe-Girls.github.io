
# 1.	除数为0
**(1)	整型——1/0**  
> 结果：运行时异常
> Exception in thread "main" java.lang.ArithmeticException: / by zero 

**(2)	浮点型——1.0/0.0**  
> 结果：0/0.0不会抛出异常，值为Infinity。
> Java中默认0.0是+0.0

# 2.	ArrayList深拷贝

```
		ArrayList<Integer> li = new ArrayList<>();
		li.add(1);li.add(2);
		ArrayList<Integer> li2 = new ArrayList<>(li);
		System.out.println(li==li2);//false
		System.out.println(li);//[1,2]
		System.out.println(li2);//[1,2]
        System.out.println(li.equals(li2));//true
```


# 3.	数据类型
**(1)	基本数据类型**  

```
byte/8，boolean/8， char/16， short/16
int/32，float/32， long/64，double/64
```


**(2)	对象数据类型——拆箱装箱**

```
Integer x = 2;     // 装箱
int y = x;         // 拆箱
```


**(3)	缓存池**
- **new Integer(123) VS Integer.valueOf(123)**  

&nbsp;&nbsp;&nbsp;&nbsp;   new Integer(123) ：每次都会新建一个对象；

&nbsp;&nbsp;&nbsp;&nbsp;  Integer.valueOf(123) 会使用缓存池中的对象，多次调用会取得同一个对象的引用。实现先判断值是否在缓存池中，如果在的话就直接返回缓存池的内容。

编译器会在自动装箱过程调用 valueOf() 方法，多个值相同且值在缓存池范围内的 Integer 实例使用自动装箱来创建，那么就会引用相同的对象。
		
```
Integer m = 123;
Integer n = 123;
System.out.println(m == n); // true
```

- **基本类型对应的缓冲池如下：**
> boolean values true and false  
> all byte values  
> short values between -128 and 127  
> int values between -128 and 127  
> char in the range \u0000 to \u007F  

**(4)	Java拆箱、装箱**  

- **装箱转换**
> 是指将一个值类型隐式地转换成一个object 类型，或者把这个值类型转换成一个被该值类型应用的接口类型interface-type。把一个值类型的值装箱，也就是创建一个object 实例并将这个值复制给这个object。

- **拆箱转换**
> 是指将一个对象类型显式地转换成一个值类型，或是将一个接口类型显式地转换成一个执行该接口的值类型。  
拆箱的过程分为两步：首先，检查这个对象实例，看它是否为给定的值类型的装箱值。然后，把这个实例的值拷贝给值类型的变量。
 
![装箱+拆箱](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_java%E5%B0%8F%E7%9F%A5%E8%AF%86%E7%82%B9/%E8%A3%85%E7%AE%B1%2B%E6%8B%86%E7%AE%B1.PNG)  

# 4.	String
**(1)	不可变性：类和数组被申明为final+内部没有改变value数组的方法**  

- &nbsp;&nbsp;&nbsp;&nbsp; 在 Java 8 中，String 内部使用 char 数组存储数据。

```
public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
    private final char value[];
}
```


- &nbsp;&nbsp;&nbsp;&nbsp; Java 9 之后，String 类的实现改用 byte 数组存储字符串，同时使用 coder 来标识使用了哪种编码。

```
public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
    private final byte[] value;
    private final byte coder;
}
```


**(2)	不可变的好处**
- **缓存hash值**
> 因为 String 的 hash 值经常被使用，例如 String 用做 HashMap 的 key。不可变的特性可以使得 hash 值也不可变，因此只需要进行一次计算。

- **string pool的需要**
> 如果一个 String 对象已经被创建过了，那么就会从 String Pool 中取得引用。只有 String 是不可变的，才可能使用 String Pool。

- **安全性**
> String 经常作为参数，String 不可变性可以保证参数不可变。例如在作为网络连接参数的情况下如果 String 是可变的，那么在网络连接过程中，String 被改变，改变 String 对象的那一方以为现在连接的是其它主机，而实际情况却不一定是。

- **线程安全性**
> 不可变性使其具备线程安全，可在多个线程中使用

**(3)	常量池**
1)	字符串常量池（String Pool）：   
保存所有字符串的字面量(两个双引号之间的字符序列)，这些字面量在编译期确定；还可以通过intern方法在运行时向常量池中加入字符串。

2)	Intern方法：  
当一个字符串调用 intern() 方法时，如果 String Pool 中已经存在一个字符串和该字符串值相等（使用 equals() 方法进行确定），那么就会返回 String Pool 中字符串的引用；否则，就会在 String Pool 中添加一个新的字符串，并返回这个新字符串的引用。

3)	new String(“aaa”)  
==每次new String都是新建一个新的对象==  
- S1："aaa" 属于字符串字面量，因此编译时期会在 String Pool 中创建一个字符串对象，指向这个 "aaa" 字符串字面量；  
- S2：而使用 new 的方式会在堆中创建一个字符串对象。

```
		String s1 = new String("aaa");
		String s2 = new String("aaa");
		System.out.println(s1 == s2);           // false
```

2：intern

```
		String s3 = s1.intern();
		String s4 = s1.intern();
		System.out.println(s3 == s4);           // true
```

3:字面量形式创建，自动放入常量池且返回对应引用，或存在直接从常量池中取对应的引用

```
		String s5 = "bbb";
		String s6 = "bbb";
		System.out.println(s5 == s6);  // true

		String a = "hello2";
		final String b = "hello";
		String c = "hello";

		System.out.println(a==(b+2));//true ==("hello"+2)
		System.out.println(a==(c+2));//false  == new String("hello2")
```


**(4)	VS StringBuilder VS StringBuffer**
- **可变性**  
    - String 不可变；  
    - StringBuffer 和 StringBuilder 可变  
    

- **线程安全**  
    - String 不可变，因此是线程安全的；  
    - StringBuilder 不是线程安全的;  
    - StringBuffer 是线程安全的，内部使用 synchronized 进行同步


- **都不能被继承**  
&nbsp;&nbsp;&nbsp;&nbsp;
三者都被final修饰，不能被继承  


- **执行效率**  
&nbsp;&nbsp;&nbsp;&nbsp;StringBuilder > StringBuffer > String

# 5.	HashMap的底层实现
**（1）底层数据结构**  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数组+链表，数组的每个元素都是链表结构  

**（2）put操作**  
- &nbsp;&nbsp;&nbsp;&nbsp;键为null的时候单独处理，第0个桶存放key为null的键值对；  
- &nbsp;&nbsp;&nbsp;&nbsp;计算key的哈希值，确定桶下标——> 对于桶中链表进行遍历，存在对应key，则将其value值更新；否则，在链表头插入新的键值对  

**（3）Get操作**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 计算key的哈希值，确定桶下标——> 对于桶中链表进行遍历，存在对应key，则将其返回；否则，返回null

**（4）扩容**
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;N/M（存储元素/容量）越小，查找复杂度越小（O(N/M)）。容量变为原来的两倍，将旧的数据重新hash插入新的中。  

**（3）Hash冲突解决**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;如果当前桶存储链表容量<=8，则采用拉链法；当>8时，采用平衡树代替链表，使得每个桶中查找复杂度从O(n)变成O(lgn)

# 6.	HashSet的底层实现
&nbsp;&nbsp;&nbsp;&nbsp; 底层实现是HashMap，将相应的内容存储在map对应的key中，value部分用一个PRESENT对象存储

```
private static final Object PRESENT = new Object();
```


# 7.	ConcurrentHashMap的底层实现
**（1）底层**
- 和 HashMap 实现上类似，最主要的差别是 ConcurrentHashMap
- 采用了分段锁（Segment），每个分段锁维护着几个桶（HashEntry），多个线程可以同时访问不同分段锁上的桶，从而使其并发度更高（并发度就是 Segment 的个数）。  

**（2）Segment 继承自 ReentrantLock**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 默认的并发级别为 16，也就是说默认创建 16 个 Segment。  

**（3）获取size**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在执行 size 操作时，需要遍历所有 Segment 然后把 count 累计起来。ConcurrentHashMap 在执行 size 操作时先尝试不加锁，如果连续两次不加锁操作得到的结果一致，那么可以认为这个结果是正确的。尝试次数使用 RETRIES_BEFORE_LOCK 定义，该值为 2，retries 初始值为 -1，因此尝试次数为 3。如果尝试的次数超过 3 次，就需要对每个 Segment 加锁。

# 8.	Java约定equals()必须是一种等价性关系
- **自反性**：x.equals(x)——true
- **对称性**：x.equals(y)——y.equals(x)
- **传递性**：x.equals(y)——true + y.equals(z)——true则，x.equals(z)——true
- **一致性**：x,y未修改，反复调用结果一致x.equals(y)
- **非空性**：x.equals(null)——false

# 9.	每当实例调用了new()，系统
- (1)为新的对象分配内存空间
- (2)调用构造函数初始化对象的值
- (3)返回该对象的一个引用

(2)和(3)可能会重排



# 10.	switch-case方法支持的参数
- byte，char，short，int
- Byte，Character，Short，Int
> java编译器在底层手动进行拆箱

- StringString类型
> 该有一个hashCode算法,结果也是int类型.

- 枚举
> 枚举类有一个ordinal方法,该方法实际上是一个int类型的数值

# 11.	Java中的四种变量
- **参数变量**：方法在被调用时被初始化为调用者提供的值，只在该方法内有效
- **局部变量**：局部有意义，系统不自定义初始值
- **实例变量**（成员变量）：该类的实例中有效，可以和局部变量重名，用this加以区分，没有this采用就近原则，系统自定义初始值
- **类变量**（static成员变量）：在类加载时创建，系统自定义初始值，一个类的实例共享类变量

# 12.	操作符的优先级
 
 ![操作符的优先级](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_java%E5%B0%8F%E7%9F%A5%E8%AF%86%E7%82%B9/%E6%93%8D%E4%BD%9C%E7%AC%A6%E7%9A%84%E4%BC%98%E5%85%88%E7%BA%A7.PNG)
 
# 13.	类初始化
**(1)	初始化顺序的规则：**
- **原则1**: 静态初始化块——>初始化块——>构造器
- **原则2**: 父类——>子类
- **原则3**: 静态内容只在类第一次加载的时候加载

**(2)	综合顺序：**
- 父类静态初始化块(或成员变量)
- 子类静态初始化块(或成员变量)
- 父类初始化块（或普通成员）
- 父类构造器
- 子类初始化块（或普通成员）
- 子类构造器

# 14.	反射机制
**(1)	反射机制**  
- 可以在程序运行时，得到一个对象所属的类Class
- 获取一个类的所有成员变量和方法
- 可以在运行时创建对象
- 在运行时调用对象的方法

**(2)	获取Class的方法**
- Class.forName(“类的路径”);
- 类名.class;
- 实例.getClass();

**(3)	Java创建对象的四种方式**  

**(4)	反射的原理**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 类加载过程中，编译:.java文件编译后生成.class字节码文件；加载：类加载器负责根据一个类的全限定名来读取此类的二进制字节流到JVM内部，并存储在运行时内存区的方法区，然后将其转换为一个与目标类型对应的java.lang.Class对象实例；.class文件中包含java类的所有信息，当你不知道某个类具体信息时，可以使用反射获取class，然后进行各种操作。

**(5)	优点：**
- **可扩展性** 
> 应用程序可以利用全限定名创建可扩展对象的实例，来使用来自外部的用户自定义类。
类浏览器和可视化开发环境 ：一个类浏览器需要可以枚举类的成员。可视化开发环境（如 IDE）可以从利用反射中可用的类型信息中受益，以帮助程序员编写正确的代码。

- **调试器和测试工具** 
> 调试器需要能够检查一个类里的私有成员。测试工具可以利用反射来自动地调用类里定义的可被发现的 API 定义，以确保一组测试中有较高的代码覆盖率。

**(6)	缺点：**
- **性能开销** 
> 反射涉及了动态类型的解析，所以 JVM 无法对这些代码进行优化。因此，反射操作的效率要比那些非反射操作低得多。我们应该避免在经常被执行的代码或对性能要求很高的程序中使用反射。

- **安全限制** 
> 使用反射技术要求程序必须在一个没有安全限制的环境中运行。如果一个程序必须在有安全限制的环境中运行，如 Applet，那么这就是个问题了。

- **内部暴露** 
> 由于反射允许代码执行一些在正常情况下不被允许的操作（比如访问私有的属性和方法），所以使用反射可能会导致意料之外的副作用，这可能导致代码功能失调并破坏可移植性。反射代码破坏了抽象性，因此当平台发生改变的时候，代码的行为就有可能也随着变化。

**(7)	获取类名**：new Test().getClass().getName();  

**(8)	获取父类类名**：new Test().getClass().getSuperclass().getName(); （只能采用反射机制）

# 15.	泛型
**(1)	泛型** 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 不固定集合元素存储对象的类型，泛指的类型，也是一个类型的占位符，本身并不能工作，一旦指定了具体的类型，就可以很好的工作。 

**(2)	工作原理** 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;通过类型擦除来实现的，编译器在编译时借助传入的类型参数检查对容器的插入，获取操作是否合法，然后擦除了所有类型相关的信息，所以在运行时不存在任何类型相关的信息。  

**(3)	优点：**
- 代码复用；
- 将代码安全性检查提前到编译期：使用泛型后，能让编译器在编译的时候借助传入的类型参数检查对容器的插入，获取操作是否合法，从而将运行时ClassCastException转移到编译时；
- 能够省去类型强制转换；
- 泛型最常见的用途是创建集合类。

**(4)	缺点：**
- 在运行时，不能使用具体类型的特定功能；
- 不能使用一些类型：基本变量类型，Throwable的派生类；
- 不能重载。

# 16.	Java中负数取模方法
- 负数除法——向0取整
- 负数取模——（a/b）*b + a%b = a

# 17.	构造方法
- (1) 构造方法的方法名必须与类名相同
- (2) 构造方法没有返回类型，也不能定义为void，在方法名前面不申明方法类型
- (3) 构造方法的主要作用是完成对象的初始化工作，它能够把定义对象时的参数传给对象的域
- (4) 一个类可以有多个构造方法，如果在定义类时没有定义构造方法，则编译系统会自动插入一个无参数的默认构造器，这个构造器不执行任何代码
- (5) 构造方法可以重载（以参数的个数和类型）
- (6) 构造方法不能被继承，可以使用super调用父类的构造方法，且只能放在第一行；

> 普通方法可以和类名、构造函数同名。

# 18.	接口VS 抽象类
**(1)	共同点** 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 都不能被实例化；当所有abstract方法被重写后，才能够创造实例对象

**(2)	不同点**
- 接口只能被实现，抽象类可以被继承；
- 接口中方法只能声明，不能有默认实现的方法(Java8之前)；抽象类中可以声明，也可以有默认实现的方法；
- 接口中的成员变量默认是public static final的，抽象类中的成员默认是default，也可以是private、protected、和public;
- 接口中的方法默认是public abstract的，而抽象类中的方法可以是public、protected、default修饰；

# 19.	面向对象的三大特征
封装 继承 多态

**封装**
> 封装就是将同一类事物的特性与功能包装在一起，对外暴露调用的接口。

**继承**
> 继承是从已有的类中派生出新的类，新的类能吸收已有类的数据属性和行为，并能扩展新的能力。

**多态**
> 面向对象的多态性主要体现在：重写与重载两方面

Tip：虚拟机是如何实现多态的  
动态绑定技术(dynamic binding)，执行期间判断所引用对象的实际类型，根据实际类型调用对应的方法。

# 20.	重写 VS 重载
**(1)	重写：**
- 方法名相同；
- 方法参数**类型**、参数**个数**或者参数**顺序**不同  

**(2)	重载：两同两小一大原则，编译时多态**
- 方法名相同；方法参数相同；
- 返回值类型小于等于父类；抛出异常小于等于父类；
- 修饰权限大于等于父类

**(3)	重写：运行时多态**
- 成员变量：编译、运行看左边；
- 静态函数：编译、运行看左边；
- 普通函数：编译看左边，运行看右边

# 21.	修饰权限

![修饰权限](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_java%E5%B0%8F%E7%9F%A5%E8%AF%86%E7%82%B9/%E4%BF%AE%E9%A5%B0%E6%9D%83%E9%99%90.PNG)

**(1)	区别default关键字**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; default关键字可以让接口中的方法可以有默认的函数体，当一个类实现这个接口时，可以不用去实现这个方法而调用它，当然，这个类若实现这个方法，就等于子类覆盖了这个方法，最终运行结果符合Java多态特性。

**(2)	static**
不能修饰类(除静态内部类)和接口，只能修饰方法和变量  

**(3)	不能修饰接口：private，protected，static**

# 22.	异或
**a^b = b^a，a^a = 0，a^0 = a，a^(a^b) = (a^a)^b = b，(a^b)^b = a^(b^b) = a**
- 常用的等式：-n = ~(n-1) = ~n+1
- 获取整数n的二进制中最后一个1：n&(-n) 或者 n&~(n-1) 
> 如：n=010100，则-n=101100，n&(-n)=000100

- 去掉整数n的二进制中最后一个1：n&(n-1)
> 如：n=010100，n-1=010011，n&(n-1)=010000

# 23.	Java语言的优势
- (1)	Java是纯面向对象的语言，开发人员更容易理解，程序更容易编写；
- (2)	平台无关性，可以跨平台执行，具有很好的可移植性
> 即，每种数据类型在不同平台上分配固定长度；同时，编译器会将Java代码编译成可以在JVM上执行的中间代码。
- (3)	内置类库多，简化开发
- (4)	安全性高
> Java的强类型机制、垃圾回收机制、异常处理和安全检查机制使得用Java语言编写的程序更安全。
- (5)	Java并发机制，利于多线程开发。

# 24.	Java VS C++的优势
- (1)	Java是解释型语言，C++是编译型语言；
- (2)	Java是纯面向对象语言，C++既面向对象，也面向过程；
- (3)	Java中没有指针，C++有
- (4)	C++需要开发人员去管理内存的分配，Java语言提供了垃圾回收机制自动内存回收。
- (5)	Java具有平台性，即每种数据类型都分配固定长度；C++却不具备；
- (6) Java只支持单继承，C++支持多继承；但Java提供了接口实现。

# 25.	main方法
**public static void main(String[] args) {}**
- (1)	main是JVM识别的特殊方法名，是程序的入口；
- (2)	void + public + static 必须有；其他也可以，但不能有abstract；
- (3)	每个类都可以定义main方法，只有和文件名相同的+public修饰的类中的main方法才能作为整个程序的入口方法。

# 26.	没有任何方法的接口：Cloneable + Serializable
**标识接口**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;没有任何方法申明的接口。仅仅充当一个标识的作用，用来表明实现它的类属于一个特定的类型。

# 27.	Java中的clone方法
**(1)	功能**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;创建一个与已有对象A具有相同状态的B

**(2)	clone()**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;返回一个新的对象，而不是原对象的引用

**(3)	使用clone**
- a)	实现clone方法的类首先需要继承Cloneable接口(标识接口)；
- b)	在类中重写Object的clone方法；
- c)	在clone中调用父类的super.clone()，则会直接或间接调用Object的clone方法；
- d)	实现自己的引用或创建新对象；  

**(4)	浅复制 VS 深复制**
- **浅复制**：返回A对象的一个新对象的引用B；但是B中元素引用的对象和A中元素引用的对象一致
- **深复制**：返回A对象的一个新对象的引用B；但是B中元素引用的某个对象是A中对应元素引用对象的浅复制(新的引用)

# 28.	Java中的package的作用
- 提供多层命名空间，解决命名冲突。处于不同package的类可以存在相同的类名。
- 对类按功能进行分类，使得项目组织结构清晰。

# 29.	Java中实现C中函数指针功能
- S1.	定义一个接口A，在接口中声明要调用的方法；
- S2.	定义类B实现接口A，重写A中要调用的方法；
- S3.	将B作为类C的对象参数传入，即可以在C中调用B的方法；

# 30.	面向对象 VS 面向过程
- 	面向对象：将数据和数据的操作进行封装，成类，对外暴露接口
- 	面向过程：以事件为中心，每个模块实现某个特定功能。

# 31.	继承
- (1)	单继承；
- (2)	只能继承父类非私有方法和成员变量；
- (3)	成员变量同名时，子类覆盖父类中的该成员变量，而非继承；
- (4)	函数签名相同时（方法名相同+参数个数类型相同），子类覆盖父类中的该方法，而非继承；

# 32.	内部类
- 	静态内部类
- 	成员内部类（普通内部类）
- 	局部内部类（定义在方法或代码块中的类）
- 	匿名内部类

# 33.	break VS continue VS return 
- 	break：跳出当前循环
- 	continue：停止当前循环的当次循环，回到循环起始处
- 	return：使程序返回到Java运行系统
- 	break+标志：跳出多层循环

# 34.	instance of
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;作用：判断一个引用类型的变量所指向的对象是否是一个类（接口、抽象类、父类）的实例 

# 35.	strictfp关键字(strict float point)
**(1)	声明类、接口、方法**  

**(2)	功能**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;声明范围内，Java编译器以及运行环境会完全依照IEEE二进制浮点数算数标准   来执行  

**(3)	注意**：当一个类被strictft修饰时，它的所有方法都被其修饰

# 36.	如何创造一个不可变类
- S1.	类中所有成员变量被private修饰；
- S2.	类中无写或修改成员变量的方法，只提供（带参）构造函数，一次生成，永不改变；
- S3.	确保类中所有方法不会被子类覆盖，可以通过将类定义为final或者将方法定义为final来实现；
- S4.	如果某个类成员变量不是不可变量，则初始化时get获取初始值，需要时用clone方法确保其不可变性；
- S5.	需要的话可以覆盖Object的equals方法和hashcode方法。

# 37.	Java中的参数传递方式：值传递+引用传递

```
public class Test {
	    public static void main(String[] args) {
	        StringBuffer s1 = new StringBuffer("hello");
	        StringBuffer s2 = new StringBuffer("hello");
	        changeStringBuffer(s1, s2);
	        System.out.println("s1=" + s1);//helloworld
	        System.out.println("s2=" + s2);//hello
	    }
	    public static void changeStringBuffer(StringBuffer ss1, StringBuffer ss2) {
	        ss1.append("world");
	        ss2 = ss1;
	    }
	}
```

![Java参数传递](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_java%E5%B0%8F%E7%9F%A5%E8%AF%86%E7%82%B9/Java%E5%8F%82%E6%95%B0%E4%BC%A0%E9%80%92.PNG)

# 38.	Java中的参数传递方式：值传递+引用传递
(1)	Java中，涉及byte，char，short运算时，首先会将这些类型的变量强转为int型，然后对int型进行运算，最后得到int型结果（即byte+byte=int，char+char=int，short+short=int）;  

(2)	Java 不能隐式执行向下转型，因为这会使得精度降低;  

(3)	+= 或者 ++ 运算符可以执行隐式类型转换  

```
short s1 = 1;
s1 = s1 + 1;--------报错，不能向下转型
s1 += 1; --------正确，+=可以隐式向下转型
s1++; --------正确，++可以隐式向下转型
```


# 39.	Math中的ceil VS round VS floor
- ceil : 向上（数值大的）取整，返回值是double型；
- round : 先增加0.5，再向下取整，返回值是int型；
- floor : 向下取整，返回值是double型；

# 40.	判断一个字符串中是否包含中文字符
> Trick：英文字符串的字节长度和字符串等长，因为一个中文字符占用两或者三个字节

# 41.	长度
- 数组：length
- 字符串：length()
- 集合：size()

# 42.	Statement VS PrepareStatement
- (1)	PrepareStatement 是 Statement的孩子；
- (2)	PrepareStatement可以防止sql注入；
- (3)	PrepareStatement会对sql语句进行预编译，以减轻数据库服务器的压力。

**CallableStatement**   
&nbsp;&nbsp;&nbsp;&nbsp;
由 prepareCall()方法所创建，它为所有数据库管理系统提供了一种以标准形式调用已储存过程的方法。它从PrepareStatement中继承了用于处理输入参数的方法，而且还增加了调用数据库中的存储过程和函数以及设置输出类型参数的功能。

# 43.	Sql注入， PrepareStatement防止sql注入的原理
**(1)	Sql注入**  

&nbsp;&nbsp;&nbsp;&nbsp;
指通过构建特殊的输入作为参数传入web应用程序，而这些输入大都是sql语法里的一些组合，通过执行sql语句而执行攻击者所要的操作。
> Eg： ….where user = “or ‘a’ = ‘a’ and password =” or ‘a’=’a’ 

(2)	PrepareStatement可以在传入sql后，执行语句前，给参数赋值，避免因普通的拼接字符串而带来的安全问题。

# 44.	Java注解的实现原理
**(1)	注解** 

&nbsp;&nbsp;&nbsp;&nbsp;对代码进行说明，可以对包，类，接口，字段，方法参数，局部变量进行注解。  

**(2)	分类**
- java自带注解
- 元注解，用于定义注解的注解
- 自定义注解

**(3)	用途**
- 生成文档：通过代码里标注的元数据生成javadoc文档；
- 编译检查：让编译器在编译期间进行检查验证；
- 编译时动态处理：eg 动态生成代码；
- 运行时动态处理：eg 用反射注入实例

**(4)	实现原理**  

&nbsp;&nbsp;&nbsp;&nbsp;注解声明；使用注解；注解处理器

**(5)	注解处理器**  

&nbsp;&nbsp;&nbsp;&nbsp;反射（通过反射读取注解中的信息）+代理。Java.lang.reflect.AnnotatedElement接口是所有程序元素(Class.Method和Constructor)的父接口，所以程序通过反射获得了某个类的AnnotatedElement对象之后，程序就可以调用该对象的4个方法来访问Annotation信息了。

# 45.	假设要查询1000万条数据，用什么办法可以提高查询效率？在数据库方面或者Java方面有什么优化方法？
**(1)	数据库设计方面(参考sql优化)**  

**(2)	数据库I/O方面**  

- 增加缓冲区
- 数据库分区：涉及表的级联，不同的表存储在不同的磁盘上，以增加I/O速度

**(3)	在sql语句方面(参考sql优化)**

**(4)	Java方面**  

&nbsp;&nbsp;&nbsp;&nbsp;
反复使用的查询，可以用PreparedStatement减少查询次数

# 46.	字节序
- **大端字节序(Big Endian)**  

&nbsp;&nbsp;&nbsp;&nbsp;
数据最高有效位，存于内存空间最低内存地址处；最低有效位存于最高内存处；


- **小端字节序(Little Endian)**  

&nbsp;&nbsp;&nbsp;&nbsp;
数据最高有效位，存于内存空间最高内存地址处；最低有效位存于最低内存处；

- **网络字节序**  

&nbsp;&nbsp;&nbsp;&nbsp;
UDP/TCP/IP协议规定的一种良好的数据表示格式，保证数据在不同主机之间能被正确解释，采用大端排序方式。把收到的第一个字节当作高位，要求发送端发送时，起始地址发出来的也是高位。

# 47.	为什么要进行垃圾回收？
- 自动释放内存空间，减轻内存负担
- 清除内存碎片

# 48.	线程安全 VS 线程不安全
&nbsp;&nbsp;&nbsp;&nbsp;代码或数据被多个线程同时运行或访问、操作时，若每次运行结果和单线程顺序运行结果一致，则认为代码安全；反之，认为不安全。

# 49.	自下向上的建堆方式，时间复杂度为：O(n)

![建堆证明](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_java%E5%B0%8F%E7%9F%A5%E8%AF%86%E7%82%B9/%E5%BB%BA%E5%A0%86%E8%AF%81%E6%98%8E.PNG)

# 50.	红黑树的特性
- 	每个节点或是红色，或是黑色；
- 	根节点是黑色的；
- 	每个叶节点是黑色的；
- 	如果一个节点是红色的，则它的两个叶节点是黑色的；
- 	对于每个节点，从该节点到其后代所有叶节点的简单路径，均包含相同数目的黑色节点。
- 	插入、查找都是lgN

# 51.	一个服务器最多能接受多少个客户端的TCP连接？
&nbsp;&nbsp;&nbsp;&nbsp;
客户端IP数（232）×客户端port数（216）  

&nbsp;&nbsp;&nbsp;&nbsp;
**系统用一个四元组来标识一个唯一的TCP连接：{local ip, local port, remote ip, remote port}**

# 52.	容量为O和P的队列，实现一个栈，栈的容量是多少？

# 53.	字符串匹配算法的复杂度
- 	朴素匹配算法：O((N-P+1)*P)
- 	KMP匹配算法：O(N+p)

# 54.	Java对象的生命周期
**(1)	创建阶段(Created)**
- 为对象分配存储空间
- 开始构造对象
- 从超类到子类对static成员进行初始化
- 超类成员变量按顺序初始化，递归调用超类的构造方法
- 子类成员变量按顺序初始化，子类构造方法调用

**(2)	应用阶段(In Use)**  

&nbsp;&nbsp;&nbsp;&nbsp;
一旦对象被创建，并被分派给某些变量赋值，这个对象的状态就切换到了应用阶段
对象至少被一个强引用持有着

**(3)	不可见阶段(Invisible)**  

&nbsp;&nbsp;&nbsp;&nbsp;
说明程序本身不再持有该对象的不论什么强引用，尽管该这些引用仍然是存在着的。简单说就是程序的运行已经超出了该对象的作用域了

**(4)	不可达阶段(Unreachable)**

&nbsp;&nbsp;&nbsp;&nbsp;
指该对象不再被不论什么强引用所持有

**(5)	收集阶段(Collected)**

&nbsp;&nbsp;&nbsp;&nbsp;
	当垃圾回收器发现该对象已经处于“不可达阶段”而且垃圾回收器已经对该对象的内存空间又一次分配做好准备时，则对象进入了“收集阶段”
	
**(6)	终结阶段(Finalized)**

&nbsp;&nbsp;&nbsp;&nbsp;
当对象运行完finalize()方法后仍然处于不可达状态时，则该对象进入终结阶段。在该阶段是等待垃圾回收器对该对象空间进行回收

**(7)	对象空间重分配阶段(De-allocated)**

&nbsp;&nbsp;&nbsp;&nbsp;
垃圾回收器对该对象的所占用的内存空间进行回收或者再分配了，则该对象彻底消失了，称之为“对象空间又一次分配阶段”。

# 55.	Java类的生命周期
加载 -> 验证 -> 准备 -> 解析 -> 初始化 -> 使用 -> 卸载

# 56.	Java的异常
**(1)	层次结构：**  

==Throwable = Error + Exception==
- Error : 错误，程序本身不可处理的异常；= VirtulMacineError + AWTError 
- Exception ： 异常，程序本身可处理的异常；= IOException + RuntimeException
- IOException ：可查异常，编译器要求强制处理的异常
- RuntimeException ：不可查异常

**(2)	异常处理机制**
- a)	抛出异常
    - 方法上：method_name  throws  Exception1, Exception2, …..
    - 方法内：throw new RuntimeException(“..”);
- b)	捕捉异常
    - try{}catch{}catch{}finally{}

# 57.	为什么要对网络分层
**(1)	各层之间相互独立**  
> 高层是不需要知道底层的功能是采取硬件技术来实现的，它只需要知道通过与底层的接口就可以获得所需要的服务；

**(2)	灵活性好**
> 各层都可以采用最适当的技术来实现，例如某一层的实现技术发生了变化，用硬件代替了软件，只要这一层的功能与接口保持不变，实现技术的变化都并不会对其他各层以及整个系统的工作产生影响； 

**(3)	易于实现和标准化**
> 由于采取了规范的层次结构去组织网络功能与协议，因此可以将计算机网络复杂的通信过程，划分为有序的连续动作与有序的交互过程，有利于将网络复杂的通信工作过程化解为一系列可以控制和实现的功能模块，使得复杂的计算机网络系统变得易于设计，实现和标准化

# 58.	网络分层的原则
- 	各个层之间有清晰的边界,便于理解;
-   每个层实现特定的功能;
- 	层次的划分有利于国际标准协议的制定;
- 	层的数目应该足够多，以避免各个层功能重复

# 59.	链表
**题**：给出奇数位置组成上升序列，偶数位置组成下降序列的链表，转化为整体上升的链表
- S1：奇数位链表节点取出为一个链表
- S2：偶数位链表节点取出为一个链表，并反转
- S3：递归 归并两个链表

# 60.	Nginx
**(1)	正向代理**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;隐藏真实的请求客户端，服务端不知道真实的客户端是谁，正向代理服务器会代替客户端向服务器发送请求。正向代理代理的对象是客户端。  

**(2)	反向代理**   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;隐藏真实的响应服务端，客户端不知道真实的服务端是谁，反向代理服务器会把请求转发到真实的服务器。反向代理代理的对象是服务端。

# 61.	JDK6 7 8中的JVM内存区域划分变化
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在Java7之前，HotSpot虚拟机中将GC分代收集扩展到了方法区，使用永久代来实现了方法区。这个区域的内存回收目标主要是针对常量池的回收和对类型的卸载。但是在之后的HotSpot虚拟机实现中，逐渐开始将方法区从永久代移除。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Java7中已经将运行时常量池从永久代移除，在Java 堆（Heap）中开辟了一块区域存放运行时常量池。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;而在Java8中，已经彻底没有了永久代，将方法区直接放在一个与堆不相连的本地内存区域，这个区域被叫做元空间。

![JVM内存区域划分变化](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_java%E5%B0%8F%E7%9F%A5%E8%AF%86%E7%82%B9/JVM%E5%86%85%E5%AD%98%E5%8C%BA%E5%9F%9F%E5%88%92%E5%88%86%E5%8F%98%E5%8C%96.PNG)

# 62.	野指针（C/C++）
**(1)	概念**   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;指向了一块随机内存空间，不受程序控制。如指针指向已经被删除的对象或者指向一块没有访问权限的内存空间，之后如果对其再解引用的话，就会出现问题

**(2)	产生的原因**  

- **指针定义时未被初始化**  
> 指针在被定义的时候，如果程序不对其进行初始化的话，它会指向随机区域，因为任何指针变量（除了static修饰的指针变量）在被定义的时候是不会被置空的，它的默认值是随机的。


- **指针被释放时没有被置空**
> 我们在用malloc开辟内存空间时，要检查返回值是否为空，如果为空，则开辟失败；如果不为空，则指针指向的是开辟的内存空间的首地址。指针指向的内存空间在用free()或者delete（注意delete只是一个操作符，而free()是一个函数）释放后，如果程序员没有对其置空或者其他的赋值操作，就会使其成为一个野指针。

- **指针操作超越变量作用域**
> 不要返回指向栈内存的指针或引用，因为栈内存在函数结束的时候会被释放

**(3)	野指针的危害**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 野指针的问题在于，指针指向的内存已经无效了，而指针没有被置空，解引用一个非空的无效指针是一个未被定义的行为，也就是说不一定导致段错误，野指针很难定位到是哪里出现的问题，在哪里这个指针就失效了，不好查找出错的原因。所以调试起来会很麻烦，有时候会需要很长的时间。

**(4)	规避方法**
- 初始化指针时将其置为NULL，之后再对其进行操作。
- 释放指针时将其置为NULL，最好在编写代码时将free()函数封装一下，在调用free()后就将指针置为NULL。
- 要想彻底地避免野指针，最好的办法就是养成一个良好的编程习惯。

# 63.	VPN的原理
**(1)	背景**
> 公共IP的短缺，我们在组建局域网时，通常使用保留地址作为内部IP，（比如最常用的C类保留地址：192.168.0.0－192.168.255.255）这些地址是不会被互联网分配的，因此它们在互联网上也无法被路由的，所以在正常情况下无法直接通过Internet外网访问到在局域网内的主机。为了实现这一目的，需要使用VPN隧道技术建立一个虚拟专用网络。

**(2)	实现原理**

a)	VPN网关一般会采用双网卡结构，内网卡接入公司总部A的内部局域网络，外网卡使用公共IP接入Internet；  

b)	比如说分公司B的终端（192.168.2.2）需要访问总部A的服务器（192.168.1.252），其发出的访问数据包的目标地址为服务器的IP：192.168.1.25

![VPN_b](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_java%E5%B0%8F%E7%9F%A5%E8%AF%86%E7%82%B9/VPN_b.PNG)
 
c)	分公司B局域网的VPN网关在接收到终端（192.168.2.2）发出的访问数据包①时对其目标地址（192.168.1.252）进行检查，发现目标地址属于公司总部A网络的地址，于是将该数据包①根据所采用的VPN技术进行封装，同时VPN网关会构造一个新的VPN数据包②，并将封装后的原数据包①作为VPN数据包②的负载，VPN数据包的目标地址为公司总部A网络的VPN网关的公共IP地址。

![VPN_c](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_java%E5%B0%8F%E7%9F%A5%E8%AF%86%E7%82%B9/VPN_c.PNG)
 
d)	分公司B局域网的VPN网关将VPN数据包发送到Internet外网中，由于VPN数据包的目标地址是总部A网络的VPN网关的外部地址，所以该数据包将被Internet中的路由正确地发送到总部A网络的VPN网关；

![VPN_d](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_java%E5%B0%8F%E7%9F%A5%E8%AF%86%E7%82%B9/VPN_d.PNG)
 
e)	总部A网络的VPN网关对接收到的数据包②进行检查，如果发现该数据包是从分公司B网络的VPN网关发出的，即可判定该数据包为VPN数据包，并对该数据包进行解包。解包的过程主要是将VPN数据包的包头剥离，将负载通VPN技术反向处理还原成原始的数据包①;  

f)	总部A网络的VPN网关将还原后的原始数据包发送至目标服务器（192.168.1.252）。在服务器（192.168.1.252）看来，它收到的数据包就跟从终端（192.168.2.2）直接发过来的一样;  

g)	从服务器（192.168.1.252）返回终端（192.168.2.2）的数据包处理过程与上述过程原理是一样的。这样就完成了整个通过VPN的访问。  

# 64.	什么是迭代器（Iterator）
**(1)	名词解释**
> Iterator是一个对象，工作是遍历并选择序列中的对象

**(2)	功能**
> 提供了一种访问某个容器中各个元素、而又不必暴露该容器内部细节的方法

**(3)	ConcurrentModificationException异常**
> 在使用Iterator遍历容器的过程中，如果对容器进行增加和删除操作，就会改变容器中对象的数量，从而抛出该异常。

> 解决方法如下：在遍历的过程中把需要删除的对象保存到一个集合中，等遍历结束后再调用removeAll来删除，或者使用iter.remove()方法。

**(4)	Iterator VS ListIterator**
- 关系：Iterator是ListIterator的父类
- 遍历方向：Iterator只能正向遍历；ListIterator可以从两个方向进行遍历，同时支持元素的修改

# 65.	ArrayList VS Vector VS LinkedList
**1. 相同点**

&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; 均在java.util包中，均为可伸缩数组，即可以动态改变长度的数组

**2. 不同点**  

**(1)	数据结构：**
- ArrayList 和 Vector都是基于存储元素 Object[] array来实现的，它们会在内存中开辟一块连续的空间进行存储；
- LinkedList采用双向链表实现； 

**(2)	操作：**
- ArrayList 和 Vector中，允许下标访问元素，存取效率高（由于在ArrayList 和 Vector中数据存储是连续的）；但增删需要动态扩容，效率低；
- LinkedList存取需要遍历，效率低；增删效率高；


**(3)	安全（同步）：**
- Vector线程安全（所有方法用sychronized修饰）；
- ArrayList和LinkedList线程不安全

# 66.	HashMap VS HashTable
**(1)	哈希算法**
- HashMap基于散列表，HashTable基于哈希表；
> hashmap取key的hashCode算法，然后对16进行异或运算和右移运算；

> hashtable直接使用对象的hashcode；

**(2)	线程安全性**
> HashMap线程不安全；

> HashTable线程安全；

**(3)	键值是否可以为null**
> HashMap允许key-value为null(但最多只允许一条记录的值为null)

> HashTable不允许；

**(4)	迭代 or 枚举？**
> HashMap使用Iterator;

> HashTable使用Enumeration;

**(5)	方法差异**
> HashMap使用containsValue()和containsKey;

> HashTable使用contains ()，易混淆；

**(6)	扩容**
> HashMap中默认大小为16，扩容为2的指数;

> HashTable中默认大小为11，扩容为odd*2+1;

# 67.	HashMap底层原理
- S1：调用key的hashcode生成hash值（Object的hashCode()方法会返回对象存储的内存地址）；hash值相同则查找、替换、拉链
- S2：hash值不同时，比较key的equals方法（Object的equals()方法默认是当前参数引用的对象和当前对象是同一个对象时，返回true）  


- 总结
> 当用HashMap存放自定义类时，重写equals()方法的同时必须重写hashCode()方法

> equals()相等，则hashCode()必然相等；反之不成立。

# 68.	Thread + Runnable
一个类可以同时继承Thread类+实现Runable接口,run方法可以有选择的覆盖。

# 69.	sleep() VS yield()
**(1)	线程状态**
- 线程执行sleep()之后转入阻塞状态；
- 线程执行yield()之后转入就绪、可执行状态；  

**(2)	声明异常**
- sleep()方法声明抛出中断异常；
- yield()没有声明任何异常；  

**(3)	优先级**
- sleep()方法给其他线程运行机会时不考虑其他线程的优先级；
- yield()方法只给相同优先级或更高优先级的线程运行机会；

# 70.	编码比较
- ASCII编码是1个字节
- Unicode编码通常是2个字节
- UTF-8编码将常用的英文字母被编码成1个字节，汉字通常是3个字节，只有很生僻的字符才会被编码成4-6个字节  

&nbsp;&nbsp;&nbsp;&nbsp;最佳实践：  
&nbsp;&nbsp;&nbsp;&nbsp;（1）在计算机内存中，统一使用Unicode编码，当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码；  
&nbsp;&nbsp;&nbsp;&nbsp;（2）用记事本编辑的时候，从文件读取的UTF-8字符被转换为Unicode字符到内存里，编辑完成后，保存的时候再把Unicode转换为UTF-8保存到文件；  
&nbsp;&nbsp;&nbsp;&nbsp;（3）浏览网页的时候，服务器会把动态生成的Unicode内容转换为UTF-8再传输到浏览器。


# 71.	实时操作系统 VS 分时操作系统
**（1）	实时操作系统：**
- **特点**
> 当优先级更高的任务2就绪的时候，即便任务1正在运行中，也必须立刻交出CPU的使用权，就跟中断一样，先执行任务2，等任务2执行完或者主动挂起(sleep)让出CPU的时候，任务1才能接着运行。

- **满足的特征**
> 多任务、抢占调度、任务间的通讯与同步、任务与中断之间的通信

- **主要应用领域**
> 主要应用于过程控制、数据采集、通信、多媒体信息处理等对时间敏感的场合。例如：机器人的运动控制、无人驾驶等。
   
**（2）	非实时操作系统（windows/linux/OSX）：**  
- **特点**
>   非实时的操作系统，CPU是不可抢占的，从上图可以看到，即便高优先级的任务就绪了，也不能马上中断低优先级任务而得到执行，必须要等到低优先级任务主动挂起(sleep)或者时间片结束才能得到执行。所以我们在使用PC的时候经常会遇到应用程序无响应的问题。即硬件资源被其他任务占用，本任务得不到立即执行。

**（3）	分时操作系统**
- **特点**
> 使一台计算机同时为几个、几十个甚至几百个用户服务的一种操作系统。把计算机与许多终端用户连接起来，分时操作系统将系统处理机时间与内存空间按一定的时间间隔，轮流地切换给各终端用户的程序使用（时间片的概念）。由于时间间隔很短，每个用户的感觉就像他独占计算机一样。
> 
- **满足的特征**
> 交互性、多路性、独立不干扰、及时性

- **主要应用领域**
> 现在流行的PC，服务器都是采用这种运行模式，即把CPU的运行分成若干时间片分别处理不同的运算请求。


![实时+非实时操作系统](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_java%E5%B0%8F%E7%9F%A5%E8%AF%86%E7%82%B9/%E5%AE%9E%E6%97%B6%2B%E9%9D%9E%E5%AE%9E%E6%97%B6%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F.PNG)

# 72.	四种线程池有哪些
**（1）	固定线程数的线程池（newFixedThreadPool）**
- 不会拒绝任务：LinkedBlockingQueue-无界队列
- 不会开辟新的线程
- 不会因为线程的长时间不使用而销毁线程
- 适合用在稳定且固定的并发场景，比如服务器

**（2）	缓存的线程池（newCachedThreadPool）**  
> 如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程
> （核心池大小为0，线程池最大线程数目为最大整型，这意味着所有的任务一提交就会加入到阻塞队列中。如果主线程提交任务的速度远远大于CachedThreadPool的处理速度，则CachedThreadPool会不断地创建新线程来执行任务，这样有可能会导致系统耗尽CPU和内存资源，所以在使用该线程池是，一定要注意控制并发的任务数，否则创建大量的线程可能导致严重的性能问题。）

**（3）	单个线程的线程池（newSingleThreadPool）**
- 不会拒绝任务：LinkedBlockingQueue-无界队列
- 唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行，所以这个比较适合那些需要按序执行任务的场景

**（4）	固定个数的线程池（newScheduledThreadPool）**
- 相比于第一个固定个数的线程池强大在  
- 可以执行延时任务
- 也可以执行带有返回值的任务

# 73.	API+SDK
- **API**
> 前端调用后端数据的一个通道，就是我们俗说的接口，通过这个通道，可以访问到后端的数据，但是又无需调用源代码。

- **SDK**
> 工程师为辅助开发某类软件的相关文档、范例和工具的集合，使用SDK可以提高开发效率，更简单的接入某个功能。举例说明：一个产品想实现某个功能，可以找到相关的SDK，工程师直接接入SDK，就不用再重新开发了。

# 74.	String split
Java中，按某个字符分割字符串使用的是String对象的split()方法，返回的是分割之后的String数组，值得注意的是分割符。  
==当分割符是 . 或者是 | 时，必须使用 \\ 进行转义。==


