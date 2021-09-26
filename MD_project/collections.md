ucollections 模块用于创建一个新的容器类型，用于保存各种对象。该模块实现了CPython模块相应模块的子集。更多信息请参阅阅CPython文档：collections
### 创建一个新的namedtuple容器类型
>mytuple = collections.namedtuple(name, fields)

创建一个具有特定名称和一组字段的新namedtuple容器类型，namedtuple是元组的子类，允许通过索引来访问它的字段

参数

|参数   |   参数类型   |    参数说明   |
| ----  | ----         | ----        |
| name   | str        | 新创建容器的类型名称          |
| fields   |tuple       |新创建容器类型包含子类型的字段   |     


示例：

	>>> import collections
	>>> mytuple = collections.namedtuple("mytuple", ("id", "name"))
	>>> t1 = mytuple(1, "foo")
	>>> t2 = mytuple(2, "bar")
	>>> print(t1.name)
	foo
### 创建deque双向队列
>dq = collections.deque(iterable, maxlen, flag)

创建deque双向队列

- 参数

|参数   |   参数类型   |    参数说明   |
| ----  | ----         | ----        |
| iterable   | tuple        |iterable必须是空元组        |
| maxlen   |int    |指定maxlen并将双端队列限制为此最大长度   |
|flag    |int   | 可选参数；0(默认)：不检查队列是否溢出，达到最大长度时继续append会丢弃之前的值 ，1：当队列达到最大设定长度会抛出IndexError: full|

- 返回值

deque对象
### deque对象
>dq.append(data)

往队列中插入值。

- 参数

|参数|参数类型|参数说明|
|data|基本数据类型|需要添加到队列的数值|

-返回值

无

### 从deque的左侧移除并返回移除的数据

>dq.popleft()

从deque的左侧移除并返回移除的数据。如果deque为空，会引起索引错误

- 参数

无

- 返回值

返回pop出的值

示例

	from ucollections import deque

	dq = deque((),5)
	dq.append(1)
	dq.append(["a"])
	dq.append("a")

	dq.popleft()  # 1
	dq.popleft()  # ["a"]
	dq.popleft()  # a

