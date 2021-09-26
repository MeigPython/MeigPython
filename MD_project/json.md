### ujson - Json编码和解码
ujson 模块实现在Python数据对象和JSON数据格式之间进行转换的功能。该模块实现相应CPython模块的子集。更多信息请参阅CPython文档：json

### 将obj数据对象转化成Json字符串
>ujson.dump(obj, stream)

将 obj 数据对象转化成 JSON字符串，将其写入到给定的 stream 中。
### 将dict类型的数据转换成str
>ujson.dumps(dict)

将dict类型的数据转换成str。
### 解析给定的数据stream
>ujson.load(stream)

解析给定的数据 stream，将其解释为JSON字符串并反序列化成Python对象。

### 解析JSON字符串并返回obj对象
>ujson.loads(str)

解析JSON字符串并返回obj对象。

### 使用示例

	#JSON数据转换示例
	import ujson
	import log

	if __name__ == '__main__':
	    #定义一个Dict型数据
	    inp = {'bar': ('baz', None, 1, 2)}
	    log.info(inp)
	    #查询inp的格式
	    type_inp = type(inp)
	    log.info(type_inp)
	
	    # 将Dict转换为json
	    s = ujson.dumps(inp)
	    log.info(s)
	    type_s = type(s)
	    log.info(type_s)
	
	    # 将json转换为Dict
	    outp = ujson.loads(s)
	    log.info(outp)
	    type_outp= type(outp)
	    log.info(type_outp)