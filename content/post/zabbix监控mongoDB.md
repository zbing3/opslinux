---
title: zabbix监控mongoDB
tags:
  - zabbix
date: 2013-09-09 00:00:00
---

# zabbix监控mongoDB

推荐文档：

官方推荐：http://docs.mongodb.org/manual/administration/monitoring/

因我使用的是zabbix.所以选择：

https://code.google.com/p/mikoomi/wiki/03

插件下载地址：

http://mikoomi.googlecode.com/svn/plugins/MongoDB%20Plugin/

学习地址：

https://blog.serverdensity.com/mongodb-monitoring-db-serverstatus/

http://www.yaukb.com/2012/05/zabbix_mongodb/

**1.在Zabbix Server上安装php MongoDB驱动：**

```
[root@localhost conf.d]# pecl install mongo

WARNING: channel “pecl.php.net” has updated its protocols, use “pecl channel-update pecl.php.net” to update
downloading mongo-1.4.3.tgz …
Starting to download mongo-1.4.3.tgz (140,481 bytes)
…………………………done: 140,481 bytes
84 source files, building
running: phpize
Configuring for:
PHP Api Version:         20100412
Zend Module Api No:      20100525
Zend Extension Api No:   220100525

……

Build process completed successfully
Installing ‘/usr/lib64/php/modules/mongo.so’
install ok: channel://pecl.php.net/mongo-1.4.3
configuration option “php_ini” is not set to php.ini location
You should add “extension=mongo.so” to php.ini
You have new mail in /var/spool/mail/root

[root@localhost conf.d]# vim /etc/php.ini

[root@localhost conf.d]# /etc/init.d/httpd reload

[root@localhost conf.d]# php -m |grep mongo

mongo
```
**2.下载：**


```
[root@localhost externalscripts]# pwd

/etc/zabbix/externalscripts

[root@localhost externalscripts]# wget http://mikoomi.googlecode.com/svn/plugins/MongoDB%20Plugin/mikoomi-mongodb-plugin.php

[root@localhost externalscripts]# wget http://mikoomi.googlecode.com/svn/plugins/MongoDB%20Plugin/mikoomi-mongodb-plugin.sh

```

**3.导入模板 建立主机：**

将`MongoDB_Plugin_template_export.xml`导入到zabbix中
修改`"Miscellaneous: Data Collector"`监控项的key值，因默认提供的值有错误：

```
mikoomi-mongodb-plugin.sh["--", "-h", "{$MONGODB_HOSTNAME}", "-p", "{$MONGODB_PORT}", "-z", "{$MONGODB_ZABBIX_NAME}"]
```

在zabbix里建立主机，定义宏：

{$MONGODB_HOSTNAME} = 10.0.199.30   #即ip地址

{$MONGODB_PORT} = 27017 #监控mongdb的端口号

{$MONGODB_ZABBIX_NAME} =MongDB1 #hostname 就是主机名 不要写显示名 这样会接受不到数据 一定是hostname

然后给主机连接上模板即可。


**4.测试：**


```
[root@localhost externalscripts]# ./mikoomi-mongodb-plugin.sh -D -h10.0.199.30 -p27017 -z MongDB1
0
[root@localhost externalscripts]# less /tmp/mikoomi-mongodb-plugin.php_MongoDB1.log

mikoomi-mongodb-plugin.php:Successfully connected to mongoDB using connect string root:passworda@MongDB1:27017
zabbix_sender [8413]: Warning: [line 66] ‘Key value’ required

zabbix_sender [8413]: Warning: [line 68] ‘Key value’ required

zabbix_sender [8414]: DEBUG: answer [{

<div class="highlight"><pre>    &quot;response&quot;:&quot;success&quot;,

    &quot;info&quot;:&quot;Processed 58 Failed 13 Total 71 Seconds spent 0.001618&quot;}]
</pre></div>

sent: 71; skipped: 2; total: 73

/tmp/mikoomi-mongodb-plugin.php_MongDB1.log (END)
```

`若出现  Failed 的数目和 Total数目相等的话 应该是  mikoomi-mongodb-plugin.sh 后面-z参数的 hostname没写对 这个hostname是zabbix主机的hostname即主机名 而不是显示名`

