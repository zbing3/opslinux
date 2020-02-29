---
title: tornado session装饰器
tags:
  - 装饰器
  - session
  - tornado
date: 2014-05-04 15:37:43
---

tornado属于轻量级，异步，高并发的代名词了，虽然开发网站的速度比较慢，但是大多数同学开发后端服务，还都是喜欢使用tornado，今天给大家分享一下，tornado中自己做session时，写装饰器的代码：

```
#coding=utf-8
import models

from tornado.web import HTTPError

def check_login(func):
    def _wrapper(self,*args, **kwargs):
        session_id = self.request.headers.get('Session_id',None)
        user_id = self.request.headers.get('User_id',None)
        session_data = models.session.query(models.Session).filter_by(user_id=user_id).first()
        if session_id:
            ret = func(self,*args, **kwargs)
            if session_data.session_id == session_id:
                return ret
            else:
                raise HTTPError(403)
        else:
            raise HTTPError(403)
    return _wrapper
```

