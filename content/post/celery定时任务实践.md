---
title: celery定时任务实践
date: 2015-09-22 09:28:23
categories: [Python]
tags: 
  - celery
  - python

---
![](/media/14428854039390.jpg)
说起celery搞python的程序员并不陌生，一般做队列任务之类的总是会用到。最近公司新项目用到类似队列的场景但是还要求定时完成，所以一下想到了celery马上搞起来。

看了资料做了需求分析，celery本身能完成队列和异步任务的功能，也有crontab但是如果做复杂的定时任务并不好控制，相对复杂。django-celery里面的定时任务功能可以通过后台控制，通过封装好的models也可以进行修改非常好。正好我们项目就是用的django所以直接django搞起来

## 安装
安装不用多说直接pip就搞定, the我们使用的是github上的development版本，与相依赖的是`celery==3.1.18`

```
pip install git+https://github.com/celery/django-celery.git
pip install celery==3.1.18 # 如果出现依赖问题那就安装
```
## django设置
在settings.py中配置：

```
import djcelery
djcelery.setup_loader()
# BROKER_URL = 'django://'  # 直接使用django做broker生产环境不建议，建议使用redis或者rabbitMQ
BROKER_URL = 'redis://:auth@127.0.0.1:22222/0' # broker使用reids
CELERYBEAT_SCHEDULER = 'djcelery.schedulers.DatabaseScheduler' # 定时任务
CELERY_RESULT_BACKEND = 'djcelery.backends.database:DatabaseBackend'
CELERY_ENABLE_UTC = False # 不是用UTC
CELERY_TIMEZONE = 'Asia/Shanghai' 
CELERY_TASK_RESULT_EXPIRES = 10 #任务结果的时效时间
CELERYD_LOG_FILE = BASE_DIR + "/logs/celery/celery.log" # log路径
CELERYBEAT_LOG_FILE = BASE_DIR + "/logs/celery/beat.log" # beat log路径
CELERY_ACCEPT_CONTENT = ['pickle', 'json', 'msgpack', 'yaml'] # 允许的格式

...
INSTALLED_APPS = (
  ...
  'djcelery',
  'kombu.transport.django',
  ...
)
```
第一二项是必须的，
在INSTALLED_APPS中添加的djcelery是必须的. kombu.transport.django则是基于Django的broker，如果使用redis就不需要了。

最后创建Celery所需的数据表(django1.8)：

```
python manage.py migrate
```

## 创建一个task

正如前面所说的, 一个task就是一个Pyhton function. 但Celery需要知道这一function是task, 因此我们可以使用celery自带的装饰器decorator: @task. 在django app目录中创建taske.py:


```
from celery import task

@task()
def add(x, y):
   return x + y
```

当settings.py中的djcelery.setup_loader()运行时, Celery便会查看所有INSTALLED_APPS中app目录中的tasks.py文件, 找到标记为task的function, 并将它们注册为celery task.

将function标注为task并不会妨碍他们的正常执行. 你还是可以像平时那样调用它: z = add(1, 2).

## 执行task

让我们以一个简单的例子作为开始. 例如我们希望在用户发出request后异步执行该task, 马上返回response, 从而不阻塞该request, 使用户有一个流畅的访问过程. 那么, 我们可以使用.delay, 例如在在views.py的一个view中:

   
``` 
from myapp.tasks import add
...
   add.delay(2, 2)
...
```
Celery会将task加入到queue中, 并马上返回. 而在一旁待命的worker看到该task后, 便会按照设定执行它, 并将他从queue中移除. 而worker则会执行以下代码:


```
import myapp.tasks.add

myapp.tasks.add(2, 2)
```
## 启动
## 启动worker

正如之前说到的, 我们需要worker来执行task. 以下是在开发环境中的如何启动worker:

首先启动terminal, 如同开发django项目一样, 激活virtualenv, 切换到django项目目录. 然后启动django自带web服务器: python manage.py runserver.

然后启动worker:


```
    python manage.py celery worker --loglevel=info
```
此时, worker将会在该terminal中运行, 并显示输出结果.

## 启动task

打开新的terminal, 激活virtualenv, 并切换到django项目目录:

    $ python manage.py shell
    >>> from myapp.tasks import add
    >>> add.delay(2, 2)
此时, 你可以在worker窗口中看到worker执行该task:


