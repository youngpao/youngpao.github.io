## limit 限制取出条目 ##

limit 一般与order by 配合使用，因为一般情况下并不需要所有数据，要限制条件取出几条数据就行。

limit 有选择性的取出1条或多条,

limit 限制取出条目

limit offset ,N , 跳过 offset 条,取 N 条

例:

取前 3 名, limit 0,3

取前 3 到前 5 名, limit 2,3

取最新的商品 order by goods_id desc limit 0,1;

对于 offset 为 0,可以简单为 limit N;

页码与 limit 的关系

1.取出价格最高的前三名商品

按照价格降序排列 -> 取出前三行

    select *from goods order by shop_price desc limit 0,3;


2.取出点击量为前三名到前五名的商品


    select *from goods order by click_count desc limit 2,3;

3.取出最新商品

    select *from goods order by goods_id desc limit 0,1;

limit 做blog分页

假设每页显示5条

第一页取出最新的5篇文章 limit 0,5

第二页取出的文章为 limit 5,5;

第n页取出的文章数为 limit (n-1)*5,5


