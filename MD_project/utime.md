utime模块用于获取当前时间和日期、测量时间间隔和延迟。该模块实现相应CPython模块的子集。更多信息请参阅CPython文档：time
### 格式化时间戳
>utime.localtime([secs])

该函数用来将一个以秒表示的时间转换为一个元组,元组包含了了年、月、日、时、分、秒、星期、一年中第几天；如果没有给定参数sec，则使用RTC时间。返回值形式如下：(year, month, mday, hour, minute, second, weekday, yearday)
- year ：年份，int型
- month ：月份，1~12，int型
- mday ：日，当月多少号，1~31，int型
- hour ：小时，0~23，int型
- minute ：分钟，0~59，int型
- second ：秒，0~59，int型
- weekday ：星期，周一到周日是0~6，int型
- yearday ：一年中的第多少天，int型

示例：

	>>> import utime
	>>> utime.localtime()
	(2020, 9, 29, 8, 54, 42, 1, 273)
	>>> utime.localtime(646898736)
	(2020, 7, 1, 6, 5, 36, 2, 183)

### 反向格式化时间戳
>utime.mktime(date)

该函数作用与locatime()相反，它将一个存放在元组中的时间转换为以秒计的时间戳。

示例：

	>>> import utime
	>>> date = (2020, 9, 29, 8, 54, 42, 1, 273)
	>>> utime.mktime(date)
	1601340882

### 休眠给定秒数的时间
>utime.sleep(seconds)

休眠给定秒数的时间。

注意：sleep()函数的调用会导致程序休眠阻塞。

### 休眠给定毫秒数的时间
>utime.sleep_ms(ms)

休眠给定毫秒数的时间。

注意：sleep_ms()函数的调用会导致程序休眠阻塞。

### 返回不断递增的微妙计数器
>utime.ticks_us()

和ticks_ms()类似，只是返回微秒计数器。

### 和ticks_ms/ticks_us类似，具有更高精度（使用CPU时钟）
>utime.ticks_cpu()

和 ticks_ms/ticks_us 类似，具有更高精度 (使用 CPU 时钟)。

### 计算两次调用ticks_ms(),ticks_us()或ticks_cpu()之间的时间
>utime.ticks_diff(ticks1, ticks2)

计算两次调用 ticks_ms()， ticks_us()，或 ticks_cpu()之间的时间。因为这些函数的计数值可能会回绕，所以不能直接相减，需要使用 ticks_diff() 函数。“旧” 时间需要在 “新” 时间之前，否则结果无法确定。这个函数不要用在计算很长的时间 (因为 ticks_*() 函数会回绕，通常周期不是很长)。通常用法是在带超时的轮询事件中调用。

示例：

	import utime
	start = utime.ticks_us()
	while pin.value() == 0:
    	if utime.ticks_diff(time.ticks_us(), start) > 500:
        	raise TimeoutError

### 返回自纪元以来的秒数
>utime.time()

返回自纪元以来的秒数（以整数形式）。如果未设置RTC，则此函数返回自特定于端口的参考时间点以来的秒数（对于不具有电池后备RTC的嵌入式板，通常是由于加电或复位）。如果要开发可移植的MicroPython应用程序，则不应依赖此功能提供高于秒的精度。如果需要更高的精度，请使用 ticks_ms()和ticks_us()函数，如果需要日历时间，则 localtime()不带参数会更好。

### 获取时区
>utime.getTimeZone()

获取当前时区，单位小时，范围[-12, 12]，负值表示西时区，正值表示东时区，0表示零时区。

### 设置时区
>utime.setTimeZone(offset)

设置时区，单位小时，范围[-12, 12]，负值表示西时区，正值表示东时区，0表示零时区。设置时区后，本地时间会随之变化为对应时区的时间。

### 使用示例

	#日期和时间utime示例
	import utime
	import log
	if __name__ == '__main__':
    	for i in [0, 1, 2, 3, 4, 5]:
        	utime.sleep(1)   # 休眠(单位 s)
        	log.info(i)
        	print(i)
    	#localtime()接收时间戳（1970纪元后经过的浮点秒数）并返回当地时间下的时间元组 
    	log.info(utime.localtime())
    	print(utime.localtime())
    	#gmtime()函数将一个时间戳转换为UTC时区（0时区）的struct_time
    	log.info(utime.gmtime())
    	print(utime.gmtime())
    	#mktime()接收struct_time对象作为参数，返回用秒数来表示时间的浮点数
    	log.info(utime.mktime(utime.localtime()))
	    print(utime.mktime(utime.localtime()))      
	    log.info(utime.ticks_ms())         #返回不断递增的毫秒计数器
	    print(utime.ticks_ms())        
	    log.info(utime.ticks_us())         #返回不断递增的微秒计数器
	    print(utime.ticks_us())
	    log.info(utime.ticks_cpu())        #和 ticks_ms/ticks_us类似，具有更高精度(使用 CPU 时钟)
	    print(utime.ticks_cpu())
	    
	    deadline = utime.ticks_add(utime.ticks_ms(), 200)
	    while utime.ticks_diff(deadline, utime.ticks_ms()) > 0:
	        print(utime.time())
	        print(utime.time_ns())
	        log.info(utime.time())
	        log.info(utime.time_ns())
	
	    for i in [0, 1, 2, 3, 4, 5]:
	        utime.sleep_ms(1000)   # 休眠(单位 ms)
	        log.onfo(i)
	        print(i)
	
	    for i in [0, 1, 2, 3, 4, 5]:
	        utime.sleep_us(1000000)   # 休眠(单位 us)
	        log.info(i)
	        print(i)



