---
title: 1.python基础
date: 2016-11-18 22:41:28
categories: [python教程]
tags:
  - python教程
---
  
# 1. 内置类型

## 1.1 [变量](http://opslinux.com/2016/11/20/1-1-1-%E5%8F%98%E9%87%8F/)

## 1.2 布尔

表示真假的类型，仅包含 True 和 False 两种取值

数字 0、None，以及元素为空的容器类对象都可视作 bool False，反之为 True。

```
>>> bool(0)
False

>>> bool(None)
False

>>> bool("")
False

>>> bool([])
False

>>> bool({})
False

>>> bool(1)
True

>>> bool([1,2])
True
```

bool类型支持的运算符


```
>>> a and b  # 如果 a 和 b 都是 True，结果就是 Ture ， 否则 False。
>>> a or b   #  a 和 b 至少有一个是 True 时结果是 True， 否则 False。
>>> not b    # 如果 a 是 False, 结果是 True， 如果 a 是 True，结果是 False。
```

## 1.3 数字
python本身支持整数以及浮点数。你可以对这些数字进行下表中的计算。

| 运算符 | 表述 | 示例 | 运算结果 |
| --- | --- | --- | :-- |
| + | 加法 | 1 + 1  | 2  |
| - | 减法 | 4 - 2 | 2 |
| * | 乘法 | 2 * 2 | 4 |
| / | 浮点数除法 | 7 / 2 | 3.5 |
| // | 整数除法 | 7 // 2 | 3 |
| / | 模（求余） | 7 % 3 | 1 |
| ** | 幂 | 2 ** 2 | 4  |


### 整数

任何仅含数字的序列在 Python 中都被认为是整数：

```
>>> 5
5
```

Python还支持运算次序，因此你可在同一个表达式中使用多种运算。你还可以使用括号来修 改运算次序，让Python按你指定的次序执行运算，如下所示:

```python
>>> 2 + 3*4
14
>>> (2 + 3) * 4 20
```

在这些示例中，空格不影响Python计算表达式的方式，它们的存在旨在让你阅读代码时，能 迅速确定先执行哪些运算。

### 浮点数
Python将带小数点的数字都称为浮点数。大多数编程语言都使用了这个术语，它指出了这样一个事实:小数点可出现在数字的任何位置。每种编程语言都须细心设计，以妥善地处理浮点数， 确保不管小数点出现在什么位置，数字的行为都是正常的。

```python
>>> 0.1 + 0.1
0.2
>>> 0.2 + 0.2
0.4
>>>2 * 0.1
0.2
>>>2 * 0.2
0.4
```

但需要注意的是，结果包含的小数位数可能是不确定的:

```python
>>> 0.2 + 0.1 
0.30000000000000004 
>>> 3 * 0.1 
0.30000000000000004
```

所有语言都存在这种问题，没有什么可担心的。Python会尽力找到一种方式，以尽可能精确地表示结果，但鉴于计算机内部表示数字的方式，这在有些情况下很难。就现在而言，暂时忽略 多余的小数位数即可。







## 1.4 字符串

字符串是由多个字符组成的序列。在Python中，用引号括起的都是字符串，字符串定义简单自由，可以是单引号、双引号或者三引号。但是个人建议使用双引号表示字符串，用单引号表示字符，和其他语言习惯保持一致。字符串是不可变序列（immutable, sequence）类型，默认存储 Unicode 文本。 python3 不再使用 str 处理二进制字节数据，改为使用 bytes 和 bytearray，前者同为不可变类型。


```
>>> s = "abc汉字"
>>> >>> len(s)
5

>>> print(ascii(s))
'abc\u6c49\u5b57'
```

> 内置函数 ascii 将目标转换为可打印 ASCII 字符组成的字符串。

构建字符串字面量很容易，单引号、双引号，以及跨行的三个引号。


```
>> "ab'c"           # 双引号。
"ab'c"

>>> 'ab"c'          # 单引号。
'ab"c'

>>> 'ab\'c'         # 引号转义。
"ab'c"

>>> """             # 多行，也可以用三个单引号。
... a
... b
... c"""
'\na\nb\nc'

>>> "a" "b" 'c'     # 自动合并多个相邻字符串。
'abc'

```

可在字面量前添加特殊指示符。


```
>>> r"abc\nd"       # raw string，禁用转义。
'abc\\nd'

>>> type(b"abc")
<class 'bytes'>

>>> type(u"abc")
<class 'str'>
```

### str() 类型转换

```
>>> str(2.2)
'2.2'
```




### 合并字符串

#### format 

```
>>> "python培训哪家强：{}".format('京峰教育')
'python培训哪家强：京峰教育'
```

```
>>> "python培训哪家强：{}, 京峰教育谁最帅？ {}".format('京峰教育', '斌哥')
'python培训哪家强：京峰教育, 京峰教育谁最帅？ 斌哥'
```

```
>>> "python培训哪家强：{0}, 京峰教育谁最帅？ {1}".format('京峰教育', '斌哥')
'python培训哪家强：京峰教育, 京峰教育谁最帅？ 斌哥'
```

#### +

```
>>> '京峰' +  '教育'
'京峰教育'
```
  

### split() 分割

```
>>> s = 'a,b,c'

>>> s.split(',')
['a', 'b', 'c']
```
### join() 合并

```
>>> l = s.split(',')

>>> l
['a', 'b', 'c']

>>> ','.join(l)
'a,b,c'
```

### find 查找子串

查找到返回该子串在原字符串中的索引位置，如果无法找到，find方法会返回值-1

```
>>> s = "abc"
>>> s.find('a')
0
>>> s.find('d')
-1
>>>
```


## 1.5 [列表](http://opslinux.com/2016/11/20/1-1-2-%E5%88%97%E8%A1%A8/)
## 1.6 元组
与列表类似，元组也是由任意类型元素组成的序列。与列表不同的是，元组是不可改变的，这意味着一但元组被定义，将无法再进行增加、删除或者修改元素等操作。因此元组就像一个常量列表。从行为上看，元组（tuple）像是列表的只读版本。 但在内在实现上有根本不同，元组的只读性使其拥有更好的内存效率和性能。除无法修改外，其普通特征和列表类似。 在需要传递 “不可变” 参数时，应鼓励用元组替代列表。 它是可哈希（hashaable）结构，可用作字典（dict）主键（key）

### 使用()创建元组

可以用()创建一个空元组：

```
>>> empty_tuple = ()
>>> empty_tuple
()
>>> type(empty_tuple)
tuple
```

创建包含一个或多个元素的元组时，没一个元素后面需要跟着一个逗号，即使只包含一个元素也不能忽略：

```
>>> num = '1',
>>> num
('1',)
>>>
```

如果创建的元组所包含的元素数量超过1，最后一个元素后面的逗号可以忽略：


```
>>> num = '1', '2', '3'
>>> num
('1', '2', '3')
```
Python的交互式解释器输出元组时会自动添加一堆圆括号。你并不需要这么做——定义元组真正靠的是每个元素的后缀逗号——但如果你习惯添加一对括号也无可厚非。可以用括号将所有元素包裹起来，这会使得程序更加清晰：

```
>>> num = ('1', '2', '3')
>>> num
('1', '2', '3')
```

可以一口气将元组赋值给多个变量：


``` 
>>> a, b, c = num
>>> a
'1'
>>> b
'2'
>>> c
'3'
>>>       
```

这个过程称为元组解包

可以利用元组在一条语句中对多个变量的值进行交换，而不需借助临时变量：

```
>>> a = 1
>>> b = 2
>>> a, b = b, a
>>> a
2
>>> b
1
>>>
```

tuple() 函数可以用其他类型的数据来创建元组：

```
>>> num = [1, 2, 3]
>>> tuple(num)
(1, 2, 3)
>>>
```

### 元组与列表
在许多地方都可以用元组代替列表，但元组的方法函数与类表相比要少一些——元组没有 append() 、insert()，等等——因为一但创建元组变无法修改。既然列表更加灵活那为什么不在所有地方都是用列表呢？原因如下：

