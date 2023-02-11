---
title: "CMD命令行模式下使用Scala进行词频统计"
date: 2018-10-04 20:51:15
draft: false
---
1.首先创建一个数组

![](https://img-blog.csdn.net/20181004204219441?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

2.对这个数组进行flatMap操作(等于先 map操作后进行 flatten 操作)

![](https://img-blog.csdn.net/20181004204417206?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

3.要进行统计就需要进行分组

![](https://img-blog.csdn.net/20181004204541335?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以看出分组之后变为了Map集合，其中都是key-value对

4.将value取出来并统计其长度

![](https://img-blog.csdn.net/20181004204740585?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

5.将结果转换为List集合

![](https://img-blog.csdn.net/20181004204839289?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

6.进行排序

![](https://img-blog.csdn.net/20181004205011168?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

sortBy(x=>-x._2)为降序