### 1．	数据库的四大特征(事务的四大特征)是什么？什么是事务？隔离性指什么？原子性指什么？ 
(1)	事务：一系列操作序列构成的执行单元，这些单元要么都做，要么都不做，是一个不可分割的工作单元。  
(2)	事务的四大特性**ACID：原子性、一致性、隔离性、持久性**  
**①原子性**：事务中包含的所有操作要么全做，要么回滚全不做；  
**②一致性**：事务执行前后数据库都必须处于一致性状态；  
> eg : 拿转账来说，假设用户A和用户B两者的钱加起来一共是5000，那么不管A和B之间如何转账，转几次账，事务结束后两个用户的钱相加起来应该还得是5000，这就是事务的一致性。

**③隔离性**：当多个用户并发访问数据库时，数据库为每一个用户开启事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离；  
**④持久性**：类似于持久化，一个事务一旦成功完成，它对数据库的改变必须是永久的，即使是在系统遇到故障的情况下也不会丢失。

### 2．	事务的隔离级别
(1)	SQL定义了**四种**隔离级别，用来限定事务内哪些数据是可见的。隔离级别越低，一致性越差，并发性越好，但是随之而来的就是带来的数据安全性问题。

(2)	数据安全性问题：  
&nbsp;&nbsp;&nbsp;&nbsp;  1）**脏读**：一个事务读取了另一事务已修改未提交的字段  
&nbsp;&nbsp;&nbsp;&nbsp;  2)	**不可重复读**：一个事务内多次查询却返回了不同的数据值，这是由于在查询间隔，被另一个事务修改并提交了  
&nbsp;&nbsp;&nbsp;&nbsp;  3)	**幻读**：一个事务处理过程中，另一事务插入新的行，之前的事务发现多出来的行，仿佛幻觉  

(3)	4个隔离级别：
事务1——>写草稿——> 原数据 <——事务2  
&nbsp;&nbsp;&nbsp;&nbsp; 1)	**Read Uncommitted（未提交读）**  
> 事务1可以提交更新、插入、删除，事务2可以看到草稿：所有事务都可以看到其他未提交事务的执行结果，读取未提交的数据。 

&nbsp;&nbsp;&nbsp;&nbsp; 2)	**Read Committed（提交读）——可避免脏读**  
> 事务1可以提交更新、插入、删除，事务2只能看原数据：大多数数据库系统的默认隔离级别，事务提交之前对其余事务不可见。

&nbsp;&nbsp;&nbsp;&nbsp; **3)	Repeatable Read（可重复读）——可避免脏读、不可重复读**  
> 事务1可以提交插入、删除，事务2只能看原数据：mysql的默认事务隔离级别，一个事务需多次从某字段读取数据，此事务持续期间，禁止其他事务更新该字段。
> InnoDB和Falcon存储引擎通过多版本并发控制(MVCC，Multiversion Concurrency Control)机制解决了该问题 

&nbsp;&nbsp;&nbsp;&nbsp; **4)	Serializable（可串行化）——避免脏读、不可重复读、幻读**  
> 一个事务持续期间，禁止其他事务的插入、更新、删除：最高的隔离级别，它强制所有事务都是串行执行的，使之不可能相互冲突，从而解决幻读问题。它是在读的每个数据行上加上共享锁。在这个级别，可能导致大量的超时现象和锁竞争。

### 3．	mysql的授权的命令是什么？怎么取消权限？
(1)	授权的命令是：grant命令

```
grant 权限 on 数据库对象 to 用户 
```
 
(2)	取消权限的命令是：revoke命令

### 4．	存储引擎MyISAM与InnoDB基本区别
(1)	数据库引擎是一段程序，各引擎的存储机制、索引技巧、锁定水平不同  
(2)	mysql最常使用的两种引擎：MyISAM ， InnoDB  
(3)	<font color=red size=3>MyISAM  VS  InnoDB</font>  
&nbsp;&nbsp;&nbsp;&nbsp; 1)	**MyISAM不支持事务，InnoDB支持事务**  
&nbsp;&nbsp;&nbsp;&nbsp; 2)	**MyISAM不支持行锁定，只支持锁定整个表；InnoDB支持数据行锁定。**  
> 注意：InnoDB表的行锁不是绝对的，假如在执行一个SQL语句时mysql不能确定要扫描的范围，InnoDB表同样会锁全表，例如update table set num=1 where name like “%aaa%” 

&nbsp;&nbsp;&nbsp;&nbsp; 3)	**MyISAM不支持外键，InnoDB支持外键**  
&nbsp;&nbsp;&nbsp;&nbsp; 4)	**MyISAM保存表的具体行数，InnoDB不保存表的具体行数**
> 执行select count() from table时，InnoDB要扫描整个表来计算行数；MyISAM读出保存好的行数
执行count()语句包含 where条件时，两种表的操作是一样的

