---
title: "Redis持久化之RDB"
date: 2018-08-06 15:26:43
draft: false
---
### RDB(Redis DataBase)简介:

RDB持久化方式是指在指定时间间隔内将内存中的数据集快照写入磁盘，也就是Snapshot快照，它恢复时是将快照文件直接读到内存中,Redis会单独创建(fork)一个子进程来进行持久化，会先将数据写入到一个临时文件中，等到持久化过程结束，再用这个临时文件替换上次持久化好的文件，整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能，如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效，RDB的缺点是最后一次的数据可能会丢失

### Fork:

fork的作用是复制一个与当前进程一样的进程，新进程的所有数据(变量，环境变量，程序计数器等)数值都和原进程一样，但这是一个全新的进程，并作为原进程的子进程

### 快照(Snapshotting)

在默认情况下， Redis 将数据库快照保存在名字为 dump.rdb的二进制文件中,这个文件默认保存在启动redis的目录下，你可以对 Redis 进行设置， 让它在“ N 秒内数据集至少有 M 个改动”这一条件被满足时， 自动保存一次数据集。你也可以通过调用 SAVE或者 BGSAVE ， 手动让 Redis 进行数据集保存操作，这种持久化方式被称为快照 snapshotting

![](https://img-blog.csdn.net/20180807085054427?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

save与bgsave的区别:

save时只管保存，其他不管，全部阻塞

bgsave: redis会在后台异步进行快照操作，快照同时还可以响应客户端请求，可以通过lastsave命令获取最后一次成功执行快 照的时间

执行flushall命令也会更新dump.rdb文件，只不过里面是空的，无意义

如果想禁用RDB的持久化策略，只需要不设置任何save命令，或者给save传一个空字符串(save " ")即可

### RDB持久化方式的优势与劣势:

优势: 适合大规模的数据恢复

对数据完整性和一致性要求不高

劣势：在一定时间间隔做一次数据备份，所以如果redis意外down掉的话，就会丢失最后一次快照后的所有修改

fork的时候，内存中的数据被克隆了一份，大约2倍的膨胀需要考虑

![](https://img-blog.csdn.net/20180807085013332?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)