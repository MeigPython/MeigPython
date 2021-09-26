模块功能：设备信息获取

### 获取设备的IMEI
>modem.getDevImei()

获取设备的IMEI。

- 参数

无

- 返回值

成功返回string类型设备的IMEI，失败返回整型值-1。

- 示例


	>>> import modem
	>>> modem.getDevImei()
	'866327040830317'

### 获取设备型号
>modem.getDevModel()

获取设备型号。

- 参数

无

- 返回值

成功返回string类型设备型号，失败返回整型值-1。

- 示例 


	>>> modem.getDevSN()
	'D1Q20GM050038341P'


### 获取设备序列号
>modem.getDevSN()

获取设备序列号。

- 参数

无

- 返回值

成功返回string类型设备序列号，失败返回整型值-1。

- 示例


	>>> modem.getDevSN()
	'D1Q20GM050038341P'


### 获取固件版本号
>modem.getDevFwVersion()

获取固件版本号

- 参数

无

- 返回值

成功返回string类型固件版本号，失败返回整型值-1。

- 示例


	>>> modem.getDevFwVersion()
	'EC100YCNAAR01A01M16_OCPU_PY'

### 获取设备制造商ID
>modem.getDevProductId()

获取设备的制造商ID。

- 参数

无

- 返回值

成功返回设备制造商ID，失败返回整型值-1。

- 示例


	>>> modem.getDevFwVersion()
	'EC100YCNAAR01A01M16_OCPU_PY'





