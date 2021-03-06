##字符型列 ##

字符型:

char varchar text/blob enum

char 定长存储内容

varchar 变长存储内容

char和varchar有什么区别呢?

就如投币公交与按站收费的长途公交的区别

char(M),varchar(M)

假设M是10,10个字符的宽度

**1.char(10); 定长**

给定列10个字符的宽度,最多能存10个字符

如果我们insert一个字符

那这一个字符也占据10个字符的宽度

char就好比你去吃自助餐,吃一次50;

那吃多吃少你都要花50元


**2.varchar(10); 变长**

给定列10个字符的宽度,最多能存10个字符

如果我们insert一个字符

那么这一个字符占据多少个字符宽度?

肯定不会不会是占据1个字符的宽度,用多少占多少,利用率100%,就没有char存在的必要了

那么varchar到底占据多少?

**3.那到底是选择char还是varchar呢?**

现在磁盘容量都非常的大,如果存储的字符不是很多,比如20个字符以内,其实都用char,速度会更快

定长,短列,适合.

而且,如果一张表所有列的字节都是定长的,这种表会被识别为 fixed 类型,速度很快.

实际开发中,存储的字符较少,都是用的定长,再浪费也浪费不到哪去


**4.char没有存够指定的长度,也能占据指定的长度,是如何做到的呢?**

char 不够指定长度时用”\0”(空格)来填充,取出时,会把右侧的空格全部抹掉

注:这意味着,如果右侧有本身有空格,将会丢失

![](http://i.imgur.com/kKmLWJX.jpg)

所有的空格都一样,mysql无法区分这是你本身的空格,所以char在取出时,将右侧的空格给删除掉了
而varchar型则不会删除后面本身的空格,因为在它的开始处有1-2个字节已经说明了这个列占几个字符

速度上:定长速度更快一些

注意:

char(M),varchar(M),限制的是字符,不是字节

即 char(2) charset utf8,能存2个utf8字符,比如'中国'

char与varchar型的选择原因:

1.空间利用效率,四字成语表,char(4),利用率100%

个人简介,微博 140字 ,varchar(140)

2.速度

用户名:char

char/varchar 最大可存多少个字符:

手册中有说明

CHAR列的长度固定为创建表时声明的长度。长度可以为从0到255的任何值。

VARCHAR列中的值为可变长字符串。长度可以指定为0到65,535之间的值。(VARCHAR的最大有效长度由最大行大小和使用的字符集确定。整体最大长度是65,532字节）。

即 char不论字符集,在[0,255]之间,varchar在[0,max]之间, 而max受字符集影响,总的原则是字符所占的字节不超65535(实际上还不到65535)

**text 类型**

TEXT[(M)]

可以存比较大的文本段,搜索速度稍慢

因此,如果不是特别大的内容,建议用char,varchar来代替

最大长度为65,535(2^16–1)字符的TEXT列。

**enum 枚举型**

ENUM('value1','value2',...)

是定义好,值,就在某几个枚举范围内

gender (‘boy’,’girl’) ,insert 时,只能选”boy”,”girl”

![](http://i.imgur.com/AUCDD0s.jpg)

**blob 二进制类型 BLOB[(M)]**

blob和text是对应存在的

其实,如果不是存图像,blob基本用不上

blob是用来存储图像,音频等二进制信息

意义:2进制,0-255都有可能出现

用blob存储的好处:

我们在用字符存储的时候,知道有utf8字符集,gbk字符集

比如:

有可能在gbk字符集下,二进制中有个253,有可能是违法的,在入库的时候,被过滤了

你的这一串信息,有可能被过滤掉一小部分,就丢失了

而blog用二进制来存储,就不用考虑字符集了,就是0,1

使用上和text一样,只不错在存储的时候,从不考虑字符集

所以我们存储什么都可以

如:

一张图片中有0xFF字节,这个在ascii字符集认为是非法的,在入库的时候,被过滤了

**set 集合**

SET('value1','value2',...)

一个设置。字符串对象可以有零个或多个值，每个值必须来自列值'value1'，'value2'，...SET列最多可以有64个成员

set相当于设置几个事先定义好的值，然后只能从这几个里面选，可以多选，比如爱好
