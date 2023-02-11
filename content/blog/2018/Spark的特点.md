---
title: "Spark的特点"
date: 2018-10-08 19:46:00
draft: false
---
**1.快**

没有接触Spark之前就听说Spark快，比Hadoop快很多很多

与Hadoop的MapReduce相比，Spark基于内存的运算要快100倍以上，基于硬盘的运算也要快10倍以上，Spark实现了高效的DAG执行引擎，可以通过基于内存来高效处理数据流

![](https://img-blog.csdn.net/20181008194244594?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**2.易用**

Spark支持Java、Python和Scala的API，还支持超过80种高级算法，使用户可以快速构建不同的应用，而且Spark支持交互式的Python和Scala的shell，可以非常方便地在这些shell中使用Spark集群来验证解决问题的方法

**3.通用**

Spark提供了统一的解决方案，Spark可以用于批处理、交互式查询（Spark SQL）、实时流处理（Spark Streaming）、机器学习(Spark MLlib)和图计算(GraphX)，这些不同类型的处理都可以在同一个应用中无缝使用，Spark统一的解决方案非常具有吸引力，毕竟任何公司都想用统一的平台去处理遇到的问题，减少开发和维护的人力成本和部署平台的物力成本

**4.兼容性**

Spark可以非常方便地与其他的开源产品进行融合。比如，Spark可以使用Hadoop的YARN和Apache Mesos作为它的资源管理和调度器，并且可以处理所有Hadoop支持的数据，包括HDFS、HBase和Cassandra等。这对于已经部署Hadoop集群的用户特别重要，因为不需要做任何数据迁移就可以使用Spark的强大处理能力。Spark也可以不依赖于第三方的资源管理和调度器，它实现了Standalone作为其内置的资源管理和调度框架，这样进一步降低了Spark的使用门槛，使得所有人都可以非常容易地部署和使用Spark。此外，Spark还提供了在EC2上部署Standalone的Spark集群的工具