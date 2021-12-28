## request - HTTP

模块功能：HTTP客户端的相关功能函数。

### 发送GET请求
> request.get(url, data, headers,decode,sizeof,ssl_params)

发送GET请求。

* 参数

| 参数|参数类型|说明|
| ---------- | -------- | --------------------- |
| <div style="width: 50pt"> url</div> | <div style="width: 50pt"> string</div>| 网址|
| data       | json     | (可选参数)附加到请求的正文，json类型，默认为None|
| headers    | dict     | (可选参数)请求头，默认为None|
| decode     | bool     | (可选参数)True 将响应的内容解码返回str类型 False 关闭解码返回bytes类型 默认True|
| sizeof     | int      | (可选参数)读取缓冲区的数据块大小 默认255 个字节 数值越大读取的速度越快(如果设置过大可能就会有丢失数据的可能性，建议255-4096)|
| ssl_params | dict     | (可选参数)SSL双向认证 {"ca": ca_content, "cert": certificate_content, "key": private_content} 传入证书的公钥密钥|

* 示例

```python
import request
import log
import utime
import checkNet


'''
下面两个全局变量是必须有的，用户可以根据自己的实际项目修改下面两个全局变量的值
'''
PROJECT_NAME = "QuecPython_Requect_Get_example"
PROJECT_VERSION = "1.0.0"

checknet = checkNet.CheckNetwork(PROJECT_NAME, PROJECT_VERSION)

# 设置日志输出级别
log.basicConfig(level=log.INFO)
http_log = log.getLogger("HTTP GET")

url = "http://httpbin.org/get"

if __name__ == '__main__':
    stagecode, subcode = checknet.wait_network_connected(30)
    if stagecode == 3 and subcode == 1:
        http_log.info('Network connection successful!')
        response = request.get(url)   # 发起http GET请求
        http_log.info(response.json())  # 以json方式读取返回
    else:
        http_log.info('Network connection failed! stagecode = {}, subcode = {}'.format(stagecode, subcode))
```

### 发送POST请求
> request.post(url, data, headers,decode,sizeof)

发送POST请求

* 参数

| 参数       | 参数类型 | 说明                  |
| ---------- | -------- | --------------------- |
| <div style="width: 50pt"> url</div> | <div style="width: 50pt"> string</div>| 网址|
| data       | json     | (可选参数)附加到请求的正文，json类型，默认为None|
| headers    | dict     | (可选参数)请求头，默认为None|
| decode     | bool     | (可选参数)True 将响应的内容解码返回str类型 False 关闭解码返回bytes类型 默认True|
| sizeof     | int      | (可选参数)读取缓冲区的数据块大小 默认255 个字节 数值越大读取的速度越快(如果设置过大可能就会有丢失数据的可能性，建议255-4096)|

* 示例

```python
import request
import ujson
import log
import utime
import checkNet


'''
下面两个全局变量是必须有的，用户可以根据自己的实际项目修改下面两个全局变量的值
'''
PROJECT_NAME = "QuecPython_Requect_Post_example"
PROJECT_VERSION = "1.0.0"

checknet = checkNet.CheckNetwork(PROJECT_NAME, PROJECT_VERSION)

# 设置日志输出级别
log.basicConfig(level=log.INFO)
http_log = log.getLogger("HTTP POST")

url = "http://httpbin.org/post"
data = {"key1": "value1", "key2": "value2", "key3": "value3"}

if __name__ == '__main__':
    stagecode, subcode = checknet.wait_network_connected(30)
    if stagecode == 3 and subcode == 1:
        http_log.info('Network connection successful!')

        # POST请求
        response = request.post(url, data=ujson.dumps(data))   # 发送HTTP POST请求
        http_log.info(response.json())
    else:
        http_log.info('Network connection failed! stagecode = {}, subcode = {}'.format(stagecode, subcode))
```

### 发送PUT请求
> request.put(url, data, headers,decode,sizeof)

发送PUT请求。

* 参数

| 参数       | 参数类型 | 说明                  |
| ---------- | -------- | --------------------- |
| <div style="width: 50pt"> url</div> | <div style="width: 50pt"> string</div>| 网址|
| data       | json     | (可选参数)附加到请求的正文，json类型，默认为None|
| headers    | dict     | (可选参数)请求头，默认为None|
| decode     | bool     | (可选参数)True 将响应的内容解码返回str类型 False 关闭解码返回bytes类型 默认True|
| sizeof     | int      | (可选参数)读取缓冲区的数据块大小 默认255 个字节 数值越大读取的速度越快(如果设置过大可能就会有丢失数据的可能性，建议255-4096)|