&nbsp;&nbsp;&nbsp;&nbsp; 5)	MyISAM只使用**非聚集索引**，增删查效率较高；InnoDB主键使用**聚集索引**，修改效率较高  
&nbsp;&nbsp;&nbsp;&nbsp; 6)	MyISAM索引和数据分离，索引压缩存储；InnoDB索引和数据紧密结合，不支持压缩，所以MyISAM内存利用率比InnoDB高  
&nbsp;&nbsp;&nbsp;&nbsp; 7)	注：MyISAM、InnoDB现在都支持全文本搜索 

### 5．	聚集函数

```
select 聚集函数(列名) AS 别名 from 表名;
SELECT SUM(price) AS sum_price FROM account WHERE num='4';
SELECT SUM(DISTINCT price) AS sum_price FROM account;
SELECT COUNT(*)AS count_acc,SUM(price) AS sum_price, MAX(price) AS max_price FROM account;
```
(1)	默认考虑重复列：函数(ALL(列名))，可指定：函数(DISTINCT(列名))  
(2)	count()		返回某列行数，count(*)统计所有行，count(列名)忽略null行  
(3)	sum()		返回某列值之和，忽略null行  
(4)	avg()		返回某列均值，只可用于单列，多列均值需用多次avg()  
(5)	max()		返回某列最大值，忽略null行  
(6)	min()		返回某列最小值，忽略null行 

### 6．	子查询：查询A作为查询B的条件，称A为子查询

```
select * from … where 列名 > (select * from ... where 条件);
```
- 比较操作符 any(子查询) —— 比较的时候**符合其中某一个**  
- 比较操作符 all(子查询) —— 比较的时候**必须全部符合**
- some	等价于 any  
- in		等价于 any		等价于 exists  
- not in	等价于 <>all	等价于 notexists  

### 7．	联结：单条select语句进行多表查询；联结表越多，性能下降越厉害
##### (1)	内部联结(等值联结)：结果仅包含符合连接条件的两表中的行

```
select 列a, 列b, 列c					select 列a，列b，列c
from 表1, 表2					或		from 表1 inner join 表2
where 表1.列b = 表2.列b;				on 表1.列b=表2.列b
```
##### (2)	自联结：一张表起2个别名

```
select 列a, 列b, 列c
from 表1 as 表别名1, 表1 as 表别名2
where 表别名1.列b = 表别名2.列b;
```
##### (3)	交叉联结（笛卡尔积）：
返回左表中所有行与右表中所有行的组合，行数 = 两表行数之积
##### (4)	外部联结
**INNER JOIN** 两边表同时有对应的数据，即任何一边缺失数据就不显示。  
**LEFT JOIN** 会读取左边数据表的全部数据，即便右边表无对应数据。  
**RIGHT JOIN** 会读取右边数据表的全部数据，即便左边表无对应数据。  
**①	左外连接(left join)：**
> 左表全部行+右表匹配行，如果左表中某行在右表中没有匹配的行，则右表该行显示null  

**②	右外连接(right join)：**
> 和左外连接相反
```
select 列a, 列b, 列c						select 列a, 列b, 列c
from 表1 left outer join 表2				from 表1 right outer join 表2
on …									on …
```

**③	全外连接(full join)：**
> 不管是否匹配，全部显示，左表在右表没有的显示null，右表在左表没有的显示null

```
select 列a, 列b, 列c						select 列a, 列b, 列c
from 表1 left outer join 表2				from 表1 right outer join 表2
on …									on …
```

```
例：
A表							B表
Id	Name		Id	Age	Parent_id
1	张三		    66	23	1
2	李四		    88	34	2
3	王五	    	99	34	5
A表的id和B表的 parent_id关联
1)内连接
    select A.*, B.* 
    from A inner join B
    on A.id=B. parent_id;
    结果：	1  张三 66 23 1
            2  李四 88 34 2
2)左外连接(左表 all 元素都有)
    select A.*, B.* 
    from A left join B
    on A.id=B.parent_id;
    结果：	1  张三  66    23    1
            2  李四  88    34    2
            3  王五  null  null  null
3)右外连接(右表 all 元素都有)
    select A.*, B.*
    from A right join B
    on A.id=B.parent_id;
    结果：	1    张三  66  23  1
            2    李四  88  34  2
            null null  99  34  5
4）完全连接(返回左表和右表中所有行，若某行在另外一个表中无匹配，则另一表的选择列表列包含空值）
    select A.*, B.*
    from A full join B
    on A.id=B.parent_id;
    结果：   1    张三   66     23    1
            2    李四   88     34    2
            3    王五   null   null  null
            null null   99     34    5
```

