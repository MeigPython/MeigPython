## Pin

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


## UART
			
类功能：uart串口数据传输。
### 常量说明
|常量|说明|
|----|---|
|UART.UART0|UART0|
|UART.UART1|UART1|
|UART.UART2|UART2|
|UART.UART3|UART3|

### 创建uart对象
>uart = UART(UART.UARTn, buadrate, databits, parity, stopbits, flowctl)

- 参数

|参数|类型|说明|
|---|----|----|
|UARTn|int|UARTn作用如下：<br>UART0 - DEBUG PORT<br>UART1 – BT PORT<br>UART2 – MAIN PORT<br>UART3 – USB CDC PORT|
|buadrate|int|波特率，常用波特率都支持，如4800、9600、19200、38400、57600、115200、230400等|
|databits|int|数据位（5~8），展锐平台当前仅支持8位|
|parity|int|奇偶校验（0 – NONE，1 – EVEN，2 - ODD）|
|stopbits|int|停止位（1~2）|
|flowctl|int|硬件控制流（0 – FC_NONE， 1 – FC_HW）|

- 示例


	>>> from machine import UART
	>>> uart1 = UART(UART.UART1, 115200, 8, 0, 1, 0)

### 获取接收缓存未读数据大小
>uart.any()

返回接收缓存器中有多少字节的数据未读。

- 参数

无

- 返回值

返回接收缓存器中有多少字节的数据未读。

- 示例


	>>> uart.any()
	20 #表示接收缓冲区中有20字节数据未读

### 串口读数据
>uart.read(nbytes)

- 参数

|参数|类型|说明|
|---|-----|---|
|nbytes|int|要读取的字节数|

- 返回值

返回读取的数据。

### 串口发数据
>uart.write(data)

发送数据到串口。

- 参数

|参数|类型|说明|
|---|----|----|
|data|string|发送的数据|

- 返回值

返回发送的字节数。

### 使用示例
	
	from machine import UART
	import utime
	import log
	#创建UART对象
	uart1 = UART(UART.UART1, 115200, 8, 0, 1, 0)
	usb_cdc = UART(UART.UART3)
	if __name__ == '__main__':
	    while True:
	        #返回接收缓存器中有多少字节的数据未读
	        read_msg_len = usb_cdc.any()
	        #判断是否接收到数据
	        if read_msg_len:
	            #从串口读取数据
	            read_msg = usb_cdc.read(read_msg_len)
	            print("usb cdc msg:{}".format(read_msg))
	            log.info("usb cdc msg:{}".format(read_msg))
	            #发送数据到串口
	            usb_cdc.write(read_msg)
	        utime.sleep_ms(1)

## Timer

类功能：硬件定时器。
PS:使用该定时器时需注意：定时器0-3，每个在同一时间内只能执行一件任务，且多个对象不可使用同一个定时器。

### 常量说明
|常量|说明|
|---|----|
|Timer.Timer0|定时器0|
|Timer.Timer1|定时器1|
|Timer.Timer2|定时器2|
|Timer.Timer3|定时器3|
|Timer.ONE_SHOT|单次模式，定时器只执行一次|
|Timer.PERIODIC|周期模式，定时器循环执行|

### 创建Timer对象
>timer = Timer(Timern)

创建Timer对象。

- 参数

|参数|类型|说明|
|---|----|----|
|Timern|int|定时器号<br>支持定时器Timer0~Timer3（使用该定时器时需注意：定时器0-3，每个在同一时间内只能执行一件任务，且多个对象不可使用同一个定时器。）|

- 示例


	>>> from machine import Timer
	>>> timer1 = Timer(Timer.Timer1)  # 使用该定时器时需注意：定时器0-3，每个在同一时间内只能执行一件任务，且多个对象不可使用同一个定时器。

### 启动定时器
>timer.start(period, mode, callback)

启动定时器

- 参数

|参数|类型|说明|
|---|----|----|
|period|int|中断周期，单位毫秒，大于等于1|
|mode|int|运行模式<br>Timer.ONE_SHOT 单次模式，定时器只执行一次<br>Timer.PERIODIC 周期模式，循环执行|
|callback|function|定时器执行函数|

- 返回值

启动成功返回整型值0，失败返回整型值-1。

