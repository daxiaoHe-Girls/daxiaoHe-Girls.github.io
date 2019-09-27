- [WireShark for MAC](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/%E5%B7%A5%E5%85%B7/Wireshark.md#1-wireshark-for-mac)
- [常用过滤条件](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/%E5%B7%A5%E5%85%B7/Wireshark.md#2-%E5%B8%B8%E7%94%A8%E8%BF%87%E6%BB%A4%E6%9D%A1%E4%BB%B6)

# 1. WireShark for MAC  

&nbsp;&nbsp;&nbsp;&nbsp;
Wireshark（前称Ethereal）是一个网络封包分析软件。网络封包分析软件的功能是撷取网络封包，并尽可能显示出最为详细的网络封包资料。Wireshark使用WinPCAP作为接口，直接与网卡进行数据报文交换。


![Wireshark的使用](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_Wireshark/Wireshark的使用.png)

# 2. 常用过滤条件

- 目的IP

```
ip.dst == 10.24.58.22   筛选目的IP是10.24.58.22 的包
```
- 源IP

```
ip.src == 10.24.58.22   筛选源IP是10.24.58.22 的包
```
- 多个条件： and（与），or（或）