* 元组占用的空间小
* 你不会意外修改元组的值
* 可以将元组用作字典的键（详细的后面会介绍）
* 命名元组可以作为对象的代替
* 函数的参数是以元组形式是传递的

## 1.7 字典

### 使用 {} 创建字典

```
>>> empty_dict = {}
>>> empty_dict
{}
```

### 使用 dict() 转换为字典
可以用 dict() 将包含双值子序列的序列转换成字典。

```
>>> lol = [ ['a', 'b'], ['c', 'd'], ['e', 'f'] ]
>>> dict(lol)
{'a': 'b', 'c': 'd', 'e': 'f'}
```
> 记住，字典中元素的顺序是无关紧要的，实际存储顺序可能取决于你添加元素的顺序。

双值元组列表：

```
>>> lot = [ ('a', 'b'), ('c', 'd'), ('e', 'f') ]
>>> dict(lot)
{'a': 'b', 'c': 'd', 'e': 'f'}
```

双字符串的字符串组成的列表：

```
>>> los = [ 'ab', 'cd', 'ef' ]
>>> dict(los)
{'a': 'b', 'c': 'd', 'e': 'f'}
```

双字符的字符串组成的元组：

```
>>> tos = ( 'ab', 'cd', 'ef' )
>>> dict(tos)
{'a': 'b', 'c': 'd', 'e': 'f'}
```


### 使用 [key] 添加或修改元素

```
>>> d = {"l1":1, "l2":2, "l3":3}
>>> d["l4"] = 4
>>> d
{'l2': 2, 'l1': 1, 'l3': 3, 'l4': 4}
```

### 使用 update() 合并字典

```
>>> d = {"l1":1, "l2":2, "l3":3}
>>> d.update({"l4":4})
>>> d
{'l2': 2, 'l1': 1, 'l3': 3, 'l4': 4}
```

### 使用 del 删除具有指定键的元素

```
>>> d = {"l1":1, "l2":2, "l3":3}
>>> del d["l1"]
>>> d
{'l2': 2, 'l3': 3}
```

### 使用 clear() 删除所有元素

```
>>> d = {"l1":1, "l2":2, "l3":3}
>>> d.clear()
>>> d
{}
```

### 使用 in 判断是否存在
如果你希望判断某一个键是否存在于一个字典中，可以使用 in 。

```
>>> d = {"l1":1, "l2":2, "l3":3}
>>> d
{'l2': 2, 'l1': 1, 'l3': 3}
>>> 1 in d
False
>>> "l1" in d
True
>>> "l4" in d
False
>>>
```

### 使用 [key] 获取元素

```
>>> d = {"l1":1, "l2":2, "l3":3}
>>> d.keys()
dict_keys(['l2', 'l1', 'l3'])
```

### 使用 values() 获取所有值

```
>>> d = {"l1":1, "l2":2, "l3":3}
>>> list( d.values() )
[2, 1, 3]
```

### 使用 items() 获取所有键值对

```
>>> d = {"l1":1, "l2":2, "l3":3}
>>> list(d.items())
[('l2', 2), ('l1', 1), ('l3', 3)]
```

### 使用=赋值，使用copy()复制
与列表一样，对字典内容进行修改会反应到所有与之相关联的变量名上：

```
>>> d = {"l1":1, "l2":2, "l3":3}
>>> d2 = d
>>> d.update({"l4":4})
>>> d
{'l2': 2, 'l1': 1, 'l3': 3, 'l4': 4}
>>> d2
{'l2': 2, 'l1': 1, 'l3': 3, 'l4': 4}
>>>
```

若想避免这种情况，可以使用 copy() 将字典复制到一个新的字典中：

```
>>> d
{'l2': 2, 'l1': 1, 'l3': 3, 'l4': 4}
>>> d2
{'l2': 2, 'l1': 1, 'l3': 3, 'l4': 4}
>>> d = {"l1":1, "l2":2, "l3":3}
>>> d2 = d.copy()
>>> d.update({"l4":4})
>>> d2
{'l1': 1, 'l2': 2, 'l3': 3}
>>> d
{'l2': 2, 'l1': 1, 'l3': 3, 'l4': 4}
```



### 两个列表转换为字典

```
>>> l1 = [1,2,3]
>>> l2 = ["one", "two", "there"]
>>> zip(l1,l2)
<zip object at 0x1022f1588>
>>> dict(zip(l1,l2))
{1: 'one', 2: 'two', 3: 'there'}
```

## 1.8 集合

集合就像舍弃了值，仅剩下的字典一样。键与键之间也不允许重复。如果你仅仅想知道某一个元素是否存在而不关心其他的，使用集合是个非常好的选择。如果需要为键附加其他信息的话建议使用字典。


### 使用 set() 创建集合

可以使用set()函数创建一个集合，或者用大括号将一系列一都好分开的值包裹起来：

```
>>> empty_set = set()
>>> empty_set
set()
>>> num_set ={1,2,3,4,5}
>>> num_set
{1, 2, 3, 4, 5}
```

与字典一样，集合是无序的。

> {} 创建的是一个空字典，这仅仅是因为字典出现的比较早抢占了花括号。


### 使用set()将其他类型转换为集合

你可以利用已有的列表、字符串、元组或字典的内容来创建集合，其中重复的值会被丢弃。

首先来试着转换一个包含重复字母的字符串：

```
>>> set('letters')
{'l', 'r', 'e', 't', 's'}
```

注意，上面得到的集合中仅含有一个 'e' 和一个 't'，尽管字符串 'letters' 里各自包含两个。

再试试用列表建立集合：

```
>>> set(['one', 'two', 'three'])
{'one', 'two', 'three'}
```

再看下元组：

```
>>> set(('one', 'two', 'three'))
{'one', 'two', 'three'}
```

当字典作为参数传入set()函数时，只有键会被使用：

```
>>> set( {'apple': 'red', 'orange': 'orange', 'cherry': 'red'} )
{'cherry', 'orange', 'apple'}
```

### 使用in测试值是否存在

```
>>> num_set = {'one', 'two', 'three'}
>>> num_set
{'one', 'two', 'three'}
>>> 'one' in num_set
True
>>> 'four' in num_set
False
```

### 添加删除数据

```
>>> num_set = {'one', 'two', 'three'}
>>> num_set.add('four')
>>> num_set
{'one', 'two', 'four', 'three'}
>>> num_set.remove('one')
>>> num_set
{'two', 'four', 'three'}
```

### 交集和并集

set可以看成数学意义上的无序和无重复元素的集合，因此，两个set可以做数学意义上的交集、并集等操作：

```
>>> s1 = set([1, 2, 3])
>>> s2 = set([2, 3, 4])
>>> s1 & s2
set([2, 3])
>>> s1 | s2
set([1, 2, 3, 4])
```
  
## 比较数据结构

列表

元组

字典

集合
  
## 建立大型数据结构

我们从最简单的布尔型、数字、字符串开始学习，到目前为止，学习了列表、元组集合以及字典等数据结构，你可以将这些内置的数据结构自由地组合成更大、更复杂的结构。试着从建立上三个不同的列表开始：

```
>>> num = [1, 2, 3]
>>> name = ["l1", "l2", "l3", "l4", "l5"]
>>> english = ["one", "two", "three"]
```
可以把上面每一个列表当做一个元素，并建立一个元组：

```
>>> tol = num, name, english
>>> tol
([1, 2, 3], ['l1', 'l2', 'l3', 'l4', 'l5'], ['one', 'two', 'three'])
>>>
```
同样，可以创建一个包含上面三个列表的列表：

```
>>> lol = [num, name, english]
>>> lol
[[1, 2, 3], ['l1', 'l2', 'l3', 'l4', 'l5'], ['one', 'two', 'three']]
```
还可以创建以这三个列表为值的字典：

