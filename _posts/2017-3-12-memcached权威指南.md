
1.1 memcached 是什么?

free & open source, high-performance, distributed memory object caching system
自由&开放源码, 高性能 ,分布式的内存对象缓存系统
由 livejounal 旗下的 danga 公司开发的老牌 nosql 应用.

1.2 什么是 NoSQL?

nosql，指的是非关系型的数据库。

相对于传统关系型数据库的"行与列",NoSQL 的鲜明特点为 k-v 存储(memcached,redis),
或基于文档存储(mongodb)

注:

nosql -- not only sql , 

不仅仅是关系型数据库, 

显著特点: key-value 键值对存储,如 memcached, redis,或基于文档存储 如,mongodb

=======
1.1 memcached 是什么?

free & open source, high-performance, distributed memory object caching system
自由&开放源码, 高性能 ,分布式的内存对象缓存系统
由 livejounal 旗下的 danga 公司开发的老牌 nosql 应用.

1.2 什么是 NoSQL?

nosql，指的是非关系型的数据库。

相对于传统关系型数据库的"行与列",NoSQL 的鲜明特点为 k-v 存储(memcached,redis),
或基于文档存储(mongodb)

注:

nosql -- not only sql , 

不仅仅是关系型数据库, 

显著特点: key-value 键值对存储,如 memcached, redis,或基于文档存储 如,mongodb


## mencached编译 ##

2.1 linux 下编译 memcached

2.1.1:准备编译环境

在 linux 编译,需要 gcc,make,cmake,autoconf,libtool 等工具, 这几件工具, 以后还要编译 redis 等使用,所以请先装. 在 linux 系统联网后,用如下命令安装

    #yum install gcc make cmake autoconf libtool

2.1.2: 编译 memcached

memcached 依赖于 libevent 库,因此我们需要先安装 libevent.

分别到 libevent.org 和 memcached.org 下载最新的 stable 版本(稳定版).

先编译 libevent ,再编译 memcached,

编译 memcached 时要指定 libevent 的路径. 过程如下: 假设源码在/usr/local/src 下, 安装在/usr/local 下

	`# tar zxvf libevent-2.0.21-stable.tar.gz
	# cd libevent-2.0.21-stable
	# ./configure --prefix=/usr/local/libevent
	# 如果出错,读报错信息,查看原因,一般是缺少库
	# make && make install
	# tar zxvf memcached-1.4.5.tag.gz
	# cd memcached-1.4.5
	#./configure--prefix=/usr/local/memcached \
	--with-libevent=/usr/local/libevent
	# make && make install`

注意: 在虚拟机下练习编译,一个容易碰到的问题---虚拟机的时间不对, 导致的 gcc 编译过程中,检测时间通不过,一直处于编译过程. 解决:

    # date -s ‘yyyy-mm-dd hh:mm:ss’ # clock -w # 把时间写入 cmos

## memcached的启动 ##

![](http://i.imgur.com/QoxQBlO.png)

我们发现 memcached 已经启动,并把信息输出到控制台.. 

如果我们想让 memcached 作为 daemon 在后台运行,只需要加-d 选项

	# /usr/local/memcached/bin/memcached -m 64 -p 11211 -u nobody -d

如果想了解 -m -p 等参数的意义, 可以通过 memcached -h 查看帮助.

![](http://i.imgur.com/4WECTdv.png)

![](http://i.imgur.com/SxB7t5X.png)

memcached 的连接

memcached 客户端与服务器端的通信比较简单,使用的基于文本的协议,而不是二进制协议. 

(http 协议也是这样), 因此我们通过 telnet 即可与 memcached 作交互. 

另开一个终端,并运行 telnet 命令 (开启 memcached 的终端不要关闭)

![](http://i.imgur.com/a9CEFa8.png)


## memcached 的命令 add ##

增: add 往内存增加一行新记录

语法: add key flag expire length 回车

key 给值起一个独特的名字

flag 标志,要求为一个正整数

expire 有效期

length 缓存的长度(字节为单位)

flag 的意义:

memcached 基本文本协议,传输的东西,理解成字符串来存储. 想:让你存一个 php 对象,和一个 php 数组,怎么办?

答:序列化成字符串,往出取的时候,自然还要反序列化成 对象/数组/json 格式等等. 这时候, flag 的意义就体现出来了. 比如, 1 就是字符串, 2 反转成数组 3,反序列化对象..... expire 的意义:

设置缓存的有效期,有 3 种格式

1:设置秒数, 从设定开始数,第 n 秒后失效.

 2:时间戳, 到指定的时间戳后失效. 比如在团购网站,缓存的某团到中午 12:00 失效. add key 0 1379209999 6

3: 设为 0. 不自动失效.

注意：

1:编译 memcached 时,指定一个最长常量,默认是 30 天. 

所以,即使设为 0,30 天后也会失效. 

2:可能等不到 30 天,就会被新数据挤出去.

**删改查**

delete 删除

delete key [time seconds]

删除指定的 key. 如加可选参数 time,则指删除 key,并在删除 key 后的 time 秒内,不允许
get,add,replace 操作此 key.

replace 替换

replace key flag expire length

参数和 add 完全一样,不单独写

get 查询

get key

返回 key 的值

set 是设置和修改值

set 想当于有 add replace 两者的功能. set key flag expire leng 时

如果服务器无此键 ----> 增加的效果

如果服务器有此键 ----> 修改的效果.

