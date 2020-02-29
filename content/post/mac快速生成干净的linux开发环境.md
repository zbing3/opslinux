---
title: mac快速生成干净的linux开发环境
date: 2016-05-09 17:13:05
categories: [develop]
tags: 
  - mac
  - develop

---

# mac快速生成干净的linux开发环境


virtualbox -> vagrant/completed -> docker-root -> docker -> ... ubuntu/alpine 

安装vagrant 或者去官方下载

```
brew tap caskroom/cask #if not already installed
brew install brew-cask #if not already installed
brew cask install vagrant
brew tap homebrew/completions
brew install vagrant-completion

```

docker-root

```
$ git clone https://github.com/ailispaw/docker-root
$ cd docker-root
$ make vagrant
$ make

```

vagrant配置文件

```
$ vagrant init
$ vim Vagrantfile
config.vm.box = "ailispaw/docker-root"
config.vm.network "forwarded_port", guest: 9999, host: 1234
config.vm.synced_folder "/Users/ce/workspace/docker/data", "/home/docker/data"
config.vm.provider "virtualbox" do |vb|
    vb.memory = "512"
    vb.cpus = "4"
end

```

启动并ssh

```
vagrant up && vagrant ssh
```

docker

```
[docker@docker-root ~]$ sudo docker pull alpine
[docker@docker-root ~]$ sudo docker pull ubuntu
[docker@docker-root ~]$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
alpine              latest              3e467a6273a3        2 days ago          4.793 MB
ubuntu              latest              17b6a9e179d7        5 days ago          120.7 MB
```

容器

```
docker create -ti -h teach --name teach -v /home/docker/data:/root/data -w /root ubuntu /bin/bash
```

查看所有容器
```
docker ps -a -q
```

删除容器

```
docker rm container_id
```

```
docker attach
```

安装必备工具

```
sudo apt-get install gcc gdb binutils make git dstat sysstat htop curl wget readelf objdump pidstat
```