### 8．	组合查询：将多条select语句查询结果（列应相同）合并，每条select语句间用union/union all
**union**：默认取消重复行；  
**union all**：返回所有匹配行  
union连接两个以上的select语句，每个select必须包含相同的列  和聚集函数，顺序可以不同

```
SELECT student2.sid,student2.sname FROM student2 WHERE sid IN (104,101) 
UNION SELECT student2.sid,student2.sname FROM student2 WHERE sid <= 103 ORDER BY sid; 
-- 101 102 103 104 都会出来结果同or 语句
```
### 9．	操作
##### (1)	数据库的操作
```
mysql -h localhost -u root -p 密码;   -- 登录
CREATE DATABASE 数据库名;            -- 创建数据库
CREATE DATABASE 数据库名 CHARACTER SET charset_name; --以指定的字符集创建数据库
SHOW DATABASES; -- 查看已有数据库
USE 数据库名;    -- 使用数据库
SELECT DATABASE();  -- 查看当前数据库
DROP DATABASE 数据库名;  -- 删除数据库，同时删除该数据库相关的目录及其目录内容
```
##### (2)	表的操作

```
CREATE TABLE account     -- 创建表
(
accId  INT,
num    INT,
price  DOUBLE,
PRIMARY KEY (accId)
);

SHOW TABLES;     -- 查看所有表
SHOW TABLES FROM 库名;
DESCRIBE 表名; -- 显示表列
SHOW COLUMNS FROM 表名;
RENAME TABLE 原表名 TO 新表名     --可对多个表进行重命名
Alter table 表名 add 字段 类型(长度) 约束;	-- 添加字段
alter table 表名 drop 字段;	-- 删除字段
alter table 表名 modify 字段 类型(长度) 约束; -- 修改类型或者约束
alter table 表名 change 旧字段 新字段 类型(长度) 约束;	-- 修改字段的名称 
alter table 表名 character set utf8;	-- 修改字符集
```
#####  (3)	数据库如何创建和删除索引

```
创建索引
①alter table语句创建索引
alter table table_name add index index_name (column_list);#创建普通索引
alter table table_name add unique (column_list); #创建唯一索引
alter table table_name add primary key (column_list);	#创建主键索引
②使用create index语句创建索引
create index index_name on table_name (column_list);	#创建普通索引
create unique index index_name on table_name (column_list); #创建唯一索引

删除索引
drop  index  index_name on table_name;
alter table  table_name drop index index_name;
alter table  table_name drop primary key;
```

#####  (4)	Insert插入

```
insert into 表名 (字段1,字段2,字段3…) values(值1,值2,值3…); #有几列就插入多少的值。
insert into 表名 values(值1,值2,值3…);		#插入所有的列
```
##### (5)	Update更新

```
update 表名 set 字段=值,字段=值... [where ];		#更新表记录
```
##### (6)	Delete删除

```
delete from 表名 [where ];	#删除数据；省略where删除所有行
truncate 表名;
```
truncate 和 delete的区别：  
(1)truncate删除数据：先删除整个表。再创建一个新的空的表（效率高，不可回滚）  
(2)delete删除数据：一条一条删除的  （效率低，可回滚）

##### (7)	Select获取：-- select-from-where-group by-having-order by-limit

