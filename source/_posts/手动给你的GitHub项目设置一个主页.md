title: 手动给你的GitHub项目设置一个主页
date: 2015-10-23 22:46:17
categories: 开发工具
tags: GitHub
---

你在GitHub上建立的每个项目(repository)都是可以拥有独立的主页的， 如果你还不知道如何完成这件事，希望这篇文章对你有所帮助。

<!--more-->

GitHub官网有英文版的教程， [Creating Project Pages manually](https://help.github.com/articles/creating-project-pages-manually/)。如果你不喜欢读英文的文章，那么就继续往下读吧。



## 克隆
项目的主页需要在当前项目上创建一个特殊的、独立的分支，先克隆项目：

```shell
    $ git clone github.com/user/repository.git

    # 克隆需要创建主页的项目
    Cloning into 'repository'...
    remote: Counting objects: 2791, done.
    remote: Compressing objects: 100% (1225/1225), done.
    remote: Total 2791 (delta 1722), reused 2513 (delta 1493)
    Receiving objects: 100% (2791/2791), 3.77 MiB | 969 KiB/s, done.
    Resolving deltas: 100% (1722/1722), done.
```

## 建立gh-pages分支
克隆完成后建立分支，注意分支的名字必须是__gh-pages__：

```shell
    $ cd repository

    $ git checkout --orphan gh-pages
    # 创建用于存放主页内容的分支，注意使用 --orphan
    # 切换到分支 'gh-pages'

    $ git rm -rf .
    # 清除工作目录下所有的内容
```

## 添加内容并提交
现在你有了一个空的工作目录，现在要做的是往这个目录里添加你主页的内容(html/js/css)，下面这段可以供你测试：
```shell
    echo "My Page" > index.html
    git add index.html
    git commit -a -m "First pages commit"
    git push origin gh-pages
```

此时你项目的主页应该已经可以访问了。如果这步操作失败了，你注册GitHub的邮箱应该会收到一封提示邮件。

## 访问主页
尝试访问吧，链接是这样的
```shell
http(s)://<username>.github.io/<projectname>
```
注意，不管你的项目仓库是否是私有的，这个主页都是对所有人开放的。


--END--

_赵彪原创，转载请注明出处_









