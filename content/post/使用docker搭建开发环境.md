---
title: 使用docker搭建开发环境
date: 2016-09-28 16:15:32
categories: [develop]
tags: 
  - python
  - develop
  - go

-------

---

现在开发环境原来越复杂，为了方便开发，让团队每个人的环境一致，最近使用docker进行打包image，发放给团队使用。

#安装docker
本人使用mac，直接下载docker for mac 方便很多，其他os网上一搜一大把。so easy~!


# 下载images

```
[docker@docker-root ~]$ sudo docker pull alpine
[docker@docker-root ~]$ sudo docker pull ubuntu
[docker@docker-root ~]$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
alpine              latest              3e467a6273a3        2 days ago          4.793 MB
ubuntu              latest              17b6a9e179d7        5 days ago          120.7 MB
```

启动

```
docker run -ti -h dev --net=host -v ~/workspace/sohu:/root/workspace -w /root develop:base /bin/bash
```
把本地目录 ~/workspace/sohu 映射到容器 /root/workspace 目录

# 自定义images

先更新下源，安装几个必备工具
```
apt-get update
apt-get install vim curl git wget 
```

## 修改阿里云源

修改成阿里云源，加快安装软件的速度

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak #备份
sudo vim /etc/apt/sources.list #修改
sudo apt-get update #更新列表

```
阿里云源


```
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
```

## python开发环境

安装工具和必备依赖

```
apt-get install gcc gdb binutils make git dstat sysstat htop curl wget
apt-get install libjpeg-dev
apt-get install net-tools
apt-get install libffi-dev
apt-get install bzip2
apt-get install libssl
apt-get install libssl-dev
```

如需要sqlit支持需要先装如下库，再安装python：

```
sudo apt-get install sqlite3 libsqlite3-dev
```

### pyenv 安装

```
$ curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
```

在 ~/.bashrc 中添加

```
export PATH="/root/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"

```

#### 查看可安装的版本

```
$ pyenv install --list
```

#### 安装指定版本

```
$ pyenv install 3.5.2 -v
```

#### 更新数据库

```
$ pyenv rehash
```

#### 查看当前已安装的python版本

```
 $ pyenv versions 
 * system (set by /Users/ce/workspace/sohu/.python-version)
  3.5.1
  sohu351
```

#### 设置全局的python版本

```
$ pyenv global 3.5.1
$ pyenv versions
system
* 3.5.1 (set by /Users/ce/workspace/sohu/.python-version)

```
#### 安装virtualenv

```
$ pyenv global system  切换到系统python
$ pip install virtualenv
```

#### 安装virtualenvwrapper

安装 virtualenvwrapper 并让pyenv支持

```
$ pip install virtualenvwrapper
$ git clone https://github.com/yyuu/pyenv-virtualenvwrapper.git ~/.pyenv/plugins/pyenv-virtualenvwrapper
```


#### 使用python3.5创建一个虚拟环境

```
$ pyenv global 3.5.2
$ mkvirtualenv env2
$ workon env2
```
## Go开发环境

编译go1.3需要的参数

```
CGO_ENABLED=0 ./make.bash
```
ToDo……

# image 字符集修改

```
$ export LANG="en_US.UTF-8"

$ sudo locale-gen "en_US.UTF-8"
Generating locales...
  en_US.UTF-8... done
Generation complete.

$ sudo dpkg-reconfigure locales
Generating locales...
  en_US.UTF-8... up-to-date
Generation complete.

$ locale charmap
UTF-8
```
# syslog 

如果使用syslog，需要在启动的时候做一次目录映射

```
-v /dev/log:/dev/log
```

# 保存images

```
$ docker ps                                                                                                              [17:06:10]
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
111be6691cc8        c5940ba1089c        "/bin/bash"              4 days ago          Up 9 hours                                        dev
```

把 image 中 c5940ba1089c 这个字段记住

```
docker commit c5940ba1089c develop:base  #进行保存
```

进行查看

```
 $ docker images                                                                                                          
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
develop             base                5a893de95205        40 hours ago        1.87 GB
ubuntu              latest              42118e3df429        9 weeks ago         124.8 MB
alpine              latest              4e38e38c8ce0        3 months ago        4.799 MB
hello-world         latest              690ed74de00f        11 months ago       960 B
```
# 私有仓库

查看如下文章 ：

[创建私有仓库](https://yeasy.gitbooks.io/docker_practice/content/repository/local_repo.html)