```
SELECT DISTINCT student2.sid FROM student2;-- distinct 去重,只distinct 后面跟的属性不重复的指
SELECT * FROM student2 LIMIT 5,5;-- 返回从5行开始的后5行，第一行为行0；省略第一个参数表示从0开始

-- order by 子句的使用 可以根据某列或多列排序
SELECT *  FROM student2 ORDER BY sname;-- 单列排序
INSERT INTO student2 VALUES ('104','adad');
INSERT INTO student2 VALUES ('103','da');
SELECT *  FROM student2 ORDER BY sid,sname;-- 多列排序103在前，先按sid在按sname
SELECT *  FROM student2 ORDER BY sname DESC;-- 按降序排，升序是ASC，默认的
SELECT *  FROM student2 ORDER BY sid DESC,sname;-- sid降序，sname升序
SELECT *  FROM student2 ORDER BY sid DESC,sname LIMIT 1;-- 取出排序后的第一个

-- 过滤数据
-- where子句
SELECT student2.sid,student2.sname FROM student2 WHERE sname='lily1';
-- where 支持=、<>(不等于)、!=、<、<=、>、>=、between
SELECT student2.sid,student2.sname FROM student2 WHERE sid<>103;-- 不等于
SELECT student2.sid,student2.sname FROM student2 WHERE sid BETWEEN 102 AND 103;-- 闭区间
SELECT student2.sid,student2.sname FROM student2 WHERE sid IS NULL;-- 无值查询
-- 组合过滤 AND OR 
SELECT student2.sid,student2.sname FROM student2 WHERE (sid='102' OR SID='101')AND sname='lily1';
-- and 和or 一起的时候and优先级最高，会导致错误组合，故要加上括号

-- in 操作符
SELECT student2.sid,student2.sname FROM student2 WHERE sid IN (102,103);-- sid为102 或者103时候的结果
-- 使用in 相比or更直观,并且一般来说执行更快，in 还可以包含其他select语句

-- not操作符
SELECT student2.sid,student2.sname FROM student2 WHERE sid NOT IN (102,103) ORDER BY sid DESC;

-- 通配符过滤 
-- like
SELECT student2.sid,student2.sname FROM student2 WHERE sname LIKE '%l%';-- %匹配0个、1个或多个字符
SELECT student2.sid,student2.sname FROM student2 WHERE sname LIKE 'l%l%1';
SELECT student2.sid,student2.sname FROM student2 WHERE sname LIKE 'l_l%';-- -只能匹配一个字符，不多不少
-- 注尽量不要把通配符放在匹配模式的开始，这样搜索起来最慢

-- 正则表达式 regexp 后面加正则表达式 .匹配任意一个字符；|两个之一;[]匹配多个字符之一;匹配特殊字符要用\\;pi
SELECT student2.sid,student2.sname FROM student2 WHERE sname REGEXP 'lil';
SELECT student2.sid,student2.sname FROM student2 WHERE sname REGEXP 'lil|lul';
SELECT student2.sid,student2.sname FROM student2 WHERE sname REGEXP 'l[i u]l';
SELECT student2.sid,student2.sname FROM student2 WHERE sid REGEXP '[1-2]';

-- 拼接concat
SELECT CONCAT (student2.sid,'(',student2.sname,')') FROM student2;-- 输出如101（lily1)
SELECT CONCAT (student2.sid,'(',student2.sname,')') AS infor FROM student2;-- 使用别名，列名为infor


-- 数据分组group by 和having
-- 统计同一个sid的人的个数
SELECT sid,COUNT(*) AS count_sid FROM student2 GROUP BY sid;
-- sid   count_sid
-- 101    4
-- 102    2
-- 103    1

-- group by 子句在where子句之后，order by 子句之前，并且不使用别名，不能为聚集函数
-- 过滤指定分组 （只选出分组人数大于2个的组，不能使用where它没有分组的概念）
SELECT sid,COUNT(*) AS count_sid FROM student2 GROUP BY sid HAVING COUNT(*)>=2;
SELECT sid,COUNT(*) AS count_sid FROM student2  WHERE sid<105 GROUP BY sid HAVING COUNT(*)>=1 ORDER BY count_sid;
```
### 10．	mysql的视图怎么理解？
##### （1）视图是什么
视图是对若干张基本表的引用，本质上是一张虚拟的表，获取查询的结果，但是不存储具体的数据(更改基本表的数据，视图也会跟着改变)。视图就是一条SELECT语句执行后返回的结果集。
##### （2）视图的作用
①一般用于检索，重用sql语句，简化复杂的sql操作；  
②更安全：使用表的组成部分而不是整个表，数据库授权命令不能限定到特定行和特定列，但是通过合理创建视图，可以把权限限定到行列级别。  
##### （3）应用
①权限控制：不希望用户访问表中某些含敏感信息的列  
②关键信息来源于多个复杂关联表，可以创建视图提取我们需要的信息，简化操作。

```
-- 创建视图 create view
-- 查看 show create view viewname
-- 删除 drop
-- 更新时可以先用drop在用create 或者create or replace view
 CREATE VIEW show_stu1 AS  SELECT sid,COUNT(*) AS count_sid FROM student2  WHERE sid<105 GROUP BY sid HAVING COUNT(*)>=1 ORDER BY count_sid;
SELECT * FROM show_stu1;
```

### 11.	数据库中的五大约束，前3个是单表约束：
（1）**主键约束（Primay Key）** :唯一性，非空性  
> 在一张表中通过主键就能准确定位到一行；主键不能有重复且不能为空

（2）**唯一约束 （Unique）唯一性**：可以空，但只能有一个
类似于主键，即表中字段值不能重复出现  
（3）**非空约束（Not Null）** 设置非空约束，该字段不能为空  
（4）**默认约束 （Default）** 设置该字段的数据的默认值
> 如果没有给这个字段赋值，数据库系统会自动为这个字段插入默认值

（5）**外键约束（Foreign Key）** 需要建立两表间的关系
外键主要用来保证数据的完整性和一致性

### 12.	数据库三大范式（设计时所遵循的规范）
**码（候选码）**—— 唯一确定表中一行；一个表可以有多个候选码  
**主属性** 		—— 包含在任何一个码中的属性

