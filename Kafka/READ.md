
# 通过Kafka自带工具生产消费测试

首先，进入到kafka的docker窗口中
```
[root@hecs-74761 Kafka]# docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED         STATUS         PORTS                                                                                          
                                      NAMES
3497b6ac237e   wurstmeister/kafka           "start-kafka.sh"         3 minutes ago   Up 3 minutes   0.0.0.0:9092->9092/tcp, :::9092->9092/tcp                                                      
                                      kafka
[root@hecs-74761 Kafka]# docker exec -it 3497b6ac237e /bin/bash

```

创建一个topic:kafeidou
```
root@3497b6ac237e:/# kafka-topics.sh --create --topic kafeidou --bootstrap-server 120.46.151.254:9092

```

查看topic
```
root@3497b6ac237e:/# kafka-topics.sh --describe --topic kafeidou --bootstrap-server 120.46.151.254:9092
Topic: kafeidou	TopicId: YxORT5ItTNeC-8H_NGvsSA	PartitionCount: 1	ReplicationFactor: 1	Configs: segment.bytes=1073741824
	Topic: kafeidou	Partition: 0	Leader: 0	Replicas: 0	Isr: 0
root@3497b6ac237e:/# 
```

运行生产，生产消息
```
[root@hecs-74761 ~]# docker exec -it 3497b6ac237e /bin/bash
root@3497b6ac237e:/# kafka-console-producer.sh --broker-list 120.46.151.254:9092 --topic kafeidou
>hello
>ooo
>
```

打开新的ssh窗口，同样进入kafka的容器中，运行消费者，进行消息的监听
```
root@3497b6ac237e:/# kafka-console-consumer.sh --bootstrap-server 120.46.151.254:9092 --topic kafeidou --from-beginning
hello
ooo

```


# KafKa 可视化工具 kafka-map

使用docker方式部署，同一docker-compose.yml与kafka同时部署，访问地址htt://ip:9093 账号密码：admin:admin
