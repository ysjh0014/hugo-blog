---
title: "Kafka中自定义分区Java API"
date: 2022-09-23 08:49:16
draft: false
featured_image: "https://hugo-ys.oss-cn-hangzhou.aliyuncs.com/static/img/kafka.png"
---
自定义分区API只是在Producer API的基础上加入了一些内容

这里直接展示出代码

ProducerPartition.java

```java
package cn.ysjh;
import java.util.Properties;
import org.apache.kafka.clients.producer.Callback;
import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerRecord;
import org.apache.kafka.clients.producer.RecordMetadata;
public class ProducerPartition {
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
        //设置分区 
        props.put("partitioner.class", "cn.ysjh.Partition");
        //2.实例化KafkaProducer 
        KafkaProducer<String, String> producer = new KafkaProducer<>(props);
        for (int i = 50; i < 100; i++) {
            //3.调用Producer的send方法，进行消息的发送，每条待发送的消息，都必须封装为一个Record对象，接口回调 
            producer.send(new ProducerRecord<String, String>("test", "hello" + i), new Callback() {
                @Override
                public void onCompletion(RecordMetadata arg0, Exception arg1) {
                    if (arg0 != null) {
                        System.out.println(arg0.partition() + "--" + arg0.offset());
                    }
                }
            });
        }
        //4.close释放资源 
        producer.close();
    }
}
```

Partition.java

```java
package cn.ysjh;
import java.util.Map;
import org.apache.kafka.clients.producer.Partitioner;
import org.apache.kafka.common.Cluster;
public class Partition implements Partitioner {
    @Override
    public void configure(Map<String, ?> arg0) {
        // TODO Auto-generated method stub 
    }
    
    @Override
    public void close() {
        // TODO Auto-generated method stub 
    }

    @Override
    public int partition(String arg0, Object arg1, byte[] arg2, Object arg3, byte[] arg4, Cluster arg5) {
        // TODO Auto-generated method stub 
        return 0;
    }
}
```

可以看出是在Produceer的基础上加入了一个设置分区的代码，并且调用了一个回调函数来输出数据的分区和偏移量