(1)**第一范式1NF**：列不可分，即列的原子性，列不能够再分成其他几列
 
(2)**第二范式2NF**：不能产生部分依赖，即非主属性列必须完全依赖于候选码，而不能只依赖于候选码的部分函数。单关键字的数据库表都符合第二范式。

> 注：假定选课关系表为SelectCourse(学号, 姓名, 年龄, 课程名称, 成绩, 学分)，关键字为组合关键字  
> (学号, 课程名称)，存在如下决定关系：  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   (学号, 课程名称) → (姓名, 年龄, 成绩, 学分)  
> 这个数据库表不满足第二范式，因为存在如下决定关系：  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (课程名称) → (学分)      (学号) → (姓名, 年龄)  
> 存在数据冗余、更新异常、插入异常和删除异常的情况

(3)**第三范式3NF**：不能传递依赖，即非主属性列必须直接依赖于候选码，不能依赖于候选码的传递函数。  
> 注：学生关系表为Student(学号, 姓名, 年龄, 所在学院, 学院地点, 学院电话)，关键字为单一关键字"学号"，存在如下决定关系：  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (学号) → (姓名, 年龄, 所在学院, 学院地点, 学院电话)  
> 这个数据库是符合2NF的，但是不符合3NF，因为存在如下决定关系：  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (学号) → (所在学院) → (学院地点, 学院电话)  
> 存在数据冗余、更新异常、插入异常和删除异常的情况

(4)**BCNF**：在3NF基础上消除主属性对码的部分依赖 / 传递依赖
> 注：仓库管理关系表(仓库ID, 存储物品ID, 管理员ID, 数量)，且一个管理员只在一个仓库工作；一个仓库可以存储多种物品。  
这个数据库表中存在如下决定关系：  
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (仓库ID, 存储物品ID) →(管理员ID, 数量)		(管理员ID, 存储物品ID) → (仓库ID, 数量)  
> 主属性之间(管理员ID) → (仓库ID)

(5)	第四范式4NF：单个表只有一个多值依赖

### 13.	非关系型数据库（例如nosql）和关系型数据库比较
(1)**成本**：nosql数据库简单易部署，基本都是开源软件，成本低  
(2)**查询速度**：nosql数据库将数据存储于缓存之中，速度更快；关系型数据库将数据存储在硬盘中，查询速度远不及nosql数据库  
(3)**存储数据的格式**：nosql可以存储基础类型以及对象或者是集合等各种格式，而关系型数据库则只支持基础类型  
(4)**扩展性**：关系型数据库有类似join这样的多表查询机制的限制，导致扩展很艰难。

### 14.	关系型数据库的优势：  
(1)	容量大，几乎不受限制；  
(2)	可以用SQL语句方便的在一个表以及多个表之间做非常复杂的数据查询；  
(3)	事务支持能够实现安全性能要求高的数据访问。

### 15.	http方法对应数据库的操作
(1)	get操作——获取  
(2)	put操作——修改，幂等操作：一个操作，不论执行多少次，产生的效果和返回的结果都是一样的  
(3)	post操作——添加 / 修改，非幂等操作  
(4)	delete操作

### 16.	select for update怎么理解(乐观锁、悲观锁），select时怎么加排它锁
SELECT ... FOR UPDATE 的Row Lock 与Table Lock。  
其实select for update其实就是在查询数据马上加排它锁的过程，防止其余事务修改数据，也就是悲观锁。for update仅适用于InnoDB，且必须在事务块(BEGIN/COMMIT)中才能生效。

对于update、delete和insert语句，InnoDB会自动给相关数据集加排它锁；  
对于普通select语句，InnoDB不会加任何锁；事务中可以通过以下语句显示地给数据集加共享锁或排它锁：

```
共享锁(S)：SELECT * FROM table_name WHERE … LOCK IN SHARE MODE;
排它锁(X)：SELECT * FROM table_name WHERE … FOR UPDATE;
```


需要注意一下SELECT ... FOR UPDATE 真正锁定(Lock)的数据。由于InnoDB 预设是Row-Level Lock，所以只有“明确”地指定主键，mysql才会执行Row lock(只锁住被选取的数据)，否则mysql将会执行Table Lock(将整个数据表单锁住)。  
> 举个例子：
> 假设有个表单products ，里面有id 跟name两个栏位，id是主键。  
> 例1：明确指定主键，并且有此数据，row lock  
> SELECT * FROM products WHERE id='3' FOR UPDATE;  
> 例2：明确指定主键，若查无此数据，无lock  
> SELECT * FROM products WHERE id='-1' FOR UPDATE;  
> 例3：无主键，table lock  
> SELECT * FROM products WHERE name='Mouse' FOR UPDATE;  
> 例4：主键不明确，table lock  
> SELECT * FROM products WHERE id<>'3' FOR UPDATE;  
> 例5：主键不明确，table lock  
> SELECT * FROM products WHERE id LIKE '3' FOR UPDATE;

