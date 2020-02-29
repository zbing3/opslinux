---
title: 使用 nginx x-accell-redirect 保护你的静态文件
date: 2016-09-29 16:13:47
tags: 
  - python
  - develop
---

在web开发过程中，有些时候需要隐藏你的静态文件url，也有些时候需要频繁更替你的静态资源服务商，如果你的静态资源服务商（七牛、又拍云）发生了变更，然后你又要修改对外暴漏的url是非常不方便的。这时候我们使用一个统一的自己能控制的对外url对于我们非常方便，也算是变相的对外解耦，是个非常有用的办法。

基于以上的想法，我们来进行技术选型，由于现在大家写web都脱离不了nginx，所以就想在nginx层面上解决这个问题。不出所料，nginx 里的 x-accell-redirect 就可以实现此功能。下面我们使用nginx和django来实现此功能。


# 配置nginx

修改 nginx.conf 新增 location

```
location ~* ^/protected/ {
        internal; # 只允许内部重定向
        rewrite ^/protected/(.*?)\|(.*) /$2 break;
        proxy_set_header Authorization $1;   # 鉴权的东西放到header中 如果没有鉴权可以去掉，$1的值是服务端传过来的，详见后面
        proxy_pass http://images.com;     # 你的静态文件地址
        }
```
保存，reload 。

# django view

这里面使用了 django-rest-framework 来进行API的开发

```
class NginxAccel(APIView):

    def get(self, request, img_name):
        auth = image_auth() # 类似私有空间的验证
        response = Response()
        response["X-Accel-Redirect"] = "/protected/{0}|{1}".format(
            auth, img_name)
        return response

```
然后通过url来访问，鉴权成功，就可以访问到你要的图片。如果不需要鉴权，渠道auth的相关代码就可以。

