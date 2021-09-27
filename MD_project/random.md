random模块提供了生成随机数的工具。
### 随机生成对象obj中的元素

>random.choice(obj)

随机生成对象obj中的元素，obj类型string。

示例：

	>>> import urandom as random
	>>> random.choice("QuecPython")
	't'
	
### 随机产生一个在k bits范围内得十进制数
>random.getrandbits(x)

随机产生一个在x bits范围内的十进制数。

示例：

	>>> import urandom as random
	>>> random.getrandbits(1)  #1位二进制位，范围为0~1（十进制：0~1）
	1
	>>> random.getrandbits(1)
	0
	>>> random.getrandbits(8)  #8位二进制位，范围为0000 0000~1111 11111（十进制：0~255）
	224

### 随机生成一个x到y之间的整数
>random.randint(x, y)

随机生成一个x到y之间的整数。

示例：

	>>> import urandom as random
	>>> random.randint(1, 4)
	4
	>>> random.randint(1, 4)
	2
### 随机生成0到1之间的浮点数
>random.random()

随机生成0到1之间的浮点数。

示例：

	>>> import urandom as random
	>>> random.random()
	0.8465231

### 随机生成x到y之间并且递增为z的正整数
>random.randrange(x, y, z)

随机生成x到y之间并且递增为z的正整数。

示例：

	>>> import urandom as random
	>>> random.randrange(0, 8, 2)
	0
	>>> random.randrange(0, 8, 2)
	6

### 指定随机数种子
>random.seed(sed)

指定随机数种子，通常和其他随机数生成函数搭配使用。

示例：

	>>> import urandom as random
	>>> random.seed(20)  #指定随机数种子
	>>> for i in range(0, 15): #生成0~15范围内的随机序列
	...     print(random.randint(1, 10))
	...     
	8
	10
	9
	10
	2
	1
	9
	3
	2
	2
	6
	1
	10
	9
	6
### 随机生成x到y之间的浮点数
>random.uniform(x, y)

随机生成 start 到 end 范围内的浮点数。

示例：

	>>> import urandom as random
	>>> random.uniform(3, 5)
	3.219261
	>>> random.uniform(3, 5)
	4.00403

### 使用用例

	#random随机数示例

	import urandom as random
	import log 

	if __name__ == '__main__':
    #产生1到4的一个整数型随机数
    num = random.randint(1, 4)
    log.info(num)
    print(num)

    #返回随机生成的一个实数
    num = random.random()
    log.info(num)
    print(num)

    #产生  1.1 到 5.4 之间的随机浮点数，区间可以不是整数
    num = random.uniform(2, 4)
    log.info(num)
    print(num)

    #getrandbits(x)生成一个x比特长的随机整数
    num = random.getrandbits(2)
    log.info(num)
    print(num)

    num = random.getrandbits(8)
    log.info(num)
    print(num)

    #生成从2到8的间隔为2的随机整数
    num = random.randrange(2, 8, 2)
    log.info(num)
    print(num)

    #从序列中随机选取一个元素
    num = random.choice("QuecPython")
    log.info(num)
    print(num)




