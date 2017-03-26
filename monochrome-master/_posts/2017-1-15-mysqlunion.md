什么是union查询?

union查询就是把2条或多条sql的查询结果,合并成1个结果集


**union 语句必须满足 1 个条件:各语句取出的列数相同**

注意:

使用 uion 时,完全相等的行,将会被合并.

合并是比较耗时的操作,两行会在比较看是否完全相等

一般不让union进行合并,使用"union all" 可以避免

实际中,一般使用union all来合并查询

sql合并后得到的总的结果,可以 order by,子句的order by 失去意义



![](http://i.imgur.com/qkQgSPy.jpg)

![](http://i.imgur.com/eekg5SE.jpg)

一道面试题：

![](http://i.imgur.com/JztHNYl.jpg)

要用到sum group-by 组合

![](http://i.imgur.com/SvGdQuJ.jpg)

