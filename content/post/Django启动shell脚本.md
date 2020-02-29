---
title: Django启动shell脚本
categories: [Django]
tags:
  - Django
  - Python
date: 2014-05-19 15:11:45
---

最近服务器在线上做测试，没用supervisord管理，有时候必须需要手动来回切换，dev和test启动的方式也不同，索性用shell写个比较low的脚本搞定事情。

```
#!/bin/bash
case $1 in
    dev)
        . ~/py2/bin/activate       #使用 virtualenv
        python manage.py runserver store.dev.service.cmos.net:8080
        ;;
    test)
        . ~/py2/bin/activate
        python manage.py runserver store.test.service.cmos.net:8080
        ;;
    *)
      echo "Usage: $0 [dev|test]"
      exit 1
      ;;
esac
exit 0
```

