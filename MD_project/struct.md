该模块实现相应CPython模块的子集。更多信息请参阅CPython文档：
struct

### 字节顺序，大小和对齐方式
默认情况下，C类型以机器的本机格式和字节顺序表示，并在必要时通过跳过填充字节来正确对齐（根据C编译器使用的规则）。根据下表，格式字符串的第一个字符可用于指示打包数据的字节顺序，大小和对齐方式：

|Character|Byte order |Size|Alignment|
|---------|-----------|----|---------|
|@|native|native|native|
|=|native|standard|none|
|<|little-endian|standard|none|
|>|big-endian|standard|none|
|!|network (= big-endian)|standard|none|
### 格式化字符表
|Format|CType|Python type|Standard size|
|------|-----|-----------|-------------|
|x|pad byte|no value| |
|c|char|bytes of length 1|1|
|b|signed char|integer|1|
|B|unsigned char|integer|1|
|?|_Bool|bool|1|
|h|short|integer|2|
|H|unsigned short|integer|2|
|i|int|integer|4|
|I|unsigned int|integer|4|
|l|long|integer|4|
|L|unsigned long|integer|4|
|q|long long|integer|8|
|Q|unsigned long long|integer|8|
|n|ssize_t|integer| |
|N|size_t|integer| |
|f|float|float|4|
|d|double|float|8|
|s|char[]|bytes| |
|p|char[]|bytes| |
|P|void *|integer| |

默认情况下，C类型以机器的本机格式和字节顺序表示，并在必要时通过跳过填充字节来正确对齐（根据C编译器使用的规则）

### 返回存放fmt需要的字节数
>ustruct.calcsize(fmt)

返回存放fmt需要的字节数。
- fmt：格式字符的类型，详情见上文格式化字符表。

示例：

	>>> import ustruct
	>>> ustruct.calcsize('i')
	4
	>>> ustruct.calcsize('f')
	4
	>>> ustruct.calcsize('d')
	8
### 按照格式字符串fmt压缩参数v1,V2,...
>ustruct.pack(fmt, v1, v2, ...)

按照格式字符串 fmt 压缩参数v1、 v2、…返回值是参数编码后的字节对象。
- fmt ：格式字符的类型，详情见上文格化式字符表

### 根据格式化字符串fmt对数据进行解压
>unstrcut.unpack(fmt, data)

根据格式化字符串 fmt 对数据进行解压，返回值为一个元组。

示例：

	>>> import ustruct
	>>> ustruct.pack('ii', 7, 9)  #打包2两个整数
	b'\x07\x00\x00\x00\t\x00\x00\x00'
	>>> ustruct.unpack('ii', b'\x07\x00\x00\x00\t\x00\x00\x00')  #解压两个整数
	(7, 9)

### 根据格式化字符串fmt将值v1,v2,...打包到从offest开始的缓冲区中
>ustruct.pack_info(fmt, buffer, offset, v1, v2, ...)

根据格式字符串fmt将值v1、v2、 …打包到从offset开始的缓冲区中。从缓冲区的末尾算起，offset可能为负。
- fmt ：格式字符的类型，详情见上文格化式字符表

### 根据格式化字符串fmt解析从offest开始的数据解压
>unstruct.unpack_from(fmt, data, offset=0)

根据格式化字符串 fmt 解析从 offest 开始的数据解压，从缓冲区末尾开始计数的偏移量可能为负值。返回值是解压值的元组。



