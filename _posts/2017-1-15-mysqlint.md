## 整型列 ##

列类型大致分为3类:

1.数值型

整型,浮点型,定点型

2.字符串

char,varchar,text

3.日期时间类型

2012-12-13

14:26:23


**整型:**

1.我们声明了t1表的 sn int

    insert into t1 values (1,'lisi');

那这个 1 在磁盘中占据几个字节? 4个字节

1个字节有8个位,每个位上有0,1两种可能

0000 0000

那4个字节 int型 在磁盘中存1 是如下的存法

00000000 00000000 00000000 00000001

int型 存储的范围是多少?

11111111 11111111 11111111 11111111

0 ~ (2^32)-1 [40多亿]

说明:

一个列,占的字节越多,存储的范围越大

mysql手册 -> 列类型 -> 数值类型

![](http://i.imgur.com/BEsdYbC.jpg)

字节：

bigint 8

int 4 字节

mediumint 3

smallint 2

tinyint 1

人的年龄: tinyint 无符号类型


**整型列属性**

理解并能应用 unsigned,zerofill 及 M 属性


unsigned:无符号

默认是有符号属性，如果要从0开始，那么就要声明一下 无符号

    create table t1(
    
    id tinyint unsigned);

zerofill：零填充

适合用于 学号,编码等,固定宽度的数字,可以用 0 填充至固定宽度

zerofill 填充至多宽? M 来指定

学号--> 1 -> 0001

学号--> 123 -> 0123


tinyint(M)

![](http://i.imgur.com/CXGVjnm.jpg)


注意:

zerofill 属性默认决定列为 unsigned

M必须要跟unsigned配合使用才有意义

**M 宽度不是限制我们插入数字的范围,0-9就一个数字宽**

**而是指,0填充时,我给你填到多宽,**

**如果不用0填充,M参数完全可以不写,因为它没有任何意义**






