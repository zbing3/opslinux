---
title: Python多版本共存之pyenv
tags:
  - python
date: 2013-10-04 00:27:00
---

需要使用新版本Python的相关功能，但是又不想要影响到系统自带的Python，这个时候就需要实现Python的多版本共存。

[pyenv](https://github.com/yyuu/pyenv)可以很好的实现Python的多版本共存。

## 安装pyenv


```
$ git clone git://github.com/yyuu/pyenv.git ~/.pyenv
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
$ echo 'eval "$(pyenv init -)"' >> ~/.bashrc
$ exec $SHELL -l
```

## 安装Python



### 查看可安装的版本


```
$ pyenv install --list
```

### 安装指定版本

使用如下命令即可安装python 3.3.2.


```
$ pyenv install 3.3.2
```

该命令会从github上下载python的源代码，并解压到/tmp目录下，然后在/tmp中执行编译工作。编译过程依赖一些其他的库文件，若库文件不能满足，则编译错误，需要重新下载、编译。。。(为什么每次都要重新下呢？)

已知的一些需要预先安装的库包括：

*   readline readline-devel readline-static
*   openssl openssl-devel openssl-static
*   sqlite-devel
*   bzip2-devel bzip2-libs

在所有python依赖库都安装好的情况下，python的安装很顺利。



### 更新数据库

安装完成之后需要对数据库进行更新：


```
$ pyenv rehash
```

### 查看当前已安装的python版本


```
$ pyenv versions
* system (set by /export/home/seisman/.pyenv/version)
3.3.2

```

其中的星号表示使用的是系统自带的python。


### 设置全局的python版本


```
$ pyenv global 3.3.2
$ pyenv versions
system
* 3.3.2 (set by /export/home/seisman/.pyenv/version)
```
当前全局的python版本已经变成了3.3.2。也可以使用<tt class="docutils literal">pyenv local</tt>或<tt class="docutils literal">pyenv shell</tt>临时改变python版本。

### 确认python版本


```
$ python
Python 3.3.2 (default, Sep 30 2013, 20:11:44)
[GCC 4.4.7 20120313 (Red Hat 4.4.7-3)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

## 使用python

*   输入<tt class="docutils literal">python</tt>即可使用新版本的python；
*   系统命令会以/usr/bin/python的方式直接调用老版本的python；
*   使用pip安装第三方模块时会安装到~/.pyenv/versions/3.3.2下，不会和系统模块发生冲突。


