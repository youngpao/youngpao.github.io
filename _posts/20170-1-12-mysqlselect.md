##select 查询操作 ##


语法：select *from 表名 where 

举例： select *from user 就是user表中所有列与行数据，但一般不这样查询，数据过大情况下这样会增加负担

select *from user where uid=2; 具体查询哪一行的数据

查询多行： select *from user where uid>=2;

查询某几行的某几列,* 代表所有列: select name from user where uid>=2;

## select 查询模型 ##

列是变量，从上到下，遍历表，如果where 为真则去取出所有行，如果假则不取出

select *from user where1;


变量是可以做计算的
 
select age+1 from user where uid=5;

投影的概念

user表有三列,我们只取出2列(部分列),叫做投影运算

就像手电筒,只照到两列,投出影子显示出来

goods表查询出来 market_price-shop_price

两个列做运算,叫做广义投影

