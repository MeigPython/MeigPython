模块功能: 实现二进制数据散列算法,目前支持sha256，sha1，MD5。
### 创建一个SHA256哈希对象
>​hash_obj = uhashlib.sha256(bytes)

创建一个SHA256哈希对象

- 参数
|参数|参数类型|参数说明|
|----|-------|-------|
|bytes|bytes|可选参数，可在创建时传入bytes数据，也可通过update方法|
- 返回值

SHA256哈希对象

### 创建一个SHA1哈希对象
>hash_obj = uhashlib.sha1(bytes)

创建一个SHA1哈希对象。
- 参数
|参数|参数类型|参数说明|
|----|-------|-------|
|bytes|bytes|可选参数，可在创建时传入bytes数据，也可通过update方法|
- 返回值

SHA1哈希对象

### 创建一个MD5哈希对象
>hash_obj = uhashlib.md5(bytes)

创建一个MD5哈希对象
- 参数
|参数|参数类型|参数说明|
|----|-------|-------|
|bytes|bytes|可选参数，可在创建时传入bytes数据，也可通过update方法|
- 返回值

MD5哈希对象

### 将更多的bytes数据加到散列
>hash_obj .update(bytes)

将更多的bytes数据加到散列。
- 参数
|参数|参数类型|参数说明|
|---|--------|-------|
|bytes|bytes|需要被加密的数据| 
- 返回值
无

### 返回通过哈希传递的所有数据的散列
>hash_obj .digest()

返回通过哈希传递的所有数据的散列，数据为字节类型。调用此方法后，无法再将更多的数据送入散列。

- 参数
无
- 返回值
返回加密后字节类型的数据	

### 使用实例

	#哈希算法hashlib示例
	import uhashlib
	import ubinascii
	import log
	if __name__ == '__main__':
	    #通过构造函数获得一个hash对象
	    hash_obj = uhashlib.sha256()
	    #使用hash对象的update方法添加消息
	    hash_obj.update(b"MeigPython")
	    #获得16进制str类型的消息摘要
	    res = hash_obj.digest()
	    print(res)
	    log.info(res)
	    #b'\xea\xee\xf7T\xcf\x9c\xcd\xdbl\xdf\x0e\xf9\xdf\x0f\xd0_\x03m\x9e\x8a\xe4\
	    #xd9\xc4\x10\x95A\xf1=\xfb\xab1\xc3'
	    #转换二进制数据为16进制字符串
	    hex_msg = ubinascii.hexlify(res)
	    print(hex_msg)
	    log.info(hex_msg)
		#b'eaeef754cf9ccddb6cdf0ef9df0fd05f036d9e8ae4d9c4109541f13dfbab31c3'
