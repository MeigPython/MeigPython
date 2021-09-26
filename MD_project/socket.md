socket模块提供对BSD套接字接口的访问。该模块实现相应CPython模块的子集。更多信息请参阅CPython文档：socket
### 创建一个新的套接字
>socket.socket(af=AF_INET, type=SOCK_STREAM, proto=IPPROTO_TCP)

根据给定的地址族、套接字类型以及协议类型参数，创建一个新的套接字。注意，在大多数情况下不需要指定proto，也不建议这样做，因为某些MicroPython端口可能会省略 IPPROTO_*常量。

#### 常量说明
af-地址族
- socket.AF_INET ：IPV4
- socket.AF_INET6 ：IPV6

type-socket类型
- socket.SOCK_STREAM ：对应TCP的流式套接字
- socket.SOCK_DGRAM ：对应UDP的数据包套接字
- socket.SOCK_RAW ：原始套接字

proto-协议号
- socket.IPPROTO_TCP
- socket.IPPROTO_UDP

其他
- socket.SOL_SOCKET - 套接字选项级别
- socket.SO_REUSEADDR - 允许绑定地址快速重用

示例：

	import socket
	# 创建基于TCP的流式套接字
	socket = socket.socket(usocket.AF_INET, usocket.SOCK_STREAM)
	# 创建基于UDP的数据报套接字
	socket = socket.socket(usocket.AF_INET, usocket.SOCK_DGRAM)

### 将主机域名(host)和端口(port)转换为用于创建套接字的5元组序列
>socket.getaddrinfo(host, port)

将主机将主机域名(host)和端口(port)转换为用于创建套接字的5元组序列，元组结构如下：

(family, type, proto, canonname, sockaddr)
### 允许服务端接受连接
>socket.listen(backlog)

允许服务端接受连接，可指定最大连接数。
- backlog ： 接受套接字的最大个数，至少为0。

### 接受连接请求
>socket.accept()

接受连接请求，返回元祖，包含新的套接字和客户端地址，形式为:(conn,address)
- conn:新的套接字对象，可以用来发送和接受数据
- address：连接到服务器的客户端地址

### 连接到指定地址address的服务器
>socket.connect(address)

连接到指定地址address的服务器。
- address：包含地址和端口号的元组或列表

### 从套接字中读取size字节数据
>socket.read([size])

从套接字中读取size字节数据，返回一个字节对象。如果没有指定size，则会从套接字读取所有可读数据，直到读取到数据结束，此时作用和 socket.readall() 相同。

### 将字节读取到缓冲区buf中
>socket.readinto(buf, [ , nbytes ])

将字节读取到缓冲区buf中。如果指定了nbytes，则最多读取nbytes数量的字节；如果没有指定nbytes，则最多读取len(buf)字节。返回值是实际读取的字节数。

### 按行读取数据
>socket.readline()

按行读取数据，遇到换行符结束，返回读取的数据行。
### 写入缓冲区的数据
>socket.write(buf)

写入缓冲区的数据，buf为待写入的数据，返回实际写入的字节数。

### 发送数据
>socket.send(bytes)

发送数据，返回实际发送的字节数。
- bytes ： bytes型数据

### 将所有数据都发送到套接字
>socket.sendall(bytes)

将所有数据都发送到套接字。与send()方法不同的是，此方法将尝试通过依次逐块发送数据来发送所有数据。

注意：该方法再非阻塞套接字上的行为是不确定的，建议再MicroPython中，使用 write() 方法，该方法具有相同的“禁止短写”策略来阻塞套接字，并且将返回在非阻塞套接字上发送的字节数。
- bytes : bytes型数据
### 将数据发送到套接字
>socket.sendto(bytes, address)

将数据发送到套接字。该套接字不应连接到远程套接字，因为目标套接字是由address指定的。
- bytes ：bytes型数据
- address ：包含地址和端口号的元组或列表

###从套接字接受数据
>socket.recv(bufsize)

从套接字接收数据。返回值是一个字节对象，表示接收到的数据。一次接收的最大数据量由bufsize指定。
- bufsize ：一次接收的最大数据量

### 将套接字标记为关闭并释放所有资源
>socket.close()

将套接字标记为关闭并释放所有资源。
### 从套接字接受数据
>socket.recvfrom(bufsize)
从套接字接收数据。返回一个元组，包含字节对象和地址。

返回值形式为：(bytes, address)
- bytes ：接收数据的字节对象
- address ：发送数据的套接字的地址

### 设置套接字选项的值
>socket.setsockopt(level, optname, value)

设置套接字选项的值。
- level ：套接字选项级别
- optname ：socket选项
- value ：既可以是一个整数，也可以是一个表示缓冲区的bytes类对象

示例：

	socket.setsockopt(usocket.SOL_SOCKET, usocket.SO_REUSEADDR, 1)

### 设置套接字为阻塞模式或者非阻塞模式
>socket.setblocking(flag)

设置套接字为阻塞模式或者非阻塞模式。如果标志为false，则将套接字设置为非阻塞，否则设置为阻塞模式。

该方法是某些settimeout() 调用的简写：

socket.setblocking(True) 相当于 socket.settimeout(None)

socket.setblocking(False) 相当于 socket.settimeout(0)
### 设置套接字的发送和接收超时时间
>socket.settimeout(value)

设置套接字的发送和接收超时时间，单位秒。
- value ：可以是表示秒的非负浮点数，也可以是None。如果给出一个非零值，则OSError在该操作完成之前已超过超时时间值，则随后的套接字操作将引发异常。如果给定零，则将套接字置于非阻塞模式。如果未指定，则套接字将处于阻塞模式。

### 返回与套接字关联的文件对象
>socket.makefile(mode='rb')

返回与套接字关联的文件对象，返回值类型与指定的参数有关。仅支持二进制模式 (rb和wb)。

### 获取套接字的状态
>socket.getsocketsta()

获取TCP套接字的状态，状态值描述如下：

|状态值|  状态  |      描述     |
|-----|--------|---------------|
| 0 |CLOSED|套接字创建了，但没有使用这个套接字|
| 1 |LISTEN|套接字正在监听连接 |
| 2 |SYN_SENT|套接字正在试图主动建立连接，即发送SYN后还没有收到ACK|
| 3 |SYN_RCVD|套接字正在处于连接的初始同步状态，即收到对方的SYN，但还没收到自己发过去的SYN的ACK|
| 4 |ESTABLISHED|连接已建立|
| 5 |FIN_WAIT_1|套接字已关闭，正在关闭连接，即发送FIN，没有收到ACK也没有收到FIN|
| 6 |FIN_WAIT_2|套接字已关闭，正在等待远程套接字关闭，即在FIN_WAIT_1状态下收到发过去FIN对应的ACK|
| 7 |CLOSE_WAIT|远程套接字已经关闭，正在等待关闭这个套接字，被动关闭的一方收到FIN|
| 8 |CLOSING|套接字已关闭，远程套接字正在关闭，暂时挂起关闭确认，即在FIN_WAIT_1状态下收到被动方的FIN|
| 9 |LAST_ACK|远程套接字已关闭，正在等待本地套接字的关闭确认，被动方在CLOSE_WAIT状态下发送FIN|
| 10|TIME_WAIT|套接字已经关闭，正在等待远程套接字的关闭，即FIN、ACK、FIN、ACK都完毕，经过2MSL时间后变为CLOSED状态|

注意：

如果用户调用了socket.close() 方法之后，再调socket.getsocketsta() 会返回-1，因为此时创建的对象资源等都已经被释放。

### socket通信示例：






