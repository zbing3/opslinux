---
title: Django实现发送邮件重置用户密码
categories: [Django]
tags:
  - Django
  - 密码重置
  - 邮件重置密码
date: 2014-05-13 10:47:19
---

Django，是个不错的框架，非常全，内置了用户系统，咱们稍微修改就可以实现发送重置密码邮件。

## url.py


```
from django.contrib.auth import views as auth_views

urlpatterns = patterns('',

   url(r'^forgot-password/$',
   views.forgot_password, name="forgot-password"),
   url(r'^password/change/$',
       auth_views.password_change,
       name='password_change'),
   url(r'^password/change/done/$',
       auth_views.password_change_done,
       name='password_change_done'),
   url(r'^resetpassword/$',
       auth_views.password_reset,
       name='password_reset'),
   url(r'^resetpassword/passwordsent/$',
       auth_views.password_reset_done,
       name='password_reset_done'),
   url(r'^reset/done/$',
       auth_views.password_reset_complete,
       name='password_reset_complete'),

   url(r'^reset/(?P<uidb64>[0-9A-Za-z_\-]+)/(?P<token>.+)/$',
       auth_views.password_reset_confirm,
       name='password_reset_confirm'),

   )
```


## templates设置

在 `django/contrib/auth/templates/registration` 中copy如下文件到自己的templates目录下的registration中：


```
password_reset_subject.txt
```

在 `django/contrib/admin/templates/registration` 中copy如下文件到自己的templates目录下的registration中：


```
logged_out.html
password_change_done.html
password_change_form.html
password_reset_complete.html  #修改密码完成的文件
password_reset_confirm.html
password_reset_done.html
password_reset_email.html   #发email的文件
password_reset_form.html
```

可根据自己的需求进行定义我在这里面，就把logged_out.html文件删除了，加入了自己写的 login.html ，然后将所有文件中的

```	
{% extends admin/base_site.html %} 
改为
{% extends base.html %}
```


这样做完还是不能用的，因为需要base.html文件:


```
<html>
<head>
    <title>{% block title %}{% endblock title %}</title>
</head>
<body>
    {% block content %}{% endblock content %}
</body>
</html>
```

## 测试

点击忘记密码：

![忘记密码](http://opslinux.qiniudn.com/5C9DDB80-95C6-46D6-A888-B749472B9191.png)

输入自己的邮箱地址。

不一会就会收到邮件：

![忘记密码](http://opslinux.qiniudn.com/AF523C9F-946E-49E0-8F43-AA25CD1FF5B6.png)

内容为：

![忘记密码](http://opslinux.qiniudn.com/2AF4146D-5E78-4DEE-8429-01AEFA212D8A.png)

想修改邮件内容可以修改`templates/registration/password_reset_email.html`文件。


