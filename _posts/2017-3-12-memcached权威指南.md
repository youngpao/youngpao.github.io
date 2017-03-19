
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

## memcached 的内存管理与删除机制 ##

内存的碎片化

如果用 c 语言直接 malloc,free 来向操作系统申请和释放内存时, 在不断的申请和释放过程中,形成了一些很小的内存片断,无法再利用. 这种空闲,但无法利用内存的现象,---称为内存的碎片化


slab allocator 缓解内存碎片化

memcached 用 slab allocator 机制来管理内存. slab allocator 原理: 预告把内存划分成数个 slab class 仓库.(每个 slab class 大小 1M)

各仓库,切分成不同尺寸的小块(chunk). 

需要存内容时,判断内容的大小,为其选取合理的仓库.

![](http://i.imgur.com/inyoRcK.png)

系统如何选择合适的 chunk?

memcached 根据收到的数据的大小, 选择最适合数据大小的 chunk 组(slab class)

memcached 中保存着 slab class 内空闲 chunk 的列表, 根据该列表选择空的 chunk, 然后将数据缓存于其中。

![](http://i.imgur.com/rDUWSra.png)

固定大小 chunk 带来的内存浪费

由于 slab allocator 机制中, 分配的 chunk 的大小是”固定”的, 因此, 对于特定的 item,可能造成内存空间的浪费. 比如, 将 100 字节的数据缓存到 122 字节的 chunk 中, 剩余的 22 字节就浪费了

![](http://i.imgur.com/evxp0b9.png)

对于 chunk 空间的浪费问题,无法彻底解决,只能缓解该问题. 开发者可以对网站中缓存中的 item 的长度进行统计,并制定合理的 slab class 中的 chunk 的大小. 可惜的是,我们目前还不能自定义 chunk 的大小,但可以通过参数来调整各 slab class 中 chunk大小的增长速度. 即增长因子, grow factor!

grow factor 调优

memcached 在启动时可以通过­f 选项指定 Growth Factor 因子, 并在某种程度上控制 slab 之
间的差异. 默认值为 1.25. 但是,在该选项出现之前,这个因子曾经固定为 2,称为”powers of 2” 策略。

我们分别用 grow factor 为 2 和 1.25 来看一看效果:

	>memcached ­f 2 ­vvv
	slab class 1: chunk size 128 perslab 8192
	slab class 2: chunk size 256 perslab 4096
	slab class 3: chunk size 512 perslab 2048
	slab class 4: chunk size 1024 perslab 1024
	....
	.....
	slab class 10: chunk size 65536 perslab 16
	slab class 11: chunk size 131072 perslab 8
	slab class 12: chunk size 262144 perslab 4
	slab class 13: chunk size 524288 perslab 2

可见，从 128 字节的组开始，组的大小依次增大为原来的 2 倍. 来看看 f=1.25 时的输出:

	>memcached -f 1.25 -vvv
	slab class 1: chunk size 88 perslab 11915
	slab class 2: chunk size 112 perslab 9362
	slab class 3: chunk size 144 perslab 7281
	slab class 4: chunk size 184 perslab 5698
	....
	....
	slab class 36: chunk size 250376 perslab 4
	slab class 37: chunk size 312976 perslab 3
	slab class 38: chunk size 391224 perslab 2
	slab class 39: chunk size 489032 perslab 2

对比可知, 当 f=2 时, 各 slab 中的 chunk size 增长很快,有些情况下就相当浪费内存. 因此,我们应细心统计缓存的大小,制定合理的增长因子.

注意:

当 f=1.25 时,从输出结果来看,某些相邻的 slab class 的大小比值并非为 1.25,可能会觉得有些计算误差，这些误差是为了保持字节数的对齐而故意设置的.

## 分布式集群算法 ##

memcached 如何实现分布式?

在第 1 章中,我们介绍 memcached 是一个”分布式缓存”,然后 memcached 并不像 mongoDB 那
样,允许配置多个节点,且节点之间”自动分配数据”. 就是说--memcached 节点之间,是不互相通信的. 因此,memcached 的分布式,要靠用户去设计算法,把数据分布在多个 memcached 节点中.


一致性哈希算法原理

通俗理解一致性哈希:
把各服务器节点映射放在钟表的各个时刻上, 把 key 也映射到钟表的某个时刻上. 该 key 沿钟表顺时针走,碰到的第 1 个节点即为该 key 的存储节点

![](http://i.imgur.com/cqwud7F.png)

一致性哈希对其他节点的影响

当某个节点 down 后,只影响该节点顺时针之后的 1 个节点,而其他节点
不受影响.因此，Consistent Hashing 最大限度地抑制了键的重新分布

![](http://i.imgur.com/ioLYLnM.png)

一致性哈希+虚拟节点对缓存命中率的影响

1) 节点在圆环上分配分配均匀,因此承担的任务也平均,但事实上, 一般的 Hash 函数对于节
点在圆环上的映射,并不均匀. 

2) 当某个节点 down 后,直接冲击下 1 个节点,对下 1 个节点冲击过大,能否把 down 节点上的
压力平均的分担到所有节点上?

![](http://i.imgur.com/ySCvkOg.png)

一致性哈希的 PHP 实现

	/*
	实现一致性哈希分布的核心功能.
	*/
	// 需要一个把字符串转成整数的接口
	interface hasher {
	public function _hash($str);
	}
	interface distribution {
	public function lookup($key);
	}
	class Consistent implements hasher,distribution {
	protected $_nodes = array();
	protected $_postion = array();
	protected $_mul = 64; //每个节点对应 64 个虚节点
	public function _hash($str) {
	return sprintf('%u',crc32($str)); // 把字符串转成 32 位符号整数
	}
	// 核心功能
	public function lookup($key) {
	$point = $this->_hash($key);
	$node = current($this->_postion); //先取圆环上最小的一个节点,当
	成结果
	foreach($this->_postion as $k=>$v) {
	if($point <= $k) {
	$node = $v;
	break;
	}
	}
	reset($this->_postion);
	return $node;
	}
	public function addNode($node) {
	if(isset($this->nodes[$node])) {
	return;
	}
	for($i=0; $i<$this->_mul; $i++) {
	$pos = $this->_hash($node . '-' . $i);
	$this->_postion[$pos] = $node;
	$this->_nodes[$node][] = $pos;
	}
	$this->_sortPos();
	}
	// 循环所有的虚节点,谁的值==指定的真实节点 ,就把他删掉
	public function delNode($node) {
	if(!isset($this->_nodes[$node])) {
	return;
	}
	foreach($this->_nodes[$node] as $k) {
	unset($this->_postion[$k]);
	}
	unset($this->_nodes[$node]);
	}
	protected function _sortPos() {
	ksort($this->_postion,SORT_REGULAR);
	}
	}
	// 测试
	$con = new Consistent();
	$con->addNode('a');
	$con->addNode('b');
	$con->addNode('c');
	$key = 'www.zixue.it';
	echo '此 key 落在',$con->lookup($key),'号节点';

