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
	'460105466870381'
### 获取ICCID
>sim.getIccid()

获取sim卡的iccid。
- 参数
无
- 返回值
成功返回string类型的iccid，失败返回整型-1。
- 示例


	>>> sim.getIccid()
	'89860390845513443049'

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

### 开启PIN码验证
>sim.enablePin(pin)

启用sim卡PIN码验证，开启后需要输入正确的PIN验证成功后，sim卡才能正常使用。只有3次输入PIN码机会，3次都错误，sim卡被锁定，需要PUK来解锁。
- 参数

|参数|参数类型|参数说明|
|----|-------|-------|
|pin|string|PIN码，一般默认是‘1234’，最大长度不超过15字节|
- 返回值

成功返回整型0，失败返回整型-1。

- 注意

BC25PA平台PIN密码最大支持八位。

- 示例


	>>> sim.enablePin("1234")
	0
### 取消PIN码验证
>sim.disablePin(pin)

关闭sim卡PIN码验证
- 参数

|参数|参数类型|参数说明|
|----|-------|-------|
|pin|string|PIN码，一般默认是‘1234’，最大长度不超过15字节|
- 返回值

成功返回整型0，失败返回整型-1。
- 注意

BC25PA平台PIN密码最大支持八位。
- 示例


	>>> sim.disablePin("1234")
	0
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

- 注意

BC25PA平台PIN密码最大支持八位。

- 示例


	>>> sim.unblockPin("12345678", "0000")
	0

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

- 注意

BC25PA平台PIN密码最大支持八位。

- 示例


	>>> sim.changePin("1234", "4321")
	0

### 读电话簿
>sim.readPhonebook(storage, start, end, username)

获取 SIM 卡上指定电话本中的一条或多条电话号码记录。

- 参数

|参数|参数类型|参数说明|
|----|-------|-------|
|storage|int|需要读取电话号码记录的电话本存储位置，可选参数如下：
0 – DC，1 – EN，2 – FD，3 – LD，4 – MC，5 – ME，6 – MT，7 – ON，
8 – RC，9 – SM，10 – AP，11 – MBDN，12 – MN，13 – SDN，14 – ICI，15 - OCI|
|start|int|需要读取电话号码记录的起始编号，start为 0 表示不使用编号获取电话号码记，start应小于等于end|
|end|int|需要读取电话号码记录的结束编号，必须满足：end - start <= 20|
|username	|string|当 start为 0 时有效，电话号码中的用户名，暂不支持中文，最大长度不超过30字节
注意：按username进行匹配时，并不是按完整的单词进行匹配，只要电话簿中已有记录的name是以username开头，那么就会匹配上|

- 返回值

读取失败返回整型-1，成功返回一个元组，包含读取记录，格式如下：

(record_number, [(index, username, phone_number), ... , (index, username, phone_number)])

返回值参数说明：

record_number – 读取的记录数量，整型

index – 在电话簿中的索引位置，整型

username – 姓名，string类型

phone_number – 电话号码，string类型

- 注意

BC25PA平台不支持此方法。

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
|storage|int|需要读取电话号码记录的电话本存储位置，可选参数如下：
0 – DC，1 – EN，2 – FD，3 – LD，4 – MC，5 – ME，6 – MT，7 – ON，8 – RC，9 – SM，10 – AP，11 – MBDN，12 – MN，13 – SDN，14 – ICI，15 - OCI|
|index|int|需要写入电话号码记录的在电话簿中的编号，范围1~500|
|username|string|电话号码的用户名，长度范围不超过30字节，暂不支持中文名|
|number|string|电话号码，最大长度不超过20字节|
- 返回值

写入成功返回整型0，写入失败返回整型-1。

- 注意

BC25PA平台不支持此方法。

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

- 注意

BC25PA平台不支持此方法。

- 示例



	import sim
	
	def cb(args):
	    simstates = args
	    print('sim states:{}'.format(simstates))
	    
	sim.setCallback(cb)


### 设置SIMdet
>sim.setSimDet(detenable, insertlevel)

设置SIM卡热插拔相关配置。

- 参数

|参数|参数类型|参数说明|
|----|-------|-------|
|detenable|int|开启或者关闭SIM卡热插拔功能，0:关闭 1:打开|
|insertlevel|int|高低电平配置(0/1)|

- 返回值

设置成功返回整型0，设置失败返回整型-1。

- 注意

BC25PA平台不支持此方法。

- 示例


	>>> sim.setSimDet(1, 0)
	0

### 获取SIMdet
>sim.getSimDet()

获取SIM卡热插拔相关配置。

- 参数

无

- 返回值

获取失败返回整型-1，成功返回一个元组，格式如下：

(detenable, insertlevel)

返回值参数说明：

detenable - 开启或者关闭SIM卡热插拔功能，0:关闭 1:打开

insertlevel – 高低电平配置(0/1)

- 注意

BC25PA平台不支持此方法。

- 示例


	>>> sim.getSimDet()
	(1, 0)