乐观锁和悲观锁策略  
悲观锁：在读取数据时锁住那几行，其他对这几行的更新需要等到悲观锁释放时才能继续，适合频繁写的场景  
乐观锁：读取数据时不锁，更新时（CAS）：检查是否数据已经被更新过，如果是则取消当前更新，反之程序重新获取数据，再次进行比较，直到程序中的数据与数据库中的数据相等才进行更新，适合频繁读的场景。

解决select for update的悲观锁问题：利用乐观锁+CAS来解决。

### 17.	mysql并发情况下怎么解决
通过事务、隔离级别、锁

### 18.	大规模的delete或则insert是否会引起表锁定？怎么解决？
会，可以通过分批次的进行delete或则是insert操作。

### 19.	InnoDB行锁的实现原理、分类
mysql执行sql时，会同时锁定这个sql中所有用到的行/表(由引擎及具体情况决定表锁还是行锁)；  
**(1)行锁原理：行级锁并不是直接锁记录，而是锁索引**：  
- 如果一条sql语句操作了主键索引，mysql就会锁定这条主键索引；  
- 如果一条sql语句操作了非主键索引，mysql先锁定该非主键索引，再锁定相关的主键索引；  
- 如果不通过索引条件检索数据，那么InnoDB将对表中所有数据加锁，实际效果跟表锁一样。  

**(2)行锁分类：**

```
    id	age
    1	20
    2	25
    3	30
    age段的索引分为：(-∞,20]、(20,25]、(25,30]、(30,+∞)
```

①	**record lock（记录锁）**：对索引项加锁，锁定一条相关的行  
②	**gap lock（间隙锁）**：锁定一个范围的记录，不包含记录本身  


```
    select * from table where age=23 for update；
    在区间(20,25]加入了gap锁，别的事务无法在此区间插入或更新数据 
```

③	**next-keylock（记录锁+间隙锁）**：锁定一个范围的记录并包含记录本身（上面两者的结合）  

```
    select * from table where age=25 for update；
    不仅使用行锁锁住了相应的数据行，同时也在两边的区间(20,25]和(25,30]都加入了gap锁  
    其他事务无法在这两个区间insert进新数据，同时也不允许update table set age=21 where id=1
    （因为这也类似于在(20,25]范围新增）,但是其他事务可以在两个区间外的区间插入数据。
```

### 20.	并发控制技术
(1)**LBCC**：Lock-Based Concurrency Control，基于锁的并发控制。  
(2)**MVCC**：Multi-Version Concurrency Control，基于多版本的并发控制协议。

①各数据库系统的MVCC机制不尽相同，但主要思想一致：把数据库的行锁与行的多个版本结合起来，只需要很小的开销,就可以实现非锁定读，从而大大提高数据库系统的并发性能。  

②mysql的InnoDB引擎实现了MVCC，且只有在read-committed和repeatable-read两种事务隔离级别下才可使用MVCC；read-uncommited由于是读未提交，所以不存在版本的问题；serializable 则会对所有读取的行加锁。

③**原理**：InnoDB实现MVCC，通过在每行记录后面保存两个隐藏的列来实现，一个保存了行的创建版本号，一个保存了行的过期版本号(删除版本号)。  

> 注：版本号就是事务版本号，create version表明这行记录由哪个事务创建，delete version表明这行记录由哪个事务删除。  
> 执行insert，27th事务创建了一条记录


 | Id | Name | create version | delete version |  
 |---|---|---|---|  
 |1 | hexiaoting |  27|  


> 执行update，28th事务更新了一条记录，并将原记录标记为已过期  


|Id | Name | create version|delete version|  
|---|---|---|---|  
|1 | hexiaoting |  27| 28 | 
|1 | hexiaojun | 28|  


> 执行delete，29th事务删除了一条记录，即把记录标记为已过期


|Id | Name | create version|delete version|  
|---|---|---|---|  
|1 | hexiaoting |  27| 28|  
|1 | hexiaojun | 28 | 29|  


> 执行select，查询事务是Xth，满足以下条件的记录才能被查出：  
> 1)X<=delect version，意味着当前事务Xth早于删除操作所在的事务  
> 2)create version <= Xth，意味着创建操作所在的事务早于当前事务Xth

### 21.	mysql的死锁
![image](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/MySql的死锁.PNG)

### 22.	mysql分页查询
客户端通过传递 start（页码）， limit（每页显示的条数）两个参数去分页查询数据库中的数据。

```
总结： select * from table limit （页数-1） *每页条数, 每页条数;
```


