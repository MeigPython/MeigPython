
### 语言的语义

Python的语言设计强调的是可读性、简洁和清晰。有些人称Python为“可执行的伪代码”。

### 使用缩进，而不是括号

Python使用空白字符（tab和空格）来组织代码，而不是像其它语言，比如R、C++、JAVA和Perl那样使用括号。看一个排序算法的`for`循环：

```python
for x in array:
    if x < pivot:
        less.append(x)
    else:
        greater.append(x)
```

冒号标志着缩进代码块的开始，冒号之后的所有代码的缩进量必须相同，直到代码块结束。不管是否喜欢这种形式，使用空白符是Python程序员开发的一部分，在我看来，这可以让python的代码可读性大大优于其它语言。虽然期初看起来很奇怪，经过一段时间，你就能适应了。

> 笔记：我强烈建议你使用四个空格作为默认的缩进，可以使用tab代替四个空格。许多文本编辑器的设置是使用制表位替代空格。某些人使用tabs或不同数目的空格数，常见的是使用两个空格。大多数情况下，四个空格是大多数人采用的方法，因此建议你也这样做。

你应该已经看到，Python的语句不需要用分号结尾。但是，分号却可以用来给同在一行的语句切分：

```python
a = 5; b = 6; c = 7
```

Python不建议将多条语句放到一行，这会降低代码的可读性。

### 万物皆对象

Python语言的一个重要特性就是它的对象模型的一致性。每个数字、字符串、数据结构、函数、类、模块等等，都是在Python解释器的自有“盒子”内，它被认为是Python对象。每个对象都有类型（例如，字符串或函数）和内部数据。在实际中，这可以让语言非常灵活，因为函数也可以被当做对象使用。

### 注释

任何前面带有井号\#的文本都会被Python解释器忽略。这通常被用来添加注释。有时，你会想排除一段代码，但并不删除。简便的方法就是将其注释掉：

```python
results = []
for line in file_handle:
    # keep the empty lines for now
    # if len(line) == 0:
    #   continue
    results.append(line.replace('foo', 'bar'))
```

也可以在执行过的代码后面添加注释。一些人习惯在代码之前添加注释，前者这种方法有时也是有用的：

```python
print("Reached this line")  # Simple status report
```

### 函数和对象方法调用

你可以用圆括号调用函数，传递零个或几个参数，或者将返回值给一个变量：

```python
result = f(x, y, z)
g()
```

几乎Python中的每个对象都有附加的函数，称作方法，可以用来访问对象的内容。可以用下面的语句调用：

```python
obj.some_method(x, y, z)
```

函数可以使用位置和关键词参数：

```python
result = f(a, b, c, d=5, e='foo')
```

后面会有更多介绍。

### 变量和参数传递

当在Python中创建变量（或名字），你就在等号右边创建了一个对这个变量的引用。考虑一个整数列表：

```python
>>> a = [1, 2, 3]
```

假设将a赋值给一个新变量b：

```python
>>> b = a
```

在有些方法中，这个赋值会将数据\[1, 2, 3\]也复制。在Python中，a和b实际上是同一个对象，即原有列表\[1, 2, 3\]（见图2-7）。你可以在a中添加一个元素，然后检查b：

```python
>>> a.append(4)

>>> b
>>> [1, 2, 3, 4]
```

![](images\1240.jpg)

理解Python的引用的含义，数据是何时、如何、为何复制的，是非常重要的。尤其是当你用Python处理大的数据集时。

> 笔记：赋值也被称作绑定，我们是把一个名字绑定给一个对象。变量名有时可能被称为绑定变量。

当你将对象作为参数传递给函数时，新的局域变量创建了对原始对象的引用，而不是复制。如果在函数里绑定一个新对象到一个变量，这个变动不会反映到上一层。因此可以改变可变参数的内容。假设有以下函数：

```python
def append_element(some_list, element):
    some_list.append(element)
```

然后有：

```python
>>> data = [1, 2, 3]

>>> append_element(data, 4)

>>> data
>>> [1, 2, 3, 4]
```

### 动态引用，强类型

