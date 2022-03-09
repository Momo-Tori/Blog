---
title: "PythonSyntax"
date: 2022-03-09T22:09:03+08:00
tags: ["python"]
categories: ["Python学习笔记"]
---

从c&cpp的角度总结python语法，并作为工具书查询使用

大部分复制至菜鸟教程，有删减

<!--more-->

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

## 目录

<!-- code_chunk_output -->

- [目录](#目录)
- [变量与数据结构](#变量与数据结构)
  - [字符串](#字符串)
    - [访问字符串](#访问字符串)
    - [格式化输出](#格式化输出)
    - [f-string](#f-string)
  - [列表list](#列表list)
  - [字典dict](#字典dict)
    - [初始化](#初始化)
    - [访问、添加与修改](#访问-添加与修改)
  - [注释](#注释)
  - [列表解析ListComprehension](#列表解析listcomprehension)
- [语法结构](#语法结构)
  - [条件语句if](#条件语句if)
  - [循环语句](#循环语句)
    - [while语句](#while语句)
    - [for语句](#for语句)
    - [pass](#pass)
  - [函数](#函数)
    - [定义](#定义)
    - [参数传递](#参数传递)
      - [可更改(mutable)与不可更改(immutable)对象](#可更改mutable与不可更改immutable对象)
      - [不定长参数](#不定长参数)
    - [lambda函数](#lambda函数)
- [模块](#模块)
  - [import](#import)
  - [from ... import](#from-import)
  - [from ... import *](#from-import-1)
  - [\_\_name\_\_](#__name__)
- [文件](#文件)
  - [open()方法](#open方法)
    - [模式](#模式)
    - [文件编码](#文件编码)
  - [文件IO函数](#文件io函数)
    - [file.read([size])](#filereadsize)
    - [file.readline([size])](#filereadlinesize)
    - [file.write(str)](#filewritestr)
    - [file.writelines(sequence)](#filewritelinessequence)
    - [file.close()](#fileclose)
- [面向对象](#面向对象)
  - [类定义](#类定义)
  - [专有函数](#专有函数)
  - [类函数](#类函数)
  - [继承](#继承)
  - [方法重写](#方法重写)
  - [私有属性、属性](#私有属性-属性)
- [迭代器与生成器](#迭代器与生成器)
  - [迭代器](#迭代器)
    - [iter(className)](#iterclassname)
    - [next(iterator)](#nextiterator)
    - [创造一个迭代器](#创造一个迭代器)
    - [StopIteration](#stopiteration)
  - [生成器](#生成器)

<!-- /code_chunk_output -->


## 变量与数据结构

python不需要程序员声明变量的数据结构，但背后仍然有严格的数据结构划分，主要有字符串、整型、浮点型、布尔型、复数型，此外将列表、字典等数据结构内置为固有结构（cpp的STL对应有vector、map等）  
这些变量的声明由赋值的对象决定（可以理解为每个变量都是auto v=...，但每个变量均可随时改变它所对应的数据结构），并且有对应的转换函数用于相互转换

### 字符串

python中的字符串利用`'`或`"`包围起来，两者没有特殊的区别  
在常值字符串前使用`r`可以让反斜杠不发生转义  


#### 访问字符串

使用`[a:b]`来截断`[a,b)`范围内即`s[a]-s[b-1]`的字符串  
若留空，则意味着取到首或尾，如`[1:]`表示包含`s[1]`及其之后的字符串  
特别地，可以用`[-l:]`来截断后l个字符的字符串，其中`-1`是最后一个字符的索引值  

#### 格式化输出

格式化字符串与c/cpp相同，参数则有一定的格式，如`"%d %d" % ( 1 , 2 )`，在字符串后使用`%(...,...)`的格式进行调用

#### f-string

f-string 是 python3.6 之后版本添加的，称之为字面量格式化字符串，是新的格式化字符串的语法。  
f-string 格式化字符串以 `f` 开头，后面跟着字符串，字符串中的表达式用大括号 `{}` 包起来，它会将变量或表达式计算后的值替换进去，实例如下：

```py
>>> name = 'Runoob'
>>> f'Hello {name}'  # 替换变量
'Hello Runoob'
>>> f'{1+2}'         # 使用表达式
'3'

>>> w = {'name': 'Runoob', 'url': 'www.runoob.com'}
>>> f'{w["name"]}: {w["url"]}'
```

### 列表list

类似于vector，有`list.append(obj)`、`list.insert(index, obj)`、`list.pop([index=-1])`等函数，可使用`[1,2,3]`这样的形式快速初始化

其中访问类似于字符串，添加或删除则使用上面提到的函数进行加减即可

### 字典dict

类似于map，是一个存储键值对的数据结构

#### 初始化

使用`{key:value,...}`进行初始化

#### 访问、添加与修改

如下程序所示

```py
dic={ "a":1, "b":2 }
print(dic)
print(dic["a"])     #访问
dic["c"]=3          #添加
dic["b"]=4          #修改
print(dic)
```

### 注释

由"#"开头，也可以使用三引号字符串（可以跨行）的形式，因为字符串不会改变python的输出结果

### 列表解析ListComprehension

利用一个列表来构建另一个列表，并可类推到字典、集合和元组的生成中

格式为`[表达式 for 变量 in 列表 if 条件]`

其中`[*]`意味着最后生成的是一个列表  
`表达式`是指结果列表元素的值，其可能与`变量`相关  
`变量`是指原列表的迭代器  
`列表`是指原列表  
`if 条件`是可省略项，若if成立则表达式的值加入结果列表

实例：

```py
>>> names = ['Bob','Tom','alice','Jerry','Wendy','Smith']
>>> new_names = [name.upper()for name in names if len(name)>3]
>>> print(new_names)
['ALICE', 'JERRY', 'WENDY', 'SMITH']
```

## 语法结构

首先需要指出的事实是，在python中缩进与程序结构相关，代码块通常用相同的缩进表示

### 条件语句if

```py
if condition_1:
    statement_block_1
elif condition_2:
    statement_block_2
else:
    statement_block_3
```

### 循环语句

#### while语句

```py
while 判断条件:
    执行语句...
[else]
    执行语句...
```

`[else]`部分可选，当while判断条件为False，则执行else对应的语句块，并跳出循环

#### for语句

```py
for <variable> in <sequence>:
    <statements>
else:
    <statements>
```

其中`<variable>`为容器中变量的值，迭代后结束循环

#### pass

NOP，没有任何操作

### 函数

#### 定义

```py
def 函数名（参数列表）:
    函数体
```

return [表达式] 结束函数，选择性地返回一个值给调用方，不带表达式的 return 相当于返回 None

#### 参数传递

可用关键词来传递参数，此时无需考虑位置关系

使用`par=sth,...`的形式来传递参数

##### 可更改(mutable)与不可更改(immutable)对象

- 可变对象：list dict set  
引用传递
- 不可变对象：tuple string int float bool  
值传递

python 中一切都是对象，严格意义我们不能说值传递还是引用传递，我们应该说传不可变对象和传可变对象。

##### 不定长参数

加了星号 * 的参数会以元组(tuple)的形式导入，存放所有未命名的变量参数。

实例

```py
#!/usr/bin/python3
 
# 可写函数说明
def printinfo( arg1, *vartuple ):
   "打印任何传入的参数"
   print ("输出: ")
   print (arg1)
   for var in vartuple:
      print (var)
   return
 
# 调用printinfo 函数
printinfo( 10 )
printinfo( 70, 60, 50 )
```

加了两个星号 ** 的参数会以字典的形式导入。实例如下

```py
#!/usr/bin/python3
  
# 可写函数说明
def printinfo( arg1, **vardict ):
   "打印任何传入的参数"
   print ("输出: ")
   print (arg1)
   print (vardict)
 
# 调用printinfo 函数
printinfo(1, a=2,b=3)
```

#### lambda函数

```py
lambda [arg1 [,arg2,.....argn]]:expression
```

使用一个表达式来定义函数，注意函数没有名字，若没有存储则无法调用

实例

```py
# 可写函数说明
sum = lambda arg1, arg2: arg1 + arg2
 
# 调用sum函数
print ("相加后的值为 : ", sum( 10, 20 ))
print ("相加后的值为 : ", sum( 20, 20 ))
```

## 模块

用来引入标准库或自己定义的函数，使用关键词`import`，有多种语法引入

### import

直接导入整个模块，语法为

```py
import module1[, module2[,... moduleN]
```

导入的同时可在后面加上`as ...`进行重命名

### from ... import

可以从模块中选择所需的函数导入所需的函数

```py
from modname import name1[, name2[, ... nameN]]
```

### from ... import *

将目标模块直接导入当前的命名空间，在使用函数时不再需要`模块名.目标函数`的形式

```py
from modname import *
```

### \_\_name\_\_

一个模块被另一个程序第一次引入时，其主程序将运行。如果我们想在模块被引入时，模块中的某一程序块不执行，我们可以用\_\_name\_\_属性来使该程序块仅在该模块自身运行时执行。

注意name前后都是两个下划线`_` `_`

## 文件

### open()方法

Python open() 方法用于打开一个文件，并返回文件对象，在对文件进行处理过程都需要使用到这个函数，如果该文件无法被打开，会抛出 OSError。

注意：使用 open() 方法一定要保证关闭文件对象，即调用 close() 方法。这点与c&cpp相同

open() 函数常用形式是接收两个参数：文件名(file)和模式(mode)。

```py
open(file, mode='r')
```

#### 模式

|模式|描述|
|---|---|
|t|文本模式 (默认)|
|x|写模式，新建一个文件，如果该文件已存在则会报错|
|b|二进制模式|
|+|打开一个文件进行更新(可读可写)|
|r|以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式|
|rb|以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是默认模式。一般用于非文本文件如图片等|
|r+|打开一个文件用于读写。文件指针将会放在文件的开头|
|rb+|以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。一般用于非文本文件如图片等|
|w|打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件|
|wb|以二进制格式打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。一般用于非文本文件如图片等|
|w+|打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件|
|wb+|以二进制格式打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。一般用于非文本文件如图片等|
|a|打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入|
|ab|以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入|
|a+|打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写|
|ab+|以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写|

#### 文件编码

以特定编码输出文件：

```py
with open("output.html","w",encoding="utf=8") as out:
    out.write(res.text)
```

### 文件IO函数

#### file.read([size])

从文件读取指定的**字节**数，如果未给定或为负则读取所有

#### file.readline([size])

读取整行，包括 "\n" 字符。

#### file.write(str)

将字符串写入文件，返回的是写入的字符长度。

#### file.writelines(sequence)

向文件写入一个序列字符串列表，如果需要换行则要自己加入每行的换行符。

#### file.close()

关闭文件

## 面向对象

### 类定义

```py
class ClassName:
    <statement-1>
    .
    .
    .
    <statement-N>
```

其中`statement`可以是数据结构也可以是方法，与cpp相同

### 专有函数

类有多种的特殊的专有方法，在各种关键词时可以调用


|函数|描述|
|-|-|
|`__init__ `| 构造函数，在生成对象时调用|
|`__del__ `| 析构函数，释放对象时使用|
|`__repr__ `| 打印，转换|
|`__setitem__ `| 按照索引赋值|
|`__getitem__`| 按照索引获取值|
|`__len__`| 获得长度|
|`__cmp__`| 比较运算|
|`__call__`| 函数调用|
|`__add__`| 加运算|
|`__sub__`| 减运算|
|`__mul__`| 乘运算|
|`__truediv__`| 除运算|
|`__mod__`| 求余运算|
|`__pow__`| 乘方|


### 类函数

类的方法与普通的函数只有一个特别的区别——它们必须有一个额外的第一个参数名称, 按照惯例它的名称是 `self`，这代表一个实例化的类对象

```py
class Test:
    def prt(self):
        print(self)
        print(self.__class__)
 
t = Test()
t.prt()
```

输出结构如下

```bash
<__main__.Test instance at 0x100771878>
__main__.Test
```

其中`self.__class__`表示是类自身

### 继承

```py
class DerivedClassName(BaseClassName):
    <statement-1>
    .
    .
    .
    <statement-N>
```

注意BaseClassName（实例中的基类名）必须与派生类定义在一个作用域内。除了类，还可以用表达式，基类定义在另一个模块中时这一点非常有用:

```py
class DerivedClassName(modname.BaseClassName):
```

同时可以通过增加BaseClass实现**多继承**

### 方法重写

直接在派生类的定义中覆盖方法定义即可，与cpp的虚拟函数相同

### 私有属性、属性

\_\_private_statement：两个下划线开头，声明该属性/方法为私有，不能在类的外部被使用或直接访问。在类内部的方法中使用时 self.\_\_private_statement

## 迭代器与生成器

### 迭代器

迭代器是一种手动定义原类后自动生成的类，它可以根据某种顺序对类对象进行遍历或者实现某种迭代

主要有两种基本的方法：`iter()` 和 `next()`

#### iter(className)

返回该类的迭代器iterator对象，迭代器直接可以用在`for...in...`的循环中

```py
#!/usr/bin/python3
 
list=[1,2,3,4]
it = iter(list)    # 创建迭代器对象
for x in it:
    print (x, end=" ")
```

#### next(iterator)

该方法返回iterator对象所指的遍历对象，并将iterator对象所指的遍历对象向后推进

#### 创造一个迭代器

`__iter__()` 方法返回一个特殊的迭代器对象， 这个迭代器对象实现了 `__next__()` 方法并通过 StopIteration 异常标识迭代的完成

`__next__()` 方法返回下一个迭代器对象

```py
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self
 
  def __next__(self):
    x = self.a
    self.a += 1
    return x
```

#### StopIteration

`StopIteration` 异常用于标识迭代的完成，防止出现无限循环的情况，在 `__next__()` 方法中我们可以设置在完成指定循环次数后触发 `StopIteration` 异常来结束迭代。

```py
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self
 
  def __next__(self):
    if self.a <= 20:
      x = self.a
      self.a += 1
      return x
    else:
      raise StopIteration

```

### 生成器

在 Python 中，使用了 yield 的函数被称为生成器（generator）。

跟普通函数不同的是，生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器。

在调用生成器运行的过程中，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值, 并在下一次执行 next() 方法时从当前位置继续运行。

调用一个生成器函数，返回的是一个迭代器对象。

以下实例使用 yield 实现斐波那契数列：

```py
#!/usr/bin/python3
 
import sys
 
def fibonacci(n): # 生成器函数 - 斐波那契
    a, b, counter = 0, 1, 0
    while True:
        if (counter > n): 
            return
        yield a
        a, b = b, a + b
        counter += 1
f = fibonacci(10) # f 是一个迭代器，由生成器返回生成
 
while True:
    try:
        print (next(f), end=" ")
    except StopIteration:
        sys.exit()
```