```
    [2014-10-07 08:47:08,076: INFO/MainProcess] Got task from broker: myapp.tasks.add[e080e047-b2a2-43a7-af74-d7d9d98b02fc]
    [2014-10-07 08:47:08,299: INFO/MainProcess] Task myapp.tasks.add[e080e047-b2a2-43a7-af74-d7d9d98b02fc] succeeded in 0.183349132538s: 4
```
## 定时任务
好了，简单的任务我们已经调通，下面我们还是来看定时任务怎么弄。
首先新建一个文件task.py（名字可以随便取）

```
import datetime
import json
from djcelery import models as celery_models
from django.utils import timezone

def create_task(name, task, task_args, crontab_time):
    '''
    创建任务
    name       # 任务名字
    task       # 执行的任务 "myapp.tasks.add"
    task_args  # 任务参数  {"x":1, "Y":1}
    crontab_time # 定时任务时间 格式：
	    {
	      'month_of_year': 9  # 月份
	      'day_of_month': 5   # 日期
	      'hour': 01         # 小时
	      'minute':05  # 分钟
	    }
    
    '''
    # task任务， created是否定时创建
    task, created = celery_models.PeriodicTask.objects.get_or_create(
        name=name,
        task=task)
   # 获取 crontab
    crontab = celery_models.CrontabSchedule.objects.filter(
        **crontab_time).first()
    if crontab is None:
    		# 如果没有就创建，有的话就继续复用之前的crontab
        crontab = celery_models.CrontabSchedule.objects.create(
            **crontab_time)
    task.crontab = crontab # 设置crontab
    task.enabled = True # 开启task
    task.kwargs = json.dumps(task_args) # 传入task参数
    expiration = timezone.now() + datetime.timedelta(day=1)
    task.expires = expiration # 设置任务过期时间为现在时间的一天以后
    task.save()
    return True


def disable_task(name):
    '''
    关闭任务
    '''
    try:
        task = celery_models.PeriodicTask.objects.get(name=name)
        task.enabled = False # 设置关闭
        task.save()
        return True
    except celery_models.PeriodicTask.DoesNotExist:
        return True

```
### 启动beat
执行定时任务时, Celery会通过celerybeat进程来完成. Celerybeat会保持运行, 一旦到了某一定时任务需要执行时, Celerybeat便将其加入到queue中. 不像worker进程, Celerybeat只有需要一个即可.

启动：
```
python manage.py celery beat --loglevel=info
```
其实还有一种简单的启动方式worker和beat一起启动：

```
    python manage.py celery worker --loglevel=info --beat
```
将定期任务储存在django数据库中. 即是在django和celery都运行的状态, 这一方式也可以让我们方便的修改定时任务. 我们只需要设置settings.py中的一项便能开启这一方式:

```
# settings.py
CELERYBEAT_SCHEDULER = 'djcelery.schedulers.DatabaseScheduler'
```
然后我们便可以通过django admin的/admin/djcelery/periodictask/添加定期任务了.
也可以直接使用刚才我写的脚本在自己的代码逻辑中自己增加和禁用定时任务
### 定时删除
可能大家用了一段时间就会想到，很多任务都是一次执行完就不需要，留在数据库里就是垃圾数据了有没有办法清除。方法肯定有因为django-celery本身就有定时任务功能我们加个任务就解决了。好我们看代码：
在django app目录中打开taske.py加入如下代码

```
from djcelery import models as celery_models
from django.utils import timezone

@task()
def delete():
    '''
    删除任务
    从models中过滤出过期时间小于现在的时间然后删除
    '''
    return celery_models.PeriodicTask.objects.filter(
        expires__lt=timezone.now()).delete()

```
大家都记得创建任务脚本里设置了 expires 1天以后过期，这样在filter的时候就能当做条件把过期的任务找到并且删除。

然后我们在django-admin中把这个任务加到定时任务中：
![](/media/14428906210406.jpg)
名字为del，任务是myapp.taks.delete,定时为每天的5点执行（crontab的格式，不熟悉的大家可以搜索学习下linux crontab格式）

## 参考
[http://www.weiguda.com/blog/73/](http://www.weiguda.com/blog/73/)
[http://my.oschina.net/kinegratii/blog/292395#OSC_h2_10](http://my.oschina.net/kinegratii/blog/292395#OSC_h2_10)