与许多编译语言（如JAVA和C++）对比，Python中的对象引用不包含附属的类型。下面的代码是没有问题的：

```python
>>> a = 5

>>> type(a)
>>> int

>>> a = 'foo'

>>> type(a)
>>> str
```

变量是在特殊命名空间中的对象的名字，类型信息保存在对象自身中。一些人可能会说Python不是“类型化语言”。这是不正确的，看下面的例子：

```text
>>> '5' + 5
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-16-f9dbf5f0b234> in <module>()
----> 1 '5' + 5
TypeError: must be str, not int
```

在某些语言中，例如Visual Basic，字符串‘5’可能被默许转换（或投射）为整数，因此会产生10。但在其它语言中，例如JavaScript，整数5会被投射成字符串，结果是联结字符串‘55’。在这个方面，Python被认为是强类型化语言，意味着每个对象都有明确的类型（或类），默许转换只会发生在特定的情况下，例如：

```text
>>> a = 4.5

>>> b = 2

# String formatting, to be visited later
>>> print('a is {0}, b is {1}'.format(type(a), type(b)))
a is <class 'float'>, b is <class 'int'>

>>> a / b
>>> 2.25
```

知道对象的类型很重要，最好能让函数可以处理多种类型的输入。你可以用`isinstance`函数检查对象是某个类型的实例：

```text
>>> a = 5

>>> isinstance(a, int)
>>> True
```

`isinstance`可以用类型元组，检查对象的类型是否在元组中：

```text
>>> a = 5; b = 4.5

>>> isinstance(a, (int, float))
>>> True

>>> isinstance(b, (int, float))
>>> True
```

### 属性和方法

Python的对象通常都有属性（其它存储在对象内部的Python对象）和方法（对象的附属函数可以访问对象的内部数据）。可以用`obj.attribute_name`访问属性和方法：

```text
>>> a = 'foo'

>>> a.<Press Tab>
a.capitalize  a.format      a.isupper     a.rindex      a.strip
a.center      a.index       a.join        a.rjust       a.swapcase
a.count       a.isalnum     a.ljust       a.rpartition  a.title
a.decode      a.isalpha     a.lower       a.rsplit      a.translate
a.encode      a.isdigit     a.lstrip      a.rstrip      a.upper
a.endswith    a.islower     a.partition   a.split       a.zfill
a.expandtabs  a.isspace     a.replace     a.splitlines
a.find        a.istitle     a.rfind       a.startswith
```

也可以用`getattr`函数，通过名字访问属性和方法：

```text
>>> getattr(a, 'split')
>>> <function str.split>
```

在其它语言中，访问对象的名字通常称作“反射”。本书不会大量使用`getattr`函数和相关的`hasattr`和`setattr`函数，使用这些函数可以高效编写原生的、可重复使用的代码。

### 鸭子类型

经常地，你可能不关心对象的类型，只关心对象是否有某些方法或用途。这通常被称为“鸭子类型”，来自“走起来像鸭子、叫起来像鸭子，那么它就是鸭子”的说法。例如，你可以通过验证一个对象是否遵循迭代协议，判断它是可迭代的。对于许多对象，这意味着它有一个`__iter__`魔术方法，其它更好的判断方法是使用`iter`函数：

```python
def isiterable(obj):
    try:
        iter(obj)
        return True
    except TypeError: # not iterable
        return False
```

这个函数会返回字符串以及大多数Python集合类型为`True`：

```text
>>> isiterable('a string')
>>> True

>>> isiterable([1, 2, 3])
>>> True

>>> isiterable(5)
>>> False
```

我总是用这个功能编写可以接受多种输入类型的函数。常见的例子是编写一个函数可以接受任意类型的序列（list、tuple、ndarray）或是迭代器。你可先检验对象是否是列表（或是NUmPy数组），如果不是的话，将其转变成列表：

```python
if not isinstance(x, list) and isiterable(x):
    x = list(x)
```

### 引入

在Python中，模块就是一个有`.py`扩展名、包含Python代码的文件。假设有以下模块：

