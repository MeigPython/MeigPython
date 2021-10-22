## dataCall - 数据拨号
模块功能：提供数据拨号相关接口。
### 拨号
>dataCall.start(profileIdx, ipType, apn, username, password, authType)

启动拨号，进行数据链路激活。
- 参数
|参数|参数类型|参数说明|
|---|-------|-------|
|profileIdx|int|PDP索引，范围1-3，一般设置为1|
|ipType|int|IP类型，0-IPV4，1-IPV6，2-IPV4和IPV6|
|apn|string|apn名称，可为空，最大长度不超过99字节|
|username|string|apn用户名，可为空，最大长度不超过64字节|
|password|string|apn密码，可为空，最大长度不超过64字节|
|authType|int|加密方式，0-不加密，1-PAP，2-CHAP|
- 返回值

成功返回整型值0，失败返回整型值-1。

- 示例 


	>>> import dataCall
	>>> dataCall.startdataCall.start(0, 1, 2, "ctnet", "", "", 0)


### 注册回调函数
>dataCall.setCallback(usrFun)

注册用户回调函数，当网络状态发生变化，比如断线、上线时，会通过该回调函数通知用户。

- 参数
|参数|参数类型|参数说明|
|----|-------|-------|
|usrFun|function|用户回调函数，函数形式见示例|
- 返回值

注册失败返回整型-1，成功返回整型0。

- 示例



	>>> import dataCall
	>>> import net
	
	>>> def nw_cb(args):
	        pdp = args[0]
	        nw_sta = args[1]
	        if nw_sta == 1:
        		print("*** network %d connected! ***" % pdp)
        		
	        else:
	            print("*** network %d not connected! ***" % pdp)
				
	            
	>>> dataCall.setCallback(nw_cb)

### 获取拨号信息
>dataCall.getInfo(profileIdx, ipType)

获取数据拨号信息，包括连接状态、IP地址、DNS等。
- 参数
|参数|参数类型|参数说明|
|----|-------|-------|
|profileIdx|int|PDP索引，ASR平台范围1-8，展锐平台范围1-7|
|ipType|int|IP类型，0-IPV4，1-IPV6，2-IPV4和IPV6|
- 返回值

错误返回整型-1，成功返回拨号信息，返回格式根据ipType的不同而有所区别： ipType =0，返回值格式如下：
(profileIdx, ipType, [nwState, reconnect, ipv4Addr, priDns, secDns])

profileIdx：PDP索引，ASR平台范围1-8，展锐平台范围1-7

ipType：IP类型，0-IPV4，1-IPV6，2-IPV4和IPV6

nwState：拨号结果，0-失败，1-成功

reconnect：重拨标志

ipv4Addr：ipv4地址

priDns：dns信息

secDns：dns信息

ipType =1，返回值格式如下：

(profileIdx, ipType, [nwState, reconnect, ipv6Addr, priDns, secDns])

profileIdx：PDP索引，ASR平台范围1-8，展锐平台范围1-7

ipType：IP类型，0-IPV4，1-IPV6，2-IPV4和IPV6

nwState：拨号结果，0-失败，1-成功

reconnect：重拨标志

ipv6Addr：ipv6地址

priDns：dns信息

secDns：dns信息

ipType =2，返回值格式如下：

(profileIdx, ipType, [nwState, reconnect, ipv4Addr, priDns, secDns], [nwState, reconnect, ipv6Addr, priDns, secDns])

- 示例


	>>> import dataCall
	>>> dataCall.getInfo(1, 0)
	(1, 0, [1, 0, '10.91.44.177', '58.242.2.2', '218.104.78.2'])

注：返回值 (1, 0, [0, 0, '0.0.0.0', '0.0.0.0', '0.0.0.0']) 表示当前没有拨号或者拨号没有成功。

### 使用用例

	import utime
	import dataCall
	import log
	import net
	
	# 设置日志输出级别
	log.basicConfig(level=log.INFO)
	Datacall_log = log.getLogger("Datacall")
	
	def nw_cb(args):
	    sim = args[0]
	    pdp = args[1]   # pdp索引
	    nw_sta = args[2]  # 网络连接状态 0未连接， 1已连接
	    if nw_sta == 1:
	        Datacall_log.info("*** network %d connected! ***" % pdp)
	        Datacall_log.info(dataCall.getInfo(0, 1))
	        #Datacall_log.info(dataCall.getInfo(0, 1))
	        #Datacall_log.info(dataCall.getInfo(0, 1))
	    else:
	        Datacall_log.info("*** network %d not connected! ***" % pdp)
	        Datacall_log.info(dataCall.getInfo(0, 1))
	
	if __name__ == '__main__':
	    #启动拨号，进行数据链路激活，成功返回0，失败返回-1
	    iret = dataCall.start(0, 1, 2, "ctnet", "", "", 0)
	    #注册用户回调函数，当网络状态发生变化，会通过回调函数通知用户
	    dataCall.setCallback(nw_cb)
	    Datacall_log.info("datacall start", iret)
	    utime.sleep(2)
	    while True :
	        utime.sleep(2)
	        Datacall_log.info("wait")
	        Datacall_log.info(iret)
	        Datacall_log.info(dataCall.getInfo(0, 1))
	   
## sim - SIM卡

模块功能：提供sim卡操作相关API，如查询sim卡状态、iccid、imsi等。

