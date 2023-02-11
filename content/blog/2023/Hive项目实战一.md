---
title: "Hive项目实战一"
date: 2018-09-10 20:56:13
draft: false
---
**1.需求描述**

统计某视频网站的常规指标,各种 TopN 指标:
视频观看数 Top10
视频类别热度 Top10
视频观看数 Top20 所属类别包含这 Top20 视频的个数
视频观看数 Top50 所关联视频的所属类别的热度排名
每个类别中的视频热度 Top10，以Music为例
每个类别中视频流量 Top10，以Music为例
上传视频最多的用户 Top10 以及他们上传的视频
每个类别视频观看数 Top10

**2.数据源结构说明**

数据源1： user.txt

数据样例:
barelypolitical 151 5106 bonk65 89 144 camelcars 26 674

数据样例中的三个字段结构：

上传者用户名 string 上传视频数 int 朋友数量 int

数据源2： video.txt

数据样例：
fQShwYqGqsw lonelygirl15 736 People & Blogs 133 151763 3.01 666 765 fQShwYqGqsw LfAaY1p_2Is 5LELNIVyMqo vW6ZpqXjCE4 vPUAf43vc-Q ZllfQZCc2_M it2d7LaU_TA KGRx8TgZEeU aQWdqI1vd6o kzwa8NBlUeo X3ctuFCCF5k Ble9N2kDiGc R24FONE2CDs IAY5q60CmYY mUd0hcEnHiU 6OUcp6UJ2bA dv0Y_uoHrLc 8YoxhsUMlgA h59nXANN-oo 113yn3sv0eo

数据样例中的字段结构：

视频唯一 id 11 位字符串 视频上传者 上传视频的用户名 String 视频年龄 视频上传日期和 2007 年 2 月
15 日之间的整数天 视频类别 上传视频指定的视频分类 视频长度 整形数字标识的视频长度 观看次数 视频被浏览的次数 视频评分 满分 5 分 流量 视频的流量,整型数字 评论数 一个视频的整数评论数 相关视频 id 相关视频的 id,最多 20 个

上面只是拿出了一两条数据来介绍数据集的结构，在后续项目中要用到的数据集可以自行下载

链接: [https://pan.baidu.com/s/1sDoLsG3JUwi8BPleL2jzWQ](https://pan.baidu.com/s/1sDoLsG3JUwi8BPleL2jzWQ) 密码: tgn5

数据集下载后可能不容易看出分隔符，可以用notepad++打开然后选择显示所有字段