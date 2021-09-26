sys 模块中提供了与QuecPython运行环境有关的函数和变量。该模块实现相应CPython模块的子集。更多信息请参阅CPython文档：sys

### 常数说明
>sys.argv

当前程序启动的可变参数列表。
>sys.byteorder

字节顺序 (‘little’ - 小端， ‘big’ - 大端)。
>sys.implementation

返回当前microPython版本信息。对于MicroPython，它具有以下属性：

- name - 字符串“ micropython”
- version - 元组（主要，次要，微型），例如（1、7、0）

建议使用此对象来将MicroPython与其他Python实现区分开。

>sys.maxsize

本机整数类型可以在当前平台上保留的最大值，如果它小于平台最大值，则为MicroPython整数类型表示的最大值（对于不支持长整型的MicroPython端口就是这种情况）。

>sys.modules

已载入模块的字典。
>sys.platform

MicroPython运行的平台。
>sys.stdin

标准输入（默认是USB虚拟串口，可选其他串口）。
>sys.stdout

标准输出（默认是USB虚拟串口，可选其他串口）。
>sys.version

MicroPython 语言版本，字符串格式。
>sys.version_info

MicroPython 语言版本，整数元组格式。
### 方法
>sys.exit(retval=0)

使用给定的参数退出当前程序。与此同时，该函数会引发SystemExit退出。如果给定了参数，则将其值作为参数赋值给SystemExit。

>sys.print_exception(exc, file=sys.stdout)

打印异常到文件对象，默认是 sys.stdout，即输出异常信息的标准输出。