### 23.	外键——取值为null或参考主键值
(1)	主键：一列（或多列），其值能唯一区分表中每个行  
(2)	外键：子表中某一列，该列指向另一个表的主键值 
(3)	对子表(外键所在的表)的作用：子表在进行写操作的时候，如果外键字段在父表中找不到对应的匹配，操作就会失败；  
(4)	对父表的作用：对父表的主键字段进行删和改时，如果对应的主键在子表中被引用，操作就会失败。  

```
外键语句：[constraint 外键名] foreign key(外键字段名) references 主表名(主键字段名);
```
![外键](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/%E5%A4%96%E9%94%AE.PNG)  

### 24.	数据库主从复制与读写分离
在实际的生产环境中，对数据库的读和写都在同一个数据库服务器中，无论是在安全性、高可用性还是高并发等各个方面都是完全不能满足实际需求的。  

![image](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/%E4%B8%BB%E4%BB%8E%E5%88%86%E7%A6%BB.PNG)

##### 主(master)从(slave)复制
①**更新记录**：master将数据更新记录到二进制日志中，这些记录是二进制日志事件；  
②**日志拷贝**：slave通过一个I/O线程与主服务器保持通信，并监控master的二进制日志文件的变化；没有变化，则睡眠并等待；有变化，则将master的二进制日志事件拷贝到它的中继日志；  
③**数据重演**：slave重做中继日志中的事件，将改变反映到它的数据(数据重演)。  

读写分离就是在主服务器上修改，数据会同步到从服务器，从服务器只能提供读取数据，不能写入，实现备份的同时也实现了数据库性能的优化，提升了服务器安全。

### 25.	索引知识
##### (1)表中所有数据页的存放，在磁盘上有两种组织方式：  
①如果表中数据页是以一种页间无序、随机存储的方式，则称这样的表为堆索引组织表  ；  
②如果表中数据页按某种方式（如表中某个字段）有序地存储与磁盘上，则称为聚集索引组织表。  

给表上了主键，那么表在磁盘上的存储结构就由堆索引组织表转变成聚集索引组织表

##### (2)**聚集和非聚集索引**：两者都是采用平衡树作为索引的数据结构。  
- 通过聚集索引可以查到需要查找的数据；  
- 通过非聚集索引可以查到记录对应的主键值，再由主键值通过聚集索引查找到需要的数据。 

以下按B+树分析：  
①聚集索引：键值的逻辑顺序决定了表中数据行的物理存储顺序；

![image](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/%E8%81%9A%E9%9B%86%E7%B4%A2%E5%BC%95.PNG)

聚集索引的根节点和中间节点统称索引节点，包含：下一层的入口指针，关键字(与主键列的值有关)；  
聚集索引的叶节点包含：表中真实的数据行，指向兄弟节点的指针。

②非聚集索引(空间索引、筛选索引、XML索引....)：仅仅是对相关联的数据列创建索引，不影响整个表的物理存储顺序。索引不仅存储了某列的所有行的数据，还存储了相应行的地址/主键值 以获取其他列的数据。  
非聚集索引的根节点和中间节点统称索引节点：包含下一层入口指针，关键字(与被索引列值有关)；  
非聚集索引的叶节点包含：表中对应的主键值，指向兄弟节点的指针。

##### **(3)下列事件索引会失效：**
- 条件中有or，即使条件中有列带索引也不会使用；若想使用or且让索引生效，需为or条件中每个列生成索引  
- like查询，以%开头   
- 若列类型为字符串，则一定要在条件中将数据用引号引起来，否则不使用索引  
- 若mysql估计使用全表扫描要比索引快，则不使用索引   
- 对索引进行运算导致索引列失效  
- 使用内部函数导致索引失效，这样应当创建基于函数的索引   
- b树，is null 不会用，is not null会用。  

##### **(4)联合索引**
两个或多个列上的索引被称为联合索引。创建复合索引时，要仔细考虑列的顺序。对索引中所有列执行搜索，or 仅对前几列执行搜索，复合索引十分有效；但若仅对后面的列进行搜索时，复合索引则没用。  

mysql 每次查询，只能用一个索引。
若我们创建了（A， B， C）的复合索引，那么其实相当于创建了（A， B， C）、（A， B）、（A）三个索引，这被称为最佳左前缀特性。因此，在创建复合索引时，应将最常用的放在最左边。

### 26.	mysql索引的实现，innodb的索引，b+树索引是怎么实现的，为什么用b+树做索引，一个节点存了多少数据，怎么规定大小，与磁盘页对应。
(1)	mysql中索引属于存储引擎级别的概念, 不同存储引擎对索引的实现方式是不同的

