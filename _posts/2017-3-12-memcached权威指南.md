
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