* 示例

```python
import request
url = "http://httpbin.org/put"
response = request.put(url)
```

### 发送HEAD请求
> request.head(url, data, headers,decode,sizeof)

发送HEAD请求。

* 参数

| 参数       | 参数类型 | 说明                  |
| ---------- | -------- | --------------------- |
| <div style="width: 50pt"> url</div> | <div style="width: 50pt"> string</div>| 网址|
| data       | json     | (可选参数)附加到请求的正文，json类型，默认为None|
| headers    | dict     | (可选参数)请求头，默认为None|
| decode     | bool     | (可选参数)True 将响应的内容解码返回str类型 False 关闭解码返回bytes类型 默认True|
| sizeof     | int      | (可选参数)读取缓冲区的数据块大小 默认255 个字节 数值越大读取的速度越快(如果设置过大可能就会有丢失数据的可能性，建议255-4096)|

* 示例

```python
import request
url = "http://httpbin.org/get"
response = request.head(url)
print(response.headers)
```

### Response类方法说明
> response =request.get(url)

发送HEAD请求。

| 方法              | 说明                  |
| ----------------- | --------------------- |
| <div style="width: 100pt"> response.content</div>  | 返回响应内容的生成器对象（使用方法详见下面的使用示例|
| response.text     | 返回文本方式响应内容的生成器对象|
| response.json()   | 返回响应的json编码内容并转为dict类型|

* request使用示例

```python
import request
import ujson
import log
import utime
import checkNet


'''
下面两个全局变量是必须有的，用户可以根据自己的实际项目修改下面两个全局变量的值
'''
PROJECT_NAME = "QuecPython_Requect_Response_example"
PROJECT_VERSION = "1.0.0"

checknet = checkNet.CheckNetwork(PROJECT_NAME, PROJECT_VERSION)

# 设置日志输出级别
log.basicConfig(level=log.INFO)
http_log = log.getLogger("HTTP RESPONSE")

url = "http://httpbin.org/get"

if __name__ == '__main__':
    stagecode, subcode = checknet.wait_network_connected(30)
    if stagecode == 3 and subcode == 1:
        http_log.info('Network connection successful!')
		# response.text
		response = request.get(url)
        for i in response.text:  # response.text为迭代器对象
            print(i)

		# response.content
		response = request.get(url)
        for i in response.content: # response.content为迭代器对象
            print(i)
		
        # response.json
        url = "http://httpbin.org/post"
        data = {"key1": "value1", "key2": "value2", "key3": "value3"}
        response = request.post(url, data=ujson.dumps(data))   # 发送HTTP POST请求
        print(response.json())
    else:
        http_log.info('Network connection failed! stagecode = {}, subcode = {}'.format(stagecode, subcode))
```

## log - 日志

模块功能：系统日志记录,分级别日志工具。

### 设置日志输出级别
> log.basicConfig(level)

设置日志输出级别,  设置日志输出级别, 默认为log.INFO，系统只会输出 level 数值大于或等于该 level 的的日志结果。

* 参数

| 参数     | 参数类型 | 说明                  |
| -------- | -------- | --------------------- |
| CRITICAL | 常量     | 日志记录级别的数值 50 |
| ERROR    | 常量     | 日志记录级别的数值 40 |
| WARNING  | 常量     | 日志记录级别的数值 30 |
| INFO     | 常量     | 日志记录级别的数值 20 |
| DEBUG    | 常量     | 日志记录级别的数值 10 |
| NOTSET   | 常量     | 日志记录级别的数值 0  |

* 示例

```python
import log
log.basicConfig(level=log.INFO)
```

### 获取logger对象

> log.getLogger(name)

获取logger对象，如果不指定name则返回root对象，多次使用相同的name调用getLogger方法返回同一个logger对象。

* 参数

| 参数 | 参数类型 | 说明     |
| ---- | -------- | -------- |
| name | string   | 日志主题 |

* 返回值

log对象。

* 示例

```python
import log
Testlog = log.getLogger("TestLog")
```