```
>>> dol = {'num': num, 'name':name, 'english':english}
>>> dol
{'english': ['one', 'two', 'three'], 'name': ['l1', 'l2', 'l3', 'l4', 'l5'], 'num': [1, 2, 3]}
>>>
```
在创建自定义数据结构的过程中，唯一的限制来自于这些内置数据本身。比如字典的键必须为不可变对象，因此列表、字典和集合都不能作为字典的键，但元组可以作为字典的键。举个例子，我们可以通过 GPS 坐标（纬度，经度，海拔）定位感兴趣的位置：

```
>>> houses = {
... (44.79, -93.14, 285): 'My House', 
(38.89, -77.03, 13): 'The White House' }
```


  
# 2. 代码格式

# 2.1. 注释
在大多数编程语言中，注释都是一项很有用的功能。随着程序越来越大、越来越复杂，就应在其中添加说明，对你解决问题的方法进行大致的阐述。注释让你能够使用自然语言在程序中添加说明。注释是程序中会被Python解释器忽略的一段文本。通过使用注释，可以解释和明确Python代码的功能，记录将来要修改的地方，甚至写下你想写的东西。在Python中使用#字符标记注释，从#开始到当前行结束的部分都是注释。你可以把注释作为单独一行：
  
单行注释：

hello.py

```
# 向大家问好
print("Hello Python people!")
```

Python解释器将忽略第一行，只执行第二行.

```
print("Hello Python people!")
```

多行注释：

```
#coding=utf-8

"""这是"nester.py"模块，提供了一个名为print_lol的函数，这个函数的作用是打印列表，其中有可能包含（也可能不包含）嵌套列表。"""
def print_lol(the_list):
    """这个函数取一个位置参数，名为"the_list",这个可以是任何python列表（也可以是包含嵌套列表的列表）。所指定的列表中的每个数据项（递归地）输出到屏幕上，各数据项各占一行。"""
    for each_item in the_list:
        if isinstance(each_item, list):
            print_lol(each_item)
        else:
            print(each_item)
```

**该编写什么样的注释?**

编写注释的主要目的是阐述代码要做什么，以及是如何做的。在开发项目期间，你对各个部分如何协同工作了如指掌，但过段时间后，有些细节你可能不记得了。当然，你总是可以通过研 究代码来确定各个部分的工作原理，但通过编写注释，以清晰的自然语言对解决方案进行概述， 可节省很多时间。
要成为专业程序员或与其他程序员合作，就必须编写有意义的注释。当前，大多数软件都是合作编写的，编写者可能是同一家公司的多名员工，也可能是众多致力于同一个开源项目的人员。 训练有素的程序员都希望代码中包含注释，因此你最好从现在开始就在程序中添加描述性注释。 作为新手，最值得养成的习惯之一是，在代码中编写清晰、简洁的注释。
如果不确定是否要编写注释，就问问自己，找到合理的解决方案前，是否考虑了多个解决方案。如果答案是肯定的，就编写注释对你的解决方案进行说明吧。相比回过头去再添加注释，删除多余的注释要容易得多。从现在开始，本书的示例都将使用注释来阐述代码的工作原理。
  
## 2.2 python 之禅

## 2.3 pep8

# 3. 表达式

## 控制流

### if

#### elif

#### 条件表达式(即"三元操作符")

三元运算符可以只需要一行完成条件判断和赋值操作

```
data = x if x < y else y
```

### while
while 是Python中的循环语句. 事实它上是一个条件循环语句. 与 if 声明相比, 如果 if 后的条件为真, 就会执行一次相应的代码块. 而 while 中的代码块会一直循环执行, 直到循环条件不再为真.

语法：

```python
 while expression:
        suite_to_repeat
```

while 循环的 suite_to_repeat 子句会一直循环执行, 直到 expression 值为布尔假. 这种 类型的循环机制常常用在计数循环中。

```python
count = 0
while (count < 9):
    print('the index is:', count)
    count += 1
```

#### 无限循环

你必须小心地使用 while 循环, 因为有可能它的条件永远不会为布尔假. 这样一来循环就永远不会结束. 这些"无限"的循环不一定是坏事, 许多通讯服务器的客户端/服务器系统就是通过它来工作的. 这取决于循环是否需要一直执行下去, 如果不是, 那么这个循环是否会结束; 也就是说, 条件表达式会不会计算后得到布尔假?


```python
while True:
    handle, indata = wait_for_client_connect()
    outdata = process_request(indata)
    ack_result_to_client(handle, outdata)
```

例如上边的代码就是故意被设置为无限循环的，因为 True 无论如何都不会变成 False. 这是因为服务器代码是用来等待客户端(可能通过网络)来连接的. 这些客户端向服务器发送请求, 服务器处理请求. 
请求被处理后, 服务器将向客户端返回数据, 而此时客户端可能断开连接或是发送另一个请求. 对于服务器而言它已经完成了对这个客户端的任务, 它会返回最外层循环等待下一个连接. 

#### while使用 else 语句
在 python 中，for … else 表示这样的意思，for 中的语句和普通的没有区别，else 中的语句会在循环正常执行完（即 for 不是通过 break 跳出而中断的）的情况下执行，while … else 也是一样。

```
count = 0
while count < 5:
   print count, " is  less than 5"
   count = count + 1
else:
   print count, " is not less than 5"
```
结果：
```
0 is less than 5
1 is less than 5
2 is less than 5
3 is less than 5
4 is less than 5
5 is not less than 5
```



### for

Python 提供给我们的另一个循环机制就是 for 语句. 它提供了 Python 中最强大的循环结构. 它可以遍历序列成员。

for 循环会访问一个可迭代对象(例如序列或是迭代器)中的所有元素, 并在所有条目都处理过后结束循环. 它的语法如下:

```python
for iter_var in iterable:
        suite_to_repeat
```

每次循环, iter_var 迭代变量被设置为可迭代对象(序列, 迭代器, 或者是其他支持迭代的对象)的当前元素, 提供给 suite_to_repeat 语句块使用.

#### else

for 循环也可以有 else 用于循环后处理(post-processing). 它和 while 循环中的 else 处理方式相同. 只要 for 循环是正常结束的(不是通过 break ), else 子句就会执行.


```
s = ["a", "b", "c", "d", "e"]
found = False
for c in s:
    if c.find("c") != -1:
        found = True
        print("发现c项")
        break
 
if not found:
    print("没有发现c项")
```

```
s = ["a", "b", "c", "d", "e"]
for c in s:
    if c.find("c") != -1 :
        print("发现c项")
        break
else:
    print("没有c项")
```

#### enumerate

```
>>> l = ['a', 'b', 'c']
>>> 
>>> for index, item in enumerate(l):
...     print(index, item)
...
0 a
1 b
2 c
```
#### range()

内建函数 range() 可以把类似 foreach 的 for 循环变成你更加熟悉的语句. 例如从 0 到 10 计数, 或者从 10 到 100 一次递增 5 .

```
range(start, end, step=1)
```

```
>>> range(5)
range(0, 5)

>>> list(range(10))
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

>>> list(range(0, 10, 2))
[0, 2, 4, 6, 8]
```

#### zip

定义：zip([iterable, ...])

zip()是Python的一个内建函数，它接受一系列可迭代的对象作为参数，将对象中对应的元素打包成一个个tuple（元组），然后返回由这些tuples组成的list（列表）。若传入参数的长度不等，则返回list的长度和参数中长度最短的对象相同。利用*号操作符，可以将list unzip（解压），看下面的例子就明白了：


```
>>> a = [1,2,3]
>>> b = [4,5,6]
>>> c = [4,5,6,7,8]
>>> zipped = zip(a,b)
[(1, 4), (2, 5), (3, 6)]
>>> zip(a,c)
[(1, 4), (2, 5), (3, 6)]
>>> zip(*zipped)
[(1, 2, 3), (4, 5, 6)]
```

### pass

空语句。

