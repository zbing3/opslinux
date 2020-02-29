---
title: python使用mechanize实现登陆
tags:
  - mechanize
date: 2014-05-14 10:53:47
---

mechanize是非常合适的模拟浏览器的模块，它的特点主要有：

*   1 http,https协议等。
*   2 简单的HTML表单填写。
*   3 浏览器历史记录和重载。
*   4 Referer的HTTP头的正确添加（可选）。
*   5 自动遵守robots.txt的。
*   6 自动处理HTTP-EQUIV和刷新。

所以你可以用mechanize来完成一些自动化浏览器想要做的事情，比如自动登录表单，自动填写表单等

确保已经安装：`pip install mechanize`


```
#!/usr/bin/python
import mechanize  #引入mechanize 需要pip安装
import cookielib  #引入cookielib 来做cookie

url = 'http://127.0.0.1:8080/login'
username = 'zhangbin'
password = 'zhangbin'

def login():
     br = mechanize.Browser()

     cj = cookielib.LWPCookieJar()
     br.set_cookiejar(cj) #关联cookies

     #设置一些参数，因为是模拟客户端请求，所以要支持客户端的一些常用功能
     br.set_handle_equiv(True)
     br.set_handle_gzip(True)
     br.set_handle_redirect(True)
     br.set_handle_referer(True)
     br.set_handle_robots(False)

     br.set_handle_refresh(mechanize._http.HTTPRefreshProcessor(), max_time=1)

     br.addheaders = [('User-agent', 'Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.9.1.11) Gecko/20100701 Firefox/3.5.11')]  #模拟浏览器头
     # br.set_debug_http(True)   #debug
     # br.set_debug_responses(True) #debug
     br.open(url) #打开连接
     br.select_form(nr = 0) #选择第一个form
     br.form['email'] = username #写入用户名
     br.form['password'] = password #写入密码
     br.submit()  #提交
     print br.response().read() #打印登录后内容

if  __name__ == '__main__':
    login()
```

