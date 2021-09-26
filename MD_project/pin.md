类功能：GPIO读写操作

### 常量说明

|常量|适配平台|说明|
|----|-------|----|
|Pin.GPIO1|  |GPIO1|
|Pin.GPIO2|  |GPIO2|
|Pin.GPIO3|  |GPIO3|
|Pin.GPIO4|  |GPIO4|
|Pin.GPIO5|  |GPIO5|
|Pin.IN|--|输入模式|
|Pin.OUT|--|输出模式|
|Pin.PULL_DISABLE|--|浮空模式|
|Pin.PULL_PU|--|上拉模式|
|Pin.PULL_PD|--|下拉模式|

####  GPIO对应引脚说明
文档中提供的GPIO引脚号对应的为模块外部的引脚编号，可参考提供的硬件资料查看模块外部的引脚编号。

### 创建GPIO对象
>gpio = Pin(GPIOn, direction, pullMode, level)

- 参数

|参数|类型|说明|
|---|----|---|
|GPIOn|int|引脚号,平台引脚对应关系如下(引脚号为外部引脚编号：<br>GPIO1 – 引脚号3   <br>GPIO2 – 引脚号4 <br>GPIO3 – 引脚号5  <br>GPIO4 – 引脚号6   <br>GPIO5 – 引脚号18|
|direction|int|IN – 输入模式，OUT – 输出模式|
|pullMode|int|PULL_DISABLE – 浮空模式 <br>PULL_PU – 上拉模式 <br>	PULL_PD – 下拉模式|
|level|int|0 - 设置引脚为低电平, 1- 设置引脚为高电平|

- 示例


	from machine import Pin
	gpio1 = Pin(Pin.GPIO1, Pin.OUT, Pin.PULL_DISABLE, 0)

### 获取引脚电平
>Pin.read()

获取PIN脚电平。

- 参数

无

- 返回值

PIN脚电平，0-低电平，1-高电平。

### 设置引脚电平
>Pin.write(value)

设置PIN脚电平,设置高低电平前需要保证引脚为输出模式。

- 参数

|参数|类型|说明|
|---|----|----|
|value|int|0 - 当PIN脚为输出模式时，设置当前PIN脚输出低;<br>1 - 当PIN脚为输出模式时，设置当前PIN脚输出高|

- 返回值

设置成功返回整型值0，设置失败返回整型值-1。

- 示例


	>>> from machine import Pin
	>>> gpio1 = Pin(Pin.GPIO1, Pin.OUT, Pin.PULL_DISABLE, 0)
	>>> gpio1.write(1)
	0
	>>> gpio1.read()
	1

### 使用示例

	from machine import pin
	import utime
	import log
	'''
	* 参数2：direction
	        IN – 输入模式
	        OUT – 输出模式
	* 参数3：pull
	        PULL_DISABLE – 禁用模式
	        PULL_PU – 上拉模式
	        PULL_PD – 下拉模式
	* 参数4：level
	        0 设置引脚为低电平
	        1 设置引脚为高电平
	'''
	#初始化GPIO3和GPIO4
	GPIO3 = pin(pin.GPIO3, pin.OUT, pin.PULL_DISABLE, 1)
	GPIO4 = pin(pin.GPIO4, pin.OUT, pin.PULL_DISABLE, 1)
	while True:
	    GPIO3.write(1)          #设置GPIO3为高电平
	    GPIO4.write(1)          #设置GPIO4为高电平
	    val3 = GPIO3.read()     #获取GPIO3的电平状态
	    val4 = GPIO4.read()     #获取GPIO4的电平状态
	    print('val3 = {}'.format(val3))
	    print('val4 = {}'.format(val4))
	    utime.sleep_ms(500)
	    GPIO3.write(0)          #设置GPIO3为低电平
	    GPIO4.write(0)          #设置GPIO4为低电平
	    val3 = GPIO3.read()     #获取GPIO3的电平状态
	    val4 = GPIO4.read()     #获取GPIO4的电平状态
	    print('val3 = {}'.format(val3))
	    print('val4 = {}'.format(val4))
	    utime.sleep_ms(500)



			