如果你在需要子语句块的地方不写任何语句, 解释器会提示你 语法错误. 因此, Python 提供了 pass 语句, 它不做任何事情 - 即 NOP , ( No OPeration , 无操作) 我们从汇编语言中借用这个概念. pass 同样也可作为开发中的小技巧, 标记你后来要完成的代码

### break

Python 中的 break 语句可以结束当前循环然后跳转到下条语句, 类似 C 中的传统 break . 常用在当某个外部条件被触发(一般通过 if 语句检查), 需要立即从循环中退出时. break 语句可以用在 while 和 for 循环中.

### continue

不管是 Python, C, Java 还是其它任何支持 continue 语句的结构化语言中, 一些初学者有这样的一个误解: continue 语句"立即启动循环的下一次迭代". 实际上, 当遇到 continue 语句时, 程序会终止当前循环, 并忽略剩余的语句, 然后回到循环的顶端. 在开始下一次迭代前, 如果是条件循环, 我们将验证条件表达式. 如果是迭代循环, 我们将验证是否还有元素可以迭代. 只有在验证成功的情况下, 我们才会开始下一次迭代.

Python 里的 continue 语句和其他高级语言中的传统 continue 并没有什么不同. 它可以被用在 while 和 for 循环里. while 循环是条件性的, 而 for 循环是迭代的, 所以 continue 在开始下一次循环前要满足一些先决条件, 否则循环会正常结束.


```
>>> for s in 'hello':
...  if s == 'l':
...     continue
...  print(s)
... 
h
e
o
```

## 推导式
推导式是从一个或者多个迭代器快速简洁地创建数据结构的一种方法。他可以讲循环和条件判断结合，从而避免语法冗长的代码。会使用推导式有时可以说明你已经超过 Python初学者的水平。也就是说，使用推导式更像 Python 风格


### 列表推导式

你可以从1到5创建一个整数列表，每次增加一项：

```
>>> number_list = []
>>> number_list.append(1)
>>> number_list.append(2)
>>> number_list.append(3)
>>> number_list.append(4)
>>> number_list.append(5)
>>> number_list
[1, 2, 3, 4, 5]
```

或者，可以结合 range() 函数使用一个迭代器：

```
>>> number_list = []
>>> for number in range(1, 6):
...     number_list.append(number)
...
>>> number_list
[1, 2, 3, 4, 5]
```
上面这些方法都是可行的Python代码，会得到相同的结果。然而，更像 Python 风格的创建列表方法是列表推导。语法如下：

```
[ expression for item in iterable ]
```

将通过列表推导创建一个整数列表：

```
>>> number_list = [number for number in range(1,6)]
>>> number_list
[1, 2, 3, 4, 5]
```
在第一行中，第一个 number 变量为列表生成值，也就是说，把循环的结果放在列表 number_list 中。 第二个 number 可以为表达式， 看下下面的例子：

```
>>> number_list = [number-1 for number in range(1,6)]
>>> number_list
[0, 1, 2, 3, 4]
```

列表推到把循环放在方括号内部。这种例子和之前碰到的不大一样，但却是更为常见的方式。同样，列表推导也可以像下面的例子加上条件表达式：

```
[expression for item in iterable if condition]
```
现在，通过推导创建一个在1到5之间的偶数列表（当 number % 2 为真时，代表奇数；为假时代表偶数）：

```
>>> a_list = [number for number in range(1,6) if number % 2 == 1]
>>> a_list
[1, 3, 5]
```

正如很多嵌套循环一样，在对应的推导中会有多个for语句，我们先来看一个简单的嵌套循环例子：

```
>>> rows = range(1,4)
>>> cols = range(1,3)
... for row in rows:
...     for col in cols:
...         print(row, col)
...
1 1
1 2
2 1
2 2
3 1
3 2
```
使用一次推导，将结果赋值给变量 cells，使 row，col 成为元组：

```
>>> rows = range(1,4)
>>> cols = range(1,3)
>>> cells = [(row, col) for row in rows for col in cols]
>>> for cell in cells:
...     print(cell)
...
(1, 1)
(1, 2)
(2, 1)
(2, 2)
(3, 1)
(3, 2)
>>>
```
另外，在对 cells 列表进行迭代时可以通过元组拆封将变量 row 和 col 的值分别取出：

```
>>> for row, col in cells:
...     print(row, col)
...
1 1
1 2
2 1
2 2
3 1
3 2
```
其中，列表推导中 for row ...和 for col ...都可以有自己单独的 if 条件判断。


```
>>> rows = range(1,4)
>>> cols = range(1,3)
>>> cells = [(row, col) for row in rows if row % 2 == 1  for col in cols if col % 2 == 1]
>>> cells
[(1, 1), (3, 1)]
>>>
```



### 字典推导式

### 集合推导式

集合也不例外，同样有推导式。最简单的版本和之前的列表、字典推导类似：

```
{expression for expression in iterable }
```

也可以使用条件判断：

```python
>>> a_set = {number for number in range(1,6) if number % 3 == 1}
>>> a_set
{1, 4}
```
# id is ==
Python中的对象包含三要素：id、type、value
其中id返回一个对象的唯一标识，type标识对象的类型，value是对象的值
is判断的是a对象是否就是b对象，是通过id来判断的
==判断的是a对象的值是否和b对象的值相等，是通过value来判断的
如下代码或许可以帮助你理解。


```
>>> a = 1
>>> b = 1.0
>>> a is b
False
>>> a == b
True
>>> id(a)
12777000
>>> id(b)
14986000
>>> a = 1
>>> b = 1
>>> a is b
True
>>> a == b
True
>>> id(a)
12777000
>>> id(b)
12777000
```

> 就 CPython 而言，id 返回的就是运行期内存地址。因此这个标识属阶段性的，不能保证不被重复使用。 但对于其他实现来说，id 返回的未必就是内存地址。

# 4. 函数

代码复用的第一步是使用函数，它是命名的用于区分的代码段。函数可以接受任何数字或者其他类型的输入作为参数，并且返回数字或者其他类型的结果。

你可以使用函数做一下两件事情：

* 定义函数
* 调用函数

## 定义：
语句 def 在运行期创建函数对象，并与指定名字关联。

```
def func_name():
    pass  # 写入你的逻辑
```

## 参数
传入到函数的值称为参数。当调用含参数的函数时，这些参数的值会被复制给函数中的对应参数。

```
>>> def get_name(num):
...     if num == 1:
...         return '老大'
...     elif num == 2:
...         return '老二'
...
...
>>> name = get_name(1)
>>> name
'老大'
```
这个函数做了如下事情：

* 把 1 赋值给函数的内部参数 num
* 运行 if-elif 的逻辑链
* 返回一个字符串
* 将该字符串赋值给变量 name

一个函数可以接受任何数量（包括0）的任何类型的值作为输入变量，并且返回任何数量（包括0）的任何类型的结果。如果函数不显示调用 return 函数，那么会默认返回 None。

```
>>> def func_name():
...     pass
...
>>> print(func_name())
None
>>>
```

**None**

None 是 Python 中一个特殊的值，虽然它不表示任何数据，但仍然具有重要的作用。 虽然 None 作为布尔值和 False 是一样的，但是它和 False 有很多差别。下面是一个例子:

```python
>>> thing = None
>>> if thing:
...     print("It's some thing")
... else:
...     print("It's no thing")
...
It's no thing
```

为了区分 None 和布尔值 False , 使用 Python 的 is 操作符:

```python
>>> if thing is None:
...     print("It's nothing")
... else:
...     print("It's something")
...
It's nothing
```

这虽然是一个微妙的区别，但是对于 Python 来说是很重要的。你需要把 None 和不含 任何值的空数据结构区分开来。0 值的整型 / 浮点型、空字符串('')、空列表([])、 空元组((,))、空字典({})、空集合(set())都等价于 False，但是不等于 None。
现在，快速写一个函数，输出它的参数是否是 None:

```python
>>> def is_none(thing):
...    if thing is None:
...        print("It's None")
...    elif thing:
...        print("It's True")
...    else:
...        print("It's False")
```

现在，运行一些测试函数:

```python
>>> is_none(None)
It's None
>>> is_none(True)
It's True
>>> is_none(False)
It's False
>>> is_none(0)
It's False
>>> is_none(0.0)
It's False
>>> is_none(())
It's False
>>> is_none([])
It's False
>>> is_none({})
It's False
>>> is_none(set())
It's False
```

## 位置参数
Python 处理参数的方式要比其他语言更加灵活。其中，最熟悉的参数类型是位置参数，传入参数的值是按照顺序依次复制过去的。

```
>>> def name (n1, n2, n3):
...     print('1', n1)
...     print('2', n2)
...     print('3', n3)
...
>>> name('老大', '老二', '老三')
1 老大
2 老二
3 老三
>>>
```

尽管这种方式很常见，但是位置参数的一个弊端是必须熟记没个位置的参数的含义。在调用函数name() 时误把最后一个参数当做第一个参数，会得到完全不同的结果：

```
>>> name('老二', '老大', '老三')
1 老二
2 老大
3 老三
>>>
```
## 关键字参数
为了避免位置参数带来的混乱，调用参数时可以指定对应的名字，甚至可以采用与函数定义不同的顺序调用：

```
>>> name(n2='老二', n1='老大', n3='老三')
1 老大
2 老二
3 老三
>>>
```
你也可以把位置参数和关键字参数混合起来。

```
>>> name('老大', n2='老二', n3='老三')
1 老大
2 老二
3 老三
```

如果同时出现两种参数形式，首先应该考虑的是位置参数。


```
>>> name(n2='老二', '老大', n3='老三')
  File "<ipython-input-60-2a320a67eb2a>", line 1
    name(n2='老二', '老大', n3='老三')
                     ^
SyntaxError: positional argument follows keyword argument
```
## 指定默认参数
当调用方没有提供对应的参数值时，你可以指定默认参数值。


```
>>> def name (n2, n3, n1='老大'):
...     print('1', n1)
...     print('2', n2)
...     print('3', n3)
...
...
>>> name('老二', '老三')
1 老大
2 老二
3 老三
>>>
```
> 默认参数值在函数被定义时已经计算出来，而不是在程序运行时。Python程序员经常犯的一个错误是把可变的数据类型（例如列表或者字典）当做默认参数值。

在函数 box() 在每次调用时，添加参数 arg 到一个空的列表 result, 然后打印输出一个单值列表。但存在一个问题：只有在第一次调用时列表是空的，第二次调用时就会存在之前调用的返回值：

```
>>> def box(arg, result=[]):
...     result.append(arg)
...     print(result)
...
>>> box('a')
['a']
>>> box('b')
['a', 'b']
```

如果写成下面的样子就会解决刚才的问题：

```
>>> def box(arg):
...     result = []
...     result.append(arg)
...     return result
...
>>> box('a')
['a']
>>> box('b')
['b']
```
这样的修改也是为了表明第一次调用跳过一些操作：

```
>>> def box(arg, result=None):
...     if result is None:
...         result = []
...     result.append(arg)
...     print(result)
...
>>> box('a')
['a']
>>> box('b')
['b']
```

# 迭代器
在Python中，很多对象都是可以通过for语句来直接遍历的，例如list、string、dict等等，这些对象都可以被称为可迭代对象。至于说哪些对象是可以被迭代访问的，就要了解一下迭代器相关的知识了。

迭代器是一个可以记住遍历的位置的对象。迭代器对象要求支持迭代器协议的对象，在Python中，支持迭代器协议就是实现对象的__iter__()和next()方法。其中__iter__()方法返回迭代器对象本身；next()方法返回容器的下一个元素，在结尾时引发StopIteration异常。

迭代器有两个基本的方法：

```
__iter__()和next()方法
```

这两个方法是迭代器最基本的方法，一个用来获得迭代器对象，一个用来获取容器中的下一个元素。
对于可迭代对象，可以使用内建函数iter()来获取它的迭代器对象：

```
>>> l = [1,2,3,4]
>>> it = iter(l)
>>> it
<list_iterator object at 0x10715dcf8>
>>> next(it)
1
>>> next(it)
2
>>> next(it)
3
>>> next(it)
4
>>> next(it)
Traceback (most recent call last):
  File "<ipython-input-60-2cdb14c0d4d6>", line 1, in <module>
    next(it)
StopIteration
```
例子中，通过iter()方法获得了list的迭代器对象，然后就可以通过next()方法来访问list中的元素了。当容器中没有可访问的元素后，next()方法将会抛出一个StopIteration异常终止迭代器。

其实，当我们使用for语句的时候，for语句就会自动的通过__iter__()方法来获得迭代器对象，并且通过next()方法来获取下一个元素。


# 生成器
生成器是用来创建 Python 序列的一个对象。使用它可以迭代庞大的序列，且不需要再内存中创建和存储整个序列。通常，生成器是为迭代器产生数据的。（生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器。）我们已经在之前学习过其中一个，就是 range()，来产生一系列整数。range()在 python 2 中返回一个列表，这也限制了它要进入内存空间。Python 2 中同样存在的生成器 xrange() 在 Python 3中成为标准的 range() 生成器。下面的例子累加从 1 到 100 的整数：

```
>>> sum(range(1, 101))
5050
```
每次迭代生成器，它会记录上一次调用的位置，并且返回下一个值。这一点和普通的函数是不一样的，一般函数都不记录之前一次调用，而且都会在函数的第一行开始执行。

如果你想创建一个比较大的序列，使用生成器推导的代码会很长，这是可以尝试写一个生成器函数。生成器函数和普通函数类似，但是它的返回值使用 yield 语句声明, 不是 return。下面编写我们自己的 range() 函数版本：

```
>>> def my_range(first=0, last=10, step=1):
...     number = first
...     while number < last:
...         yield number
...         number += step
```

这是一个普通的函数：

```
>>> my_range
<function my_range at 0x1070c4e18>
```

并且它返回的是一个生成器对象

```
>>> ranger = my_range(1, 5)
>>> ranger
<generator object my_range at 0x1070b5eb8>
>>>
```

可以对这个生成器对象进行迭代：

```
>>> for x in ranger:
...     print(x)
...
1
2
3
4
```
在调用生成器运行的过程中，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回yield的值。并在下一次执行 next()方法时从当前位置继续运行。

# 装饰器
有时你需要在不改变源代码的情况下修改已经存在的函数。常见的例子是增加一句调试声明，已查看传入的参数。

装饰器本质上是一个函数。它把一个函数作为输入并且返回另一个函数。在装饰器中，通常使用下面这些 Python 技巧：

* *args 和 **kwargs
* 闭包
* 作为参数的函数

函数 document_it() 定义了一个装饰器，会实现如下功能：

* 打印输出函数的名字和参数的值
* 执行含有参数的函数
* 打印输出结果
* 返回修改后的函数

看下面代码：

```python
>>> def document_it(func):
...     def new_function(*args, **kwargs):
...         print('Running function:', func.__name__)
...         print('Positional arguments:', args)
...         print('Keyword arguments:', kwargs)
...         result = func(*args, **kwargs)
...         print('Result:', result)
...         return result
...     return new_function
...
```
无论传入 document_it() 函数 func 是什么，装饰器都会返回一个新的函数，其中包含函数 document_it() 增加的额外语句。事实上，装饰器并不需要执行函数 func 中的代码，只是在结束前函数 document_it() 调用函数 func 以便得到 func 的返回结果和附加代码的结果。

那么，如何使用装饰器？当然，可以通过人工赋值：

```
>>> def add_ints(a, b):
...     return a + b
...
>>> add_ints(3, 5)
8
>>> cooler_add_ints = document_it(add_ints) # 人工对装饰器赋值
>>> cooler_add_ints(3, 5)
Running function: add_ints
Positional arguments: (3, 5)
Keyword arguments: {}
Result: 8
8
```

