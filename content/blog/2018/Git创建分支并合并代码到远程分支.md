---
title: "Git创建分支并合并代码到远程分支"
date: 2018-08-08 12:05:38
draft: false
---
打开git命令窗口后，首先切换到你项目的目录下，下图是我的项目目录

![](https://img-blog.csdn.net/20180808115253866?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

后面括号中的是代表当前所处的分支，当前应该在自己的本地分支

然后使用 git status查看修改了哪些地方

![](https://img-blog.csdn.net/20180808115507285?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

如图所示，红色的都是修改过的文件

使用git add . 来将修改的提交到缓存区，这时候再使用git status查看会发现修改的都由红色变成绿色的了

![](https://img-blog.csdn.net/20180808115750627?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

git commit -m "你要写的注释"

然后切换到你要推送代码的远程分支， git checkout 分支名

git pull 拉取远程分支的最新代码

![](https://img-blog.csdn.net/20180808120105558?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

然后切换到自己的本地分支，合并代码

git checkout 本地分支名

git rebase 远程分支名

![](https://img-blog.csdn.net/20180808120508565?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

最后切换到远程分支合并代码并推送

git checkout 本地分支名

git merge 本地分支名

git push

![](https://img-blog.csdn.net/20180808120524661?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

大功告成

回退回修改之前的状态:

只要没有执行push操作，都可以修改回来，在自己分支下执行 git reset --hard即可