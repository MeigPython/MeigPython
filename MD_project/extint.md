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

### 读取引脚映射行号

>extint.line()

返回引脚映射的行号。

- 参数

无

- 返回值

引脚映射的行号。

- 示例


	>>> extint = ExtInt(ExtInt.GPIO1, ExtInt.IRQ_FALLING, ExtInt.PULL_PU, fun)
	>>> extint.line()
	32








