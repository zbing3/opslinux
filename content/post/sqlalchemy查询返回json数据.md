---
title: sqlalchemy查询返回json数据
tags:
  - sqlalchemy
  - python
date: 2014-02-19 11:17:00
---

最近公司开发新项目使用tornado+mysql，传统的SQL开发方式大家都感觉不爽（其实我不太会，哈哈），所以开始使用orm方式，这也是python社区大家所推崇的方式，技术选型就选择了霸道的神器[sqlalchemy](http://www.sqlalchemy.org/)

## 返回json数据

使用tornado做apps的服务端，很多时候需要返回json数据，但是sqlalchemy默认没提供这种方法，所以经过今天查询网上的资料自己实现了一个方法。

为了在tornado中增加json支持，需要在model（默认tornado是没有model的，使用sqlalchemy，大家一般会写个models.py文件来做model）里面增加`__json__`方法：


```
#coding=utf-8
import json
from sqlalchemy import create_engine, DDL, event
from sqlalchemy.orm import sessionmaker
from sqlalchemy import Column, Integer, String, Date, Boolean, Unicode
from sqlalchemy.ext.declarative import declarative_base
from settings import mysql_passwd

DB_CONNECT_STRING = 'mysql+mysqlconnector://root:%s@localhost:3306/test?charset=utf8'%mysql_passwd
engine = create_engine(DB_CONNECT_STRING, echo=False)
DB_Session = sessionmaker(bind=engine)
session = DB_Session()

Base = declarative_base()

#给Base添加__json__方法 使输出JSON数据
def sqlalchemy_json(self):
    obj_dict = self.__dict__
    return dict((key, obj_dict[key]) for key in obj_dict if not key.startswith("_"))
Base.__json__ = sqlalchemy_json
```

这样搞定以后在你的程序想要调用model的时候如下使用就会输出json数据：

```
class users(BaseHandler):
def get(self):
    user_id = self.get_argument("user_id")#获取user_id
    user_info = self.session.query(models.User).filter_by(id=user_id).first()#查询user信息
    self.write(json.dumps({"status":0,"msg":"返回成功","user_info":models.User.__json__(user)},ensure_ascii=False,indent=4))#返回自定义数据，models.User.__json__(user)就是使用在Base创建的__json__方法来返回json数据，ensure_ascii=False是不使用ascii为了显示中文，indent=4是缩进，格式化输出json比较美观
```