```python
# some_module.py
PI = 3.14159

def f(x):
    return x + 2

def g(a, b):
    return a + b
```

如果想从同目录下的另一个文件访问`some_module.py`中定义的变量和函数，可以：

```python
import some_module
result = some_module.f(5)
pi = some_module.PI
```

或者：

```python
from some_module import f, g, PI
result = g(5, PI)
```

使用`as`关键词，你可以给引入起不同的变量名：

```python
import some_module as sm
from some_module import PI as pi, g as gf

r1 = sm.f(pi)
r2 = gf(6, pi)
```

### 二元运算符和比较运算符

大多数二元数学运算和比较都不难想到：

```python
>>> 5 - 7
>>> -2

>>> 12 + 21.5
>>> 33.5

>>> 5 <= 2
>>> False
```

表2-3列出了所有的二元运算符。

要判断两个引用是否指向同一个对象，可以使用`is`方法。`is not`可以判断两个对象是不同的：

```python
>>> a = [1, 2, 3]

>>> b = a

>>> c = list(a)

>>> a is b
>>> True

>>> a is not c
>>> True
```

因为`list`总是创建一个新的Python列表（即复制），我们可以断定c是不同于a的。使用`is`比较与`==`运算符不同，如下：

```python
>>> a == c
>>> True
```

`is`和`is not`常用来判断一个变量是否为`None`，因为只有一个`None`的实例：

```python
>>> a = None

>>> a is None
>>> True
```

![](images\1241.jpg)

### 可变与不可变对象

Python中的大多数对象，比如列表、字典、NumPy数组，和用户定义的类型（类），都是可变的。意味着这些对象或包含的值可以被修改：

```python
>>> a_list = ['foo', 2, [4, 5]]

>>> a_list[2] = (3, 4)

>>> a_list
>>> ['foo', 2, (3, 4)]
```

其它的，例如字符串和元组，是不可变的：

```python
>>> a_tuple = (3, 5, (4, 5))

>>> a_tuple[1] = 'four'
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-47-b7966a9ae0f1> in <module>()
----> 1 a_tuple[1] = 'four'
TypeError: 'tuple' object does not support item assignment
```

记住，可以修改一个对象并不意味就要修改它。这被称为副作用。例如，当写一个函数，任何副作用都要在文档或注释中写明。如果可能的话，我推荐避免副作用，采用不可变的方式，即使要用到可变对象。

### 标量类型

Python的标准库中有一些内建的类型，用于处理数值数据、字符串、布尔值，和日期时间。这些单值类型被称为标量类型，本书中称其为标量。表2-4列出了主要的标量。日期和时间处理会另外讨论，因为它们是标准库的`datetime`模块提供的。

![](images\1242.jpg)

### 数值类型

Python的主要数值类型是`int`和`float`。`int`可以存储任意大的数：

```python
>>> ival = 17239871

>>> ival ** 6
>>> 26254519291092456596965462913230729701102721
```

浮点数使用Python的`float`类型。每个数都是双精度（64位）的值。也可以用科学计数法表示：

```python
>>> fval = 7.243

>>> fval2 = 6.78e-5
```

不能得到整数的除法会得到浮点数：

```python
>>> 3 / 2
>>> 1.5
```

要获得C-风格的整除（去掉小数部分），可以使用底除运算符//：

```python
>>> 3 // 2
>>> 1
```

### 字符串

许多人是因为Python强大而灵活的字符串处理而使用Python的。你可以用单引号或双引号来写字符串：

```python
a = 'one way of writing a string'
b = "another way"
```

