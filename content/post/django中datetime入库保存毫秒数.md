---
title: 一次Django和MariaDB中保存dateime毫秒数的斗争
date: 2015-09-23 09:41:04
categories: [Django]
tags: 
  - django
  - python

---
![](/media/14429764677979.jpg)

前段时间写私信项目，这种即时消息的系统肯定是存毫秒数的。本地环境是django1.8+mysql5.6一直这么开发屡试不爽，不料到测试线上环境的时候出现了问题。

我们的线上环境数据库竟然是MariaDB10.0.16，存入datetime类型竟然惊奇的把毫秒数去掉了。由于MariaDB和mysql同出一辙我决定深入虎穴一查究竟。

本地环境没问题那就先从本地的mysql表结构看起:
![](/media/14429729316499.jpg)
这么一看为啥大问题，接着去线上数据库看看：
![](/media/14429731170336.jpg)
神奇的事情出现了datetime竟然没有长度，一顿跟DBA部门交涉最后对方似乎听懂了我的问题，然后直接就把长度6给我加上了。我一顿流汗啊，就不能给我升级下版本？这里我要说明一下mysql从5.6以后就支持了datetime存毫秒数,MariaDB也在10.1.0以后的版本进行了支持，所以我这种抱怨也是对的，无奈跨部门沟通比较困难别人给你改了长度你就用呗。

接着我就做了测试竟然发现还是存不了毫秒，但是惊奇的发现毫秒的位数都是用0补全的，举个例子：

```
2015-09-10 13:56:01.542410  # 我想要的数据
2015-09-10 13:56:01.000000  # 现实的数据
```
现实就是这么残酷，我打开了django DEBUG模式，看到sql中的毫秒数本来就没有，那我在线上数据库使用sql带着毫秒数写入一条数据测试了一下，惊奇的发现竟然可以保存毫秒数，那意思就是说线上的数据库经过对dateime长度的修改其实是可以存毫秒数的，只是现在django不知道怎么处理的竟然没有存入。本着对自己负责任的心里，我决定看下django源码了解他是如何实现的：

```
@cached_property
def data_types(self):
   if self.features.supports_microsecond_precision:
       return dict(self._data_types, DateTimeField='datetime(6)', TimeField='time(6)')
   else:
       return self._data_types
```
这段代码我看到了让人欣喜的`datetime(6)`这么说只要是`self.features.supports_microsecond_precision`为`True`那么就会支持毫秒数，那我们接着往下看找到这个方法

```
@cached_property
def supports_microsecond_precision(self):
    # See https://github.com/farcepest/MySQLdb1/issues/24 for the reason
    # about requiring MySQLdb 1.2.5
    return self.connection.mysql_version >= (5, 6, 4) and Database.version_info >= (1, 2, 5)
```
从段代码就能看出来了，丫判断了mysq版本是不是大于5.6.4且Database（import MySQLdb as Database 其实就是连接驱动）大于1.2.5如果都大于那就支持，不大于就不支持很阴险啊，因为我们线上MariaDB10.0.16其实跟mysql5.5左右的版本是对应的所以肯定不支持啊。那就得想个比较淫荡的方便，所以在线上配置文件中加入如下代码：

```
from django.db.backends.mysql.base import DatabaseFeatures
DatabaseFeatures.supports_microsecond_precision = True
```
这样强制让等于True就会支持毫秒。搞定，收工！