### 输出debug级别的日志

> log.debug(tag, msg)

输出debug级别的日志。

* 参数

| 参数 | 参数类型 | 说明                         |
| ---- | -------- | ---------------------------- |
| tag  | string   | 模块或功能名称，作为日志前缀 |
| msg  | string   | 可变参数，日志内容           |

* 返回值

无

* 示例 

```python
import log
Testlog = log.getLogger("TestLog")
Testlog.debug("Test message: %d(%s)", 100, "foobar")
```

### 输出info级别的日志

> log.info(tag,msg)

输出info级别的日志。

* 参数

| 参数 | 参数类型 | 说明                         |
| ---- | -------- | ---------------------------- |
| tag  | string   | 模块或功能名称，作为日志前缀 |
| msg  | string   | 可变参数，日志内容           |

* 返回值

无

* 示例

```python
import log
Testlog = log.getLogger("TestLog")
Testlog.info("Test message: %d(%s)", 100, "foobar")
```

### 输出warning级别的日志

> log.warning(tag,msg)

输出warning级别的日志。

* 参数

| 参数 | 参数类型 | 说明                         |
| ---- | -------- | ---------------------------- |
| tag  | string   | 模块或功能名称，作为日志前缀 |
| msg  | string   | 可变参数，日志内容           |

* 返回值

无

* 示例

```python
import log
Testlog = log.getLogger("TestLog")
Testlog.warning("Test message: %d(%s)", 100, "foobar")
```

### 输出error级别的日志

> log.error(tag,msg)

输出error级别的日志。

* 参数

| 参数 | 参数类型 | 说明                         |
| ---- | -------- | ---------------------------- |
| tag  | string   | 模块或功能名称，作为日志前缀 |
| msg  | string   | 可变参数，日志内容           |

* 返回值

无

* 示例

```python
import log
Testlog = log.getLogger("TestLog")
Testlog.error("Test message: %d(%s)", 100, "foobar")
```

### 输出critical级别的日志

> log.critical(tag,msg)

输出critical级别的日志。

* 参数

| 参数 | 参数类型 | 说明                         |
| ---- | -------- | ---------------------------- |
| tag  | string   | 模块或功能名称，作为日志前缀 |
| msg  | string   | 可变参数，日志内容           |

* 返回值

无

* 示例

```python
import log
Testlog = log.getLogger("TestLog")
Testlog.critical("Test message: %d(%s)", 100, "foobar")
```

### log使用示例

```python
import log
import utime
import checkNet


'''
下面两个全局变量是必须有的，用户可以根据自己的实际项目修改下面两个全局变量的值
'''
PROJECT_NAME = "MeigPython_Log_example"
PROJECT_VERSION = "1.0.0"

# 设置日志输出级别
log.basicConfig(level=log.ERROR)
# 获取logger对象，如果不指定name则返回root对象，多次使用相同的name调用getLogger方法返回同一个logger对象
log = log.getLogger("error")

if __name__ == '__main__':
    log.error("Test error message!!")
	log.debug("Test debug message!!")
    log.critical("Test critical message!!")
    log.info("Test info message!!")
    log.warning("Test warning message!!")
```

## umqtt - MQTT

模块功能：提供创建MQTT客户端发布订阅功能。

```python
QoS级别说明
在MQTT协议中，定义了三个级别的QoS，分别是：
QoS0 – 最多一次，是最低级别；发送者发送完消息之后，并不关心消息是否已经到达接收方；
QoS1 – 至少一次，是中间级别；发送者保证消息至少送达到接收方一次；
QoS2 – 有且仅有一次，是最高级别；保证消息送达且仅送达一次。
```

### 构建mqtt连接对象
> MQTTClient(client_id, server, port=0, user=None, password=None, keepalive=0, ssl=False, ssl_params={})

构建mqtt连接对象。

* 参数

