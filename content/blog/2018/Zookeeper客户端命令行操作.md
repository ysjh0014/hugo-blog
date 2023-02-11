---
title: "Zookeeper客户端命令行操作"
date: 2018-08-23 21:45:45
draft: false
---
启动客户端:

bin/zkCli.sh

显示所有操作命令:

help

![](https://img-blog.csdn.net/20180821173803167?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

查看当前节点中所包含的内容:

ls /

![](https://img-blog.csdn.net/20180821173831376?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

查看当前节点内容和详细信息:

ls2 /

![](https://img-blog.csdn.net/20180821173910470?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

获取节点的值:

get /节点名

![](https://img-blog.csdn.net/20180821174012611?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

创建普通节点:

create /ys ysjh0014.cn

![](https://img-blog.csdn.net/20180821174316199?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

创建短暂节点:

create -e /ys ysjh0014.cn

使用quit退出客户端后该节点就会被删除

创建带序号的节点:

先创建一个普通的根节点 create /ys ysjh0014.cn

然后创建带序号的节点 create /ys/jh ysjh create /ys/ys ysjh

![](https://img-blog.csdn.net/20180823191947484?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

修改节点数据:

set /csdn 666

删除节点:

delete /csdn

递归删除节点:(递归删除所有的节点，包括子节点)

rmr /csdn

查看节点状态:

stat /csdn

监听节点值的变化:

get /ys/jh watch 这里只要这个节点值变化，就会得到相应，但是只能有一次响应，即节点值改变一次之后就不会再监听

监听节点的子节点变化(路径变化)

ls /path watch 只要路径变化就会相应，同样的只会响应一次

查看节点状态:

stat /csdn