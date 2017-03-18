
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

incr ,decr 命令:增加/减少值的大小

语法: incr/decr key num

注意:incr,decr 操作是把值理解为 32 位无符号来+-操作的. 值在[0-2^32-1]范围内

应用场景------秒杀功能, 一个人下单,要牵涉数据库读取,写入订单,更改库存,及事务要求, 对于传统型数据库来说, 压力是巨大的. 可以利用 memcached 的 incr/decr 功能, 在内存存储 count 库存量, 秒杀 1000 台
每人抢单主要在内存操作,速度非常快, 抢到 count<=1000 的号人,得一个订单号,再去另一个页面慢慢支付

**统计命令: stats**

把 memcached 当前的运行信息统计出来

stats

stat pid 2296 进程号

stat uptime 4237 持续运行时间

stat time 1370054990

stat version 1.2.6

stat pointer_size 32

stat curr_items 4 当前存储的键个数

stat total_items 13

stat bytes 236

stat curr_connections 3

stat total_connections 4

stat connection_structures 4

stat cmd_get 20

stat cmd_set 16

stat get_hits 13

stat get_misses 7 // 这 2 个参数 可以算出命中率

stat evictions 0

stat bytes_read 764

stat bytes_written 618

stat limit_maxbytes 67108864

stat threads 1

end

缓存有一个重要的概念: 命中率. 

命中率是指: (查询到数据的次数/查询总数)*100%

如上, 13/(13+7) = 60+% , 的命中率.

**flush_all **清空所有的存储对象

## 编译 PHP 及 memcached 扩展 ##

**编译 apache+php**

apache 编译:

	#1 解压
	# tar zxvf http-2.2.45.tar.gz
	# cd http-2.2.45
	# ./configure --prefix=/usr/local/httpd (你也可以指定自己的路径)
	#make && make install

php 编译并与 apache 整合:

	#1 编译 php
	# yum install libxml2 libxml2-devel
	# tar zxvf php-xxx.tar.gz
	# cd php-xxx
	#./configure--prefix=/usr/local/php \
	--with-apxs2=/usr/local/httpd/bin/apxs
	# make && make install
	# 2. 与 apache 整合
	# vim 编辑 http.conf,添加如下
	# addtype application/x-httpd-php .php
	# 3: 重启 apache

注:

如果在 configure 过程中,提示缺少 libxml2 的库,则如下操作:

	#yum install libxml2 libxml2-devel

**编译 php-memcache 扩展**

1: 到软件的官方(如 memcached)或 pecl.php.net 去寻找扩展源码并下载解压

2: 进入到 path/memcache 目录

3: 根据当前的 php 版本动态的创建扩展的 configure 文件

	#/xxx/path/php/bin/phpize \
	--with-php-config=/xxx/path/php/bin/php-config
	4: ./configure -with-php-config=/xxx/path/php/bin/php-config
	5: make && make install
	6:把生成的.so 扩展, 在 php.ini 里引入.
	7:重启 apache

## windows 下安装 php-memcached 扩展 ##

1) 通过 phpinfo()观察如下 3 个参数,即 php 版本, ts/nts, vc6/vc9

2) 根据上步中的参数,到 http://downloads.php.net/pierre/ 下载匹配的 memcache.dll

3) 再次观察 phpinfo()信息,找出 extension_dir, 并把下载的 memcache.dll 放入该路径

4) 并修改 php.ini, 加入 extension=php_memcache.dll,引入该 dll

5) 重启 apache

## memcached 实战 ##

<<<<<<< HEAD
	<?php
	$sql = 'select goods_id,goods_name from ecs_goods where is_hot=1 limit
	5';
	// 判断 memcached 中是否缓存热门商品,如果没有,则查询数据库
	$hot = array();
	if( !($hot=$memcache->get($sql)) ) {
	$hot = $mysql->getAll($sql);
	echo '<font color="red">查询自数据库</font>';
	//从数据库取得数据后,把数据写入 memcached
	$memcache->add($sql,$hot,0,300); // 并设置有效期 300 秒
	} else {
	echo '<font color="red">查询自 memcached</font>';
	}
	?>

中继 MySQL 主从延迟数据

MySQL 在做 replication 时,主从复制之间必然要经历一个复制过程,即主从延迟的时间. 尤其是主从服务器处于异地机房时,这种情况更加明显. 把 facebook 官方的一篇技术文章,其加州的主数据中心到弗吉尼亚州的主从同步延期达到70ms;

考虑如下场景:

①: 用户 U 购买电子书 B, insert into Master (U,B);

②: 用户 U 观看电子书 B, select 购买记录[user=’A’,book=’B’] from Slave.

③: 由于主从延迟,第②步中无记录,用户无权观看该书. 这时,可以利用 memached 在 master 与 slave 之间做过渡(图 5.2):

①: 用户 U 购买电子书 B, memcached->add(‘U:B’,true)

②: 主数据库 insert into Master (U,B);

③: 用户 U 观看电子书 B, select 购买记录[user=’U’,book=’B’] from Slave.

如果没查询到,则 memcached->get(‘U:B’),查到则说明已购买但 Slave 延迟. ④: 由于主从延迟,第②步中无记录,用户无权观看该书.

![](http://i.imgur.com/iMYQ2d9.png)

## 永久数据被踢现象 ##

其实,这要从 2 个方面来找原因:

即前面介绍的 惰性删除,与 LRU 最近最少使用记录删除. 分析:

1:如果 slab 里的很多 chunk,已经过期,但过期后没有被 get 过, 系统不知他们已经过期.

2:永久数据很久没 get 了,不活跃,如果新增 item,则永久数据被踢了. 

3: 当然,如果那些非永久数据被 get,也会被标识为 expire,从而不会再踢掉永久数据

![](http://i.imgur.com/iF3nQXe.png)

解决方案: 永久数据和非永久数据分开放

## 缓存雪崩现象 ##

缓存雪崩一般是由某个缓存节点失效,导致其他节点的缓存命中率下降, 缓存中缺失的数据
去数据库查询.短时间内,造成数据库服务器崩溃. 重启 DB,短期又被压跨,但缓存数据也多一些. DB 反复多次启动多次,缓存重建完毕,DB 才稳定运行. 或者,是由于缓存周期性的失效,比如每 6 小时失效一次,那么每 6 小时,将有一个请求”峰值”, 严重者甚至会令 DB 崩溃.

=======
memcached
>>>>>>> origin/master