注意：能成功获取IMSI、ICCID、电话号码的前提是SIM卡状态为1，可通过sim.getStatus()查询。

### 获取IMSI
>sim.getImsi()

获取sim卡的imsi。
- 参数
无
- 返回值
成功返回string类型的imsi，失败返回整型-1。
- 示例


	>>> import sim
	>>> sim.getImsi()
	b'460011442119027'

### 获取ICCID
>sim.getIccid()

获取sim卡的iccid。
- 参数
无
- 返回值
成功返回string类型的iccid，失败返回整型-1。
- 示例


	>>> sim.getIccid()
	b'8986012180115044751'

### 获取电话号码
>sim.getPhoneNumber()

获取sim卡的电话号码。
- 参数

无
- 返回值

|返回值|说明|
|------|----|
|0|SIM was removed.|
|1|SIM is ready.|
|2|Expecting the universal PIN./SIM is locked, waiting for a CHV1 password.|
|3|Expecting code to unblock the universal PIN./SIM is blocked, CHV1 unblocking password is required.|
|4|SIM is locked due to a SIM/USIM personalization check failure.|
|5|SIM is blocked due to an incorrect PCK; an MEP unblocking password is required.|
|6|Expecting key for hidden phone book entries.|
|7|Expecting code to unblock the hidden key.|
|8|SIM is locked; waiting for a CHV2 password.|
|9|SIM is blocked; CHV2 unblocking password is required.|
|10|SIM is locked due to a network personalization check failure.|
|11|SIM is blocked due to an incorrect NCK; an MEP unblocking password is required.|
|12|SIM is locked due to a network subset personalization check failure.|
|13|SIM is blocked due to an incorrect NSCK; an MEP unblocking password is required.|
|14|SIM is locked due to a service provider personalization check failure.|
|15|SIM is blocked due to an incorrect SPCK; an MEP unblocking password is required.|
|16|SIM is locked due to a corporate personalization check failure.|
|17|SIM is blocked due to an incorrect CCK; an MEP unblocking password is required.|
|18|SIM is being initialized; waiting for completion.|
|19|Use of CHV1/CHV2/universal PIN/code to unblock the CHV1/code to unblock the CHV2/code to unblock the universal PIN/ is blocked.|
|20|Invalid SIM card.|
|21|Unknow status.|

### PIN码验证
>sim.verifyPin(pin)

sim卡PIN码验证。需要在调用sim.enablePin(pin)成功之后，才能进行验证，验证成功后，sim卡才能正常使用。
- 参数

|参数|参数类型|参数说明|
|----|------|--------|
|pin|string|PIN码，一般默认是‘1234’，最大长度不超过15字节|

### SIM卡解锁
>sim.unblockPin(puk, newPin)

sim卡解锁。当多次错误输入 PIN/PIN2 码后，SIM 卡状态为请求 PUK/PUK2 时，输入 PUK/PUK2 码和新的 PIN/PIN2 码进行解锁，puk码输入10次错误，SIM卡将被永久锁定自动报废。

- 参数

|参数|参数类型|参数说明|
|----|-------|-------|
|puk|string|PUK码，长度8位数字，最大长度不超过15字节|
|newPin|string|新PIN码，最大长度不超过15字节|

- 返回值

解锁成功返回整型0，解锁失败返回整型-1。


- 示例


	>>> sim.unblockPin("12345678", "0000")

### 更改SIM卡PIN码
>sim.changePin(oldPin, newPin)

更改sim卡PIN码。

- 参数

|参数|参数类型|参数说明|
|----|-------|-------|
|oldpin|string|PUK码，长度8位数字，最大长度不超过15字节|
|newpin|string|新PIN码，最大长度不超过15字节|

- 返回值

更改成功返回整型0，更改失败返回整型-1。


- 示例


	>>> sim.changePin("1234", "4321")

### 读电话簿
>sim.readPhonebook(storage, start, end, username)

获取 SIM 卡上指定电话本中的一条或多条电话号码记录。

- 参数

|参数|参数类型|参数说明|
|----|-------|-------|
|storage|int|需要读取电话号码记录的电话本存储位置，可选参数如下：<br>0 – DC，1 – EN，2 – FD，3 – LD，4 – MC，5 – ME，6 – MT，7 – ON，8 – RC，9 – SM，10 – AP，11 – MBDN，12 – MN，13 – SDN，14 – ICI，15 - OCI|
|start|int|需要读取电话号码记录的起始编号，start为 0 表示不使用编号获取电话号码记，start应小于等于end|
|end|int|需要读取电话号码记录的结束编号，必须满足：end - start <= 20|
|username	|string|当 start为 0 时有效，电话号码中的用户名，暂不支持中文，最大长度不超过30字节<br>注意：按username进行匹配时，并不是按完整的单词进行匹配，只要电话簿中已有记录的name是以username开头，那么就会匹配上|

- 返回值

读取失败返回整型-1，成功返回一个元组，包含读取记录，格式如下：

(record_number, [(index, username, phone_number), ... , (index, username, phone_number)])

返回值参数说明：

record_number – 读取的记录数量，整型

index – 在电话簿中的索引位置，整型

username – 姓名，string类型

phone_number – 电话号码，string类型


