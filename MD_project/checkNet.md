模块功能：checkNet模块主要用于【开机自动运行】的用户脚本程序，该模块提供API用来阻塞等待网络就绪，如果超时或者其他异常退出会返回错误码，所以如果用户的程序中有涉及网络相关的操作，那么在用户程序的开始应该调用 checkNet 模块中的方法以等待网络就绪。当然，用户也可以自己实现这个模块的功能。

注意：BC25PA平台不支持此模块。

### 创建checkNet对象
>import checkNet

>PROJECT_NAME = "MGPython_Math_example" PROJECT_VERSION = "1.0.0" checknet = checkNet.CheckNetwork(PROJECT_NAME, PROJECT_VERSION)

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

PROJECT_NAME : 用户项目名称 PROJECT_VERSION : 用户项目版本号 FIRMWARE_VERSION : 固件版本号 POWERON_REASON : 开机原因 SIM_CARD_STATUS : SIM卡状态

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
	FIRMWARE_VERSION : EC600UCNLBR01A01M16_OCPU_V01
	POWERON_REASON   : 2
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
	        # 如果是其他值，请参考官方wiki文档中关于sim卡状态值的描述，确认sim卡当前状态，然后做相应处理
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
	            # 请参考官方Wiki文档中 net.getState() 接口的返回值说明，确认注网失败原因
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





