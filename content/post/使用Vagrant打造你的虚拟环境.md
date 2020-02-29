---
title: 使用Vagrant打造你的虚拟环境
tags:
  - vagrant
date: 2013-09-14 00:00:00
---

#使用Vagrant打造你的虚拟环境

因为要做mongDB的replication+sharding的实验，领导给我推荐Vagrant来模拟虚拟环境做部署实践，稍微了解够大为震惊，因网上文档不是特别容易理解，留下一份以备后用

vagrant的强大在于是一个镜像，配置完以后镜像可以放到任何地方去，真正做到了一劳永逸了。

总结一下自己使用vagrant的一点笔记，以免以后忘记还得再去翻官方文档。

vagrant的官方网站：http://www.vagrantup.com/ 现在又改版了，挺漂亮的。

vagrant的一些镜像：http://www.vagrantbox.es/ 各种linux都有。
然后按照官方说的，执行这三部，然后一个虚拟机就起来了。
注：先要安装VirtualBox

##配置box


```
$ vagrant box add debian http://ergonlogic.com/files/boxes/debian-current.box  #增加一个box,debian就是box的title 后面跟vagrant上的virtualbox镜像地址
$ vagrant init debian #初始化debian
$ vagrant up   #这个是真正的启动
```
注意国内网速访问很慢 这里可以先去 `http://www.vagrantbox.es/` 下载你需要的镜像 然后把http那行直接换成你本地镜像的路径就ok比较方便和快捷

###连接虚拟主机

你会看到终端显示了启动过程，启动完成后，我们就可以用 SSH 登录虚拟机了，剩下的步骤就是在虚拟机里配置你要运行的各种环境和参数了。


```
$ vagrant ssh  # SSH 登录 ssh的后面可以跟你的title来连接不同的vm主机
```

###打包分发

当你配置好开发环境后，退出并关闭虚拟机。在终端里对开发环境进行打包：


```
$ vagrant package
```

打包完成后会在当前目录生成一个 package.box 的文件，将这个文件传给其他用户，其他用户只要添加这个 box 并用其初始化自己的开发目录就能得到一个一模一样的开发环境了。

###常用命令


```
$ vagrant init  # 初始化
$ vagrant up  # 启动虚拟机
$ vagrant halt  # 关闭虚拟机
$ vagrant reload  # 重启虚拟机
$ vagrant ssh  # SSH 至虚拟机
$ vagrant status  # 查看虚拟机运行状态
$ vagrant destroy  # 销毁当前虚拟机
```

**box管理**


```
$vagrant box list
$vagrant box add
$vagrant box remove
```

更多内容请查阅官方文档 http://docs.vagrantup.com/

###Multi-VM 多虚拟机


```
VAGRANTFILE_API_VERSION = "2"    #定义版本
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|  #使用内部2版本
  config.vm.define :debian1 do |debian1|   #定义第一台虚拟机，||里面就类似一个变量设置参数时使用 
     debian1.vm.box = "debian1"             #设置box名为debian1
     debian1.vm.host_name = "debian1"      #设置hostname为debian1
     debian1.vm.network :private_network, ip: "192.168.1.11" #设置网络为内部网络 ip为192.168.1.11
  end
  config.vm.define :debian2 do |debian2|
     debian2.vm.box = "debian2"
     debian2.vm.host_name = "debian2"
     debian2.vm.network :private_network, ip: "192.168.1.12"
  end
  config.vm.define :debian3 do |debian3|
     debian3.vm.box = "debian3"
     debian3.vm.host_name = "debian3"
     debian3.vm.network :private_network, ip: "192.168.1.13"
  end

end
```

注意语法格式就好，配置前关闭虚拟机，配置完后打开虚拟机。

**注意事项**

使用 Apache/Nginx 时会出现诸如图片修改后但页面刷新仍然是旧文件的情况，是由于静态文件缓存造成的。需要对虚拟机里的 Apache/Nginx 配置文件进行修改：

Apache 配置添加:

```
EnableSendfile off
```

Nginx 配置添加:

```
sendfile off;
```