| 参数       | 参数类型    | 说明                  |
| ---------- | --------    | --------------------- |
| <div style="width: 50pt"> client_id</div> | <div style="width: 50pt"> string</div>| 客户端 ID，具有唯一性|
| server     | string      | 服务端地址，可以是 IP 或者域名|
| port       | int         | 服务器端口（可选）。 默认为1883，请注意，MQTT over SSL/TLS的默认端口是8883|
| user       | string      | (可选参数)在服务器上注册的用户名|
| password   | string      | (可选参数)在服务器上注册的密码| 
| keepalive  | int         | (可选参数)客户端的keepalive超时值。 默认为0，范围（60~1200）s|
| ssl        | bool        | (可选参数)是否使能 SSL/TLS 支持|
| ssl_params | dict        | (可选参数)SSL双向认证 {"ca": ca_content, "cert": certificate_content, "key": private_content} 传入证书的公钥密钥|

* 返回值

mqtt对象。

### 设置回调函数
> MQTTClient.set_callback(callback)

设置回调函数，收到消息时会被调用。

* 参数

| 参数       | 参数类型 | 说明                  |
| ---------- | -------- | --------------------- |
| callback   | function | 消息回调函数 |

* 返回值

无

### 设置异常回调函数
> MQTTClient.error_register_cb(callback)

设置异常回调函数，umqtt内部线程异常时通过回调返回error信息

* 参数

| 参数       | 参数类型 | 说明                  |
| ---------- | -------- | --------------------- |
| callback   | function | 异常回调函数 |

* 返回值

无

* 异常回调函数示例

```python
from umqtt import MQTTClient

def err_cb(err):
    print("thread err:")
    print(err)
    
c = MQTTClient("umqtt_client", "mq.tongxinmao.com", 18830)
c.error_register_cb(err_cb)
```

### 设置要发送给服务器的遗嘱
> MQTTClient.set_last_will(topic,msg,retain=False,qos=0)

设置要发送给服务器的遗嘱，客户端没有调用disconnect()异常断开，则发送通知到客户端。

* 参数

| 参数       | 参数类型 | 说明                  |
| ---------- | -------- | --------------------- |
| topic      | string   | 遗嘱主题 |
| msg        | string   | 遗嘱的内容 |
| retain     | bool     | retain = True boker会一直保留消息，默认False |
| qos        | int      | 消息服务质量(0~1) |

* 返回值

无

### 与服务器建立连接
> MQTTClient.connect(clean_session=True)

与服务器建立连接，连接失败会导致MQTTException异常。

* 参数

| 参数             | 参数类型 | 说明                  |
| ---------------- | -------- | --------------------- |
| <div style="width: 80pt"> clean_session</div> | <div style="width: 50pt"> bool</div>| 可选参数，一个决定客户端类型的布尔值。<br>如果为True，那么代理将在其断开连接时删除有关此客户端的所有信息。<br>如果为False，则客户端是持久客户端，当客户端断开连接时，订阅信息和排队消息将被保留。默认为False|

* 返回值

成功返回0，失败则抛出异常

### 与服务器断开连接
> MQTTClient.disconnect()

与服务器断开连接。

* 参数

无

* 返回值

无

### 发布消息
> MQTTClient.publish(topic,msg, retain=False, qos=0)

发布消息。

* 参数

| 参数       | 参数类型 | 说明                  |
| ---------- | -------- | --------------------- |
| <div style="width: 50pt"> topic</div> | <div style="width: 50pt"> string</div>| 消息主题 |
| msg        | string   | 需要发送的数据 |
| retain     | bool     | 默认为False, 发布消息时把retain设置为true，即为保留信息<br>                                           MQTT服务器会将最近收到的一条RETAIN标志位为True的消息保存在服务器端,每当MQTT客户端连接到MQTT服务器并订阅了某个topic，如果该topic下有Retained消息，那么MQTT服务器会立即向客户端推送该条Retained消息<br> 特别注意：MQTT服务器只会为每一个Topic保存最近收到的一条RETAIN标志位为True的消息！也就是说，如果MQTT服务器上已经为某个Topic保存了一条Retained消息，客户端再次发布一条新的Retained消息，那么服务器上原来的那条消息会被覆盖！|
| qos        | int      | MQTT消息服务质量（默认0，可选择0或1）0：发送者只发送一次消息，不进行重试 1：发送者最少发送一次消息，确保消息到达Broker |

* 返回值

无

### 订阅mqtt主题
> MQTTClient.subscribe(topic,qos)

订阅mqtt主题。

* 参数