(2)	MyISAM引擎使用B+树作为索引结构，叶节点的data域存放的是数据记录的地址，主索引和辅助索引在结构上没有区别，只是主索引要求key是唯一的，而辅助索引的key可以重复；  

(3)	InnoDB引擎使用B+树作为索引结构，主索引的叶节点的data域保存了完整的数据记录，所有辅助索引的叶节点的data域存储相应记录主键的值而不是地址。  

InnoDB要求表必须有主键（MyISAM可以没有），如果没有显式指定，则MySQL系统会自动选择一个可以唯一标识数据记录的列作为主键，如果不存在这种列，则MySQL自动为InnoDB表生成一个隐含字段作为主键，这个字段长度为6个字节，类型为长整形。

##### 为什么用B+树，即B+树的优点：
①不同于B树只适合随机检索，B+树同时支持随机检索和顺序检索；  

②B+树的磁盘读写代价更低.
> B+树的内部结点并没有指向关键字具体信息的指针，其内部结点比B树小，盘块能容纳的结点中关键字数量更多，一次性读入内存中可以查找的关键字也就越多，相对的，IO读写次数也就降低了，而IO读写次数是影响索引检索效率的最大因素。  

③B+树的查询效率更加稳定，B树搜索有可能会在非叶子结点结束，越靠近根节点的记录查找时间越短；在B+树中，随机检索时，任何关键字的查找都必须走一条从根节点到叶节点的路，所有关键字的查找路径长度相同，导致每一个关键字的查询效率相当。 

④更利于对数据库扫描，增删文件（节点）时，效率更高（数据库索引采用B+树的主要原因）。
> B树元素遍历的效率低下；B+树的叶子节点使用指针顺序连接在一起，只要遍历叶子节点就可以实现整棵树的遍历。而且在数据库中基于范围的查询是非常频繁的。

![索引](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/%E7%B4%A2%E5%BC%95.PNG)

### 27.	sql优化
![sql优化](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/sql%E4%BC%98%E5%8C%96.PNG)

### 28.	char和varchar
![char_varchar](https://github.com/daxiaoHe-Girls/daxiaoHe-Girls.github.io/blob/master/images/char_varchar.PNG)

### 29.	存储过程
##### (1) 定义
将完成特定功能的sql语句集进行编译优化，存储在数据库服务器中，用户通过指定存储过程的名字进行调用；  

##### (2) 操作

```
创建：create procedure sp_name @ [参数名] [类型] begin….end
调用：call/exec sp_name [参数名]
删除：drop procedure sp_name
```

##### (3) 存储过程的优点：
- 代码封装，简化操作，保证了一定的安全性；
- 代码复用；
- 预先编译可以提高性能。

##### (4) 存储过程 VS 函数
- 存储过程实现复杂功能，作为一个独立的部分执行，不能被调用；函数通常专注于某个功能，可以作为查询的一部分进行调用；
- 存储过程可以返回多个参数，函数只能返回一个值或表对象；
- 存储过程在创建时在服务器上进行了编译，执行速度比函数快；
- 函数不能直接操作实体表，只能操作内建表。

```
-- 创建存储过程
 CREATE  proceddure productpricing()
 BEGIN 
 SELECT student2.sid,student2.sname FROM student2 WHERE sid IN (104,101) UNION SELECT student2.sid,student2.sname FROM student2 WHERE sid <= 103 ORDER BY sid; -- 101 102 103 104 都会出来结果同or 语句
 END;
 -- 调用存储过程
 CALL productpricing;
 -- 删除存储过程
 DROP PROCEDURE productpricing;
```

### 30.	触发器
##### (1) 定义
一种特殊类型的存储过程，由事件触发，而不是程序调用或者手工启动；

##### (2) 触发器 VS 存储过程
- 触发器由事件隐式调用，存储过程显示调用；
- 触发器不能接受参数输入，存储过程可以；
- 触发器内禁止使用commit和rollback，存储过程可以。

##### (3) 分类：
- DML触发器：数据库服务器发生数据操作语言事件时执行的存储过程；
- DLL触发器：数据发生定义语言事件时执行的存储过程。

##### (4) 功能：
&nbsp;&nbsp;&nbsp;&nbsp; 增加安全性；跟踪用户对数据库的操作；对特定事件进行监控和响应。

### 31.	游标
##### (1) 定义
一种能从包含多条数据记录的结果集中进行遍历，每次提取一条记录的机制。

##### (2) 优点：
- 在使用游标的表中，对行提供删除和更新的功能；
- 游标将面向集合的数据库管理系统和面向行的程序设计连接了起来。

### 32.	如果数据库日志满了，会出现什么情况
日志文件：记录所有对数据库数据的修改，主要是保护数据库以防故障发生，以及恢复数据时使用。
日志文件满之后，只能进行查询等读操作，不能进行更改、备份等操作。


































