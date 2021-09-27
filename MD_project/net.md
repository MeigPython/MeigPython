模块功能：该模块提供配置和查询网络模式信息等接口。
### 设置APN
>net.setApn(apn, simid)

设置APN。
- 参数

|参数|参数类型|参数说明|
|----|------|--------|
|apn|string|apn name|
|simid|int|simid (0:卡1 1:卡2)|

- 返回值

设置成功返回整型值0，设置失败返回整型值-1。


### 获取当前APN
>net.getApn(simid)

- 参数

|参数|参数类型|参数说明|
|----|-------|-------|
|simid|int|simid|

- 返回值

成功返回获取到的APN，失败返回整型值-1。


### 获取csq信号强度
>net.csqQueryPoll()

获取csq信号强度。

- 参数

无

- 返回值

成功返回整型的csq信号强度值，失败返回整型值-1，返回值为99表示异常；

信号强度值范围0~31，值越大表示信号强度越好。

- 示例


	>>> import net
	>>> net.csqQueryPoll()
	31

### 获取小区信息
>net.getCellInfo()

获取邻近 CELL 的信息。

- 参数

无

- 返回值

失败返回整型值-1，成功返回包含三种网络系统（GSM、UMTS、LTE）的信息的list，如果对应网络系统信息为空，则返回空的List。返回值格式如下：

([(flag, cid, mcc, mnc, lac, arfcn, bsic, rssi)], [(flag, cid, licd, mcc, mnc, lac, arfcn, bsic, rssi)], [(flag, cid, mcc, mnc, pci, tac, earfcn, rssi),...])

GSM网络系统返回值说明

|参数|参数意义|
|----|-------|
|flag|返回 0 - 2， 0：present，1：inter，2：intra|
|cid|返回cid信息，0则为空|
|mcc|移动设备国家代码|
|mnc|移动设备网络代码|
|lac|位置区码|
|arfcn|无线频道编号|
|bsic|基站识别码|
|rssi|接收的信号强度|

UMTS网络系统返回值说明

|参数|参数意义|
|----|-------|
|flag|返回 0 - 2， 0：present，1：inter，2：intra|
|cid|返回cid信息，0则为空|
|lcid|区域标识号|
|mcc|移动设备国家代码|
|mnc|移动设备网络代码|
|lac|位置区码u|
|uarfcn|无线频道编号|
|psc|基站识别码|
|rssi|接收的信号强度|

LTE网络系统返回值说明

|参数|参数意义|
|----|-------|
|flag|返回 0 - 2， 0：present，1：inter，2：intra|
|cid|返回cid信息，0则为空|
|mcc|移动设备国家代码|
|mnc|移动设备网络代码|
|pci|小区标识|
|tac|Tracing area code|
|earfcn|无线频道编号 范围: 0 - 65535|
|rssi|接收的信号强度|

- 示例


	>>> net.getCellInfo()
	([], [], [(0, 14071232, 1120, 0, 123, 21771, 1300, -69), (3, 0, 0, 0, 65535, 0, 40936, -140), (3, 0, 0, 0, 65535, 0, 3590, -140), (3, 0, 0, 0, 63, 0, 40936, -112)])

### 获取网络制式及漫游配置
>net.getConfig()

获取当前网络模式、漫游配置。

- 参数

无

- 返回值

失败返回整型值-1，成功返回一个元组，包含当前首选的网络制式与漫游打开状态。

网络制式

|值|网络制式|
|--|-------|
|0|GSM|
|1|UMTS . not supported in EC100Y|
|2|GSM_UMTS, auto. not supported in EC100Y and EC200S|
|3|GSM_UMTS, GSM preferred. not supported in EC100Y and EC200S|
|4|SM_UMTS, UMTS preferred. not supported in EC100Y and EC200S|
|5|LTE|
|6|GSM_LTE, auto, single link|
|7|GSM_LTE, GSM preferred, single link|
|8|GSM_LTE, LTE preferred, single link|
|9|UMTS_LTE, auto, single link. not supported in EC100Y and EC200S|
|10|UMTS_LTE, UMTS preferred, single link. not supported in EC100Y and EC200S|
|11|UMTS_LTE, LTE preferred, single link . not supported in EC100Y and EC200S|
|12|GSM_UMTS_LTE, auto, single link. not supported in EC100Y and EC200S|
|13|GSM_UMTS_LTE, GSM preferred, single link. not supported in EC100Y and EC200S|
|14|GSM_UMTS_LTE, UMTS preferred, single link. not supported in EC100Y and EC200S|
|15|GSM_UMTS_LTE, LTE preferred, single link. not supported in EC100Y and EC200S|
|16|GSM_LTE, dual link|
|17|UMTS_LTE, dual link. not supported in EC100Y and EC200S|
|18|GSM_UMTS_LTE, dual link. not supported in EC100Y and EC200S|

- 示例


	>>>net.getConfig ()
	(8, False)

### 设置网络制式及漫游配置
>net.setConfig(mode, roaming)

设置网络模式，漫游配置。

- 参数

|参数|参数类型|参数说明|
|----|-------|------|
|mode|int|网络制式，0~18，详见上述网络制式表格|
|roaming|int|漫游开关(0：关闭， 1：开启)|

- 返回值