- 示例



	>>> sim.readPhonebook(9, 1, 4, "")
	(4,[(1,'Tom','15544272539'),(2,'Pony','15544272539'),(3,'Jay','18144786859'),(4,'Pondy','15544282538')])
	>>> sim.readPhonebook(9, 0, 0, "Tom")
	(1, [(1, 'Tom', '18144786859')])
	>>> sim.readPhonebook(9, 0, 0, "Pony")
	(1, [(2, 'Pony', '17744444444')])
	>>> sim.readPhonebook(9, 0, 0, "Pon") #注意，这里只要是包含了‘pon’,都会被匹配上
	(2, [(2, 'Pony', '17744444444'),(4,'Pondy','15544282538')])

### 写电话簿
>sim. writePhonebook(storage, index, username, number)

写入一条电话号码记录。

- 参数

|参数|参数类型|参数说明|
|----|-------|-------|
|storage|int|需要读取电话号码记录的电话本存储位置，可选参数如下：<br>0 – DC，1 – EN，2 – FD，3 – LD，4 – MC，5 – ME，6 – MT，7 – ON，8 – RC，9 – SM，10 – AP，11 – MBDN，12 – MN，13 – SDN，14 – ICI，15 - OCI|
|index|int|需要写入电话号码记录的在电话簿中的编号，范围1~500|
|username|string|电话号码的用户名，长度范围不超过30字节，暂不支持中文名|
|number|string|电话号码，最大长度不超过20字节|
- 返回值

写入成功返回整型0，写入失败返回整型-1。


- 示例


	>>> sim.writePhonebook(9, 1, 'Tom', '18144786859')
	0

### 注册监听回调函数
>sim.setCallback(usrFun)

注册监听回调函数。在接收短信时，会触发该回调函数。

(该函数只有在SIM卡热插拔的宏打开的情况下才会存在，一般默认打开)

- 参数

|参数|参数类型|参数说明|
|----|-------|------|
|usrFun|function|监听回调函数，回调具体形式及用法见示例|

- 返回值

注册成功返回整型0，失败返回整型-1。

- 示例


	import sim
	
	def cb(args):
	    simstates = args
	    print('sim states:{}'.format(simstates))
	    
	sim.setCallback(cb)

### 使用示例

	#sim示例
	import sim
	import utime
	import log 
	
	#设置日志输出级别
	log.basicConfig(level=log.INFO)
	Sim_log = log.getLogger("Sim")
	
	if __name__ == '__main__':
	    #开机需要延时一段时间
	    for i in [0, 1, 2, 3, 4, 5]:
	        utime.sleep(1)
	        Sim_log.info("delay",i+1)
	        print("delay",i+1,"s")
	    #获取sim卡的状态    
	    status = sim.getStatus()
	    Sim_log.info(status)
	    #获取sim卡的Imei号
	    Imei = sim.getImei()
	    Sim_log.info(Imei)
	    #获取sim卡的Imsi号
	    Imsi = sim.getImsi()
	    Sim_log.info(Imsi)
	    #获取sim卡的Iccid号
	    Iccid = sim.getIccid()
	    Sim_log.info(Iccid)
    

## net - 网络相关功能
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

## checkNet - 等待网络就绪

模块功能：checkNet模块主要用于【开机自动运行】的用户脚本程序，该模块提供API用来阻塞等待网络就绪，如果超时或者其他异常退出会返回错误码，所以如果用户的程序中有涉及网络相关的操作，那么在用户程序的开始应该调用 checkNet 模块中的方法以等待网络就绪。当然，用户也可以自己实现这个模块的功能。


### 创建checkNet对象

	import checkNet
	PROJECT_NAME = "MGPython_Math_example" 
	PROJECT_VERSION = "1.0.0" 
	checknet = checkNet.CheckNetwork(PROJECT_NAME, PROJECT_VERSION)

- 功能

创建checkNet对象。PROJECT_NAME 和 PROJECT_VERSION 是必须有的两个全局变量，用户可以根据自己的需要修改这两个变量的值。

- 参数

|参数|描述|
|---|----|
|PROJECT_NAME|用户项目名称，字符串类型|
|PROJECT_VERSION|用户项目版本号，字符串类型|

- 返回值

无

- 示例


	import checkNet
	PROJECT_NAME = "XXXXXXXX"
	PROJECT_VERSION = "XXXX"
	checknet = checkNet.CheckNetwork(PROJECT_NAME, PROJECT_VERSION)


### 开机打印信息
>checknet.poweron_print_once()

- 功能

开机时打印一些信息，主要用于提示用户。打印内容如下：

PROJECT_NAME : 用户项目名称 

PROJECT_VERSION : 用户项目版本号 

FIRMWARE_VERSION : 固件版本号 

POWERON_REASON : 开机原因 

SIM_CARD_STATUS : SIM卡状态

- 参数

无

- 返回值

无

- 示例


	import checkNet
	
	PROJECT_NAME = "MGPython_Math_example"
	PROJECT_VERSION = "1.0.0"
	checknet = checkNet.CheckNetwork(PROJECT_NAME, PROJECT_VERSION)
	if __name__ == '__main__':
	    # 在用户程序运行前增加下面这一句
	    checknet.poweron_print_once()
	    ......
	    
	# 当用户程序开始运行时，会打印下面信息
	==================================================
	PROJECT_NAME     : MGPython_Math_example
	PROJECT_VERSION  : 1.0.0
	FIRMWARE_VERSION : SLM320P_878463C_20211014_V42_T01
	POWERON_REASON   : 0
	SIM_CARD_STATUS  : 1
	==================================================


