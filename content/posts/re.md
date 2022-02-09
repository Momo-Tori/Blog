---
title: "正则表达式"
date: 2022-02-09T16:29:08+08:00
tags: ["re","python"]
categories: ["Python学习笔记"]
---

<!--more-->
## 语法

正则表达式描述了一种字符串匹配度的模式（pattern），其由普通字符以及特殊字符（称为“元字符”）组成，描述在搜索文本时要匹配的一个或多个字符串

### 普通字符

普通字符是没有显式指定为元字符的所有字符，即可见的字符

|字符|描述|
|:-:|-|
|`[ABC]`|匹配`[...]`中的所有字符，即方括号内的一个集合，对应原字符串的一个字符|
|`[^ABC]`|匹配**除了**`[...]`内字符外的所有字符，即方括号内的一个补集|
|`[A-Z]`|匹配一个区间|
|`.`|匹配除了换行符(`\r,\n`)外的任意单个字符|
|`[\s\S]`|匹配所有。`\s`是所有空白符，包括换行，`\S`是非空白符|
|`\w`|匹配字母、数字、下划线|
|`\W`|匹配字母、数字、下划线的补集，大写和小写互为补集，下不表|
|`\s`|匹配任意空白字符，等价于[\t\n\r\f]|
|`\d`|匹配数字，[0-9]|


### 特殊字符

带有特殊含义的字符，若要匹配原本的字符需要特殊转义的字符，若要匹配`***`则需要字符模式为`\*\*\*`

