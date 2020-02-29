---
title: python通过telnet连接华为设备
tags:
  - python
date: 2015-01-29 17:40:07
---

最近在研究华为防火墙，要搞个web版的程序来控制防火墙，那底层就需要用telnet或者snmp来控制设置，今天先共享下telnet的代码


```
#coding=utf-8
import telnetlib
import re
import time

HOST = '192.168.1.231'
user = 'admin'
password = 'admin'

tn = telnetlib.Telnet(HOST)
# tn.set_debuglevel(2) #开启调试模式

tn.expect([re.compile(b"Username:"),]) #用正则匹配Username
tn.write(user + "\n")  #匹配成功，输入user
tn.expect([re.compile(b"Password:"),]) #同上
tn.write(password + "\n") #同上
time.sleep(.1)

tn.read_until("<HWJC.HL-DDoS.SDA>") #如果读到<HWJC.HL-DDoS.SDA>提示符，执行下面命令
tn.write("display clock\n")  #输入命令
tn.read_until("display clock") #如果读到"display clock"，执行下面命令，这里的操作是在后面获取返回值的时候排除"display clock"这一行数据
time.sleep(.1) #延时以确保下调命令能读到数据
print tn.read_very_eager() #打印执行"display clock"的返回值
# tn.write("quit\n") #退出
# print tn.read_all() #获取全部返回值
tn.close() #关闭连接
```

