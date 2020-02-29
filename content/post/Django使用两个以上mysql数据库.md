---
title: Django使用两个以上mysql数据库
categories: [Django]
tags:
  - Django
  - mysql
date: 2014-11-20 17:11:04
---


看了Django的官方文档，关于model这有介绍Multiple databases，但是没有介绍超过两个数据库的连接情况，这里我用mysql举例子，连接3个mysql数据库。估计这么用的不多哈哈哈，我们的需求比较复杂两个mysql上的都是公用数据，然后默认的数据库是我项目使用的数据库，保存着自己项目的账户系统和使用的一些信息。


## [settings.py](#id1)

下面是settings文件的配置，添加三个数据库，default为本项目自己的数据库，剩下两个为外部数据库


```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'my_db',
        'USER': 'root',
        'PASSWORD': '123456',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    },
    'web_db': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'web_db',
        'USER': 'root',
        'PASSWORD': '123456',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    },
    'admin_db': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'admin_db',
        'USER': 'root',
        'PASSWORD': '123456',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    },

}
```

## [router](#id2)

根据官方文档提示需要自己写个router文件，我的修改如下：


```
class AuthRouter(object):
    """ 控制 adminapp 应用中模型的
    所有数据库操作的路由 """

    def db_for_read(self, model, **hints):
        if model._meta.app_label == 'adminapp':
            return 'admin_db'
        elif model._meta.app_label == 'webapp':
            return 'web_db'
        return None

    def db_for_write(self, model, **hints):
        if model._meta.app_label == 'adminapp':
            return 'admin_db'
        elif model._meta.app_label == 'qingsong':
            return 'web_db'
        return None

    def allow_relation(self, obj1, obj2, **hints):
        if obj1._meta.app_label == 'adminapp' or obj2._meta.app_label == 'adminapp':
            return True
        elif obj1._meta.app_label == 'qingsong' or obj2._meta.app_label == 'qingsong':
            return True
        return None

    def allow_syncdb(self, db, model):
        #django1.7
        print "db:%s,model:%s"%(db,model)
        print model._meta.app_label
        if db == 'admin_db':
            return model._meta.app_label == 'adminapp'
        elif model._meta.app_label == 'adminapp':
            return False
        if db == 'qingsong_db':
            return model._meta.app_label == 'qingsong'
        elif model._meta.app_label == 'qingsong':
            return False
        return None

    def allow_migrate(self, db, model):
        #django1.7
        if db == 'admin_db':
            return model._meta.app_label == 'adminapp'
        elif model._meta.app_label == 'adminapp':
            return False
        return None
```
然后根据官方文档在settings中加入：


```
DATABASE_ROUTERS = ['path.to.AuthRouter', 'path.to.PrimaryReplicaRouter']

```

## [model中的使用](#id3)

关于app_label在1.6和1.7中使用稍有不同，官方文档有详细记载，这里我举例Django1.6的方式：

在model中加入app_label标签，这个model属于哪个数据库，写上数据库的名字（settings.py文件中的名字），例如：


```
class Test(models.Model):
    name=models.CharField(max_length=30, unique=True)
    class Meta:
        db_table = 'test'
        app_label = 'adminapp'

    def __unicode__(self):
        return self.name
```


