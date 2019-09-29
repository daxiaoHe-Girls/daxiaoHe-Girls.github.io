- [1. Redis数据类型](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/%E4%B8%AD%E9%97%B4%E4%BB%B6/Redis.md#1redis%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)
- [2. Redis中value数据类型的具体实现——底层数据类型——C语言编写](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/%E4%B8%AD%E9%97%B4%E4%BB%B6/Redis.md#2redis%E4%B8%ADvalue%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%85%B7%E4%BD%93%E5%AE%9E%E7%8E%B0%E5%BA%95%E5%B1%82%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8Bc%E8%AF%AD%E8%A8%80%E7%BC%96%E5%86%99)
- [3. mySQL里有2000w数据，redis中只存20w的数据，如何保证redis中的数据都是热点数据](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/%E4%B8%AD%E9%97%B4%E4%BB%B6/Redis.md#3mysql%E9%87%8C%E6%9C%892000w%E6%95%B0%E6%8D%AEredis%E4%B8%AD%E5%8F%AA%E5%AD%9820w%E7%9A%84%E6%95%B0%E6%8D%AE%E5%A6%82%E4%BD%95%E4%BF%9D%E8%AF%81redis%E4%B8%AD%E7%9A%84%E6%95%B0%E6%8D%AE%E9%83%BD%E6%98%AF%E7%83%AD%E7%82%B9%E6%95%B0%E6%8D%AE)
- [4. redis和memcached的区别](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/%E4%B8%AD%E9%97%B4%E4%BB%B6/Redis.md#4redis%E5%92%8Cmemcached%E7%9A%84%E5%8C%BA%E5%88%AB)
- [5. Redis集群](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/%E4%B8%AD%E9%97%B4%E4%BB%B6/Redis.md#5redis%E9%9B%86%E7%BE%A4)
- [6. redis持久化](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/%E4%B8%AD%E9%97%B4%E4%BB%B6/Redis.md#6redis%E6%8C%81%E4%B9%85%E5%8C%96)
- [7. redis应用场景](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/%E4%B8%AD%E9%97%B4%E4%BB%B6/Redis.md#7redis%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF)

### 1.	Redis数据类型
Redis 数据库里面的每个键值对（key-value） 都是由对象（object）组成的：  
键——总是一个字符串对象（string object）  
值——字符串对象、列表对象、哈希对象、集合对象、有序集合这五种对象中的一种  
- string（字符串）
> 一个key对应一个value

- list（列表）
> 字符串列表，按照插入顺序排序

- hash（哈希）
> 键值(key=>value)对集合

- set（集合）
> string类型的无序集合，集合保证元素唯一性——哈希表实现，添加，删除，查找的复杂度都是O(1)

- zset(sorted set：有序集合)
> string类型元素的集合，且不允许重复的成员——每个元素都会关联一个double类型的分数，通过分数来为集合中的成员进行从小到大的排序，zset的成员是唯一的，但分数(score)却可以重复

### 2.	Redis中value数据类型的具体实现——底层数据类型——C语言编写
##### (1)简单动态字符串（simple dynamic string）SDS

![SDS](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_redis/SDS.PNG)
 
相比于C语言中的字符串，SDS的空间分配策略完全杜绝了缓冲区溢出的可能性。
> 原因：
> 当需要对一个SDS 进行修改的时，redis 会在执行拼接操作之前，预先检查给定SDS空间是否足够，如果不够，会先拓展SDS的空间，然后再执行拼接操作

##### (2)字典 / 符号表 / 关联数组 / 映射(map)——保存键值对的抽象数据结构
 
![字典](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_redis/%E5%AD%97%E5%85%B8.PNG) 
 
①解决hash冲突——链地址法 / 拉链法  
②渐进式rehash——不是一次性、集中完成，而是分多次、渐进完成，避免了集中式的庞大计算量  

主要流程：根据拓展或收缩，设定h[1]大小；将ht[0]中的数据，重新计算哈希值，转移到ht[1]中；将ht[0]释放，然后将ht[1]设置成ht[0]，最后为ht[1]分配一个空白哈希表
### (3)链表
 
![链表](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_redis/hash%E8%A1%A8.PNG) 

### (4)跳跃表——用于有序集合键

![跳跃表](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_redis/%E8%B7%B3%E8%B7%83%E8%A1%A8.PNG)
 
##### 提出原因：
- 普通链表：检索时间复杂度O(n)
- 跳跃表：多层结构，从最高维度的表进行检索再逐渐降低维度，从而效率和平衡树媲美 ——查找、删除、添加等操作都可以在对数期望时间下完成

##### (5)整数集合
集合键的底层实现之一，当一个集合中只包含整数，且元素数量不多时，redis就会使用整数集合intset作为集合的底层实现

![整数集合](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_redis/%E6%95%B4%E6%95%B0%E9%9B%86%E5%90%88.PNG)
 
整数集合的升级：  
存入的整数不符合整数集合中的编码格式时，Redis 采用升级策略来解决：  
一开始插入的整数都是16位整数（就用16位存），当插入32位整数时，全体用32位存

##### (6)压缩列表
##### (7)对象
 
### 3.	mySQL里有2000w数据，redis中只存20w的数据，如何保证redis中的数据都是热点数据
redis内存数据集到一定大小时，会施行数据淘汰策略。redis 提供 6种数据淘汰策略：
- ##### voltile-lru
> 从已设置过期时间的数据集server.db[i].expires中挑选最近最少使用数据淘汰

- ##### volatile-ttl
> 从已设置过期时间的数据集server.db[i].expires中挑选将要过期的数据淘汰

- ##### volatile-random
> 从已设置过期时间的数据集server.db[i].expires中任意选择数据淘汰

- ##### allkeys-lru
> 从数据集（server.db[i].dict）中挑选最近最少使用的数据淘汰

- ##### allkeys-random
> 从数据集（server.db[i].dict）中任意选择数据淘汰

- ##### no-enviction（驱逐）
> 禁止驱逐数据

### 4.	redis和memcached的区别
- **数据类型**：memcached的值都是简单字符串；Redis支持更丰富的数据类型；
- **虚拟内存**：Redis当物理内存用完时，可以将一些很久没用到的value 交换到磁盘；
- **持久化**：memcached挂掉后，数据没了；redis可以定期保存到磁盘（持久化）；
- **灾难恢复**：memcached挂掉后，数据不可恢复；redis数据丢失后可以通过aof恢复；
- **应用场景不同**：Redis除了作为NoSQL数据库，还能用做消息队列、数据堆栈和数据缓存等；Memcached适合于缓存SQL语句、数据集、用户临时性数据、延迟查询数据和session等。

### 5.	Redis集群
##### (1)二八定律
网站的访问特点遵循二八定律：80%的业务访问集中在20%的数据上。  

既然大部分业务访问集中在一小部分数据上，把这一小部分数据缓存在内存中，就可以减少数据库的访问压力，提高整个网站的数据访问速度。缓存主要用来存放读写比很高、很少变化的数据。

##### (2)Redis集群
##### ①主从同步
- 读写分离，尽管增加Slaver可以提高并发读的能力，但是——Master写能力是瓶颈；  
- Master维护Slaver开销会变成瓶颈；  
- Master的Disk大小也会成为整个Redis集群存储容量的瓶颈。

##### ②分库分表 / 分片模型（哈希slot）
&nbsp;&nbsp;&nbsp;&nbsp; Sharding（分片）的基本思想是把一个数据库切分成多个部分放到不同的服务器的数据库上，从而缓解单一数据库的性能问题。

![Redis集群](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_redis/Redis%E9%9B%86%E7%BE%A4.PNG)
 
##### ③读写分离模型和数据分片模型互补
&nbsp;&nbsp;&nbsp;&nbsp; 每一套主从看作一个Node，就是Redis集群的终极形态，先hash分逻辑节点，然后每个逻辑节点内部是主从。  

比如：当根据sessionId计算得到的hash值=0~5460，相应的session都存在Master1及其小弟服务器  
- S1：哈希slot：对象保存到redis之前，先经过CRC16哈希到一个指定的master上面，写；  
- S2：主从分离，读

##### (3)一致性hash
**a)	背景**
> 设计分布式缓存集群，需要考虑集群的伸缩性，也就是当向集群(已有n台)中增加服务器的时，普通hash算法导致不命中率 n/(n+1)。一致性hash算法就是用来解决集群伸缩性

**b)	原理**  
&nbsp;&nbsp;&nbsp;&nbsp; 一致性hash算法通过构造一个长度为2^32的整数环，根据节点名的hash值将缓存服务器节点放置在这个环上，然后计算要缓存的数据的key的hash值，顺时针找到最近的服务器节点，将数据放到该服务器上。  

**c)	伸缩性**  
&nbsp;&nbsp;&nbsp;&nbsp; 当有新的节点NODE3进来的时候，或者有一台机器down掉的时候，只需要rehash部分的key，而不需要全部rehash。  

**d)	虚拟IP**  
&nbsp;&nbsp;&nbsp;&nbsp; Redis也增加了虚拟IP，和真是IP之间只存在对应关系，实际上都存储到了真实IP中，这是为了平衡性，使值分散的更均匀。

![一致性hash](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_redis/%E4%B8%80%E8%87%B4%E6%80%A7hash.PNG)

 

### 6.	redis持久化
 
RDB持久化也称为快照持久化。

![持久化](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/images_redis/%E6%8C%81%E4%B9%85%E5%8C%96.PNG)

### 7.	redis应用场景
1)	会话缓存（最常用）；
2)	消息队列（支付）；
3)	活动排行榜或计数；
4)	发布、订阅消息（消息通知）；
5)	商品列表、评论列表等。