设置成功返回整型值0，设置失败返回整型值-1。

### 获取网络配置模式
>net.getNetMode()

获取网络配置模式。

- 参数

无

- 返回值

失败返回整型值-1，成功返回一个元组，格式为：(selection_mode, mcc, mnc, act)

返回值参数说明： selection_mode ：方式，0 - 自动，1 - 手动 mcc ：移动设备国家代码 mnc ：移动设备网络代码 act ：首选网络的ACT模式

ACT模式

|值|ACT模式|
|---|------|
|0|GSM|
|1|COMPACT|
|2|UTRAN|
|3|GSM wEGPRS|
|4|UTRAN wHSDPA|
|5|UTRAN wHSUPA|
|6|UTRAN wHSDPA HSUPA|
|7|E UTRAN|
|8|UTRAN HSPAP|
|9|E TRAN A|
|10|NONE|

- 示例


	>>> net.getNetMode()
	(0, '460', '46', 7)

### 获取详细信号强度信息
>net.getSignal()

获取详细信号强度。

- 参数

无

- 返回值

失败返回整型值-1，成功返回一个元组，包含两个List(GW 、LTE)，返回值格式如下：

([rssi, bitErrorRate, rscp, ecno], [rssi, rsrp, rsrq, cqi])

返回值参数说明：

GW list：

rssi ：接收的信号强度

bitErrorRate ：误码率

rscp ：接收信号码功率

ecno ：导频信道

LTE list：

rssi ：接收的信号强度

rsrp ：下行参考信号的接收功率

rsrq ：下行特定小区参考信号的接收质量

cqi ：信道质量

- 示例


	>>>net.getSignal()
	([99, 99, 255, 255], [-51, -76, -5, 255])

### 获取当前基站时间
>net.nitzTime()

获取当前基站时间。

- 参数

无

- 返回值 

失败返回整型值-1，成功返回一个元组，包含基站时间与对应时间戳与闰秒数（0表示不可用），格式为：

(date, abs_time, leap_sec)

date ：基站时间，string类型

abs_time ：基站时间的绝对秒数表示，整型

leap_sec ：闰秒数，整型

- 示例


	>>> net.nitzTime()
	('20/11/26 02:13:25+32', 1606356805, 0)

### 获取运营商信息
>net.operatorName()

获取当前注网的运营商信息。

- 参数

无

- 返回值

失败返回整型值-1，成功返回一个元组，包含注网的运营商信息，格式为：

(long_eons, short_eons, mcc, mnc)

long_eons ：运营商信息全称，string类型

short_eons ：运营商信息简称，string类型

mcc ：移动设备国家代码，string类型

mnc ：移动设备网络代码，string类型

- 示例


	>>> net.operatorName()
	('CHN-UNICOM', 'UNICOM', '460', '01')

### 获取附近小区ID
>net.getCi()

获取附近小区ID。

- 参数

无

- 返回值

成功返回一个list类型的数组，包含小区id，格式为：[id, ……, id]。数组成员数量并非固定不变，位置不同、信号强弱不同等都可能导致获取的结果不一样。

失败返回整型值-1。

- 示例


	>>> net.getCi()
	[14071232, 0]

### 获取附近小区的mnc

>net.getMnc()

获取附近小区的mnc。

- 参数

无

- 返回值

成功返回一个list类型的数组，包含小区mnc，格式为：[mnc, ……, mnc]。数组成员数量并非固定不变，位置不同、信号强弱不同等都可能导致获取的结果不一样。

失败返回整型值-1。

- 示例


	>>> net.getMnc()
	[0, 0]

### 获取附近小区的额mcc
>net.getMcc()

获取附近小区的mcc。

- 参数

无

- 返回值

成功返回一个list类型的数组，包含小区mcc，格式为：[mcc, ……, mcc]。数组成员数量并非固定不变，位置不同、信号强弱不同等都可能导致获取的结果不一样。

失败返回整型值-1。

- 示例
	

	>>> net.getMcc()
	[1120, 0]

### 获取附近小区的lac

>net.getLac()

获取附近小区的Lac。

- 参数

无

- 返回值

成功返回一个list类型的数组，包含小区lac，格式为：[lac, ……, lac]。数组成员数量并非固定不变，位置不同、信号强弱不同等都可能导致获取的结果不一样。

失败返回整型值-1。

- 示例


	>>> net.getLac()
	[21771, 0]

### 获取当前工作模式
>net.getModemFun()

获取当前工作模式。

- 参数

无

- 返回值

成功返回当前SIM模式：

0 ：全功能关闭

1 ：全功能开启（默认）

4 ：飞行模式

失败返回整型值-1。

- 示例


	>>> net.getModemFun()
	1

### 设置当前工作模式

>net.setModemFun(function, rst)

设置当前SIM模式

- 参数

|参数|参数类型|参数说明|
|----|-------|------|
|function|int|设置SIM卡模式，0 - 全功能关闭， 1 - 全功能开启， 4 - 飞行模式 (RDA平台不支持cfun4)|
|rst|int|可选参数 ，0 - 设置立即生效（默认为0），1 - 设置完重启|

- 返回值

设置成功返回整型值0，设置失败返回整型值-1。

- 示例


	>>> net.setModemFun(4)
	0
















