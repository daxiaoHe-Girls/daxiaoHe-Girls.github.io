



# 1. 背景
&nbsp;&nbsp;&nbsp;&nbsp;
客户端需要向服务端获取数据的时候，通常会发送一个请求，比如http请求，服务器端在接收到这个请求之后，会进行响应，并返回客户端需要的数据。**一般情况下，即使服务器端知道了客户端的IP，服务器端也不能主动向客户端发送消息，因为客户端几乎也不会监听端口接受服务器发来的消息。**  

&nbsp;&nbsp;&nbsp;&nbsp;
**服务器端向客户端发送数据的过程，一般称之为“推送”**。  

&nbsp;&nbsp;&nbsp;&nbsp;
目前，很多网站采用的是Ajax轮询去实现推送技术。轮询是在特定的的时间间隔（如每1秒），由浏览器对服务器发出HTTP请求，然后由服务器返回最新的数据给客户端的浏览器（如图1）。这种传统的模式带来很明显的缺点，即浏览器需要不断的向服务器发出请求，然而HTTP请求可能包含较长的头部，其中真正有效的数据可能只是很小的一部分，显然这样会浪费很多的带宽等资源。

**图1: ajax轮询事例**

<img src="https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_WebSocket/AJAX%E8%BD%AE%E8%AF%A2.png" width="400" hegiht="300" align=center />

&nbsp;&nbsp;&nbsp;&nbsp;
**长轮询**: 客户端向服务器请求信息，并在设定的时间段内打开一个连接，服务器如果没有任何信息，会保持请求打开，直到有客户端可用的信息，或者直到指定的超时时间用完为止。这个时候，客户端会重新向服务器请求信息。



# 2. Websocket是什么？

- **名词解释**  

&nbsp;&nbsp;&nbsp;&nbsp;
 是 HTML5 开始提供的一种在==单个 TCP 连接==上进行全双工通讯的协议。在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。  
 

- **特点**
    -  服务端可以主动向客户端推送数据
    -  但是连接的建立只能由客户端发起
    -  它能更好的节省服务器资源和带宽，并且能够更实时地进行通讯

**图2: websocket事例**  

<div align="center">
    <img src="https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_WebSocket/ws%E5%9B%BE%E7%A4%BA.png" width="400" hegiht="300"/>
</div> 


- **通信机制** 

&nbsp;&nbsp;&nbsp;&nbsp;
如图2所示：WebSocket 协议本质上是一个基于 TCP 的协议。为了建立一个 WebSocket 连接，客户端浏览器首先要向服务器发起一个 HTTP请求，这是一次特殊的Http请求，为什么是一次特殊的Http请求呢？Http请求头中Connection:Upgrade Upgrade:websocket,Upgrade代表升级到较新的Http协议或者切换到不同的协议。很明显WebSocket使用此机制以兼容的方式与HTTP服务器建立连接。  

&nbsp;&nbsp;&nbsp;&nbsp;
WebSocket协议有两个部分：握手建立升级后的连接，然后进行实际的数据传输。首先，客户端通过使用Upgrade: WebSocket和Connection: Upgrade头部以及一些特定于协议的头来请求WebSocket连接，以建立正在使用的版本并设置握手。服务器，如果它支持协议，回复与相同Upgrade: WebSocket和Connection: Upgrade标题，并完成握手。握手完成后，数据传输开始。
                 

# 3. WebSocket原理（Wireshark抓包分析）

> Wireshark（前称Ethereal）是一个网络封包分析软件。网络封包分析软件的功能是撷取网络封包，并尽可能显示出最为详细的网络封包资料。  
Wireshark使用WinPCAP作为接口，直接与网卡进行数据报文交换。

**图3: websocket的建立连接+数据传输+关闭连接**

![ws协议总过程](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_WebSocket/ws%E5%8D%8F%E8%AE%AE%E6%80%BB%E8%BF%87%E7%A8%8B.png)

> TCP是传输层协议，HTTP是应用层协议，所以，NO.12,14是一组，NO.15, 16是一组,

### （1）第一步，连接建立  

ws连接建立包括两个部分
- TCP三次握手：(图3中 NO.8, 10, 11)
- 协议升级：一次特殊的http请求 (图3中 NO.12, 14, 15, 16)

##### S1
&nbsp;&nbsp;&nbsp;&nbsp;
TCP第一次握手，从下图可以看出，SYN（连接建立）=1，ACK（应答包）=0

![tcp连接1](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_WebSocket/tcp%E8%BF%9E%E6%8E%A51.png)

##### S2
&nbsp;&nbsp;&nbsp;&nbsp;
TCP第二次握手，从下图可以看出，SYN（连接建立）=1，ACK（应答包）=1

![tcp连接2](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_WebSocket/tcp%E8%BF%9E%E6%8E%A52.png)

##### S3
&nbsp;&nbsp;&nbsp;&nbsp;
TCP第三次握手，从下图可以看出，SYN（连接建立）=0，ACK（应答包）=1

![tcp连接3](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_WebSocket/tcp%E8%BF%9E%E6%8E%A53.png)

##### S4

- 特殊的http请求，客户端告诉服务端，“我”想将请求升级为websocket。  
&nbsp;&nbsp;&nbsp;&nbsp;
- 该请求中，"Connection"字段对应的值告诉服务端，这是一个请求升级包；"Upgrade"字段对应的值告诉服务端，“我”想升级到哪种请求，如果服务端可以升级到改种请求，就会同样返回个"Upgrade"

![协议升级源到目的](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_WebSocket/%E5%8D%8F%E8%AE%AE%E5%8D%87%E7%BA%A7%E6%BA%90%E5%88%B0%E7%9B%AE%E7%9A%84.png)


##### S5
- 特殊的http请求，服务端告诉服务端，“我”同意将请求升级为websocket。  
&nbsp;&nbsp;&nbsp;&nbsp;
- 该请求中，"Connection"字段对应的值告诉服务端，这是一个请求升级包；"Upgrade"字段对应的值告诉服务端，“我”想升级到哪种请求，如果服务端可以升级到改种请求，就会同样返回个"Upgrade"

![协议升级目的到源](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_WebSocket/%E5%8D%8F%E8%AE%AE%E5%8D%87%E7%BA%A7%E7%9B%AE%E7%9A%84%E5%88%B0%E6%BA%90.png)


##### S6
&nbsp;&nbsp;&nbsp;&nbsp;
有兴趣的可以深入了解一下ws数据包格式

![ws数据传输](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_WebSocket/ws%E6%95%B0%E6%8D%AE%E4%BC%A0%E8%BE%93.png)

##### S7
&nbsp;&nbsp;&nbsp;&nbsp;
ws关闭。从抓的包中可以看到，ws首先会进行双方共两次ws连接关闭。然后进行正常的TCP四次挥手。

首先，客户端向服务端发起ws关闭请求，FIN：True

![ws关闭源到目的](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_WebSocket/ws%E5%85%B3%E9%97%AD1.png)

然后，服务端接对收到客户端的ws关闭请求进行响应，FIN：True

![ws关闭目的到源](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_WebSocket/ws%E5%85%B3%E9%97%AD2.png)


##### S8 
&nbsp;&nbsp;&nbsp;&nbsp;
TCP四次正常挥手

![tcp四次挥手](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_WebSocket/tcp%E5%9B%9B%E6%AC%A1%E6%8C%A5%E6%89%8B.png)







