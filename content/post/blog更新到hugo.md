---
title: "Blog更新到hugo"
date: 2020-02-29 15:04:07
draft: false
tags: [blog, hugo]
categories: [blog, hugo]
---

其实好久没有更新博客了，但是这段时间并没有放弃和总结一些东西，只不过都是草草了事，也没做总结，不方便拿出来分享。一直在创业和输出的阶段，时间上确实也紧了一些。最近由于疫情的关系一直在休息，恰巧想起自己的 blog，闲来无事折腾一番，把 hexo 替换成了 hugo。如下是替换过程，一个是作为记录，一个是方便有缘人吧。

# 安装

在 mac 上安装 hugo

```shell
$ brew install hugo
```

然后建立站点

```bash
hugo new site blog
```

安装主题，我采用比较简单的 even 主题


```bash
$ git clone https://github.com/olOwOlo/hugo-theme-even themes/even
```

## 设置 Archetypes
Archetypes，原型，其实也可以理解为模板。
在使用hugo new PAGE_NAME.md来创造一个新页面时，会根据archetypes目录下的模板，来生成新文件。
对于复杂的Front Matter，每次都手写，会很不方便。 一般可以把所需的变量及其默认值，放在Archetypes文件中。
本文生成时的 archetypes/default.md 文件，内容如下：
```
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ now.Format "2006-01-02 15:04:05" }}
draft: false
tags: [Django, Python, Linux]
categories: [Django, Python, Linux]
---
```
大部分是为了兼容我之前写博客时候的使用习惯，比如 new 一篇新文章的时候 date 是现在的时间。

## 配置站点信息
然后对于自己博客的信息和自己想对博客的一些自定义，可以编辑 config.toml 文件，hugo 提供了一些可配置功能。
## 预览
使用 hugo server 命令，用浏览器打开 http://127.0.0.1:1313/ 就能看到自己博客的预览了
## 发文 
如果想要写新文章 使用命令：

```bash
hugo new post/blog更新到hugo.md
```
这样在 blog 下面的 content 下面的 post 里面就新建了 blog更新到hugo.md 文件，并且里面包含了模板里预留的信息。
# 迁移

整体迁移起来比较简单，其实就是对 md 文件的 copy，然后遵循 hugo 解析 Front Matter 的方式就好。其实就是为了把原来 hexo 的格式改成 hugo 兼容的格式，如果文件比较多的话可以写一个脚本去修改。

迁移文章时必须做到不影响以前文章的 URL，不然会影响以前发出去的链接，如果别人访问的话就会变成 404 了，而且会对 SEO 有影响。

我博客的
```
xxxxx.md
yyyyy.md
zzzzz.md
```

然后配置 Hexo 会生成以下的 URL：
```
/2017/01/02/xxxxx
/2017/03/04/yyyyy
/2017/05/06/zzzzz
```
为了兼容这种格式我们编辑 config.toml 文件：
```toml
[frontmatter]
date = [":filename", ":default"]

[permalinks]  # 兼容 hexo 的时间格式
  post = "/:year/:month/:day/:filename/"
```

# 评论
博客还是需要一个评论的，自从多说挂了我就没怎么折腾过，关注过 girment，本想使用他但是还是考虑到安全问题，毕竟私钥和 client_id 都明文写到了配置文件或者前端文件上了。

通过评估最后还是便捷的 [utterances](https://github.com/utterance/utterances) 打动了我，它安全又方便。


## 安装
进入 [https://utteranc.es/](https://utteranc.es/) 进行安装即可，我这是单独简历了一个 public repo，而没使用自己博客默认的 repo。
 
## 配置

我使用的主题 even 支持 utterances，编辑 config.toml

```
[params.utterances]       # https://utteranc.es/
    owner = "zbing3"              # Your GitHub ID
    repo = "opslinux-comment"     # The repo to store comments
```

# 部署
部署我一直习惯自己之前的方案 在 github repo 里面 master 分支保存我整个博客程序作为备份，或者切换多电脑时候的同步，使用 gh-pages 分支来放 Markdown 生成的静态文件。
[这篇文章](https://zhuanlan.zhihu.com/p/37752930)很好的解决了，我迁移过后的部署问题。

# 发布到 master
第一次部署，需要新建一个 public 文件夹，并将原来部署了 Hexo 博客的 repo 关联上，步骤如下：

```shell
$ mkdir public
$ hugo -t even
$ cd public
$ git init
$ git remote add upstream git@github.com:zbing3/opslinux.git
$ git add .
$ git commit -m "switch to hugo"
$ git push -f upstream master
```
如果之前的 repo 里面有老代码， 使用 `git push -f upstream master -f` 进行强行push

这样你的博客程序就全都 push 到 master 分支了，如果换了电脑，可以从这个 repo 里面进行 pull 到最新的程序。

# 发布到 gh-pages branch
接着我们就来使用 `gh-pages` branch 的方法托管到 Github Pages
先将 `/public` 子目录添加到 `.gitignore` 中，让 `master` branch忽略其更新，然后在本地和Github端添加一个名为 `gh-pages` 的 branch ：

```shell
//忽略public子目录
echo "public" >> .gitignore
//初始化gh-pages branch
git checkout --orphan gh-pages
git reset --hard
git commit --allow-empty -m "Initializing gh-pages branch"
git push origin gh-pages
git checkout master
```
为了提高每次发布的效率，可以将下述命令存在脚本中，每次只需要运行该脚本即可将 `gh-pages` branch中的文章发布到 Github 的 repo 中：

```shell
#!/bin/sh

if [[ $(git status -s) ]]
then
    echo "The working directory is dirty. Please commit any pending changes."
    exit 1;
fi

echo "Deleting old publication"
rm -rf public
mkdir public
rm -rf .git/worktrees/public/

echo "Checking out gh-pages branch into public"
git worktree add -B gh-pages public origin/gh-pages

echo "Removing existing files"
rm -rf public/*

echo "Generating site"
hugo

echo "Updating gh-pages branch"
cd public && git add --all && git commit -m "Publishing to gh-pages (publish.sh)"

echo "Push to origin"
git push origin gh-pages
```
保存名为 deploy.sh，然后 `chmod a+x deploy.sh` 加上可执行权限。
以后就可以将博客程序和源文档 push 到 master branch中，部署的时候就执行 deploy.sh 脚本把网页 push 到 gh-pages branch中，这样页面就进行更新了。

> 准备之后还要改下脚本，在进行 deploy 部署的时候，把博客程序自动备份到 master 上。

# 参考
https://blindwith.science/2019/08/447.html/
https://scarletsky.github.io/2019/05/02/migrate-hexo-to-hugo/
https://zhuanlan.zhihu.com/p/37752930
https://rileyng.github.io/post/hugo-utter/
