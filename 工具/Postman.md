- [1. Postman功能](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/%E5%B7%A5%E5%85%B7/Postman.md#1-Postman功能)
- [2. Postman的简单使用(抓包示例)](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/%E5%B7%A5%E5%85%B7/Postman.md#2-postman%E7%9A%84%E7%AE%80%E5%8D%95%E4%BD%BF%E7%94%A8%E6%8A%93%E5%8C%85%E7%A4%BA%E4%BE%8B)
- [3. Postman进阶：Collections的使用](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/%E5%B7%A5%E5%85%B7/Postman.md#3-postman%E8%BF%9B%E9%98%B6collections%E7%9A%84%E4%BD%BF%E7%94%A8)
- [4. Postman进阶：环境变量](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/%E5%B7%A5%E5%85%B7/Postman.md#4-postman%E8%BF%9B%E9%98%B6%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
- [5. Postman进阶：全局变量](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/%E5%B7%A5%E5%85%B7/Postman.md#5-postman%E8%BF%9B%E9%98%B6%E5%85%A8%E5%B1%80%E5%8F%98%E9%87%8F)


# 1. Postman功能

&nbsp;&nbsp;&nbsp;&nbsp;
抓包，主要是HTTP包

# 2. Postman的简单使用(抓包示例)

##### 如下图 

<div align="center">
    <img src="https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_postman/postman%E7%AE%80%E5%8D%95%E6%8A%93%E5%8C%85.png" width="650" hegiht="650" />
</div>

- **S1**:设置请求类型，是get请求还是post请求等（chrome浏览器提供了开发者工具，可以通过开发者工具抓http包，但chrome浏览器只允许用户发送get请求）
- **S2**: 给定请求的url
- **S3**: 请求设置

```
Params —— get请求如果有参数的化，可以在此以键值对的形式输入
Headers —— 请求头的设置，也是以键值对的形式输入
Body —— 通常是用于post请求的参数输入，支持多种形势输入，包括键值对、原始形势、二进制文件等
Authentication —— 身份验证
                Basic Auth：直接把用户名、密码的信息放在请求的 Header 中；
                Digest Auth： 使用当前填写的值生成authorization header；
Pre-request Script —— 请求前需要执行的脚本，支持js脚本
Tests —— 对于该请求的测试脚本
```
- **S4**: 发送请求
- **S5**: 发送请求之后，对应的response就会在S5区域显示，可以查看具体的response内容，包括Cookie，headers等；同时可以这只response的显示格式，包括json，xml，html等。

##### 到此，一个包即抓取完毕。

# 3. Postman进阶：Collections的使用

Collections是一个集合的概念，也可以理解为是一个文件夹，可以存放多个请求，我们也可以为其命名和添加描述。

##### 如下图

<div align="center">
    <img src="https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_postman/Collections1.png" width="750" hegiht="750" />
</div>

- **S1**: 新建一个Collection
- **S2**: 对于Collection的操作，点击箭头末端的小三角形，可以显示操作当前Collection的工具栏。然后点击箭头首端的三个点，可以显示具体工具，如下图。提供的工具包括：请求创建，文件夹创建，删除当前Collection等。另外，点击“Run” 可以同时执行该Collection里面的所有请求。

<div align="center">
    <img src="https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_postman/Collections2.png" width="750" hegiht="750" />
</div>

- **S3**: Collection内部的显示

# 4. Postman进阶：环境变量
什么是环境变量？是特定的环境下引用的变量，必须要制定对应的环境才能引用到其中的变量。

- 第一步： 环境变量添加的位置，点击箭头指向的“设置”按钮即可添加

<div align="center">
    <img src="https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_postman/Environment1.png" width="750" hegiht="750" />
</div>

-  第二步: 点击add

<div align="center">
    <img src="https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_postman/Environment2.png" width="750" hegiht="750" />
</div>

- 第三步: 
    - S1: 指定环境名
    - S2: 以键值对的方式设置该环境名下的环境变量

<div align="center">
    <img src="https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_postman/Environment3.png" width="750" hegiht="750" />
</div>

- 第四步:使用环境变量
    - S1: 指定环境名
    - S2: 用"{{}}" 的方式使用环境变量

<div align="center">
    <img src="https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_postman/Environment4.png" width="750" hegiht="750" />
</div>

# 5. Postman进阶：全局变量
什么是全局变量？postman只可以设置一组全局变量，作用于整个postman，可以直接引用全局变量中的变量，而不用指定环境。

- 第一步： 全局变量添加的位置，点击箭头指向的“设置”按钮即可添加

<div align="center">
    <img src="https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_postman/Environment1.png" width="750" hegiht="750" />
</div>

-  第二步: 点击Globals

<div align="center">
    <img src="https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_postman/Environment2.png" width="750" hegiht="750" />
</div>

- 第三步: 以键值对的方式设置全局变量

<div align="center">
    <img src="https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_postman/Globals1.png" width="750" hegiht="750" />
</div>

- 第四步: 用"{{}}" 的方式使用全局变量，不需要指定当前环境

<div align="center">
    <img src="https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_postman/Globals2.png" width="750" hegiht="750" />
</div>

> 变量优先级：环境变量>全局变量
