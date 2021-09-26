math模块提供数学运算函数。该模块实现相应CPython模块的子集。更多信息请参阅CPython文档：math
### 返回x的y次方
>math.pow(x,y)

返回x的y次方，返回值是浮点数。

示例：

	>>> import math
	>>> math.pow(2, 3)
	8.0
### 返回x的反余弦弧度值
>math.acos(x)

返回x的反余弦弧度值，返回值为浮点数。x是-1~1之间的数，包括-1和1，如果小于-1或者大于-1，会产生错误。

示例：

	>>> import math
	>>> math.acos(0.6)
	0.9272952
### 返回x的反正弦弧度值
>math.asin(x)

返回x的反正弦弧度值，返回值为浮点数。x是-1~1之间的数，包括-1和1，如果小于-1或者大于1，会产生错误。

示例：

	>>> import math
	>>> math.asin(-1)
	-1.570796
### 返回x的反正弦弧度值
>math.atan(x)

返回x的反正切弧度值，返回值为浮点数。

示例：

	>>> import math
	>>> math.atan(-8)
	-1.446441
	>>> math.atan(6.4)
	1.4158
### 返回给定的x及y坐标值的反正切值
>math.atan2(x, y)

返回给定的x和y坐标值的反正切纸，返回值为浮点数。

示例：

	>>> import math
	>>> math.atan2(-0.50,0.48)
	-0.8058035
	>>> math.atan2(7, 9)
	0.6610432
### 返回数字的上入正数
>math.ceil(x)

返回数字的上入整数

示例：

	>>> import math
	>>> math.ceil(4.1)
	5
### 把y的正负号加到x前面
>math.copysign(x, y)

把y的正负号加到x前面，可以使用0，返回值为浮点数。

示例：

	>>> import math
	>>> math.copysign(5, 0)
	5.0
	>>> math.copysign(5, -4)
	-5.0
	>>> math.copysign(5, 9)
	5.0
### 返回x的弧度的余弦值
>math.cos(x)

返回x的弧度的余弦值，范围再-1~1之间，返回值为浮点数。

示例：

	>>> import math
	>>> math.cos(3)
	-0.9899925
### 将弧度转换为角度
>math.degrees(x)

将弧度转换为角度，返回值为浮点数

示例：

	>>> import math
	>>> math.degrees(5)
	286.4789
	>>> math.degrees(math.pi/2)
	90.0
### 数学常量e
>math.e

数学常量e，e即自然常数。
### 返回e的x次幂
>math.exp(x)

返回e的x次幂，返回值为浮点数。

示例：

	>>> import math
	>>> math.exp(1)
	2.718282
	>>> print(math.e)
	2.718282

### 返回数字的绝对值
>math.fabs(x)

返回数字的绝对值，返回值为浮点数。

示例：

	>>> import math
	>>> math.fabs(-3.88)
	3.88
### 返回数字的下舍整数
>math.floor(x)

返回数字的下舍整数。

示例：

	>>> import math
	>>> math.floor(8.7)
	8
	>>> math.floor(9)
	9
	>>> math.floor(-7.6)
	-8
###返回x/y的余数
>math.fmod(x,y)

返回x/y的余数，返回值为浮点数。

示例：

	>>> import math
	>>> math.fmod(15, 4)
	3.0
	>>> math.fmod(15, 3)
	0.0
### 返回由x的小数部分和整数部分组成的元组
>math.modf(x)

返回由x的小数部分和整数部分组成的元组。

示例：

	>>> import math
	>>> math.modf(17.592)
	(0.5919991, 17.0)
### 返回一个元组(m,e)
>math.frexp(x)

返回一个元组(m,e),其计算方式为：x分别除0.5和1,得到一个值的范围，2e的值在这个范围内，e取符合要求的最大整数值,然后x/(2e)，得到m的值。如果x等于0，则m和e的值都为0，m的绝对值的范围为(0.5,1)之间，不包括0.5和1。

示例：

	>>> import math
	>>> math.frexp(52)
	(0.8125, 6)
### 判断x是否为有限数
>math.isfinite(x)

判断x是否为有限数，是则返回True，否则返回False。

示例：

	>>> import math
	>>> math.isfinite(8)
	True
### 判断是否无穷大或负无穷大
>math.isinf(x)

如果x是正无穷大或负无穷大，则返回True,否则返回False。

示例：

	>>> import math
	>>> math.isinf(123)
	False
### 判断是否数字
>math.isnan(x)

如果x不是数字，返回True,否则返回False。

示例：

	>>> import math
	>>> math.isnan(23)
	False
### 返回x*(2**i)的值
>math.ldexp(x, exp)

返回x*(2**i)的值。

示例：

	>>> import math
	>>> math.ldexp(2, 1)
	4.0

