---
title: python跳出多层循环
tags:
  - python
date: 2014-11-26 16:34:33
---

今天同事问我一个python面试题，关于python跳出多层循环，原来还真没用过，网上一查还真有点意思，下面记录一下：

Python 本身没有“break n” 和“goto” 的语法，这也造成了Python 难以跳出多层（特定层数）循环。下面是几个跳出多层（特定层数）循环的tip。


## 1、自定义异常


```
class getoutofloop(Exception): pass

try:
    for i in range(5):
        for j in range(5):
            for k in range(5):
                if i == j == k == 3:
                    raise getoutofloop()
                else:
                    print i, '----', j, '----', k
except getoutofloop:
    pass
```

## 2、封装为函数return


```
def test():
    for i in range(5):
        for j in range(5):
            for k in range(5):
                if i == j == k == 3:
                    return
                else:
                    print i, '----', j, '----', k
test()
```
## 3、for ... else ... 用法

上面的两种都是只能跳出多层而不能跳出特定层数的循环，接下来的这个正是为了跳出特定层数的循环。


```
for i in range(5):
    for j in range(5):
        for k in range(5):
            if i == j == k == 3:
                break
            else:
                print i, '----', j, '----', k
        else: continue
        break
    else: continue
    break
```

else在 while和for 正常循环完成之后执行，和直接写在 while和for 之后没有区别，但是如果用break结束循环之后else就不会执行了。这也是个很新奇的做法。

才知道原来可以作为跳出多层循环用。不过要是有多次跳出不同层的循环的需求，也没辙了。

