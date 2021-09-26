binascii模块实现了二进制数据与各种ASCII编码之间的转换（双向），该模块实现了CPython模块响应模块的子集。更多信息请参阅CPython文档：
binascii

### 解码base64编码的数据
>binascii.a2b_base64(data)

解码base64编码的数据，会自动忽略输入中的无效字符，返回bytes对象。
###  以base64格式编码二进制数据
>binascii.b2a_base64(data)

以base64格式编码二进制数据，返回编码数据。后面跟换行符，作为 bytes 对象。
### 将二进制数据转换为十六进制字符串表示
>ubinascii.hexlify(data, [sep])

将二进制数据转换为十六进制字符串表示。

示例：

	>>> import binascii
	# 没有sep参数
	>>> binascii.hexlify('\x11\x22123')
	b'1122313233'
	>>> binascii.hexlify('abcdfg')
	b'616263646667'
	# 指定了第二个参数sep，它将用于分隔两个十六进制数
	>>> binascii.hexlify('\x11\x22123', ' ')
	b'11 22 31 32 33'
	>>> binascii.hexlify('\x11\x22123', ',')
	b'11,22,31,32,33'

### 将十六进制形式的字符串转换成二进制形式的字符串表示
>binascii.unhexlify(data）

将十六进制形式的字符串转换成二进制形式的字符串表示，

示例：
	>>> import ubinascii
	>>> binascii.unhexlify('313222')
	b'12"