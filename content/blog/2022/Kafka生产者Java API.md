---
title: "Kafka生产者Java API"
date: 2022-09-22 23:05:43
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/kafka.png"
---
准备工作：

maven工程，zookeeper集群

**1.开启Kafka集群，这里可以参考我之前的文章，里面有详细的教程**

**2.Java API编程**

maven的pom.xml文件

```java
<dependencies>
    <dependency>
        <groupId>org.apache.kafka</groupId>
        <artifactId>kafka-clients</artifactId>
        <version>2.0.0</version>
    </dependency>
</dependencies>
```

ProducerTest.java

```java
package cn.ysjh;

import java.util.Properties;
import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerRecord;
public class ProducerTest {
    public static void main(String args[]) {
        //1.配置生产者属性 
        Properties props = new Properties();
        // Kafka服务端的主机名和端口号，可以是多个 
        props.put("bootstrap.servers", "172.17.0.3:9092");
        //配置发送的消息是否等待应答 
        props.put("acks", "all");
        //配置消息发送失败的重试 
        props.put("retries", 0);
        // 批量处理数据的大小：16kb 
        props.put("batch.size", 16384);
        // 设置批量处理数据的延迟，单位：ms 
        props.put("linger.ms", 1);
        // 设置内存缓冲区的大小 
        props.put("buffer.memory", 33554432);
        //数据在发送之前一定要序列化 
        // key序列化 
        props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
        // value序列化 
        props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
        //2.实例化KafkaProducer 
        KafkaProducer<String, String> producer = new KafkaProducer<>(props);
        for (int i = 0; i < 50; i++) {
            //3.调用Producer的send方法，进行消息的发送，每条待发送的消息，都必须封装为一个Record对象 
            producer.send(new ProducerRecord<String, String>("test", Integer.toString(i), Integer.toString(i)));
        }
        //4.close释放资源 
        producer.close();
    }
}
```

**3.在Kafka集群中开启一个消费者**

bin/kafka-console-consumer.sh --bootstrap-server cdh0:9092 --topic test

我之前按照这个步骤一直不能成功写入数据，一直没有找到原因，后来才发现原因，是这里需要进行额外一个配置文件的配置

修改 server.properties中的

<img referrerpolicy="no-referrer" src="https://img-blog.csdn.net/20180922230425471?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70">

这里要改为ip地址，改过之后就能成功写入数据了