对于有换行符的字符串，可以使用三引号，'''或"""都行：

```python
c = """
This is a longer string that
spans multiple lines
"""
```

字符串`c`实际包含四行文本，"""后面和lines后面的换行符。可以用`count`方法计算`c`中的新的行：

```python
>>> c.count('\n')
>>> 3
```

Python的字符串是不可变的，不能修改字符串：

```python
>>> a = 'this is a string'

>>> a[10] = 'f'
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-57-5ca625d1e504> in <module>()
----> 1 a[10] = 'f'
TypeError: 'str' object does not support item assignment

>>> b = a.replace('string', 'longer string')

>>> b
>>> 'this is a longer string'
```

经过以上的操作，变量`a`并没有被修改：

```python
>>> a
>>> 'this is a string'
```

许多Python对象使用`str`函数可以被转化为字符串：

```python
>>> a = 5.6

>>> s = str(a)

>>> print(s)
5.6
```

字符串是一个序列的Unicode字符，因此可以像其它序列，比如列表和元组（下一章会详细介绍两者）一样处理：

```python
>>> s = 'python'

>>> list(s)
>>> ['p', 'y', 't', 'h', 'o', 'n']

>>> s[:3]
>>> 'pyt'
```

语法`s[:3]`被称作切片，适用于许多Python序列。后面会更详细的介绍，本书中用到很多切片。

反斜杠是转义字符，意思是它备用来表示特殊字符，比如换行符\n或Unicode字符。要写一个包含反斜杠的字符串，需要进行转义：

```python
>>> s = '12\\34'

>>> print(s)
12\34
```

如果字符串中包含许多反斜杠，但没有特殊字符，这样做就很麻烦。幸好，可以在字符串前面加一个r，表明字符就是它自身：

```python
>>> s = r'this\has\no\special\characters'

>>> s
>>> 'this\\has\\no\\special\\characters'
```

r表示raw。

将两个字符串合并，会产生一个新的字符串：

```python
>>> a = 'this is the first half '

>>> b = 'and this is the second half'

>>> a + b
>>> 'this is the first half and this is the second half'
```

字符串的模板化或格式化，是另一个重要的主题。Python 3拓展了此类的方法，这里只介绍一些。字符串对象有`format`方法，可以替换格式化的参数为字符串，产生一个新的字符串：

```python
>>> template = '{0:.2f} {1:s} are worth US${2:d}'
```

在这个字符串中，

* `{0:.2f}`表示格式化第一个参数为带有两位小数的浮点数。
* `{1:s}`表示格式化第二个参数为字符串。
* `{2:d}`表示格式化第三个参数为一个整数。

要替换参数为这些格式化的参数，我们传递`format`方法一个序列：

```python
>>> template.format(4.5560, 'Argentine Pesos', 1)
>>> '4.56 Argentine Pesos are worth US$1'
```

字符串格式化是一个很深的主题，有多种方法和大量的选项，可以控制字符串中的值是如何格式化的。推荐参阅Python官方文档。

这里概括介绍字符串处理，第8章的数据分析会详细介绍。

### 字节和Unicode

在Python 3及以上版本中，Unicode是一级的字符串类型，这样可以更一致的处理ASCII和Non-ASCII文本。在老的Python版本中，字符串都是字节，不使用Unicode编码。假如知道字符编码，可以将其转化为Unicode。看一个例子：

```python
>>> val = "español"

>>> val
>>> 'español'
```

可以用`encode`将这个Unicode字符串编码为UTF-8：

```python
>>> val_utf8 = val.encode('utf-8')

>>> val_utf8
>>> b'espa\xc3\xb1ol'

>>> type(val_utf8)
>>> bytes
```

如果你知道一个字节对象的Unicode编码，用`decode`方法可以解码：

```python
>>> val_utf8.decode('utf-8')
>>> 'español'
```

虽然UTF-8编码已经变成主流，但因为历史的原因，你仍然可能碰到其它编码的数据：

```python
>>> val.encode('latin1')
>>> b'espa\xf1ol'

>>> val.encode('utf-16')
>>> b'\xff\xfee\x00s\x00p\x00a\x00\xf1\x00o\x00l\x00'

>>> val.encode('utf-16le')
>>> b'e\x00s\x00p\x00a\x00\xf1\x00o\x00l\x00'
```

工作中碰到的文件很多都是字节对象，盲目地将所有数据编码为Unicode是不可取的。

虽然用的不多，你可以在字节文本的前面加上一个b：

```python
>>> bytes_val = b'this is bytes'

>>> bytes_val
>>> b'this is bytes'

>>> decoded = bytes_val.decode('utf8')

>>> decoded  # this is str (Unicode) now
>>> 'this is bytes'
```

### 布尔值

Python中的布尔值有两个，True和False。比较和其它条件表达式可以用True和False判断。布尔值可以与and和or结合使用：

```python
>>> True and True
>>> True

>>> False or True
>>> True
```

### 类型转换

str、bool、int和float也是函数，可以用来转换类型：

```python
>>> s = '3.14159'

>>> fval = float(s)

>>> type(fval)
>>> float

>>> int(fval)
>>> 3

>>> bool(fval)
>>> True

>>> bool(0)
>>> False
```

### None

None是Python的空值类型。如果一个函数没有明确的返回值，就会默认返回None：

```python
>>> a = None

>>> a is None
>>> True

>>> b = 5

>>> b is not None
>>> True
```

None也常常作为函数的默认参数：

```python
def add_and_maybe_multiply(a, b, c=None):
    result = a + b

    if c is not None:
        result = result * c

    return result
```

另外，None不仅是一个保留字，还是唯一的NoneType的实例：

```python
>>> type(None)
>>> NoneType
```

### 日期和时间

Python内建的`datetime`模块提供了`datetime`、`date`和`time`类型。`datetime`类型结合了`date`和`time`，是最常使用的：

```python
>>> from datetime import datetime, date, time

>>> dt = datetime(2011, 10, 29, 20, 30, 21)

>>> dt.day
>>> 29

>>> dt.minute
>>> 30
```

根据`datetime`实例，你可以用`date`和`time`提取出各自的对象：

```python
>>> dt.date()
>>> datetime.date(2011, 10, 29)

>>> dt.time()
>>> datetime.time(20, 30, 21)
```

`strftime`方法可以将datetime格式化为字符串：

```python
>>> dt.strftime('%m/%d/%Y %H:%M')
>>> '10/29/2011 20:30'
```

`strptime`可以将字符串转换成`datetime`对象：

```python
>>> datetime.strptime('20091031', '%Y%m%d')
>>> datetime.datetime(2009, 10, 31, 0, 0)
```

表2-5列出了所有的格式化命令。

![](images\1243.jpg)

当你聚类或对时间序列进行分组，替换datetimes的time字段有时会很有用。例如，用0替换分和秒：

```python
>>> dt.replace(minute=0, second=0)
>>> datetime.datetime(2011, 10, 29, 20, 0)
```

因为`datetime.datetime`是不可变类型，上面的方法会产生新的对象。

两个datetime对象的差会产生一个`datetime.timedelta`类型：

```python
>>> dt2 = datetime(2011, 11, 15, 22, 30)

>>> delta = dt2 - dt

>>> delta
>>> datetime.timedelta(17, 7179)

>>> type(delta)
>>> datetime.timedelta
```

结果`timedelta(17, 7179)`指明了`timedelta`将17天、7179秒的编码方式。

将`timedelta`添加到`datetime`，会产生一个新的偏移`datetime`：

```python
>>> dt
>>> datetime.datetime(2011, 10, 29, 20, 30, 21)

>>> dt + delta
>>> datetime.datetime(2011, 11, 15, 22, 30)
```

### 控制流

Python有若干内建的关键字进行条件逻辑、循环和其它控制流操作。

### if、elif和else

if是最广为人知的控制流语句。它检查一个条件，如果为True，就执行后面的语句：

```python
if x < 0:
    print('It's negative')
```

`if`后面可以跟一个或多个`elif`，所有条件都是False时，还可以添加一个`else`：

```python
if x < 0:
    print('It's negative')
elif x == 0:
    print('Equal to zero')
elif 0 < x < 5:
    print('Positive but smaller than 5')
else:
    print('Positive and larger than or equal to 5')
```

如果某个条件为True，后面的`elif`就不会被执行。当使用and和or时，复合条件语句是从左到右执行：

```python
>>> a = 5; b = 7

>>> c = 8; d = 4

>>> if a < b or c > d:
   .....:     print('Made it')
Made it
```

在这个例子中，`c > d`不会被执行，因为第一个比较是True：

也可以把比较式串在一起：

```python
>>> 4 > 3 > 2 > 1
>>> True
```

### for循环

for循环是在一个集合（列表或元组）中进行迭代，或者就是一个迭代器。for循环的标准语法是：

```python
for value in collection:
    # do something with value
```

你可以用continue使for循环提前，跳过剩下的部分。看下面这个例子，将一个列表中的整数相加，跳过None：

```python
sequence = [1, 2, None, 4, None, 5]
total = 0
for value in sequence:
    if value is None:
        continue
    total += value
```

可以用`break`跳出for循环。下面的代码将各元素相加，直到遇到5：

```python
sequence = [1, 2, 0, 4, 6, 5, 2, 1]
total_until_5 = 0
for value in sequence:
    if value == 5:
        break
    total_until_5 += value
```

break只中断for循环的最内层，其余的for循环仍会运行：

```python
>>> for i in range(4):
   .....:     for j in range(4):
   .....:         if j > i:
   .....:             break
   .....:         print((i, j))
   .....:
(0, 0)
(1, 0)
(1, 1)
(2, 0)
(2, 1)
(2, 2)
(3, 0)
(3, 1)
(3, 2)
(3, 3)
```

如果集合或迭代器中的元素序列（元组或列表），可以用for循环将其方便地拆分成变量：

```python
for a, b, c in iterator:
    # do something
```

### While循环

while循环指定了条件和代码，当条件为False或用break退出循环，代码才会退出：

```python
x = 256
total = 0
while x > 0:
    if total > 500:
        break
    total += x
    x = x // 2
```

### pass

pass是Python中的非操作语句。代码块不需要任何动作时可以使用（作为未执行代码的占位符）；因为Python需要使用空白字符划定代码块，所以需要pass：

```python
if x < 0:
    print('negative!')
elif x == 0:
    # TODO: put something smart here
    pass
else:
    print('positive!')
```

### range

range函数返回一个迭代器，它产生一个均匀分布的整数序列：

```python
>>> range(10)
>>> range(0, 10)

>>> list(range(10))
>>> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

range的三个参数是（起点，终点，步进）：

```python
>>> list(range(0, 20, 2))
>>> [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

>>> list(range(5, 0, -1))
>>> [5, 4, 3, 2, 1]
```

可以看到，range产生的整数不包括终点。range的常见用法是用序号迭代序列：

```python
seq = [1, 2, 3, 4]
for i in range(len(seq)):
    val = seq[i]
```

可以使用list来存储range在其他数据结构中生成的所有整数，默认的迭代器形式通常是你想要的。下面的代码对0到99999中3或5的倍数求和：

```python
sum = 0
for i in range(100000):
    # % is the modulo operator
    if i % 3 == 0 or i % 5 == 0:
        sum += i
```

虽然range可以产生任意大的数，但任意时刻耗用的内存却很小。

### 三元表达式

Python中的三元表达式可以将if-else语句放到一行里。语法如下：

```python
value = true-expr if condition else false-expr
```

`true-expr`或`false-expr`可以是任何Python代码。它和下面的代码效果相同：

```python
if condition:
    value = true-expr
else:
    value = false-expr
```

下面是一个更具体的例子：

```python
>>> x = 5

>>> 'Non-negative' if x >= 0 else 'Negative'
>>> 'Non-negative'
```

和if-else一样，只有一个表达式会被执行。因此，三元表达式中的if和else可以包含大量的计算，但只有True的分支会被执行。因此，三元表达式中的if和else可以包含大量的计算，但只有True的分支会被执行。

虽然使用三元表达式可以压缩代码，但会降低代码可读性。



本章讨论Python的内置功能，这些功能本书会用到很多。虽然扩展库，比如pandas和Numpy，使处理大数据集很方便，但它们是和Python的内置数据处理工具一同使用的。

我们会从Python最基础的数据结构开始：元组、列表、字典和集合。然后会讨论创建你自己的、可重复使用的Python函数。最后，会学习Python的文件对象，以及如何与本地硬盘交互。
