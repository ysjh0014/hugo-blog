---
title: "博客迁移到hugo"
date: 2023-01-12 15:24:33
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/hugo.jpg"
---
##### 为什么迁移，又为什么是Hugo？
2020年之前我的博客主阵地还是CSDN，但是相信大家都有程序员苦CSDN久矣的感受。全是复制粘贴雷同文章非常多，只能解决一些很常见的问题，最最重要的是它还有广告
那我又为什么选择hugo呢，

##### 一、为什么是Hugo
Hugo 的优点：

Go语言编写，只有一个可执行二进制文件，生成速度极快
主题大多都比较简洁，可以突出内容
完全是静态博客，没有后端，通过源文件生成HTML文件，杜绝一切黑客攻击，静态页面访问速度也比较快。
主题大部分都比较简单，我一个不怎么会前端的，折腾折腾折腾也能魔改。。
渲染引擎支持完整的 Markdown 语法与数学公式
可以 Servless ，静态页面托管在任何地方都比较方便。
源代码可以放在 GitHub 上，有版本控制，通过 GitHub Actions 可以实现静态博客自动部署。
不需要后端，不需要数据库，维护起来及其方便。
省钱，如果使用 GitHub Pages 的方法，则完全可以不要钱，通过各大云服务厂商的静态页面托管，按量付费，也很便宜。比云服务器不知道便宜哪里去了。
##### 二、如何迁移
> 1.首先安装hugo

首先在hugo的github地址上下载所需要的版本，然后解压到本地目录下，并配置系统环境变量，可以通过hugo version来检测hugo是否安装成功
> 2.选取主题

可以从hugo主题网站上选择，主题丰富，总有一款适合你！
> 3.爬取csdn上老博客为markdown格式

github上有很多开源的项目可以实现，下面给两个参考项目：
https://github.com/sp4rkw/CSDNtoHEXO
https://github.com/DmrfCoder/CsdnBlogToHexo   
具体可以根据项目中的提示进行微调
> 4.解决csdn老博客图片访问问题

直接爬取下来的csdn文章中的图片地址是不能直接访问的(主要是csdn做了限制)
下载的csdn文章中的图片应该如下：
![](https://img-blog.csdnimg.cn/201905171006208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10....)可以替换为
```<img referrerpolicy="no-referrer" src='#这里放csdn图片的地址'>```
就可以访问了，当然如果在服务器上做一个图床，然后在第3步中的python项目进行修改，将csdn中的图片下载到下来并且替换为图床地址最好
##### 三、使用GitHub Action自动化部署hugo到ESC
具体可以参考：https://www.yulan.work/posts/github-actions-automatically-deploy-hugo-to-ecs/
首先你要在GitHub上创建一个新的仓库，然后提交本地hugo到仓库中，具体可以自行百度，因为GitHub在国外的原因，每次提交代码成功与否全看运气，所以建议配置GitHub ssh key，这个也可以自行百度，很简单。
提交到GitHub仓库之后，接下来需要在GitHub中创建几个Actions secrets，具体如下：
> HOST：云服务器域名 或 IP 地址
> SSH_USERNAME：登录用户
> SSH_PASSWORD：用户密码
> SSH_PORT：服务端口号
##### 四、评论系统的选取与部署
评论系统选取的是twikoo，部署简单，易用，可以参考官方文档```https://twikoo.js.org/quick-start.html```

我是部署在docker里面，非常简单，一条命令即可。然后在你的hugo项目的config.toml中配置twikoo的地址即可，如下：
```
enableTwikoo = true 
twikooEnvId = "http://..."
```
##### 五、总结
经过好几天的折腾，我成功了，新的博客看起来还不错。但是为了保险起见，后续的文章可能还是会在csdn和Hugo各发一遍

