---
title: "Kafka中Producer拦截器"
date: 2022-09-23 15:08:10
draft: false
---
**1.拦截器原理(interceptor)**

Producer拦截器(interceptor)是在Kafka 0.10版本被引入的，主要用于实现clients端的定制化控制逻辑

对于producer而言，interceptor使得用户在消息发送前以及producer回调逻辑前有机会对消息做一些定制化需求，比如修改消息

等，同时producer允许用户指定多个interceptor按序作用于同一条消息从而形成一个拦截链(interceptor chain)，Intercetpor的实现接口是org.apache.kafka.clients.producer.ProducerInterceptor，需要实现的方法包括：

1)configure(configs)：

获取配置信息和初始化数据时调用

2)onSend(ProducerRecord)：

Producer确保在消息被序列化以及计算分区前调用该方法。用户可以在该方法中对消息做任何操作，但最好保证不要修改消息所属的topic和分区，否则会影响目标分区的计算

3)onAcknowledgement(RecordMetadata, Exception)：

该方法会在消息被应答或消息发送失败时调用

4)close：

关闭interceptor，主要用于执行一些资源清理工作

**2.拦截器实现**

1)需求：

实现一个简单的双interceptor组成的拦截链，第一个interceptor会在消息发送前将时间戳信息加到消息value的最前部；第二个interceptor会在消息发送后更新成功发送消息数或失败发送消息数

2)代码实现

第一个拦截器实现：

Interceptor1.java

```java
package cn.ysjh;
import java.util.Map;
import org.apache.kafka.clients.producer.ProducerInterceptor;
import org.apache.kafka.clients.producer.ProducerRecord;
import org.apache.kafka.clients.producer.RecordMetadata;

public class Interceptor1 implements ProducerInterceptor<String, String> {
    @Override
    public void configure(Map<String, ?> arg0) {
       // TODO Auto-generated method stub
    }

    @Override
    public void close() {
     // TODO Auto-generated method stub
    }

    @Override

    public void onAcknowledgement(RecordMetadata arg0, Exception arg1) {
        // TODO Auto-generated method stub
    }

    @Override
    public ProducerRecord<String, String> onSend(ProducerRecord<String, String> record) {
        return new ProducerRecord(record.topic(), record.partition(), record.timestamp(), record.key(), System.currentTimeMillis() + "," + record.value().toString());
    }
}
```

第二个拦截器实现：

Interceptor2.java

```java
package cn.ysjh;
import java.util.Map;
import org.apache.kafka.clients.producer.ProducerInterceptor;
import org.apache.kafka.clients.producer.ProducerRecord;
import org.apache.kafka.clients.producer.RecordMetadata;
public class Interceptor2 implements ProducerInterceptor<String, String> {
    private int errorCounter = 0;
    private int successCounter = 0;
    @Override
    public void configure(Map<String, ?> arg0) {
        // TODO Auto-generated method stub 
        //  } 
        @Override
        public void close () {
            // 保存结果 
            System.out.println("Successful sent: " + successCounter);
            System.out.println("Failed sent: " + errorCounter);
        }
        @Override
        public void onAcknowledgement (RecordMetadata arg0, Exception arg1){
            // 统计成功和失败的次数 
            if (arg1 == null) {
                successCounter++;
            } else {
                errorCounter++;
            }
        }
        @Override
        public ProducerRecord<String, String> onSend (ProducerRecord < String, String > record){
            // TODO Auto-generated method stub return record; 
        }
    }
}
```

在Producer中进行注册： Interceptor会按照注册的顺序进行依次拦截

```java
package cn.ysjh;
import java.util.ArrayList;
import java.util.List;
import java.util.Properties;
import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerConfig;
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
        //批量处理数据的大小：16kb
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
        // 2 构建拦截链 
        List<String> interceptors = new ArrayList<>();
        interceptors.add("cn.ysjh.Interceptor1");
        interceptors.add("cn.ysjh.Interceptor2");
        props.put(ProducerConfig.INTERCEPTOR_CLASSES_CONFIG, interceptors);
        //2.实例化KafkaProducer 
        KafkaProducer<String, String> producer = new KafkaProducer<>(props);
        for (int i = 0; i < 50; i++) {
            //3.调用Producer的send方法，进行消息的发送，每条待发送的消息，都必须封装为一个Record对象
            producer.send(new ProducerRecord<String, String>("test", Integer.toString(i), "hello world" + i));
        }
        //4.close释放资源 
        producer.close();
    }
  }
}
```

最后在Kafka集群中开启一个消费者，运行程序即可，效果如图：

<img referrerpolicy="no-referrer" src='https://img-blog.csdn.net/20180923150759601?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lzXzIzMDAxNA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70'>