- 示例



	// 使用该定时器时需注意：定时器0-3，每个在同一时间内只能执行一件任务，且多个对象不可使用同一个定时器。
	>>> def fun(args):
	        print(“###timer callback function###”)
	>>> timer.start(period=1000, mode=timer.PERIODIC, callback=fun)
	0

### 关闭定时器
>timer.stop()

关闭定时器

- 参数

无

- 返回值

成功返回整型值0，失败返回整型值-1。

### 使用示例
#timer模块示例

	import utime
	import log
	from machine import timer
	
	num = 0
	state = 1
	
	t = timer(1)
	#创建一个执行函数
	def timer_test(t):
	    print("timer run",num)
	    log.info("timer run",num)
	
	if __name__ == '__main__':
	    print("create")
	    #传入timer实例，中断时间2000ms，周期执行timer_test函数
	    t.start(period=2000, mode=t.PERIODIC, callback=timer_test)
	    print("ok")
	    while state :
	        num += 1
	        if num > 10:
	            print("timer stop")
	            log.info("timer stop")
	            state = 0
	            t.stop()   #关闭定时器
	        pass
	        utime.sleep(2)

## ExtInt

类功能：用于配置I/O引脚在发生外部事件时中断。

### 常量说明
|常量|适配平台|说明|
|---|-------|----|
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

### 创建ExtInt对象
>extint = ExtInt(GPIOn, mode, pull, callback)

- 参数

|参数|类型|说明|
|---|----|---|
|GPIOn|int|引脚号,平台引脚对应关系如下(引脚号为外部引脚编号：<br>GPIO1 – 引脚号3   <br>GPIO2 – 引脚号4 <br>GPIO3 – 引脚号5  <br>GPIO4 – 引脚号6   <br>GPIO5 – 引脚号18|
|mode|int|设置触发方式<br>IRQ_RISING – 上升沿触发<br>IRQ_FALLING – 下降沿触发<br>IRQ_RISING_FALLING – 上升和下降沿触发|
|pull|int|PULL_DISABLE – 浮空模式<br>PULL_PU – 上拉模式<br>PULL_PD – 下拉模式|
|callback|int|中断触发回调函数|

- 示例


	>>> from machine import ExtInt
	>>> def fun(args):
	        print('### interrupt  {} ###'.format(args))
	>>> extint = ExtInt(ExtInt.GPIO1, ExtInt.IRQ_FALLING, ExtInt.PULL_PU, fun)

### 使能中断
>extint.enable()

使能extint对象外部中断，当中断引脚收到上升沿或者下降沿信号时，会调用callback执行 。

- 参数

无

- 返回值

使能成功返回整型值0，使能失败返回整型值-1。

### 关闭中断
>extint.disable()

禁用与extint对象关联的中断 。

- 参数

无

- 返回值

使能成功返回整型值0，使能失败返回整型值-1。

## RTC


类功能：提供获取设置rtc时间方法。

### 创建RTC对象
>from machine import RTC

>rtc = RTC()

### 设置和获取RTC时间
>rtc.datetime([year, month, day, week, hour, minute, second, microsecond])

设置和获取RTC时间，不带参数时，则用于获取时间，带参数则是设置时间；设置时间的时候，参数week不参于设置，microsecond参数保留，暂未使用，默认是0。

- 参数

|参数|类型|说明|
|---|----|----|
|year|int|年|
|month|int|月，范围1~12|
|day|int|日，范围1~31|
|week|int|星期，范围0 ~ 6，其中0表示周日，1 ~ 6分别表示周一到周六；设置时间时，该参数不起作用，保留；获取时间时该参数有效|
|hour|int|时，范围0~23|
|minute|int|分，范围0~59|
|second|int|秒，范围0~59|
|microsecond|int|微秒，保留参数，暂未使用，设置时间时该参数写0即可|

- 返回值

获取时间时，返回一个元组，包含日期时间，格式如下：

[year, month, day, week, hour, minute, second, microsecond]

设置时间时，设置成功返回整型值0，设置失败返回整型值-1 。

- 示例


	>>> from machine import RTC
	>>> rtc = RTC()
	>>> rtc.datetime()
	(2020, 9, 11, 5, 15, 43, 23, 0)
	>>> rtc.datetime([2020, 3, 12, 1, 12, 12, 12, 0])
	0
	>>> rtc.datetime()
	(2020, 3, 12, 4, 12, 12, 14, 0)













	






			
