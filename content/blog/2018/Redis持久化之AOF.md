---
title: "Redis持久化之AOF"
date: 2018-08-08 16:51:34
draft: false
---
因为RDB持久化方式有可能会丢失最后几分钟的数据，所以出现了AOF来弥补这个缺陷，AOF是将每一个写操作命令都追加到一个文件中，回复的时候直接用这个文件进行恢复，因此最多丢失一秒的数据，

### AOF(Append only file)简介:

以日志的形式来记录每个写操作，将redis执行过的所有写指令记录下来(读操作不记录)，只许追加文件不许改写文件，redis启动之初会读取该文件重新构建数据换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作

Redis中的AOF持久化方式与RDB不一样，RDB是redis自己生成并写入的，但AOF默认是关闭的，因此需要先开启AOF

![](https://img-blog.csdn.net/20180807092149238?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

将appendonly no改为yes,AOF持久化就会开启

AOF和RDB是可以共存的，但是默认情况下是会先加载AOF，如果对AOF的appendonly.aof文件进行不正常的修改，这时候redis就会启动失败，因为AOF持久化方式的优点，所以如果出现网络故障，丢包，延迟，病毒等外部因素时，ap[pendomly.aof文件可能会写入一些不正确的命令，这时候可以使用redis-check-aof --fix appendonly.aof的命令，来修复appendomly.aof文件

### AOF持久化的配置:

appendonly: aof的开启

appendfilename: aof的文件名

appendfsync:

always: 同步持久化，每次发生数据变更会被立即记录到磁盘，性能较差但数据完整性较好

everysec: redis默认设置，异步操作，每秒记录，如果一秒内宕机，有数据丢失

no: 从不同步

no-appendfsync-on-rewrite： 重写是是否可以运用appendfsync,用默认no即可，保证数据安全性

### AOF的rewrite(重写):

AOF采用文件追加方式，文件会越来越大，为避免此类情况，新增了重写机制，当AOF文件的大小超过了所设定的阈值时，Redis就会启动AOF文件的内容压缩，只保留可以恢复数据的最小指令集，可以使用命令bgrewriteaof

重写原理:AOF文件持续增长而过大时，会fork出一条新进程来将文件重写(也是先写临时文件再rename),遍历新进程的内存中数据，每条记录有一条的set语句，重写AOF文件的操作，并没有读取旧的aof文件，而是将内存中的数据库内容用命令的方式重写了一个新的aof文件，这点和快照有点类似

触发机制: redis会记录上次重写的aof文件的大小，默认配置是当aof文件大小是上次重写后大小的一倍且文件大于64M时触发

AOF的劣势:

相同数据集的数据而言aof文件要远远大于aof文件，恢复速度慢于rdb

aof运行效率要慢于rdb，每秒同步策略效率较好，不同步效率和rdb相当

![](https://img-blog.csdn.net/20180808165122223?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

官网建议:

RDB持久化方式能够在指定的时间间隔对你的数据进行快照存储

AOF持久化方式记录每次对服务器写的操作，当服务器重启时会重新执行这些命令来恢复原始的数据，AOF命令以Redis协议追加保存每次写的操作到文件末尾，Redis还能对AOF文件进行后台重写，使得AOF文件的体积不至于过大

只做缓存: 如果你只希望你的数据再服务器运行的时候存在，你也可以不使用任何持久化方式