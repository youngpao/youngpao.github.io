## 浮点列与定点列 ##

float 浮点型

double 范围更大的浮点型

decimal 定点型

1.存小数


手册 -> float
float -3.402823466E+38 到-1.175494351E-38、0 和 1.175494351E-38 到 3.402823466E+38。

这些是理论限制，基于 IEEE 标准。实际的范围根据硬件或操作系统的不同可能稍微小些。

double 更大,没必要去记忆他们的范围,除非搞天文

2.float(M,D ) ,double(M,D)

FLOAT[(M,D)] [UNSIGNED] [ZEROFILL]

整型的M遇到zerofill才会起作用

float的M和D 只要设置,就会立即起作用

M 是精度,总位数, D 标度, 小数点后面的位数

float(5,2) -999.99 ~~ 999.99, 分别用范围之外的数字测试

float和double区别知识存储范围

![](http://i.imgur.com/hLgwSm1.jpg)


**decimal(M,D) 定点**

float/double , 有精度损失

decimal 定点型,更精确

定点型,是将整数部分和小数部分用分别用数字来存储的,所以定点型更精确