|特殊字符|描述|
|:-:|-|
|`$`|匹配输入字符串的结尾位置|
|`()`|标记一个子表达式（括号的一般作用）|
|`*`|匹配前面的子表达式零次或多次|
|`+`|匹配前面的子表达式一次或多次|
|`.`|匹配除换行符\n之外任意单字符|
|`[`|标记一个中括号表达式的开始|
|`?`|匹配前面的子表达式零次或一次，或指明一个[非贪婪限定符](#贪婪与非贪婪)|
|`\`|转义符号|
|`^`|匹配输入字符串的开始位置，除非在方括号表达式中使用，当该符号在方括号表达式中使用时，表示不接受该方括号表达式中的字符集合（补集）|
|`{`|标记限定符表达式的开始|
|`|`|指明两项之间的选择|


### 限定符

限定符用来指定正则表达式的一个给定组件必须要出现多少次才能满足匹配

有`*`，`+`，`?`，`{n}`，`{n,}`，`{n,m}`共六种

|字符|描述|
|:-:|-|
|`*`|匹配前面的子表达式零次或多次|
|`+`|匹配前面的子表达式一次或多次|
|`?`|匹配前面的子表达式零次或一次|
|`{n}`|匹配前面的子表达式n次|
|`{n,}`|匹配前面的子表达式至少n次|
|`{n,m}`|匹配前面的子表达式至少n次至多m次|

#### 贪婪与非贪婪

贪婪：尽可能多的匹配文字，即取可能匹配文字的最大匹配

非贪婪：尽可能少的匹配文件，即取最小匹配

`*`和`+`都是贪婪的，但在它们的后面加上一个`?`就可以实现非贪婪或最小匹配

### 定位符

定位符用来限定字符串或单词的边界

|字符|描述|
|:-:|-|
|`^`|匹配输入字符串开始的位置（每一行的开头）|
|`$`|匹配输入字符串结束的位置（每一行的结尾）|
|`\b`|匹配一个单词边界，即字与空格之间的位置|
|`\B`|非单词边界匹配|

### 选择

就是简单的或，可以将多个表达式合起来作为一个整体进行匹配，一般形式为`(xxx|yyy|...)`，所得到的就是**捕获组**

同时对一个正则表达式模式或部分模式两边添加圆括号将导致相关匹配存储到一个临时缓冲区中，所捕获的每个子匹配都按照在正则表达式模式中从左到右出现的顺序存储。缓冲区编号从 1 开始，最多可存储 99 个捕获的子表达式。每个缓冲区都可以使用`\n`访问，其中`n`为一个标识特定缓冲区的一位或两位十进制数。

**非捕获组**就是在捕获组的基础上在括号内部最左侧加上`?:`，此时只是实现或，并不会保存分组

### 反向引用

|表达式|效果|
|:-:|-|
|`exp1(?=exp2)`|查找 exp2 前面的 exp1|
|`(?<=exp2)exp1`|查找 exp2 后面的 exp1|
|`exp1(?!exp2)`|查找后面不是 exp2 的 exp1|
|`(?<!exp2)exp1`|查找前面不是 exp2 的 exp1|

> 注意其中exp1和exp2的关系是相邻的，中间没有其他字符

## 修饰符(标记)

|修饰符|含义|描述|
|:-:|-|-|
|`i`|ignore - 不区分大小写|将匹配设置为不区分大小写，搜索时不区分大小写: A 和 a 没有区别。|
|`g`|global - 全局匹配|查找所有的匹配项。|
|`m`|multi line - 多行匹配|使边界字符 ^ 和 $ 匹配每一行的开头和结尾，记住是多行，而不是整个字符串的开头和结尾。|
|`s`|特殊字符圆点 . 中包含换行符 \n|默认情况下的圆点 . 是 匹配除换行符 \n 之外的任何字符，加上 s 修饰符之后, . 中包含换行符 \n。|

不同的语言有不同的修饰符输入格式，具体再查询即可

## 在python中的运用

python提供了re模块，可以用其进行正则表达式的匹配，搜索与替换等

### 匹配

```py
re.match(pattern,string,flag=0)
```

从头开始匹配，若匹配成功返回一个匹配的对象，否则返回None

其中`flag`的参数传递举例如下：`re.M|re.I`

对于匹配成功的对象可以使用`group(num)`或`groups()`两个方法来获得匹配的字符串

```py
Obj=re.match(pattern,string,flag=0)
print(Obj.group()) #返回匹配的字符串
print(Obj.group(0)) #返回匹配的字符串，与上一行相同
print(Obj.group(1)) #返回第一个小组
#...
print(Obj.groups()) #返回一个包含所有小组字符串的元组
```

### 搜索

```py
re.search(pattern,string,flags=0)
```

从头开始逐个字符为起点进行匹配，直到找到一个与格式相符的字符串返回一个匹配的对象，否则返回None

对于匹配成功的对象的匹配字符串的调用与上面match相同

### 检索和替换

re模块提供了re.sub用于替换字符串中的匹配项。

```py
re.sub(pattern, repl, string, count=0, flags=0)
```

其中repl为替换的字符串，也可以是一个函数，该函数的参数为一个匹配对象

### compile 函数

compile 函数用于编译正则表达式，生成一个正则表达式（Pattern）对象，其有`match()`和`search()`这两个方法可以使用。

语法格式为：

```py
re.compile(pattern[, flags])
```

实例如下

```py
>>>import re
>>> pattern = re.compile(r'\d+')                    # 用于匹配至少一个数字
>>> m = pattern.match('one12twothree34four')        # 查找头部，没有匹配
>>> print( m )
None
>>> m = pattern.match('one12twothree34four', 2, 10) # 从'e'的位置开始匹配，没有匹配
>>> print( m )
None
>>> m = pattern.match('one12twothree34four', 3, 10) # 从'1'的位置开始匹配，正好匹配
>>> print( m )                                        # 返回一个 Match 对象
<_sre.SRE_Match object at 0x10a42aac0>
>>> m.group(0)   # 可省略参数 0
'12'
>>> m.span()
(3, 5)
```

### findall

在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，如果有多个匹配模式，则返回元组列表，如果没有找到匹配的，则返回空列表。

注意： match 和 search 是匹配一次 findall 匹配所有。

语法格式为：

```py
re.findall(pattern, string, flags=0)
或
pattern.findall(string[, pos[, endpos]])
```

### finditer
和 findall 类似，在字符串中找到正则表达式所匹配的所有子串，并把它们作为一个迭代器返回。

```py
re.finditer(pattern, string, flags=0)
```

### split

split 方法按照能够匹配的子串将字符串分割后返回字符串列表，它的使用形式如下：

```py
re.split(pattern, string[, maxsplit=0, flags=0])
```