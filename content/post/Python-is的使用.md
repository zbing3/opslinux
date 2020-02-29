---
title: Python is的使用
tags:
  - python
date: 2014-11-25 19:00:27
---

python is是种很特殊的语法，你在其它的语言应该不会见到这样的用法.
python is主要是判断2个变量是否引用的是同一个对象，如果是的话，则返回True，否则返回False

比如:

```
a = 'abc'
b = 'abc'
```

a is b 返回True,因为变量a和b都存储了字符串'abc'对象的地址。

如果是:


```
a = 'abc'
c = 'abcd'
```

print a is c 返回False,因为变量a和c存储了字符串对象地址不一致。

