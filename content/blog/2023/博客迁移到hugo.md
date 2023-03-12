---
title: "博客迁移到hugo"
date: 2023-02-12 15:24:33
draft: false
---
##### 为什么迁移，又为什么是hugo？
2020年之前我的博客主阵地还是CSDN，但是相信大家都有程序员苦CSDN久矣的感受。全是复制粘贴雷同文章非常多，只能解决一些很常见的问题，最最重要的是它还有广告
那我又为什么选择hugo呢，

##### 一、为什么迁移

##### 二、如何迁移
> 1.首先安装hugo

首先在hugo的github地址上下载所需要的版本，然后解压到本地目录下，并配置系统环境变量，可以通过hugo version来检测hugo是否安装成功
> 2.选取主题

可以从hugo主题网站上选择，主题丰富，总有一款适合你！
> 3.爬取csdn上老博客为markdown格式

> 4.解决csdn老博客图片访问问题

##### 三、使用GitHub Action自动化部署hugo到ESC
具体可以参考：https://www.yulan.work/posts/github-actions-automatically-deploy-hugo-to-ecs/
首先你要在GitHub上创建一个新的仓库，然后提交本地hugo到仓库中，具体可以自行百度，因为GitHub在国外的原因，每次提交代码成功与否全看运气，所以建议配置GitHub ssh key，这个也可以自行百度，很简单。
提交到GitHub仓库之后，接下来需要在GitHub中创建几个Actions secrets，具体如下：
> HOST：云服务器域名 或 IP 地址
> SSH_USERNAME：登录用户
> SSH_PASSWORD：用户密码
> SSH_PORT：服务端口号
##### 四、评论系统的选取与部署

##### 五、一些自己的看法

>>>>>>> cbf3b25a1819765d8127989740a17d4f871d42a3

