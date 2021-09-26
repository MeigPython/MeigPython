
os模块包含文件系统访问和挂载构建，该模块实现了CPython模块相应模块的子集。
### 删除文件
>os.remove(path)


删除文件，path表示文件名。
### 改变当前目录
>os.chdir(path)

改变当前目录，path表示目录名。
### 获取当前目录
>os.getcwd()

获取当前路径。
### 列出指定目录文件
>os.listdir([dir])

没有参数列出当前目录文件，否则列出给定目录的文件。dir为可选参数，表示目录名，默认为 ‘/’ 目录

示例：

	>>>os.listdir()
					
	[‘file1’, ‘read.txt’, ‘demo.py’]


### 创建新目录
>os.mkdir(path)

创建一个新的目录，path表示准备创建的目录名

示例：

	>>>os.mkdir('testdir')

	>>>os.listdir()

	[‘file1’, ‘read.txt’, ‘demo.py’, 'testdir']

### 重命名文件
>os.rename(old_path, new_path)

重命名文件，old_path表示旧文件或目录名，new_path表示新文件或目录名。

示例：
	>>>os.rename('testdir', 'testdir1')

### 删除指定目录
>os.rmdir(path)

删除指定目录，path表示目录名

示例：

	>>>os.rmdir('testdir')

	>>>os.listdir()

	[‘file1’, ‘read.txt’, ‘demo.py’]

### 列出当前目录参数

>os.ilistdir([dir])


该函数返回一个迭代器，该迭代器会生成所列出条目对应的3元组。dir为可选参数，表示目录名，没有参数时，默认列出当前目录，有参数时，则列出dir参数指定的目录。元组的形式为 (name, type, inode[, size]):

- name 是条目的名称，字符串类型，如果dir是字节对象，则名称为字节;
- type 是条目的类型，整型数，0x4000表示目录，0x8000表示常规文件；
- 是一个与文件的索引节点相对应的整数，对于没有这种概念的文件系统来说，可能为0；
- 一些平台可能会返回一个4元组，其中包含条目的size。对于文件条目，size表示文件大小的整数，如果未知，则为-1。对于目录项，其含义目前尚未定义。

### 获取文件或目录的状态

>os.stat(path)

获取文件或目录的状态。path表示文件或目录名。返回值是一个元组，返回值形式为：

(mode, ino, dev, nlink, uid, gid, size, atime, mtime, ctime)

- mode – inode保护模式
- ino – inode节点号
- dev – inode驻留的设备
- nlink – inode的链接数
- uid  – 所有者的用户ID
- gid – 所有者的组ID
- size – 文件大小，单位字节
- atime – 上次访问的时间
- mtime – 最后一次修改的时间
- ctime – 操作系统报告的“ctime”，在某些系统上是最新的元数据更改的时间，在其它系统上是创建时间，详细信息参见平台文档

### 获取文件系统状态信息

>os.statvfs(path)

获取文件系统状态信息。path表示文件或目录名。返回一个包含文件系统信息的元组：

(f_bsize, f_frsize, f_blocks, f_bfree, f_bavail, f_files, f_ffree, f_favail, f_flag, f_namemax)

- f_bsize – 文件系统块大小，单位字节
- f_frsize – 分栈大小，单位字节
- f_blocks – 文件系统数据块总数
- f_bfree – 可用块数
- f_bavai – 非超级用户可获取的块数
- f_files – 文件结点总数
- f_ffree – 可用文件结点数
- f_favail – 超级用户的可用文件结点数
- f_flag – 挂载标记
- f_namemax – 最大文件长度，单位字节

示例:

	>>>import os
	>>>res = os.statvfs("main.py")
	>>>print(res)
	(4096, 4096, 256, 249, 249, 0, 0, 0, 0, 255)

### 获取关于底层信息或其操作系统的信息

>os.uname()


获取关于底层信息或其操作系统的信息。该接口与micropython官方接口返回值形式有所区别，返回一个元组，形式为：

(sysname, nodename, release, version, machine)

- sysname – 底层系统的名称，string类型
- nodename – 网络名称(可以与 sysname 相同) ，string类型
- release – 底层系统的版本，string类型
- version – MicroPython版本和构建日期，string类型
- machine – 底层硬件(如主板、CPU)的标识符，string类型
- qpyver – QuecPython 短版本号，string类型

示例：

	>>> import os
	>>> os.uname()
	('sysname=EC600S-CNLB', 'nodename=EC600S', 'release=1.12.0', 'version=v1.12 on 2020-06-23', 'machine=EC600S with QUECTEL', 'qpyver=V0001')
	>>> os.uname()[0].split('=')[1] # 可通过这种方式来获取sysname的值
	'EC600S-CNLB'

>os.uname2()

获取关于底层信息或其操作系统的信息。该接口与micropython官方接口返回值形式一致。注意与上面uos.uname()接口返回值的区别，返回值形式为:

(sysname='xxx', nodename='xxx', release='xxx', version='xxx', machine='xxx', qpyver='xxx')

- sysname – 底层系统的名称，string类型
- nodename – 网络名称(可以与 sysname 相同) ，string类型
- release – 底层系统的版本，string类型
- version – MicroPython版本和构建日期，string类型
- machine – 底层硬件(如主板、CPU)的标识符，string类型
- qpyver – QuecPython 短版本号，string类型

示例：

	>>> import os
	>>> os.uname2()
	(sysname='EC600S-CNLB', nodename='EC600S', release='1.12.0', version='v1.12 on 2020-06-23', machine='EC600S with QUECTEL', qpyver='V0001')
	>>> os.uname2().sysname  # 可通过这种方式直接获取sysname的值
	'EC600S-CNLB'
	>>> os.uname2().machine
	'EC600S with QUECTEL'
### 返回具有n个随机字节的bytes对象

>os.urandom(n)

返回具有n个随机字节的bytes对象，只要有可能，它就会由硬件随机数生成器生成。

示例：

	>>> import uos
	>>> os.urandom(5)
	b'\xb3\xc9Y\x1b\xe9'
	



