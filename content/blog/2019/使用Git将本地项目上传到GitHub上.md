---
title: "使用Git将本地项目上传到GitHub上"
date: 2019-05-16 15:01:04
draft: false
---
前提：

此时你已经有了自己的GitHub账户和Git，并且本地Git和远程GitHub已经关联起来了

在本地创建一个空的文件夹，将你的项目放到这个文件夹里面，然后打开git

依次输入以下命令
touch README.md git init git add . git commit -m "注释" git remote add origin github仓库地址（origin是别名） git pull --rebase origin master git push -u origin master

这样就将本地项目添加到github上的仓库上了

如果想要修改项目该怎么提交哪？

这时候进入到本地仓库文件夹，然后依次输入以下命令
git add . git commit -m "注释" git push -u origin master