### 等待网络就绪

>checknet.wait_network_connected(timeout)

- 功能

阻塞等待网络就绪，超时时间内，只要检测到拨号成功，则会立即返回，否则阻塞到超时时间到才会退出。

- 参数

|参数|类型|说明|
|----|----|----|
|timeout|整型|超时时间，单位秒，可设范围 [1, 3600]，默认值60s。|

- 返回值

返回值有2个，形式如下：
stagecode, subcode

各返回值说明如下：

|返回值|类型|说明|
|------|-----|----|
|stagecode|整型|阶段码，表示 checkNet 模块当前在哪个阶段。
1 - 程序在获取SIM卡状态阶段，因为超时或者SIM卡状态异常，退出时的值；
2 - 程序在获取注网状态阶段，因为超时退出时的值；
3 - 程序在获取拨号状态阶段，返回时的值；
用户使用时，stagecode 正常返回值应该是3，如果是前两个值，说明是不正常的。|
|subcode|整型|  码，结合 stagecode 的值，来表示 checkNet 在不同阶段的具体状态。
当 stagecode = 1 时：
subcode 表示 SIM卡的状态，范围[0, 21]，每个值的详细说明，请参考：  |

- 示例

		import checkNet
		
		PROJECT_NAME = "MGPython_Math_example"
		PROJECT_VERSION = "1.0.0"
		checknet = checkNet.CheckNetwork(PROJECT_NAME, PROJECT_VERSION)
		
		if __name__ == '__main__':
		    # 在用户程序运行前增加下面这一句
		    stagecode, subcode = checknet.wait_network_connected(30)
		    print('stagecode = {}, subcode = {}'.format(stagecode, subcode))
		    ......
		# 当用户程序开始运行时，如果网络已就绪，则返回值如下：
		stagecode = 3, subcode = 1
		# 如果用户没有插sim卡，则返回值如下：
		stagecode = 1, subcode = 0
		# 如果sim卡处于被锁的状态，则返回值如下：
		stagecode = 1, subcode = 2


### checkNet异常返回处理
根据前面 checknet.wait_network_connected(timeout) 接口返回值描述，用户可参考如下处理方式来排查和解决问题：

	import checkNet
	PROJECT_NAME = "MGPython_checkNet_example"
	PROJECT_VERSION = "1.0.0"
	checknet = checkNet.CheckNetwork(PROJECT_NAME, PROJECT_VERSION)
	
	if __name__ == '__main__':
	    # 在用户程序运行前增加下面这一句
	    stagecode, subcode = checknet.wait_network_connected(30)
	    print('stagecode = {}, subcode = {}'.format(stagecode, subcode))
	    
	    if stagecode == 1:
	        # 如果 subcode = 0，说明没插卡，或者卡槽松动，需要用户去检查确认；
	        # 如果是其他值，请参考官方文档中关于sim卡状态值的描述，确认sim卡当前状态，然后做相应处理
	    elif stagecode == 2:
	        if subcode == -1:
	            # 这种情况说明在超时时间内，获取注网状态API一直执行失败，在确认SIM卡可正常使用且能正常被模块识
	            # 别的前提下，可联系我们的FAE反馈问题；
	        elif subcode == 0:
	            # 这种情况说明在超时时间内，模块一直没有注网成功，这时请按如下步骤排查问题：
	            # （1）首先确认SIM卡状态是正常的，通过 sim 模块的 sim.getState() 接口获取，为1说明正常；
	            # （2）如果SIM卡状态正常，确认当前信号强度，通过 net模块的 net.csqQueryPoll() 接口获取，
	            #     如果信号强度比较弱，那么可能是因为当前信号强度较弱导致短时间内注网不成功，可以增加超时
	            #      时间或者换个信号比较好的位置再试试；
	            # （3）如果SIM卡状态正常，信号强度也较好，但就是注不上网，请联系我们的FAE反馈问题；最好将相应
	            #     SIM卡信息，比如哪个运营商的卡、什么类型的卡、卡的IMSI等信息也一并提供，必要时可以将
	            #     SIM卡寄给我们来排查问题。
	        else:
	            #net.getState() 接口的返回值说明，确认注网失败原因
	    elif stagecode == 3:
	        if subcode == 1:
	            # 这是正常返回情况，说明网络已就绪，即注网成功，拨号成功
	        else:
	            # 这种情况说明在超时时间内，拨号一直没有成功，请按如下步骤尝试：
	            # （1）通过 sim 模块的 sim.getState() 接口获取sim卡状态，为1表示正常；
	            # （2）通过 net 模块的 net.getState() 接口获取注网状态，为1表示正常；
	            # （3）手动调用拨号接口尝试拨号，看看能否拨号成功，可参考官方Wiki文档中的 dataCall 模块
	            #     的拨号接口和获取拨号结果接口；
	            # （4）如果手动拨号成功了，但是开机拨号失败，那么可能是默认的apn配置表中没有与当前SIM卡匹配
	            #     的apn，用户可通过 sim 模块的 sim.getImsi() 来获取 IMSI 码，确认IMSI的第四和第五              #     位字符组成的数字是否在 01~13 的范围内，如果不在，说明当前默认apn配置表中无此类SIM卡对
	            #     应的apn 信息，这种情况下，用户如果希望开机拨号成功，可以使用 dataCall.setApn(...)
	            #     接口来设置保存用户自己的apn信息，然后开机重启，就会使用用户设置的apn来进行开机拨号；
	            # （5）如果手动拨号也失败，那么请联系我们的FAE反馈问题，最好将相应SIM卡信息，比如哪个运营商
	            #     的卡、什么类型的卡、卡的IMSI等信息也一并提供，必要时可以将SIM卡寄给我们来排查问题。


