exists 存在的意思

举例：

查询所有有商品的栏目：

1.有商品的栏目

select * from goods where cat_id=1;

2.查询栏目表

select *from category;

合并起来的语法：

    select*from category  where exists(select *from goods where category.cat_id=goods.cat_id);



