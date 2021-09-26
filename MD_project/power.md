模块功能，提供关机，重启，获取电池电压。


>关机以及软件重启

### 模块关机
>Power.powerDown()

模块关机。

- 参数

无

- 返回值

无

### 模块重启
>Power.powerRestart()

模块重启。

- 参数

无

- 返回值

无

### 获取模块启动原因
>Power. powerOnReason()

获取模块启动原因。

- 参数

无

- 返回值

返回int数值，解释如下：

1：正常电源开机

2：重启

3：VBAT

4：RTC定时开机

5：Fault

6：VBUS

0：未知

- 注意

C25PA平台支持仅不支持重启原因5。

### 获取模块上次关机原因
>Power. powerDownReason()

获取模块上次关机原因。

- 参数

无

- 返回值

1：正常电源关机

2：电压过高

3：电压偏低

4：超温

5：WDT

6：VRTC 偏低

0：未知

### 获取电池电压
>Power. getVbatt()

获取电池电压，单位mV。

- 参数

无

- 返回值

int类型电压值。

- 示例


	>>> Power.getVbatt()
	3590





