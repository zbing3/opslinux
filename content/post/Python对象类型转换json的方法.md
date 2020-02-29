---
title: Python对象类型转换json的方法
tags:
  - python
date: 2014-11-27 14:44:10
---

有时候，在Django的model中,直接查询出来的orm对象，想直接转成json会报错：


```
TypeError is not JSON serializable
```


```
def convert_to_builtin_type(obj):
    # print 'default(', repr(obj), ')'
    # Convert objects to a dictionary of btheir representation
    d = { '__class__':obj.__class__.__name__,
          '__module__':obj.__module__,
        }
    d.update(obj.__dict__)
    return d
```

然后在函数中调用：


```
ip = Ip.objects.filter(ip=ip)
context = {'ip': list(ip)}
return HttpResponse(json.dumps(context, ensure_ascii=False, indent=4, default=convert_to_builtin_type))
```

如上先filter出ip=ip的条目保存在ip变量中，然后格式化下保存在context变量中。调用时放在default中。

如果喜欢pythonic，可以用下面lambda方式搞定：


```
return HttpResponse(json.dumps(context, ensure_ascii=False, indent=4, default=lambda o: o.__dict__))
```


