### 一. 覆盖率定义
&nbsp;&nbsp;&nbsp;&nbsp; 我们通常会将测试覆盖率分为两个部分，即“需求覆盖率”和“代码覆盖率”。
- **需求覆盖率**  
> 指的是测试人员对需求的了解程度，根据需求的可测试性来拆分成各个子需求点，来编写相应的测试用例，最终建立一个需求和用例的映射关系，以用例的测试结果来验证需求的实现，可以理解为黑盒覆盖。

- **代码覆盖率** 
> 为了更加全面的覆盖，我们可能还需要理解被测程序的逻辑，需要考虑到每个函数的输入与输出，逻辑分支代码的执行情况，这个时候我们的测试执行情况就以代码覆盖率来衡量，可以理解为白盒覆盖。

### 二. Jacoco简介
&nbsp;&nbsp;&nbsp;&nbsp; JaCoCo是一个开源的覆盖率工具(官网地址：http://www.eclemma.org/JaCoCo/).  
它针对的开发语言是java，其使用方法很灵活，可以嵌入到Ant、Maven中；可以作为Eclipse插件，可以使用其JavaAgent技术监控Java程序等等。很多第三方的工具提供了对JaCoCo的集成，如sonar、Jenkins等。其执行的最小Java版本为1.5.

&nbsp;&nbsp;&nbsp;&nbsp; 包含了多种尺度的覆盖率计数器：  
- **行覆盖率**：度量被测程序的每行代码是否被执行，判断标准行中是否至少有一个指令被执行。
- **类覆盖率**：度量计算class类文件是否被执行。
- **分支覆盖率**：度量if和switch语句的分支覆盖情况，计算一个方法里面的总分支数，确定执行和不执行的 分支数量。
- **方法覆盖率**：度量被测程序的方法执行情况，是否执行取决于方法中是否有至少一个指令被执行。
- **指令覆盖**：计数单元是单个java二进制代码指令，指令覆盖率提供了代码是否被执行的信息，度量完全 独立源码格式。
- **圈复杂度**：在（线性）组合中，计算在一个方法里面所有可能路径的最小数目，缺失的复杂度同样表示测 试案例没有完全覆盖到这个模块。

### 三. Jacoco原理
Jacoco<font color=red size=3>基于ASM技术,通过注入或修改</font> 来实现字节码插桩。其中，字节码插桩又可以分为两种模式On-The-Fly和Offline。  
Jacoco技术概述图：

![Jacoco技术体系](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_jacoco/Jacoco%E6%8A%80%E6%9C%AF%E4%BD%93%E7%B3%BB.PNG)

##### 1. On-The-Fly插桩 Java Agent

- JVM中通过-javaagent参数指定特定的jar文件，启动Instrumentation的代理程序；  
- 代理程序在每装载一个class文件前判断是否已经给该文件插入探针，如果没有则需要将探针插入class文件中；
- 代码覆盖率就可以在JVM执行代码的时候实时获取。
- 典型代表：Jacoco

##### 2. On-The-Fly插桩 Class Loader

- 自定义classloader实现自己的类装载策略，在类加载之前将探针插入class文件中。
- 典型代表：Emma 
> Tip：Emma和Jacoco是同一个团队开发的，但后期不再维护Emma。

##### 3. Offine插桩
- 在测试之前先对文件进行插桩，生成插过桩的class文件或者jar包，执行插过桩的class文件或者jar包之后，会生成覆盖率信息到文件，最后统一对覆盖率信息进行处理，并生成报告。
- Offline插桩又分为两种：
> &nbsp;&nbsp;&nbsp;&nbsp; Replace：修改字节码生成新的class文件  
> &nbsp;&nbsp;&nbsp;&nbsp; Inject：在原有字节码文件上进行修改
- 典型代表：Jacoco、Cobertura

##### 4. On-the-fly和offline比较：

（1）On-the-fly模式更方便简单进行代码覆盖分析，无需提前进行字节码插桩，无需考虑classpath 的设置。

（2）存在如下情况不适合on-the-fly，需要采用offline提前对字节码插桩：

- 运行环境不支持java agent。
- 部署环境不允许设置JVM参数。
- 字节码需要被转换成其他的虚拟机如Android Dalvik VM。
- 动态修改字节码过程中和其他agent冲突。
- 无法自定义用户加载类。

##### 5. 常见的探针插入位置
（1）序列：将探针简单的插在两条顺序指令之间

![序列](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_jacoco/%E5%BA%8F%E5%88%97.PNG)

（2）无条件跳转：在跳转之前插入探针 

![无条件跳转](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_jacoco/%E6%97%A0%E6%9D%A1%E4%BB%B6%E8%B7%B3%E8%BD%AC.PNG)

（3）有条件跳转：在跳转命令之后插入探针  

![有条件跳转](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_jacoco/%E6%9C%89%E6%9D%A1%E4%BB%B6%E8%B7%B3%E8%BD%AC.PNG)

（4）出口：在return或throw之前插入探针

![出口](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_jacoco/%E5%87%BA%E5%8F%A3.PNG)