## modem - 设备相关

模块功能：设备信息获取

### 获取设备的IMEI
>modem.getimei()

获取设备的IMEI。

- 参数

无

- 返回值

成功返回string类型设备的IMEI，失败返回整型值-1。

- 示例


	>>> import modem
	>>> modem.getDevImei()
	b'352273017386340

### 获取设备型号
>modem.getModel()

获取设备型号。

- 参数

无

- 返回值

成功返回string类型设备型号，失败返回整型值-1。

- 示例 


	>>> modem.getDevSN()
	b'SLM320P_878463C_20211014_V42_T01'


### 获取设备序列号
>modem.getSn()

获取设备序列号。

- 参数

无

- 返回值

成功返回string类型设备序列号，失败返回整型值-1。

- 示例


	>>> modem.getDevSN()


### 获取固件版本号
>modem.getVersion()

获取固件版本号

- 参数

无

- 返回值

成功返回string类型固件版本号，失败返回整型值-1。

- 示例


	>>> modem.getDevFwVersion()
	b'SLM320P_V42_T01

### 获取设备制造商ID
>modem.getProductId()

获取设备的制造商ID。

- 参数

无

- 返回值

成功返回设备制造商ID，失败返回整型值-1。

- 示例


	>>> modem.getDevFwVersion()
	b'MeiG'

### 使用示例

	#设备信息modem示例
	import modem
	import log
	#设置日志输出级别
	log.basicConfig(level=log.INFO)
	Modem_log = log.getLogger("Modem")
	
	if __name__ == '__main__':
	    #获取设备的信息
	    Model = modem.getModel()
	    Modem_log.info(Model)
	    #获取设备的版本
	    Version = modem.getVersion()
	    Modem_log.info(Version)
	    #获取设备的Sn码
	    sn = modem.getSn()
	    Modem_log.info(sn)
	    #获取设备的Imei号
	    imei = modem.getimei()
	    Modem_log.info(imei)
	    #获取设备的生产商信息
	    id = modem.getProductId()
	    Modem_log.info(id)

## ure - 正则
模块功能：提供通过正则表达式匹配数据。

### 支持的操作符

|字符|说明|
|----|----|
|‘.’|匹配任意字符|
|‘[]’|匹配字符集合，支持单个字符和一个范围，包括负集|
|‘^’|匹配字符串的开头。|
|‘$’|匹配字符串的结尾。|
|‘?’|匹配零个或前面的子模式之一。|
|‘*’|匹配零个或多个先前的子模式。|
|‘+’|匹配一个或多个先前的子模式。|
|‘??’	|非贪婪版本的 ? ，匹配0或1。|
|‘*?’ |非贪婪版本的*，匹配零个或多个。|
|‘+?’|非贪婪版本的+，匹配一个或多个|
| ‘∣’ |匹配该操作符的左侧子模式或右侧子模式。|
|‘\d’|数字匹配|
|‘\D’|非数字匹配|
|'\s'|匹配空格|
|'\S'|匹配非空格|
|‘\w’|匹配”单词字符” (仅限ASCII)|
|‘\W’|匹配非“单词字符”（仅限ASCII）|

### 不支持的操作符
- 重复次数 ({m,n})
- 命名组 ((?P<name>...))
- 非捕获组((?:...))
- 更高级的断言(\b, \B)
- 特殊字符转义，如 \r, \n - 改用Python自己的转义。

### 编译并生成正则表达式对象
>ure.compile(regex)

compile 函数用于编译正则表达式，生成一个正则表达式（ Pattern ）对象，供 match() 和 search() 这两个函数使用。

- 参数

|参数|类型|说明|
|----|----|----|
|regex|string|正则表达式|

- 返回值

返回 regex 对象

### 匹配
>ure.match(regex, string)

将正则表达式对象与string匹配，匹配通常从字符串的起始位置进行。

- 参数

|参数|类型|说明|
|----|----|----|
|regex|string|正则表达式|
|string|string	|需要匹配的字符串数据|

- 返回值

匹配成功返回一个匹配的对象，否则返回None。

### 查找
>ure.search(regex, string)

ure.search 扫描整个字符串并返回第一个成功的匹配。

- 参数

|参数|类型|说明|
|----|----|----|
|regex|string|正则表达式|
|string|string	|需要匹配的字符串数据|

- 返回值

匹配成功返回一个匹配的对象，否则返回None。


### 匹配单个字符串
>match.group(index)

匹配的整个表达式的字符串

- 参数

|参数|类型|说明|
|----|----|----|
|index|int|正则表达式中，group()用来提出分组截获的字符串, index=0返回整体，根据编写的正则表达式进行获取，当分组不存在时会抛出异常|

- 返回值

