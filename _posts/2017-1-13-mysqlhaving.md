## having筛选 ##

我们知道mysql是数据库服务器,帮我们管理数据

它是将我们的数据管理到哪里去了呢?管理到了文件上

我们的商品表 -> data/test/goods.MYD

where -> 条件筛选的是磁盘上的表的字段


而sheng是结果集上的字段,结果集查出来的是临时放在内存里的

我们需要筛选结果集上的字段,我们可以将结果集当成表,再一步筛选,只不过不能用where,需要用having;

where发挥的比较早,是针对内存中的表发挥的作用,筛选出结果集

从载从结果集中进一步筛选,需要用到having

也就是说先where再数据库表内筛选一遍，在筛选出来的结果上用having再来一遍

5.查询比市场价省钱200元以上的商品及该商品所省的钱(where和having分别实现)

原先语法：

    select goods_id,(market_price-shop_price)as sheng from goods where (market_price- shop_price)>200;
    

用having结合where表示：

    selece goods_id,(market_price-shop_price) as sheng from goods having sheng>200;


where和having都存在,where要放在having的前面;

因为where是对内存中的表进行筛选

而having是对进一步计算出来的结果集进行再一步筛选

**where-group by-having-综合运用**
    
    有如下表及数据
    +------+---------+-------+
    | name | subject | score |
    +------+---------+-------+
    | 张三 | 数学 | 90 |
    | 张三 | 语文 | 50 |
    | 张三 | 地理 | 40 |
    | 李四 | 语文 | 55 |
    | 李四 | 政治 | 45 |
    | 王五 | 政治 | 30 |
    +------+---------+-------+

要求:查询出2门及2门以上不及格者的平均成绩


有一种典型错误做法是：


    select name,count(score<60)as k,avg(score) from stu group by name having k>=2;

但是显示出来的结果是：

    +------+---+------------+
    | name | k | avg(score) |
    +------+---+------------+
    | 张三 | 3 |60.0000 |
    | 李四 | 2 |50.0000 |
    +------+---+------------+


这样的结果有一个错误地方是，张三挂科数目是三门了，实际是两门。

正确：

    slect name,sum(score<60)as gk,avg(score)as pj from stu group by name having gk>=2;

也可以简写：

    select name,sum(score<60)as k,avg(score) from stu group by name having k>=2;

显示的效果：

    +------+------+---------+
    | name | gk   | pj  |
    +------+------+---------+
    | 张三 |2 | 60.0000 |
    | 李四 |2 | 50.0000 |
    +------+------+---------+


这样张三显示的挂科数目就对了

这样写的原因是：count统计的只是行数，所以有几行就统计行，count取决筛选环境，但不是括号里的判断条件，所以count(score<60)=count(*)，所以要用sum，用括号内的筛选条件再取和

