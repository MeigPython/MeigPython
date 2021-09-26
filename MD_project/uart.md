类功能：uart串口数据传输。
### 常量说明
|常量|说明|
|----|---|
|UART.UART0|UART0|
|UART.UART1|UART1|
|UART.UART2|UART2|
|UART.UART3|UART3|

### 创建uart对象
>uart = UART(UART.UARTn, buadrate, databits, parity, stopbits, flowctl)

- 参数

|参数|类型|说明|
|---|----|----|
|UARTn|int|UARTn作用如下：<br>UART0 - DEBUG PORT<br>UART1 – BT PORT<br>UART2 – MAIN PORT<br>UART3 – USB CDC PORT|
|buadrate|int|波特率，常用波特率都支持，如4800、9600、19200、38400、57600、115200、230400等|
|databits|int|数据位（5~8），展锐平台当前仅支持8位|
|parity|int|奇偶校验（0 – NONE，1 – EVEN，2 - ODD）|
|stopbits|int|停止位（1~2）|
|flowctl|int|硬件控制流（0 – FC_NONE， 1 – FC_HW）|

- 示例


	>>> from machine import UART
	>>> uart1 = UART(UART.UART1, 115200, 8, 0, 1, 0)

### 获取接收缓存未读数据大小
>uart.any()

返回接收缓存器中有多少字节的数据未读。

- 参数

无

- 返回值

返回接收缓存器中有多少字节的数据未读。

- 示例


	>>> uart.any()
	20 #表示接收缓冲区中有20字节数据未读

### 串口读数据
>uart.read(nbytes)

- 参数

|参数|类型|说明|
|---|-----|---|
|nbytes|int|要读取的字节数|

- 返回值

返回读取的数据。

### 串口发数据
>uart.write(data)

发送数据到串口。

- 参数

|参数|类型|说明|
|---|----|----|
|data|string|发送的数据|

- 返回值

返回发送的字节数。

### 关闭串口
>uart.close()

关闭串口。

- 参数

无

- 返回值

成功返回整型0，失败返回整型-1。

### 控制485通信方向
>uart.control_485(UART.GPIOn, direction)

串口发送数据之前和之后进行拉高拉低指定GPIO，用来指示485通信的方向。

- 参数

|参数|类型|说明|
|----|---|----|
|GPIOn|int|需要控制的GPIO引脚号，参照Pin模块的定义|
|direction|int|1 - 表示引脚电平变化为：串口发送数据之前由低拉高、发送数据之后再由高拉低<br>0 - 表示引脚电平变化为：串口发送数据之前由高拉低、发送数据之后再由低拉高|

- 返回值

成功返回整型0，失败返回整型-1。

- 示例


	>>> from machine import UART
	>>> uart1 = UART(UART.UART1, 115200, 8, 0, 1, 0)
	>>> uart1.control_485(UART.GPIO24, 1)


### 使用示例
	
	from machine import UART
	import utime
	import log
	#创建UART对象
	uart1 = UART(UART.UART1, 115200, 8, 0, 1, 0)
	usb_cdc = UART(UART.UART3)
	if __name__ == '__main__':
	    while True:
	        #返回接收缓存器中有多少字节的数据未读
	        read_msg_len = usb_cdc.any()
	        #判断是否接收到数据
	        if read_msg_len:
	            #从串口读取数据
	            read_msg = usb_cdc.read(read_msg_len)
	            print("usb cdc msg:{}".format(read_msg))
	            log.info("usb cdc msg:{}".format(read_msg))
	            #发送数据到串口
	            usb_cdc.write(read_msg)
	        utime.sleep_ms(1)






