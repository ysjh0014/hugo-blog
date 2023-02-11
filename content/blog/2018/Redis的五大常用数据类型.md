---
title: "Redis的五大常用数据类型"
date: 2018-08-04 09:48:52
draft: false
---
Redis命令大全： [redisdoc.com](http://redisdoc.com)

键（键）

keys /*：查找当前数据库的所有密钥

exists key的名字：判断某个key是否存在

move key的名字数据库：移动当前的某个key到指定数据库

expire key的名字时间：给指定key设置过期时间

ttl key的名字：查看key还有多少时间过期，-1表示永不过期，-2表示已经过期

type key的名字：查看key的类型

del key的名字：删除某个键

flushall : 删除当前数据库的所有key

### String（单值单value）

追加键值：在已有的key后面追加值

strlen key：返回键对应的字符串的长度

下面几个命令只能对数字：

-------------------------------------------------- ---------

incr key：每次加一

decr key：每次减一

incrby key要加的数字：每次加指定数字

decrby key要减数数字：每次减指定数字

-------------------------------------------------- ---------

getrange：获取指定区间范围内的值，类似于......和.....的关系，从0到-1显示全部

setrange：设置指定区间内的值

setex key second value：创建密钥并设置过期时间，如果密钥已经存在则覆盖旧值

setnx key value：将key的值设为值，如果key存在，则不做任何动作，相当于set if not exist的简写

mset / mget：同时设置多个键值对/同时查看多个键

msetnx：同时将多个键的值设为值，但是这里只要有一个键是存在的所有的都会设置失败

### List（单值多value）

lpush key value： 将一个或多个value插入到列表key的表头

rpush key value: 将一个或多个value插入到列表key的表尾

lrange key start stop: 返回列表key中指定区间内的元素

lpop/rpop key: 移除并返回列表key的头元素/尾元素

lindex key index: 返回列表key中，下标为index的元素

llen key: 返回列表key的长度

lrem key: 移除列表中count个value元素

ltrim key start stop: 保留列表内指定区间的元素

lset key index value: 将列表key中下标为index的元素值设置为value

linsert key before/after pivot value: 将值value插入到列表key中，位于值pivot之前或之后

性能总结:

它是一个字符串链表，left,right都可以插入添加

如果键不存在，创建新的链表

如果键已存在，新增内容

如果值全移除，对应的键也消失了

链表的操作无论是头和尾效率都极高，但如果是对中间元素进行操作，效率就很低了

### Set集合(单值多value)

sadd key member: 将一个或多个member元素加入到集合key中,已经存在的member元素将被忽略,即重复的member值将被忽略

smembers key: 返回集合key中的所有成员

sismemeber key memeber: 判断member元素是否是集合key的成员

scard key: 返回集合中元素的数量

srem key member: 移除集合key中的一个或多个member元素，不存在的member元素会被忽略

spop key: 移除并返回集合中的一个随机元素(随机出栈)

smove key1 key2 member: 将member元素从key1集合移动到key2集合

sdiff key1 key2: 差集，在第一个set里面而不在第二个set里面的元素

sinter key: 交集

sunion key: 并集

### Hash(KV不变，但V是一个键值对)

hset key field value: 将哈希表key中的域field的值设为value

hget key field: 返回哈希表key中域field的值

hmset key field value: 同时将多个field-value对设置到哈希表key中

hmget key field: 返回哈希表中一个或多个域field的值

hgetall key:返回哈希表中所有的域和值

hdel key field: 删除哈希表中key的一个或多个域

hlen key:返回哈希表key中域的数量

hexists key field: 查看哈希表key中，给定域field是否存在

hkeys key: 返回哈希表key的所有域

hvals key: 返回哈希表key中所有域的值

hsetnx key field value: 将哈希表key中域field的值设为value,当且仅当域field不存在时

### Zset有序集合(SortedSet)

zadd key score member: 将一个或多个memeber元素及其score值加入到有序集合key中，注意这里是有序集合，即score值必须是有序的

zrange key start stop: 返回有序集合key中，指定区间内的成员，并按score值从小到大来排序

zrem key member: 移除有序集key中一个或多个成员

zcard key: 返回有序集key的基数

zcount key min max: 返回有序集key中，score值在min和max之间(默认包括score值等于min或max)的成员的数量