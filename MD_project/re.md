模块功能：提供通过正则表达式匹配数据。

### 支持的操作符

|字符|说明|
|----|----|
|‘.’|匹配任意字符|
|‘[]’|匹配字符集合，支持单个字符和一个范围，包括负集|
|‘^’|匹配字符串的开头。|
|‘$’|匹配字符串的结尾。|
|‘?’|匹配零个或前面的子模式之一。|
|‘*’|匹配零个或多个先前的子模式。|
|‘+’|匹配一个或多个先前的子模式。|
|‘??’	|非贪婪版本的 ? ，匹配0或1。|
|‘*?’ | 非贪婪版本的*，匹配零个或多个。|
|‘+?’|非贪婪版本的+，匹配一个或多个|
| ‘∣’ |匹配该操作符的左侧子模式或右侧子模式。|
|‘\d’|数字匹配|
|‘\D’|非数字匹配|
|'\s'|匹配空格|
|'\S'|匹配非空格|
|‘\w’|匹配”单词字符” (仅限ASCII)|
|‘\W’|匹配非“单词字符”（仅限ASCII）|

### 不支持的操作符
- 重复次数 ({m,n})
- 命名组 ((?P<name>...))
- 非捕获组((?:...))
- 更高级的断言(\b, \B)
- 特殊字符转义，如 \r, \n - 改用Python自己的转义。

### 编译并生成正则表达式对象
>ure.compile(regex)

compile 函数用于编译正则表达式，生成一个正则表达式（ Pattern ）对象，供 match() 和 search() 这两个函数使用。

- 参数

|参数|类型|说明|
|----|----|----|
|regex|string|正则表达式|

- 返回值

返回 regex 对象

### 匹配
>ure.match(regex, string)

将正则表达式对象与string匹配，匹配通常从字符串的起始位置进行。

- 参数

|参数|类型|说明|
|----|----|----|
|regex|string|正则表达式|
|string|string	|需要匹配的字符串数据|

- 返回值

匹配成功返回一个匹配的对象，否则返回None。

### 查找
>ure.search(regex, string)

ure.search 扫描整个字符串并返回第一个成功的匹配。

- 参数

|参数|类型|说明|
|----|----|----|
|regex|string|正则表达式|
|string|string	|需要匹配的字符串数据|

- 返回值

匹配成功返回一个匹配的对象，否则返回None。


### 匹配单个字符串
>match.group(index)

匹配的整个表达式的字符串

- 参数

|参数|类型|说明|
|----|----|----|
|index|int|正则表达式中，group()用来提出分组截获的字符串, index=0返回整体，根据编写的正则表达式进行获取，当分组不存在时会抛出异常|

- 返回值

返回匹配的整个表达式的字符串。

### 匹配多个字符串
>match.groups()

匹配的整个表达式的字符串

- 参数

无

- 返回值

返回一个包含该匹配组的所有字符串的元组。

### 获取起始索引
>match.start(index)

返回匹配的子字符串组的起始原始字符串中的索引。

- 参数

|参数|类型|说明|
|----|---|----|
|index|int|index 默认为整个组，否则将选择一个组|

- 返回值

返回匹配的子字符串组的起始原始字符串中的索引。

### 获取结束索引
>match.end(index)

返回匹配的子字符串组的结束原始字符串中的索引。

- 参数

|参数|类型|说明|
|---|-----|---|
|index|int|index 默认为整个组，否则将选择一个组|

- 返回值

返回匹配的子字符串组的结束原始字符串中的索引。

### 使用示例

	import ure
	import log
	if __name__ == '__main__':
	    #re.match尝试从字符串的起始位置匹配一个模式,如果不是起始位置匹配成功的话,match()就返回none
	    print(ure.match('meig', 'meig is the best!').span())  #起始位置匹配
	    print(ure.match('meig', 'meig is the best!'))         #不在起始位置匹配
	    #re.search 扫描整个字符串并返回第一个成功的匹配
	    print(ure.search('meig', 'meig is the best!').span())  #起始位置匹配
	    print(ure.search('the', 'meig is the best!').span())  #不在起始位置匹配
	    log.info(ure.match('meig', 'meig is the best!').span())
	    log.info(ure.match('meig', 'meig is the best!'))
	    log.info(ure.search('meig', 'meig is the best!').span())
	    log.info(ure.search('the', 'meig is the best!').span())
	    phone = "2021-008-026 #这是一个电话号码" 
	    #用于替换字符串中的匹配项
	    #删除字符串中的 Python注释
	    num = ure.sub(r'#.*$', "", phone)
	    print(num)
	    log.info(num)
	    #删除非数字(-)的字符串
	    num = ure.sub(r'\D', "", phone)
	    print(num)
	    log.info(num)
	
	    #用于编译正则表达式,生成一个正则表达式（Pattern）对象,供 match()和 search()这两个函数使用
	    print(ure.compile(r'\s+').split('meig python is the best'))
	    print(ure.compile(r'\s+').split('meig python is the best', 2))
	    log.info(ure.compile(r'\s+').split('meig python is the best'))
	    log.info(ure.compile(r'\s+').split('meig python is the best', 2))
	    #匹配两个数字
	    pattern = ure.compile(r'(\d)(\d)')
	    m = pattern.match('3146566544')
	    print(m)                #匹配成功，返回一个 Match 对象
	    print(m.group(0))       #返回匹配成功的整个子串     
	    print(m.span(0))        #返回匹配成功的整个子串的索引 
	    print(m.group(1))       #返回第一个分组匹配成功的子串
	    print(m.span(1))        #返回第一个分组匹配成功的子串的索引
	    print(m.group(2))       #返回第二个分组匹配成功的子串
	    print(m.span(2))        #返回第二个分组匹配成功的子串的索引
	    log.info(m)
	    log.info(m.group(0)) 
	    log.info(m.span(0))
	    log.info(m.group(1))
	    log.info(m.span(1))
	    log.info(m.group(2))
	    log.info(m.span(2))
	    #匹配字符
	    line = "Cats are smarter than dogs"
	    matchObj = ure.match(r'(.*) are (.*?) .*', line)
	    if matchObj:
	        print("matchObj.group(0) : ", matchObj.group(0))
	        print("matchObj.group(1) : ", matchObj.group(1))
	        print("matchObj.group(2) : ", matchObj.group(2))
	        print(m.start(0))
	        print(m.end(0))
	        print(m.span(0))
	        log.info("matchObj.group(0) : ", matchObj.group(0))
	        log.info("matchObj.group(1) : ", matchObj.group(1))
	        log.info("matchObj.group(2) : ", matchObj.group(2))
	        log.info(m.start(0))
	        log.info(m.end(0))
	        log.info(m.span(0))
	    else:
	        print ("No match!!")




