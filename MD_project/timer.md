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






	