```
@decorator
def func():
    pass
```

其解释器会解释成下面这样的语句：

```
func = decorator(func)
```

作为对前面人工装饰器赋值的替代，可以直接再要装饰的函数前添加装饰器名字 @decorator_name:

```
>>> @document_it
... def add_ints(a, b):
...     return a + b
...
>>> add_ints(3, 5)
Running function: add_ints
Positional arguments: (3, 5)
Keyword arguments: {}
Result: 8
8
```
同样一个函数可以有多个装饰器。下面，我们写一个对结果求平方的装饰器 square_it():

```python
>>> def square_it(func):
...     def new_function(*args, **kwargs):
...         result = func(*args, **kwargs)
...         return result * result
...     return new_function
...
```
靠近函数定义（def 上面）的装饰器最先执行，然后一次执行上面的。任何顺序都会得到相同的最终结果。下面的例子中会看到中间步骤的变化：

```
>>> @document_it
... @square_it
... def add_ints(a, b):
...     return a + b
...
>>> add_ints(3, 5)
Running function: new_function
Positional arguments: (3, 5)
Keyword arguments: {}
Result: 64
64
```

交换两个装饰器的顺序：

```
>>> @square_it
... @document_it
... def add_ints(a, b):
...     return a + b
...
>>> add_ints(3, 5)
Running function: add_ints
Positional arguments: (3, 5)
Keyword arguments: {}
Result: 8
64
```




# 命名空间和作用域

# 5. [类](http://opslinux.com/2017/02/25/5-%E7%B1%BB/)


# 6. [文件异常](http://opslinux.com/2017/02/15/6-%E6%96%87%E4%BB%B6%E4%B8%8E%E5%BC%82%E5%B8%B8/)



# 7. 模块

# 8.标准库
Python 的一个显著特点是具有庞大的模块标准库，这些模块可以执行很多有用的任务，并且和核心 Python 语言分开以避免臃肿。我们开始写代码时，首先要检查是否存在想要的标准模块。在标准库中你会经常碰到一些"珍宝"！

关于标准库的学习资料：

