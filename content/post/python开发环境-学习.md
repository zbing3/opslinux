---
title: python开发环境
date: 2016-12-13 11:42:21
categories: [python]
tags:
  - python
---

# docker 配置部分

拉取 alpine

```
$ alpine docker pull alpine
```

查看当前机 下的 docker 镜像 

```
$ docker images
```

创建容器

```
$ docker create -ti --name python -h python -p 8080:8080 -w /root alpine sh
```

启动容器

```
$ docker start python
```

进入容器 

```
$ docker attach python
```

停止容器

```
$ docker stop python
```
删除容器

```
$ docker rm python
```

# alpine 配置

更改下alpine镜像的源

```
echo "http://mirrors.aliyun.com/alpine/v3.4/main/" > /etc/apk/repositories 
```

```
apk update
```

安装常用工具

```
$ apk add bash vim musl-dev gcc g++ python3 python3-dev
```

启动bash

```
$ bash
```

更新pip

```
$ pip3 install -U pip
```

查看 pip 版本号

```
$ pip -V
pip 9.0.1 from /usr/lib/python3.5/site-packages (python 3.5)
```
显示了全局的系统库路径


# Python 项目管理思路

1.全局的只安装开发工具类，全局的保持最小化
2.建立虚拟隔离环境，各个项目管理自己的第三方依赖库

例如： 
项目 a 依赖 python2
项目 b 依赖 python3

所以需要简历独立运行环境

python3 自带 venv, 但是不够好用，推荐第三方工具 virtualenv

virtualenv 提供了核心功能 
virtualenvwrapper 提供了丰富功能，包装器

# 安装虚拟环境

docker 环境下依赖包检查不完整， 所以先安装pbr
```
pip install -U pbr
```

```
pip install -U virtualenvwrapper
```

查看安装的库

```
pip list
```


配置bashrc

```
vim ~/.bashrc

export PS1="\[\e[32m\]\u:\w \$\[\e[m\] "  # 命令行提示符样式设置
export WORKON_HOME=/root/py/venv          # 虚拟环境目录
export PROJECT_HOME=/root/py              # 项目目录
export VIRTUALENVWRAPPER_PYTHON=`which python3`   # 指定虚拟环境使用 python 版本
source /usr/bin/virtualenvwrapper.sh  # 执行 vwrapper 脚本
```

让 bashrc 生效

```
source .bashrc
```


