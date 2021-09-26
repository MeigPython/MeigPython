模块功能：提供数据拨号相关接口。
### 拨号
>dataCall.start(profileIdx, ipType, apn, username, password, authType)

启动拨号，进行数据链路激活。
- 参数
|参数|参数类型|参数说明|
|---|-------|-------|
|profileIdx|int|PDP索引，ASR平台范围1-8，展锐平台范围1-7，一般设置为1，设置其他值可能需要专用apn与密码才能设置成功|
|ipType|int|IP类型，0-IPV4，1-IPV6，2-IPV4和IPV6|
|apn|string|apn名称，可为空，最大长度不超过63字节|
|username|string|apn用户名，可为空，最大长度不超过15字节|
|password|string|apn密码，可为空，最大长度不超过15字节|
|authType|int|加密方式，0-不加密，1-PAP，2-CHAP|
- 返回值

成功返回整型值0，失败返回整型值-1。
- 注意

BC25PA不支持此方法。

- 示例 


	>>> import dataCall
	>>> dataCall.start(1, 0, "3gnet.mnc001.mcc460.gprs", "", "", 0)
	0

### 注册回调函数
>dataCall.setCallback(usrFun)

注册用户回调函数，当网络状态发生变化，比如断线、上线时，会通过该回调函数通知用户。

- 参数
|参数|参数类型|参数说明|
|----|-------|-------|
|usrFun|function|用户回调函数，函数形式见示例|
- 返回值

注册失败返回整型-1，成功返回整型0。

- 注意

BC25PA不支持此方法。

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
	0
	>>> net.setModemFun(4)  # 进入飞行模式
	0
	>>> *** network 1 not connected! *** # 进入飞行模式导致断网，通过回调告知用户
	>>> net.setModemFun(1)  # 退出飞行模式
	0
	>>> *** network 1 connected! *** # 退出飞行模式，自动拨号，等待联网成功，通过回调告知用户

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

	#dataCall示例
	import utime
	import dataCall
	import log
	import net
	def nw_cb(args):
	    sim = args[0]
	    pdp = args[1]   # pdp索引
	    nw_sta = args[2]  # 网络连接状态 0未连接， 1已连接
	    if nw_sta == 1:
	        print("*** network %d connected! ***" % pdp)
	        print(dataCall.getInfo( 1, 2))
	        #print(dataCall.getInfo(0, 1, 0))
	        #print(dataCall.getInfo(0, 1, 1))
	    else:
	        print("*** network %d not connected! ***" % pdp)
	        print(dataCall.getInfo( 1, 2))
	
	if __name__ == '__main__':
	    #启动拨号，进行数据链路激活，成功返回0，失败返回-1
	    iret = dataCall.start(0, 1, 2, "ctnet", "", "", 0)
	    #注册用户回调函数，当网络状态发生变化，会通过回调函数通知用户
	    dataCall.setCallback(nw_cb)
	    print("datacall start", iret)
	    utime.sleep(2)
	    while True :
	        utime.sleep(2)
	        print("wait")
	        print(iret)
	        print(dataCall.getInfo(1, 2))
	   
