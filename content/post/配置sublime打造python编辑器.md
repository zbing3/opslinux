---
title: 配置sublime打造python编辑器
tags:
  - sublime
  - python
date: 2014-05-26 09:46:24
---

Contents

*   [插件](#id1)
    *   [Package Control](#package-control)
    *   [SublimeCodeIntel插件](#sublimecodeintel)
    *   [SublimeLinter](#sublimelinter)
    *   [Python PEP8 Autoformat](#python-pep8-autoformat)
    *   [GitGutter](#gitgutter)
    *   [SideBarEnhancements](#sidebarenhancements)

*   [快捷键](#id2)

昨晚长夜漫漫，无心睡眠，运维开发群里交谈不断，突然一位常用vim的同学想开速的学习下sublime的配置，来开发python，由于本人一直使用sublime开发python web项目，索性把自己的配置过程写下来，方便大家交流。

* * *

首先，安装上sublime这个就不多说了，本人使用的是宿便sublime Text2 虽然出3了，但是有些插件还没有支持索性不用3。

进入下面的配置文件：

Preferences ——》Settings - Default

找到对应的关键字，修改成下面的值，切忌：不是复制下面粘贴进去。


```
{
     "auto_indent": true,
     "drag_text": false,
     "font_face": "",
     "font_size": 17.5,
     "ignored_packages":
     [
          "Vintage"
     ],
     "tab_size": 4,
     "translate_tabs_to_spaces": true,
     "trim_automatic_white_space": true,
     "word_wrap": true
}
```

## [插件](#id3)


### [Package Control](#id4)

包管理器是必备的，新下载的Sublime Text第一个装的肯定是这个，有了它，装其他的包就很方便了。

适用于 Sublime Text 3：

1、通过快捷键 `ctrl+`` 或者 View -> Show Console 菜单打开控制台

2、粘贴对应版本的代码后回车安装


```
import  urllib.request,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();urllib.request.install_opener(urllib.request.build_opener(urllib.request.ProxyHandler()));open(os.path.join(ipp,pf),'wb').write(urllib.request.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())
```

适用于 Sublime Text 2：
安装方式有两种，第一种是在线下载安装：在 Sublime Text 2 中按下 &quot;ctrl+`&quot;（就是大键盘数字1左边的那个键），拷贝以下命令到窗口下部的终端中，


```
import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print 'Please restart Sublime Text to finish installation'
```
然后回车确认，安装完毕之后重启sublime，如果发现在Perferences中看到package control这一项，则安装成功。

然后就可以通过&quot;command+Shift+P&quot;打开命令面板，输入&quot;install&quot;命令，选择“Package Control: Install Package”，然后输入要安装的包的名称，就可以在线安装了。


### [SublimeCodeIntel插件](#id5)

智能提示插件，这个插件的智能提示功能非常强大，可以自定义提示的内容库

Preferences ——》Package Settings ——》SublimeCodeIntel ——》Settings - Default

修改里面python的配置，如下：


```
"codeintel_config": {
    "Python": {
            "PATH": "usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:$PATH",
            "pythonExtraPaths": ["/usr/local/lib/python2.7/site-packages"],
            "python":"/usr/bin/python2.7",
            "PYTHONPATH": "/usr/local/lib/python2.7/site-packages:/System/Library/Frameworks/Python.framework/Versions/2.7/:/Users/ce/workspace:$PYTHONPATH"
    },
}
```

安装pep8和pylint 实现代码风格和错误检查


```
sudo pip install pylint
```


```
sudo pip install pep8
```


### [SublimeLinter](#id6)

这是用来在写代码时做代码检查的。可以在包管理器中安装。写Python程序的话，它还会帮你查代码是否符合PEP8的要求。有问题有代码会出现白框，点击时底下的状态栏会提示出什么问题。

### [Python PEP8 Autoformat](#id7)

这是用来按PEP8自动格式化代码的。可以在包管理器中安装。如果以前写程序不留意的话，用SublimeLinter一查，满屏都是白框框，只要装上这个包，按ctrl+shift+r，代码就会按PEP8要求自动格式化了，一屏的白框几乎都消失了


### [GitGutter](#id8)

它基于git查看代码行是被增加，修改还是删除，在行数的前面显示如下：

![GitGutter](http://opslinux.qiniudn.com/b4d952ccb1_183250-7WIb-865233.png)

### [SideBarEnhancements](#id9)

可以大大加强在侧栏目录树中右键的选项


## [快捷键](#id10)


```
command + p            //快速查找特定文件
command + shift + f   //多文件搜素
command + r          //查看文件内块列表（如果是类文件，展示的是函数）
command + alt + num //多屏查看文件
command + k + b    //切换左侧目录面板
```