* python 标准库
* [python cookbook](http://python3-cookbook.readthedocs.io/zh_CN/latest/index.html)
* 模块官方文档 https://docs.python.org/3/library 以及使用指南 https://docs.python.org/3.3/tutorial/stdlib.html
* Doug Hellmann 的网站 Python Module of the week（[https://pymotw.com/3/](https://pymotw.com/3/)）

一般标准库里涉及数据结构、网络、系统、数据库等模块。下面来学习一些常用的标准模块。



## 使用 setdefault() 和 defaultdic() 处理缺失的键 

## 使用 Counter() 计数

## 使用有序字典 OrderedDict() 按键排序

## 双端队列：栈+队列

## 使用 itertools 迭代代码结构

## 使用 pprint() 友好输出

## argparse 命令行解析

python中的命令行解析最简单最原始的方法是使用sys.argv来实现，更高级的可以使用argparse这个模块。argparse从python 2.7开始被加入到标准库中，所以如果你的python版本还在2.7以下，那么需要先手动安装。




# 基础课实战

密码管理器

* 主密码(retry 3次 给一个提示) 加密
* 保存密码
    * 密码 title
    * 真实的密码 （加密）
* 自动生成安全密码 （用户可以输入长度）
* 密码分类
    * web
    * 应用
    * 银行
    * 服务器
* 获取密码
* 密码文件的云备份   


# 9.第三方库

## 运维常用的第三方库

# psutil
系统性能信息模块
psutil是一个跨平台库，能够轻松的实现获取系统运行的进程和系统利用率（包括CPU、内存、磁盘、网络等）信息。它主要应用于系统监控，分析和限制系统资源及进程的管理。它实现了同等命令行工具提供的功能，如ps、top、lsof、netstat、ifconfig、who、df、kill、free、nice、ionice、iostat、iotop、uptime、pidof、tty、taskset、pmap等。目前支持32位和64位的Linux、Windows、OS X、FreeBSD和Sun Solaris等操作系统，支持从2.6 到 3.5的Python版本，目前最新版本为5.1.3。

项目地址：https://github.com/giampaolo/psutil

## 安装

```
pip install psutil
```
## CPU

```
#显示所有逻辑CPU信息。
psutil.cpu_times(percpu=True)
#获取CPU的逻辑个数
psutil.cpu_count()
#获取物理cpu个数
psutil.cpu_count(logical=False)

```

## Memory

```
#内存信息
psutil.virtual_memory()
#获取内存总数
psutil.virtual_memory().total
#获取空闲内存数
psutil.virtual_memory().free
#获取swap信息
psutil.swap_memory()
```

## Disks

```
#磁盘信息
psutil.disk_partitions(all=True)
#获取分区参数
psutil.disk_usage('/')
#获取磁盘IO个数
psutil.disk_io_counters()
```

## Network

```
#获取网络总的IO信息
psutil.net_io_counters()
#获取每个网络接口的io信息
psutil.net_io_counters(pernic=True)
```

## Other system info

```
#返回当前用户登录信息
psutil.users()
#获取开机时间（以时间戳的方式）
psutil.boot_time()
#让人能看懂
import datetime
datetime.datetime.fromtimestamp(psutil.boot_time()).strftime("%Y-%m-%d %H:%M:%S")
```

## Process management

```
# 进程信息
psutil.pids() #获取所有进程的PID
p = psutil.Process(2128) #实例化一个Process对象，参数为一个进程PID
p.name() #进程名
p.exe() #经常bin路径
p.cwd() #进程工作目录路径
p.status() #进程状态
p.create_time() #进程创建时间，时间戳格式
p.uids() #进程uid信息
p.gids() #进程gid信息
p.cpu_times() #进程cpu时间信息，包括use,system两个时间
p.cpu_affinity() #get进程CPU亲合度，如果设置了进程cpu的亲和度，降CPU号作为参数即可
p.memory_percent() #进程内存利用率
p.memory_info() #进程内存rss.vms信息
p.io_counters() #进程IO信息，包括读写IO数及字节数
p.connections() #返回打开进程socket的namedutples列表，包括fs、family、laddr等信息
p.num_threads() #进程开启的线程数

#popen类的使用
from subprocess import PIPE
#通过psutil的popen方法启动的应用程序，可以跟踪程序运行的所有相关信息
p = psutil.Popen(['/usr/bin/python','-c','print ("hello")'],stdout=PIPE)
p.name()
p.username()
p.communicate()
p.cpu_times()
```


# Ipy
IP地址处理模块
IP地址规划是网络设计中非常重要的一个环节，规划的好坏会直接影响路由协议算法的效率，包括网络性能、可扩展性等方面，在这个过程当中，免不了要计算大量的IP地址，包括网段、网络掩码、广播地址、子网数、IP类型等。

官方地址： https://github.com/autocracy/python-ipy

## 安装

版本号 0.83

```
pip install IPy
```

IPy模块包含IP类，使用它可以方便处理绝大部分格式为IPv6及IPv4的网络和地址。比如通过version方法就可以区分出IPv4与IPv6，如：

```
from IPy import IP
IP('192.168.1.0/24').version()
IP('::1').version()

ip = IP('192.168.0.0/16')
ip.len() #输出192.168.0.0网段的ip个数
for x in ip:
    print(x)

test_ip = IP('192.168.17.8')
test_ip.reverseName() #反向解析地址格式
test_ip.iptype() #192.168.17.8为私网类型'PRIVATE'
IP('8.8.8.8').iptype() #8.8.8.8 为公网类型
IP('8.8.8.8').int() #转换为整形格式
IP('8.8.8.8').strHex() #转换为16进制
IP('8.8.8.8').strBin() #转换为2进制
IP(0x8080808) #16进制转换成IP格式
```

IP方法也支持网络地址的转换，例如根据IP与掩码生产网段格式，如下：

```
>>> from IPy import IP
>>> IP('192.168.1.0').make_net('255.255.255.0')
IP('192.168.1.0/24')
>>> IP('192.168.1.0/255.255.255.0', make_net=True)
IP('192.168.1.0/24')
>>> IP('192.168.1.0-192.168.1.255', make_net=True)
IP('192.168.1.0/24')
```
也可以通过strNormal方法指定不同wantprefixlen参数值以定制不同输出类型的网段。输出类型为字符串，如下：

```
>>>IP('192.168.1.0/24').strNormal(0)
'192.168.1.0'
>>>IP('192.168.1.0/24').strNormal(1)
'192.168.1.0/24'
>>>IP('192.168.1.0/24').strNormal(2)
'192.168.1.0/255.255.255.0'
>>>IP('192.168.1.0/24').strNormal(3)
'192.168.1.0-192.168.1.255'
```

示例　根据输入的IP或子网返回网络、掩码、广播、反向解析、子网数、IP类型等信息。

```
#!/usr/bin/env python
#-*- coding:utf-8 -*-

from IPy import IP

ip_s = input('请输入一个ip或 网段地址：')
ips = IP(ip_s)
if len(ips) > 1:
    print('net: %s' % ips.net()) #输出网络地址
    print('netmask: %s' % ips.netmask()) #输出网络地址掩码
    print('broadcast: %s' % ips.broadcast()) #输出网络广播地址
    print('reverse adress: %s' % ips.reverseName()[0]) #输出地址反向解析
    print('subnet: %s' %len(ips)) #输出网络子网数
else: #为单个ip地址
    print('reverse adress: %s' % ips.reverseName()[0])
print('十六进制地址：%s' % ips.strHex())
print('二进制地址：%s' % ips.strBin())
print('IP地址类型：%s' % ips.iptype())
```



# dnspython
DNS处理模块
dnspython（http://www.dnspython.org/） 是Python实现的一个DNS工具包，它支持几乎所有的记录类型，可以用于查询、传输并动态更新ZONE信息，同时支持TSIG（事务签名）验证消息和EDNS0（扩展DNS）。在系统管理方面，我们可以利用其查询功能来实现DNS服务监控以及解析结果的校验，可以代替nslookup及dig等工具，轻松做到与现有平台的整合。

项目地址：https://github.com/rthalley/dnspython

安装： 

```
pip install dnspython
```
最新版本 dnspython-1.15.0

dnspython模块提供了大量的DNS处理方法，最常用的方法是域名查询。dnspython提供了一个DNS解析器类—resolver，使用它的query方法来实现域名的查询功能。query方法的定义如下：

```
query(self, qname, rdtype=1, rdclass=1, tcp=False, source=None, raise_on_no_answer=True, source_port=0)
```
* qname参数为查询的域名。
* rdtype参数用来指定RR资源的类型，常用的有以下几种：
    * A记录，将主机名转换成IP地址；
    * MX记录，邮件交换记录，定义邮件服务器的域名；
    * CNAME记录，指别名记录，实现域名间的映射；
    * NS记录，标记区域的域名服务器及授权子域；
    * PTR记录，反向解析，与A记录相反，将IP转换成主机名；
    * SOA记录，SOA标记，一个起始授权区的定义。
* rdclass参数用于指定网络类型，可选的值有IN、CH与HS，其中IN为默认，使用最广泛。tcp参数用于指定查询是否启用TCP协议，默认为False（不启用）。
* source与source_port参数作为指定查询源地址与端口，默认值为查询设备IP地址和0。
* raise_on_no_answer参数用于指定当查询无应答时是否触发异常，默认为True。

## A记录查询

```
#!/usr/bin/env python
#-*- coding:utf-8 -*-

import dns.resolver
domain = raw_input('Please input an domain: ')    #输入域名地址
A = dns.resolver.query(domain, 'A')    #指定查询类型为A记录
for i in A.response.answer:   #通过response.answer方法获取查询回应信息
        print(i)
```

## mx记录查询

```
#!/usr/bin/env python
#-*- coding:utf-8 -*-

import dns.resolver
domain = raw_input("Please input an domain:")
MX = dns.resolver.query(domain,'MX')#指定查询记录类型为mx
for i in MX:
    print（'MX preference=', i.preference, ' mail exchanger=', i.exchange）
```

## NS记录查询
只限输入一级域名，baidu.com。

```
#!/usr/bin/env python
#-*- coding:utf-8 -*-

import dns.resolver
domain = raw_input("Please input an domain:")
ns = dns.resolver.query(domain,'NS') #指定查询类型为NS记录
for i in ns.response.answer:
    for j in i.items:
        print j.to_text()
```

CNAME记录查询

```
#!/usr/bin/env python
#-*- coding:utf-8 -*-

import dns.resolver
domain = raw_input("Please input an domain:")
cname = dns.resolver.query(domain,'CNAME') #指定查询类型为CNAME记录
for i in cname.response.answer:
    for j in i.items:
        print j.to_text()
```
结果返回cname后的目标域名。

## DNS域名轮询业务监控

```
#!/usr/bin/env python
# coding=utf-8

import dns.resolver
# import httplib
import http.client

iplist = []  # 定义域名IP列表变量
appdomain = "jingfengjiaoyu.com"  # 定义业务域名

def get_iplist(domain=""):  # 域名解析函数，解析成功IP将被追加到iplist
    try:
        A = dns.resolver.query(domain, 'A')  # 解析A记录类型
    except Exception as e:
        print("dns resolver error:" + str(e))
        return
    for i in A.response.answer:
        for j in i.items:
            iplist.append(j.address)  # 追加到iplist
    return True


def checkip(ip):
    checkurl = ip + ":80"
    getcontent = ""
    http.client.socket.setdefaulttimeout(5)  # 定义http连接超时时间(5秒)
    conn = http.client.HTTPConnection(checkurl)  # 创建http连接对象

    try:
        conn.request("GET", "/", headers={"Host": appdomain})  # 发起URL请求，添
        # 加host主机头
        r = conn.getresponse()
        getcontent = r.read(7)  # 获取URL页面前15个字符，以便做可用性校验
        print(getcontent)
    finally:
        if getcontent == "<html>":  # 监控URL页的内容一般是事先定义好的，比如 “HTTP200”等
            print(ip + " [OK]")
        else:
            print(ip + " [Error]")  # 此处可放告警程序，可以是邮件、短信通知


if __name__ == "__main__":
    if get_iplist(appdomain) and len(iplist) > 0:  # 条件：域名解析正确且至少返回一个IP
        for ip in iplist:
            checkip(ip)
    else:
        print("dns resolver error.")

```

# paramiko
模仿ssh登录执行命

官方网站：http://www.paramiko.org/
项目网站：https://github.com/paramiko/paramiko

## 安装

```
pip install paramiko
```
paramiko-2.1.2

## 连接服务器

### 用户名和密码连接

```
import paramiko
ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy()) #作用是允许连接不在know_hosts文件中的主机
ssh.connect('139.199.228.59', 22, 'root', '!@#)(*WGK')
stdin,stdout,stderr = ssh.exec_command('df -h\nls')
stdout.read()
ssh.close()
```

### 上传和下载文件

```
import paramiko
import os,sys
t = paramiko.Transport(('192.168.17.248',22))
t.connect(username='root',password='123456')
sftp = paramiko.SFTPClient.from_transport(t)
#上传
sftp.put('D:\log.conf','/tmp/log.conf')
#下载
sftp.get('/tmp/ks-script-mZm5Oi','D:\ks-script-mZm5Oi')
t.close()
```

## 通过公私钥免密码SSH连接服务器

#公私钥生成

```
ssh-keygen -t rsa  # 生成密钥
ssh-copy-id -i ~/ssh/id_rsa.pub root@192.168.17.258  # 将本机的公钥复制到远程机器的authorized_keys文件中，ssh-copy-id也能让你有到远程机器的home, ~./ssh , 和 ~/.ssh/authorized_keys的权利

import paramiko

private_key_path = '/home/auto/.ssh/id_rsa'
key = paramiko.RSAKey.from_private_key_file(private_key_path)

ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect('192.168.17.258 ', 22, 'root', key)

stdin, stdout, stderr = ssh.exec_command('df')
print stdout.read()
ssh.close();
```

paramiko sftp —— SSH上传和下载文件

```
import paramiko
#建立一个加密的管道
scp=paramiko.Transport(('192.168.0.102', 22))
#建立连接
scp.connect(username='root',password='361way')
#建立一个sftp客户端对象，通过ssh transport操作远程文件
sftp=paramiko.SFTPClient.from_transport(scp)
#Copy a remote file (remotepath) from the SFTP server to the local host
sftp.get('/root/testfile','/tmp/361way')
#Copy a local file (localpath) to the SFTP server as remotepath
sftp.put('/root/crash-6.1.6.tar.gz','/tmp/crash-6.1.6.tar.gz')
scp.close()
```
一个目录下多个文件上传下载的示例：bx

```
#!/usr/bin/env python
#-*- coding: utf-8 -*-
import paramiko,datetime,os
hostname='139.199.228.59'
username='root'
password='!@#)(*WGK1'
port=22
local_dir='/tmp/getfile'
remote_dir='/tmp/abc'
try:
    t=paramiko.Transport((hostname,port))
    t.connect(username=username,password=password)
    sftp=paramiko.SFTPClient.from_transport(t)
    files=sftp.listdir(remote_dir)
    for f in files:
        print ''
        print '#########################################'
        print 'Beginning to download file  from %s  %s ' % (hostname,datetime.datetime.now())
        print 'Downloading file:',os.path.join(remote_dir,f)
        sftp.get(os.path.join(remote_dir, f),os.path.join(local_dir, f))#下载
        #sftp.put(os.path.join(local_dir, f),os.path.join(remote_dir, f))#上传
        print 'Download file success %s ' % datetime.datetime.now()
        print ''
        print '##########################################'
    t.close()
except Exception:
       print "connect error!"  
```

注：本处的目录下所有文件进行下载或上传的示例中，在遇到目录下还有嵌套的目录存在时，会将目录也当做文件进行处理，所以如果想要更加的完美的话，可以通过引入stat模块下的S_ISDIR方法进行处理



# pexpect
pexpect 是linux下expect的python封装，通过pexpect我们可以实现对ssh、ftp、password、telnet等命令进行自动交互，无需人工达到自动化需求。

项目地址：https://github.com/pexpect/pexpect

安装：

```
pip install pexpect
```

## 登陆脚本
```
#!/usr/bin/env python
# coding=utf-8


import pexpect 

username = 'root'
ip = '139.199.228.59'
child.interact()mypassword = '!@#)(*WGK1'

child = pexpect.spawn('ssh {0}@{1}'.format(username, ip))
child.expect ('password:')
child.sendline (mypassword)
child.expect('$')
child.sendline('sudo -s')
child.expect (':')
child.sendline (mypassword)
child.expect('#')
child.sendline('ls -la')
child.expect('#')
print child.before   # Print the result of the ls command.
child.interact()     # Give control of the child to the user.
child.close()
```




# fabric
Fabric是基于python2.5及以上版本实现的SSH命令行工具，简化了SSH应用程序部署及系统管理任务，他提供了系统基础的操作组件，可以实现本地或远程shell命令，包括命令执行、文件上传、下载及完整执行日志输出等功能。Fabric在paramiko的基础上做更高一层的封装，操作起来更加简单。
官网地址：http://www.fabfile.org/
项目地址：https://github.com/fabric/fabric/

2.0 支持 python3

fab的常用参数

```
Usage: fab [options] <command>[:arg1,arg2=val2,host=foo,hosts='h1;h2',...] ...
```

* -l,显示定义好的任务函数名；
* -f,指定fab入口文件，默认入口文件名为fabfile.py
* -g,指定网关（中转）设备，比如堡垒机环境，填写堡垒机ip即可
* -H,指定目标主机，多台主机用“，”号分隔；
* -P,以异步并行方式运行多主机任务，默认为串行运行；
* -R,指定role，以角色名区分不同业务组设备；
* -t，设置设备连接超时时间（秒）；
* -T,设置远程主机命令执行超时时间（秒）；
* -w,当命令执行失败，发出告警，而非默认终止任务。

有时候我们可已不需要写一行python代码也可以完成远程操作，直接使用命令的形式，例如

```
fab -p 123456(密码) -H 192.168.1.21,192.168.1.22 -- 'uname -s'
```

## fabric的环境变量

fabric的环境变量有很多，存放在一个字典中， fabric.state.env，而它包含在fabric.api中。 为了方便，我们一般使用env来指代环境变量。 env环境变量可以控制很多fabric的行为，一般通过env.xxx可以进行设置。

fabric默认使用本地用户通过ssh进行连接远程机器，不过你可以通过env.user变量进行覆盖。 当你进行ssh连接时，fabric会让你交互的让你输入远程机器密码，如果你设置了env.password变量，则就不需要交互的输入密码。 下面介绍一些常用的环境变量：

* abort_on_prompts 设置是否运行在交互模式下，例如会提示输入密码之类，默认是false
* connection_attempts fabric尝试连接到新服务器的次数，默认1次
* cwd 目前的工作目录，一般用来确定cd命令的上下文环境
* disable_known_hosts 默认是false，如果是true，则会跳过用户知道的hosts文件
* exclude_hosts 指定一个主机列表，在fab执行时，忽略列表中的机器
* fabfile 默认值是fabfile.py在fab命令执行时，会自动搜索这个文件执行。
* host_string 当fabric连接远程机器执行run、put时，设置的user/host/port等
* hosts 一个全局的host列表
* keepalive 默认0 设置ssh的keepalive
* loacl_user 一个只读的变量，包含了本地的系统用户，同user变量一样，但是user可以修改
* parallel 默认false，如果是true则会并行的执行所有的task
* pool_size 默认0 在使用parallel执行任务时设置的进程数
* password ssh远程连接时使用的密码，也可以是在使用sudo时使用的密码
* passwords 一个字典，可以为每一台机器设置一个密码，key是ip，value是密码
* path 在使用run/sudo/local执行命令时设置的$PATH环境变量
* port 设置主机的端口
* roledefs 一个字典，设置主机名到规则组的映射
* roles 一个全局的role列表
* shell 默认是/bin/bash -1 -c 在执行run命令时，默认的shell环境
* skip_bad_hosts 默认false，为ture时，会导致fab跳过无法连接的主机
* sudo_prefix 默认值"sudo -S -p '%(sudo_prompt)s' " % env 执行sudo命令时调用的sudo环境
* sudo_prompt 默认值"sudo password:"
* timeout 默认10 网络连接的超时时间
* user ssh使用哪个用户登录远程主机
* local 执行本地命令，如： local('uname -s')
* lcd 切换本地目录，如： lcd('/home')
* cd 切换远程目录，如： cd('/data/logs')
* run 执行远程命令 如： run('free -m')
* sudo sudo方式执行命令，如sudo('/etc/init.d/httpd start')
* put 上传本地文件到远程主机，如put('/home/user.info',/data/user.info'')
* get 从远程主机下载文件到本地，如get('/home/user.info',/data/user.info'')
* prompt 获得用户输入信息，如： prompt('please input user:')
* confirm 获得提示信息确认， 如： confirm('Tests failed. Continue[Y/N]?')
* reboot 重启远程主机，如： reboot();
* @task 函数修饰符，标识的函数为fab可调用的，非标记对fab不可见，纯业务逻辑。
* @runs_once, 函数修饰符，标识的函数只会执行一次，不会受多台主机的影响。


# 学习资源：
http://www.pythondoc.com/pythontutorial3/index.html
http://python3-cookbook.readthedocs.io/zh_CN/latest/index.html

