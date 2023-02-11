---
title: "Hive文件存储格式"
date: 2018-09-08 20:04:18
draft: false
---
Hive 支持的存储数的格式主要有:textfile sequencefile orc parquet

**1.列式存储和行式存储**

右边第一个为行式存储,第二个为列式存储

行存储的特点: 查询满足条件的一整行数据的时候,列存储则需要去每个聚集的字段找到对应的每个列的值,行存储只需要找到其中一个值,其余的值都在相邻地方,所以此时
行存储查询的速度更快。列存储的特点: 因为每个字段的数据聚集存储,在查询只需要少数几个字段的时候,能大大减少读取的数据量;每个字段的数据类型一定是相同的,列式存储可以针对性的设计更好的设计压缩算法

textfile 和 sequencefile 的存储格式都是基于行存储的

orc和 parquet 是基于列式存储的

**2.TestFile格式**

默认格式,数据不做压缩,磁盘开销大,数据解析开销大。可结合 Gzip、Bzip2 使用(系统自动检查,执行查询时自动解压),但使用这种方式,hive 不会对数据进行切分,从而无法对数据进行并行操作

**3.Orc格式**

Orc (Optimized Row Columnar)是 hive 0.11 版里引入的新的存储格式
可以看到每个 Orc 文件由 1 个或多个 stripe 组成,每个 stripe250MB 大小,这个 Stripe实际相当于 RowGroup 概念,不过大小由 4MB->250MB,这样应该能提升顺序读的吞吐率。每个 Stripe 里有三部分组成,分别是 Index Data,Row Data,Stripe Footer：

1)Index Data:一个轻量级的 index,默认是每隔 1W 行做一个索引。这里做的索引应该只是记录某行的各字段在 Row Data 中的 offset。
2)Row Data:存的是具体的数据,先取部分行,然后对这些行按列进行存储。对每个列进行了编码,分成多个 Stream 来存储。
3)Stripe Footer:存的是各个 Stream 的类型,长度等信息。每个文件有一个 File Footer,这里面存的是每个 Stripe 的行数,每个 Column 的数据类型信息等;每个文件的尾部是一个 PostScript,这里面记录了整个文件的压缩类型以及 FileFooter的长度信息 等。在读取文件时,会 seek 到文件尾部读 PostScript,从里面解析到 File Footer长度,再读 FileFooter,从里面解析到各个 Stripe 信 息,再读各个 Stripe,即从后往前读

**4.Parquet格式**

Parquet 是面向分析型业务的列式存储格式,由 Twitter 和 Cloudera 合作开发,2015 年 5月从 Apache 的孵化器里毕业成为 Apache 顶级项目。

Parquet 文件是以二进制方式存储的,所以是不可以直接读取的,文件中包括该文件的数据和元数据,因此 Parquet 格式文件是自解析的。

通常情况下,在存储 Parquet 数据的时候会按照 Block 大小设置行组的大小,由于一般情况下每一个 Mapper 任务处理数据的最小单位是一个 Block,这样可以把每一个行组由一个 Mapper 任务处理,增大任务执行并行度。Parquet 文件的格式如下图所示

上图展示了一个 Parquet 文件的内容,一个文件中可以存储多个行组,文件的首位都是该文件的 Magic Code,用于校验它是否是一个 Parquet 文件,Footer length 记录了文件元数据的大小,通过该值和文件长度可以计算出元数据的偏移量,文件的元数据中包括每一个行组的元数据信息和该文件存储数据的 Schema 信息。除了文件中每一个行组的元数据,每一页的开始都会存储该页的元数据,在 Parquet 中,有三种类型的页:数据页、字典页和索引页。数据页用于存储当前行组中该列的值,字典页存储该列值的编码字典,每一个列块中最多包含一个字典页,索引页用来存储当前行组下该列的索引,目前 Parquet 中还不支持索引页