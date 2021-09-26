
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
