---
title: Django ajax post
categories: [Django]
tags:
  - django ajax post
date: 2014-05-07 20:35:03
---

在web项目中，ajax运用非常频繁，今天就给大家展示下Django ajax Post的使用方法

## templates 模板

index.html


```
<html>
<header></header>
<body>

<p>name: <input type="text" name="nickname" /></p>
<input class="name_submit" type="submit" value="Submit" />
<div class="name_value"></div>

</body>

<script src="/static/js/jquery.js"></script>
<script type="text/javascript">

$(".name_submit").click(function(){
    var nickname = $("input[name='nickname']").val();
    $.ajax({
      url:"/nickname",
      type:"post",
      data:{"nickname":nickname},
      dataType:"json",
      success:function(data){
        $(".name_value").text("")
       $(".name_value").append("名字为:"+data)
      }

    }); // Ajax End
  });

</script>

</html>
```
显示如下：

![html](http://opslinux.qiniudn.com/9045993C-0EE1-4C5F-8B71-91452D31720A.png)

当点击提交按钮时，就执行ajax，把获取的nickname传递到/nickname url中。

## url


```
from django.conf.urls import patterns, include, url

from django.contrib import admin
from views import index,nickname
admin.autodiscover()

urlpatterns = patterns('',
    # Examples:
    url(r'^$', index),
    url(r'^nickname$', nickname),
    url(r'^admin/', include(admin.site.urls)),

)
```
从这里可以看到/nickname URL是views文件中的nickname函数


## views


```
#coding=utf-8
from django.shortcuts import render, render_to_response
from django.http import HttpResponseRedirect, HttpResponse
from django.template import Template,Context

def index(request):
    if request.method == 'GET':
        return render_to_response("index.html")
def nickname(request):
    if request.method == 'POST':   #当request为POST的时候
        nickname = request.POST.get('nickname','')  #获取ajax POST的nickname值
        return HttpResponse(nickname)  #为了方便显示，直接在浏览器显示nickname
```
nickname函数 当时POST的时候，获取nickname值，然后为了方便显示就直接使用return HttpResponse(nickname) 直接展示出来nickname值



