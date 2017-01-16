## 时间型列 ##

日期时间类型

Year 年(1字节) 95/1995

范围[1901-2155]

在insert是,可以简写面的后2位,但是不推荐这样

如果我们只写两位,计算年份按:

[00-69]+2000

[70-99]+1900

即: 填2位,表示1970-2069

Date 日期 1998-12-31 [年月日]

范围:1000/01/01 ~ 9999/12/31

time 时间 13:56:23 [时分秒]

范围: -838:59:59 -> 838:59:59

datetime 日期时间 1998-12-31 13:56:23 [年月日 时分秒]

范围: 1000/01/01 00:00:00 -> 9999:12:31 23:59:59

时间戳:

是1970-01-01 00:00:00 到当前的秒数

一般存储注册时间,商品发布时间等,并不是用datetime存储,而是用时间戳

因为datetime虽然直观,但计算不便


    create table t12 (
    ye year,
    dt date,
    tm time,
    dttm datetime
    );

1.测试 year

    insert into t12 (ye) values (1901);
    insert into t12 (ye) values (07);
    insert into t12 (ye) values (77);

如果我们只写两位 values (07),(77)

按如下方法计算年份:

[00-69]+2000

[70-99]+1900

不建议只写两位


timestamp 时间戳

如果我们插入改行,时间戳这列会自动插入当前时间戳

当我们更改这行数据时,timestamp列会自动更改时间为更改数据时的时间戳

    create table t13 (
    id int,
    tt timestamp
    );
    insert into t13 (id) values (1);
    select * from t13;
    update t13 set id=2 where id=1;
    select * from t13;

tt列显示当前的时间

TIMESTAMP列的显示格式与DATETIME列相同

显示宽度固定在19字符，并且格式为YYYY-MM-DD HH:MM:SS

不建议这样使用,如果需要插入精确的时间

用 int unsigned 来存储它的精确正整数时间戳即可