返回匹配的整个表达式的字符串。

### 匹配多个字符串
>match.groups()

匹配的整个表达式的字符串

- 参数

无

- 返回值

返回一个包含该匹配组的所有字符串的元组。

### 获取起始索引
>match.start(index)

返回匹配的子字符串组的起始原始字符串中的索引。

- 参数

|参数|类型|说明|
|----|---|----|
|index|int|index 默认为整个组，否则将选择一个组|

- 返回值

返回匹配的子字符串组的起始原始字符串中的索引。

### 获取结束索引
>match.end(index)

返回匹配的子字符串组的结束原始字符串中的索引。

- 参数

|参数|类型|说明|
|---|-----|---|
|index|int|index 默认为整个组，否则将选择一个组|

- 返回值

返回匹配的子字符串组的结束原始字符串中的索引。

### 使用示例

	#re模块示例
	import ure
	import log
	
	#设置日志输出级别
	log.basicConfig(level=log.INFO)
	Re_log = log.getLogger("Re")
	
	if __name__ == '__main__':
	    #re.match尝试从字符串的起始位置匹配一个模式,如果不是起始位置匹配成功的话,match()就返回none
	    #re.search 扫描整个字符串并返回第一个成功的匹配
	    Re_log.info(ure.match('meig', 'meig is the best!').span())
	    Re_log.info(ure.match('meig', 'meig is the best!'))
	    Re_log.info(ure.search('meig', 'meig is the best!').span())
	    Re_log.info(ure.search('the', 'meig is the best!').span())
	    phone = "2021-008-026 #这是一个电话号码" 
	    #用于替换字符串中的匹配项
	    #删除字符串中的 Python注释
	    num = ure.sub(r'#.*$', "", phone)
	    Re_log.info(num)
	    #删除非数字(-)的字符串
	    num = ure.sub(r'\D', "", phone)
	    Re_log.info(num)
	
	    #用于编译正则表达式,生成一个正则表达式（Pattern）对象,供 match()和 search()这两个函数使用
	    Re_log.info(ure.compile(r'\s+').split('meig python is the best'))
	    Re_log.info(ure.compile(r'\s+').split('meig python is the best', 2))
	    #匹配两个数字
	    pattern = ure.compile(r'(\d)(\d)')
	    m = pattern.match('3146566544')
	    Re_log.info(m)
	    Re_log.info(m.group(0)) 
	    Re_log.info(m.span(0))
	    Re_log.info(m.group(1))
	    Re_log.info(m.span(1))
	    Re_log.info(m.group(2))
	    Re_log.info(m.span(2))
	    #匹配字符
	    line = "Cats are smarter than dogs"
	    matchObj = ure.match(r'(.*) are (.*?) .*', line)
	    if matchObj:
	        Re_log.info("matchObj.group(0) : ", matchObj.group(0))
	        Re_log.info("matchObj.group(1) : ", matchObj.group(1))
	        Re_log.info("matchObj.group(2) : ", matchObj.group(2))
	        Re_log.info(m.start(0))
	        Re_log.info(m.end(0))
	        Re_log.info(m.span(0))
	    else:
	        Re_log.info("No match!!")

## misc - 其他

### Power - 电源

模块功能，提供关机，重启，获取电池电压。


#### 模块关机
>Power.powerDown()

模块关机。

- 参数

无

- 返回值

无

#### 模块重启
>Power.powerRestart()

模块重启。

- 参数

无

- 返回值

无

#### 获取模块启动原因
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


#### 获取模块上次关机原因
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

#### 获取电池电压
>Power. getVbatt()

获取电池电压，单位mV。

- 参数

无

- 返回值

int类型电压值。

- 示例


	>>> Power.getVbatt()
	3590

#### 使用示例

	#Power示例
	from misc import Power
	import utime
	import log 
	
	log.basicConfig(level=log.INFO)
	Power_log = log.getLogger("Power")
	
	if __name__ == '__main__':
	    for i in range(10):
	        utime.sleep(1)
	        Power_log.info("hello")
	    #获取电池电压，单位mv
	    u = Power.getVbatt()
	    Power_log.info(u)
	    #获取模块启动原因
	    reason = Power.powerOnReason()
	    Power_log.info(reason)
	    #模块重启
	    Power.powerRestart()

### PowerKey

提供power key按键回调注册功能接口。

#### 创建PowerKey对象

> from misc import PowerKey
>
> pk = PowerKey()

- 参数

  无

- 返回值

  返回一个对象




#### 注册回调函数

> pk.powerKeyEventRegister(usrFun)

- 参数

| 参数   | 参数类型 | 参数说明                                    |
| ------ | -------- | ------------------------------------------- |
| usrFun | function | 回调函数，按下或松开power key按键时触发回调 |

- 返回值

  注册成功返回整型0，失败返回整型-1。

- 注意


  展锐平台，对于powerkey，只在按键松开时才会触发回调函数，并且按键按下的时间需要维持500ms以上。




### PWM


#### 常量说明

| 常量     | 说明 | 
| -------- | ---- |
| PWM.PWM0 | PWM0 | 
| PWM.PWM1 | PWM1 |             
| PWM.PWM2 | PWM2 |              
| PWM.PWM3 | PWM3 |      



#### 创建一个pwm对象

