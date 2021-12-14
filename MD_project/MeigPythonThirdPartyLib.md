## request - HTTP

模块功能：HTTP客户端的相关功能函数。

### 发送GET请求
> request.get(url, data, headers,decode,sizeof,ssl_params)

发送GET请求。

* 参数

| 参数       | 参数类型 | 说明                  |
| ---------- | -------- | --------------------- |
| url        | string   | 网址 |
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
| url        | string   | 网址 |
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
| url        | string   | 网址 |
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
| url        | string   | 网址 |
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
| response.content  | 返回响应内容的生成器对象（使用方法详见下面的使用示例|
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