### 返回x的自然对数
>math.log(x）

返回x的自然对数，x>0,小于0会报错。

示例：

	>>> import math
	>>> math.log(2)
	0.6931472
### 数学常量pi
>math.pi

数学常量pi（圆周率，一般以π来表示）。
### 将角度转换为弧度
>math.radians(x)

将角度转换为弧度，返回值为浮点数。

示例：

	>>> import math
	>>> math.radians(90)
	1.570796
### 返回x弧度的正弦值
>math.sin(x)

返回x弧度的正弦值，数值在-1到1之间。

示例：

	>>> import math
	>>> math.sin(-18)
	0.7509873
	>>> math.sin(50)
	-0.2623749
### 返回数字x的平方根
>math.sqrt(x)

返回数字x的平方根，返回值为浮点数。

示例：

	>>> import math
	>>> math.sqrt(4)
	2.0
	>>> math.sqrt(7)
	2.645751
### 返回x弧度的正切值
>math.tan(x)

返回x弧度的正切值，数值在-1到1之间，为浮点数。

示例：

	>>> import math
	>>> math.tan(9)
	-0.4523157
### 返回x的整数部分
>math.trunc(x)

返回x的整数部分。

示例：

	>>> import math
	>>> math.trunc(7.123)
	7
### 使用示例

	# 数学运算math函数示例
	import math
	import log

	if __name__ == '__main__':
	    #paw(x,y),输出x的y次方
	    result = math.pow(2,3)
	    log.info(1,result)
	    print(result)
	    #ceil(x),取大于x的最小整数，如果x为一个整数，则返回本身
	    result = math.ceil(5.21)
	    log.info(2,result)
	    print(result)
	    #acos(x),x的反余弦弧度值
	    result = math.acos(0.4)
	    log.info(3,result)
	    print(result)
	    #asin(x),x的反正弦弧度值
	    result = math.asin(0.5)
	    log.info(4,result)
	    print(result)
	    #atan(x),x的反正切弧度制
	    result = math.atan(1)
	    log.info(5,result)
	    print(result)
	    #atan2(x,y),返回给定的X及Y坐标值的反正切值。
	    result = math.atan2(1,2)
	    log.info(6,result)
	    print(result)
	    #copysign(x,y),把y的正负号加到x前面,可以使用0
	    result = math.copysign(2,-3)
	    log.info(7,result)
	    print(result)
	    #cos(x)求x的余弦，x必须是弧度
	    result = math.cos(math.pi)
	    log.info(8,result)
	    print(result)
	    #degrees(x)把x从弧度转化为角度
	    result = math.degrees(math.pi)
	    log.info(9,result)
	    print(result)
	    #e表示一个常数
	    result = math.e
	    log.info(10,result)
	    print(result)
	    #exp(x),返回math.e的x次方
	    result = math.exp(2)
	    log.info(11,result)
	    print(result)
	    #fabs(x)返回x的绝对值
	    result = math.fabs(-0.782)
	    log.info(12,result)
	    print(result)
	    #floor(x)取小于等于x的最大的整数值，x为整数时取自己本身
	    result = math.floor(5.21)
	    log.info(13,result)
	    print(result)
	    #fmod(x,y),取x/y的余数，其值为一个浮点数
	    result = math.fmod(3,1)
	    log.info(14,result)
	    print(result)
	    #modf(x)返回由x的小数部分和整数部分组成的元组
	    result = math.modf(5.1)
	    log.info(15,result)
	    print(result)
	    #frexp(x)返回一个元组(m,e),其计算方式为：x分别除0.5和1,得到一个值的范围，2e的值在这个范围内，e取符合要求的最大整数值,然后x/(2e),得到m的值。如果x等于0,则m和e的值都为0,m的绝对值的范围为(0.5,1)之间，不包括0.5和1
	    result = math.frexp(12)
	    log.info(16,result)
	    print(result)
	    #isfinite(x),如果x不是无穷大的数字，返回True，否则返回false
	    result = math.isfinite(123)
	    log.info(17,result)
	    print(result)
	    #isinf(x),如果x是正无穷大或者负无穷大，则返回True，否则返回false
	    result = math.isinf(1)
	    log.info(18,result)
	    print(result)
	    #isnan(x),如何X不是数字True，否则返回false
	    result = math.isnan(12)
	    log.info(19,result)
	    print(result)
	    #ldexp(x,y)返回值为x*(2的y次方)的值
	    result = math.ldexp(2,3)
	    log.info(20,result)
	    print(result)
	    #log(x)
	    result = math.log(4)
	    log.info(21,result)
	    print(result)
	    #pi：数字常量，表示圆周率π
	    result = math.pi
	    log.info(22,result)
	    print(result)
	    #radians(x)
	    result = math.radians(10)
	    log.info(23,result)
	    print(result)
	    #sin(x),求x的正弦值，x为弧度
	    result = math.sin(math.pi/2)
	    log.info(24,result)
	    print(result)
	    #sqrt(x),求x的平方根
	    result = math.sqrt(4)
	    log.info(25,result)
	    print(result)
	    #tan(x),求x的正切值，x必须是弧度
	    result = math.tan(math.pi/3)
	    log.info(26,result)
	    print(result)
	    #trunc(x),返回x的整数部分
	    result = math.trunc(5.21)
	    log.info(27,result)
	    print(result)
	
	
	
	
	





