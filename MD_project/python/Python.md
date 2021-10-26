

Python是解释性语言。Python解释器同一时间只能运行一个程序的一条语句。标准的交互Python解释器可以在命令行中通过键入`python`命令打开：

```text
$ python
Python 3.6.0 | packaged by conda-forge | (default, Jan 13 2017, 23:17:12)
[GCC 4.8.2 20140120 (Red Hat 4.8.2-15)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> a = 5
>>> print(a)
5
```

`>>>`提示输入代码。要退出Python解释器返回终端，可以输入`exit()`或按Ctrl-D。

运行Python程序只需调用Python的同时，使用一个`.py`文件作为它的第一个参数。假设创建了一个`hello_world.py`文件，它的内容是：

```python
print('Hello world')
```

你可以用下面的命令运行它（`hello_world.py`文件必须位于终端的工作目录）：

```python
$ python hello_world.py
Hello world
```

一些Python程序员总是这样执行Python代码的，从事数据分析和科学计算的人却会使用IPython，一个强化的Python解释器，或Jupyter notebooks，一个网页代码笔记本，它原先是IPython的一个子项目。当你使用`%run`命令，IPython会同样执行指定文件中的代码，结束之后，还可以与结果交互：

```text
$ ipython
Python 3.6.0 | packaged by conda-forge | (default, Jan 13 2017, 23:17:12)
Type "copyright", "credits" or "license" for more information.

IPython 5.1.0 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.

In [1]: %run hello_world.py
Hello world

In [2]:
```

IPython默认采用序号的格式`In [2]:`，与标准的`>>>`提示符不同。