> from misc import PWM
>
> pwm = PWM(PWM.PWMn,PWM.ABOVE_xx, highTime, cycleTime)

- 参数

| 参数      | 参数类型 | 参数说明                                                     |
| --------- | -------- | ------------------------------------------------------------ |
| PWMn      | int      | |



#### 开启PWM输出

> pwm.open()

开启PWM输出。

- 参数

无

- 返回值

成功返回整型0，失败返回整型-1。



#### 关闭PWM输出

> pwm.close()

关闭PWM输出。

- 参数

无

- 返回值

成功返回整型0，失败返回整型-1。


### ADC

#### 常量说明

| 常量     | 说明     |               
| -------- | -------- | 
| ADC.ADC0 | ADC通道0 | 
| ADC.ADC1 | ADC通道1 |   
| ADC.ADC2 | ADC通道2 | 
| ADC.ADC3 | ADC通道3 |  



#### 创建一个ADC对象

> from misc import ADC
>
> adc = ADC()



#### ADC功能初始化

> adc.open()

ADC功能初始化。

- 参数

无

- 返回值

成功返回整型0，失败返回整型-1。



#### 读取通道电压值

> adc.read(ADCn)

读取指定通道的电压值，单位mV。

- 参数

| 参数 | 参数类型 | 参数说明                                                     |
| ---- | -------- | ------------------------------------------------------------ |
| ADCn | int      |      |

- 返回值

成功返回指定通道电压值，错误返回整型-1。

- 示例

```python
>>>adc.read(ADC.ADC0)  #读取ADC通道0电压值
613
>>>adc.read(ADC.ADC1)  #读取ADC通道1电压值
605
```



#### 关闭ADC

> adc.close()

关闭ADC。

- 参数

无

- 返回值

0关闭成功，-1关闭失败。

### USB

提供USB插拔检测接口。


#### 创建USB对象

> from misc import USB
>
> usb = USB()

- 参数

  无

- 返回值

  无

  

#### 获取当前USB连接状态

> usb.getStatus()

- 参数

  无

- 返回值

  -1 - 获取状态失败

  0 - USB当前没有连接

  1 - USB已连接



#### 注册回调函数

> usb.setCallback(usrFun)

- 参数

| 参数   | 参数类型 | 参数说明                                                     |
| ------ | -------- | ------------------------------------------------------------ |
| usrFun | function | 回调函数，当USB插入或者拔出时，会触发回调来通知用户当前USB状态。注意：回调函数中不要进行阻塞性的操作。 |

- 返回值

  注册成功返回整型0，失败返回整型-1。




## machine - 硬件相关功能

### Pin

类功能：GPIO读写操作

#### 常量说明

|常量|适配平台|说明|
|----|-------|----|
|Pin.GPIO1|--  |GPIO1|
|Pin.GPIO2|--  |GPIO2|
|Pin.GPIO3| -- |GPIO3|
|Pin.GPIO4| -- |GPIO4|
|Pin.GPIO5| -- |GPIO5|
|Pin.IN|--|输入模式|
|Pin.OUT|--|输出模式|
|Pin.PULL_DISABLE|--|浮空模式|
|Pin.PULL_PU|--|上拉模式|
|Pin.PULL_PD|--|下拉模式|

#####  GPIO对应引脚说明
文档中提供的GPIO引脚号对应的为模块外部的引脚编号，可参考提供的硬件资料查看模块外部的引脚编号。

#### 创建GPIO对象
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

#### 获取引脚电平
>Pin.read()

获取PIN脚电平。

- 参数

无

- 返回值

PIN脚电平，0-低电平，1-高电平。

#### 设置引脚电平
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

#### 使用示例

	# Pin使用示例
	from machine import Pin
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
	#设置日志输出级别
	log.basicConfig(level=log.INFO)
	Pin_log = log.getLogger("Pin")
	
	if __name__ == '__main__':
	    #初始化GPIO3和GPIO4
	    GPIO3 = Pin(Pin.GPIO3, Pin.OUT, Pin.PULL_DISABLE, 1)
	    GPIO4 = Pin(Pin.GPIO4, Pin.OUT, Pin.PULL_DISABLE, 1)
	    while True:
	        GPIO3.write(1)          #设置GPIO3为高电平
	        GPIO4.write(1)          #设置GPIO4为高电平
	        val3 = GPIO3.read()     #获取GPIO3的电平状态
	        val4 = GPIO4.read()     #获取GPIO4的电平状态
	        Pin_log.info('val3 = {}'.format(val3))
	        Pin_log.info('val4 = {}'.format(val4))
	        utime.sleep_ms(500)
	        GPIO3.write(0)          #设置GPIO3为低电平
	        GPIO4.write(0)          #设置GPIO4为低电平
	        val3 = GPIO3.read()     #获取GPIO3的电平状态
	        val4 = GPIO4.read()     #获取GPIO4的电平状态
	        Pin_log.info('val3 = {}'.format(val3))
	        Pin_log.info('val4 = {}'.format(val4))
	        utime.sleep_ms(500)



### UART
			
类功能：uart串口数据传输。
#### 常量说明
|常量|说明|备注|
|----|---|---|
|UART.UART0|UART0|DEBUG PORT----硬件串口3 需要引线|
|UART.UART1|UART1|BT PORT  ----硬件串口2 需要引线，如果有蓝牙功能，就不能作为uart功能|
|UART.UART2|UART2|MAIN PORT---硬件串口1   就是数传at 串口|
|UART.UART3|UART3|USB CDC PORT -- usb口虚拟的第七个口，  就是usb两个预留口的一个|

