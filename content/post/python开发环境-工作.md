---
title: python开发环境
date: 2016-05-25 14:05:18
categories: [python]
tags: 
  - python
  - develop

---

操作系统：mac OSX 10.11 或 Ubuntu 16.04  
编辑器： vim、 sublime、PyCharm

# python环境

版本： python3.5.1

## 安装工具

###1. pyenv

#### 安装 pyenv

**linux:**

```
$ git clone git://github.com/yyuu/pyenv.git ~/.pyenv
$ echo 'export PYENV_ROOT="$HOME/.pyenv"'>> ~/.bashrc # 指明环境变量
$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"'>> ~/.bashrc
$ echo 'eval"$(pyenv init -)"' >> ~/.bashrc  # 开启shims and autocompletion
$ exec $SHELL -l  # 重新启动shell让其生效 
```

**mac:**

```
$ brew update
$ brew install pyenv //安装
$ brew upgrade pyenv //升级
$ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile //只需要执行一次即可
```

#### 查看可安装的版本

```
$ pyenv install --list
```
####  安装指定版本

```
$ pyenv install 3.5.1 -v
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

### 2. virtualenvwrapper


**linux**

```
$ pip install virtualenvwrapper
$ git clone https://github.com/yyuu/pyenv-virtualenvwrapper.git ~/.pyenv/plugins/pyenv-virtualenvwrapper
```

**mac**

```
$ pip install virtualenvwrapper
$ brew install pyenv-virtualenvwrapper
```

#### 使用python3.5创建一个虚拟环境

```
$ mkvirtualenv env2 -p $(which python3.5)
$ workon env2
```

### direnv

#### 安装

```
git clone https://github.com/direnv/direnv
cd direnv
make install
```
### 配置

设置全局配置文件

vim .direnvrc

```
use_venv () {
      export VIRTUAL_ENV = "${HOME} /.virtualenvs/${1}"
      PATH_add "$VIRTUAL_ENV/bin"
  }

 use_vwrapper () {
     source /usr/local/bin/virtualenvwrapper.sh
 }

 use_python() {
       local python_root=$HOME/.pyenv/versions/$1
       load_prefix "$python_root"
       layout_python "$python_root/bin/python"
 }
```

在项目中创建.envrc文件

```
layout python
use vwrapper
workon env2
```

https://github.com/direnv/direnv/wiki/Python











