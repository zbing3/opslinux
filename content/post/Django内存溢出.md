---
title: Django内存溢出
categories: [Django]
tags:
  - python
date: 2014-12-14 21:43:36
---

前两天工作需求，把python脚本从crontab的方式改成守护进程(daemon)，上线后发现，内存飙升，cpu飙高，程序直接死掉，让我伤心欲绝，郁郁寡欢，终日不得眠啊。开始以为是内存溢出用gc调试好久也看不出问题。由于项目使用了Django-ORM，所以开始怀疑是不是因为Django引发的性能问题。

搜索了一下关于django内存泄露的文章，这片文章 —— [Django&quot;内存泄漏&quot;的问题](http://www.igigo.net/post/archives/66)  给我了启示:

> 文中第一条就提到:Make sure that you set DEBUG to False in settings.py，在DEBUG模式下，所有的SQL查询都会被保存在内存中.我顿时想起，由于我整个项目还在调试阶段，这个DEBUG变量是设置为True的
> 
> DEBUG设置为False后，内存泄漏的问题就不复存在了
> 
> 可我整个项目还需要调试，不能就这么简单的关闭DEBUG模式，怎么办? 我一开始想在后台模块import settings后再重新设置DEBUG变量

下面分析一下脚本程序，import了Django的models,并且对数据库的数据做了轮询操作，所以执行大量的sql去读数据库的全部数据，由于线上DEBUG改成了True，所有的SQL查询保存在了内存中，这样导致内存飙高，好吧不得不说为了把脚本弄成守护进程方式，还用了个while True的死循环，虽然改成了while 1，但是心里还是怪怪的。

找到问题的原因，我就把DEBUG改成False重启一下,果然内存不再飙高，就此搞定。在此记录一下，解决线上问题的一个小思路。