#### 创建uart对象
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

#### 获取接收缓存未读数据大小
>uart.any()

返回接收缓存器中有多少字节的数据未读。

- 参数

无

- 返回值

返回接收缓存器中有多少字节的数据未读。

- 示例


	>>> uart.any()
	20 #表示接收缓冲区中有20字节数据未读

#### 串口读数据
>uart.read(nbytes)

- 参数

|参数|类型|说明|
|---|-----|---|
|nbytes|int|要读取的字节数|

- 返回值

返回读取的数据。

#### 串口发数据
>uart.write(data)

发送数据到串口。

- 参数

|参数|类型|说明|
|---|----|----|
|data|string|发送的数据|

- 返回值

返回发送的字节数。

#### 使用示例
	
	#串口UART示例
	from machine import UART
	import utime
	import log
	
	log.basicConfig(level=log.INFO)
	Uart_log = log.getLogger("Uart")
	
	#创建UART对象
	uart1 = UART(UART.UART1, 115200 , 8, 0, 1, 0)
	usb_cdc = UART(UART.UART3)
	if __name__ == '__main__':
	    while True:
	        #返回接收缓存器中有多少字节的数据未读
	        read_msg_len = usb_cdc.any()
	        #判断是否接收到数据
	        if read_msg_len:
	            #从串口读取数据
	            read_msg = usb_cdc.read(read_msg_len)
	            Uart_log.info("usb cdc msg:{}".format(read_msg))
	            #发送数据到串口
	            usb_cdc.write(read_msg)
	        utime.sleep_ms(1)

### Timer

类功能：硬件定时器。
PS:使用该定时器时需注意：定时器0-3，每个在同一时间内只能执行一件任务，且多个对象不可使用同一个定时器。

#### 常量说明
|常量|说明|
|---|----|
|Timer.Timer0|定时器0|
|Timer.Timer1|定时器1|
|Timer.Timer2|定时器2|
|Timer.Timer3|定时器3|
|Timer.ONE_SHOT|单次模式，定时器只执行一次|
|Timer.PERIODIC|周期模式，定时器循环执行|

#### 创建Timer对象
>timer = Timer(Timern)

创建Timer对象。

- 参数

|参数|类型|说明|
|---|----|----|
|Timern|int|定时器号<br>支持定时器Timer0~Timer3（使用该定时器时需注意：定时器0-3，每个在同一时间内只能执行一件任务，且多个对象不可使用同一个定时器。）|

- 示例


	>>> from machine import Timer
	>>> timer1 = Timer(Timer.Timer1)  #使用该定时器时需注意：定时器0-3，每个在同一时间内只能执行一件任务，且多个对象不可使用同一个定时器。

#### 启动定时器
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

#### 关闭定时器
>timer.stop()

关闭定时器

- 参数

无

- 返回值

成功返回整型值0，失败返回整型值-1。

#### 使用示例
	
	#timer模块示例
	import utime
	import log
	from machine import Timer
	
	#设置日志输出格式
	log.basicConfig(level=log.INFO)
	Timer_log = log.getLogger("Timer")
	
	num = 0
	state = 1
	
	t = Timer(1)
	#创建一个执行函数
	def timer_test(t):
	    print("timer run",num)
	    Timer_log.info("timer run",num)
	
	if __name__ == '__main__':
	    print("create")
	    #传入timer实例，中断时间2000ms，周期执行timer_test函数
	    t.start(period=2000, mode=t.PERIODIC, callback=timer_test)
	    print("ok")
	    while state :
	        num += 1
	        if num > 10:
	            Timer_log.info("timer stop")
	            state = 0
	            t.stop()   #关闭定时器
	        pass
	        utime.sleep(2)


### ExtInt

类功能：用于配置I/O引脚在发生外部事件时中断。

#### 常量说明
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

#### 创建ExtInt对象
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

#### 使能中断
>extint.enable()

使能extint对象外部中断，当中断引脚收到上升沿或者下降沿信号时，会调用callback执行 。

- 参数

无

- 返回值

使能成功返回整型值0，使能失败返回整型值-1。

#### 关闭中断
>extint.disable()

禁用与extint对象关联的中断 。

- 参数

无

- 返回值

使能成功返回整型值0，使能失败返回整型值-1。

### RTC


类功能：提供获取设置rtc时间方法。

#### 创建RTC对象
>from machine import RTC
>rtc = RTC()

#### 设置和获取RTC时间
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

#### 使用示例

	#RTC示例
	from machine import RTC
	import log
	
	#设置日志输出级别
	log.basicConfig(level=log.INFO)
	Rtc_log = log.getLogger("Rtc")
	
	if __name__ == '__main__':
	    Rtc_log.info("hello")
	    #创建RTC对象
	    rtc = RTC()
	    Rtc_log.info("hello")
	    #获取时间
	    Rtc_log.info(rtc.datetime())
	    Rtc_log.info("hello")
	    #设置时间
	    Rtc_log.info(rtc.datetime([2020, 3, 12, 1, 12, 12, 12, 0]))
	    Rtc_log.info(rtc.datetime())
	    

    










	






			












