## update 修改操作 ##

语法是： update 表名 set 列名=值 （新值) where（指向具体位置） 


举例： update user set age=12 where name='alice';

意思就是 在名字为alice的这列，修改年龄为12

如果不添加where，那么就是所有列

数据是很宝贵的

如果我们update不加where条件,后果是很可怕的

mysql可以设置新手模式,在新手模式下,删除和更改不加where条件,它是拒绝执行的.