| 参数       | 参数类型  | 说明                  |
| ---------- | --------- | --------------------- |
| <div style="width: 50pt"> topic</div> | <div style="width: 50pt"> string</div>| 消息主题 |
| qos        | int       | MQTT消息服务质量（默认0，可选择0或1）0：发送者只发送一次消息，不进行重试 1：发送者最少发送一次消息，确保消息到达Broker |

* 返回值

无

### 检查服务器是否有待处理消息
> MQTTClient.check_msg(timeout)

检查服务器是否有待处理消息。

* 参数

| 参数       | 参数类型 | 说明                  |
| ---------- | -------- | --------------------- |
| timeout    | int      | 超时时间 |

* 返回值

无

### 阻塞等待服务器消息响应
> MQTTClient.wait_msg()

阻塞等待服务器消息响应。

* 参数

无

* 返回值

无

### 获取mqtt连接状态
> MQTTClient.get_mqttsta()

获取mqtt连接状态

* 参数

无

* 返回值

0：连接成功

1：连接中

2：服务端连接关闭

-1：连接异常

* 示例代码

```python
from umqtt import MQTTClient
import utime
import log
import checkNet

PROJECT_NAME = "mPython_MQTT_example"
PROJECT_VERSION = "1.0.0"

checknet = checkNet.CheckNetwork(PROJECT_NAME, PROJECT_VERSION)

# 设置日志输出级别
log.basicConfig(level=log.INFO)
mqtt_log = log.getLogger("MQTT")

state = 0

def sub_cb(topic, msg):
    global state
    mqtt_log.info("Subscribe Recv: Topic={},Msg={}".format(topic.decode(), msg.decode()))
    state = 1

if __name__ == '__main__':
    stagecode, subcode = checknet.wait_network_connected(30)
    if stagecode == 3 and subcode == 1:
        mqtt_log.info('Network connection successful!')
        # 创建一个mqtt实例
        c = MQTTClient("umqtt_client", "mq.tongxinmao.com", 18830)
        # 设置消息回调
        c.set_callback(sub_cb)
        #建立连接
        c.connect()
        # 订阅主题
        c.subscribe(b"/public/TEST/mPython")
        mqtt_log.info("Connected to mq.tongxinmao.com, subscribed to /public/TEST/mPython topic" )
        # 发布消息
        c.publish(b"/public/TEST/mPython", b"my name is mPython!")
        mqtt_log.info("Publish topic: /public/TEST/mPython, msg: my name is mPython")
        while True:
            c.wait_msg()  # 阻塞函数，监听消息
            if state == 1:
                break       
        # 关闭连接
        c.disconnect()
    else:
        mqtt_log.info('Network connection failed! stagecode = {}, subcode = {}'.format(stagecode, subcode))
```

## ntptime - NTP对时

模块功能：该模块用于时间同步。

### 返回当前的ntp服务器
> ntptime.host

返回当前的ntp服务器，默认为"ntp.aliyun.com"。

### 设置ntp服务器
> ntptime.sethost(host)

设置ntp服务器。

* 参数

| 参数       | 参数类型 | 说明                  |
| ---------- | -------- | --------------------- |
| host       | string   | ntp服务器地址|

* 返回值

成功返回整型值0，失败返回整型值-1。

### 同步ntp时间
> ntptime.settime()

同步ntp时间。

* 参数

无

* 返回值

成功返回整型值0，失败返回整型值-1。

* ntptime使用示例

```python
import ntptime
import log
import utime
import checkNet
import utime

PROJECT_NAME = "mPython_ntp_example"
PROJECT_VERSION = "1.0.0"

checknet = checkNet.CheckNetwork(PROJECT_NAME, PROJECT_VERSION)

# 设置日志输出级别
log.basicConfig(level=log.INFO)
ntp_log = log.getLogger("NTP")

if __name__ == '__main__':
    stagecode, subcode = checknet.wait_network_connected(30)
    if stagecode == 3 and subcode == 1:
        mqtt_log.info('Network connection successful!')
        ntp_log.info(utime.localtime())
        # 查看默认ntp服务
        ntp_log.info(ntptime.host)
        # 设置ntp服务
        ntptime.sethost('pool.ntp.org')
        ntp_log.info(ntptime.host)
        # 同步ntp服务时间
        ntptime.settime()

        ntp_log.info(utime.localtime())
    else:
        mqtt_log.info('Network connection failed! stagecode = {}, subcode = {}'.format(stagecode, subcode))
```

