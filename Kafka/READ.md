
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

# KafKa 可视化工具 kafka-eagle

1、安装JDK 省略步骤

2、下载 kafka-eagle安装包kafka-eagle-bin-2.0.5.tar.gz：https://github.com/smartloli/kafka-eagle-bin/tags

3、下载完成后将kafka-eagle解压(需要解压两次)到指定目录
```
tar -zxvf kafka-eagle-web-2.0.5-bin.tar.gz
```

4、安装Mysql并添加数据库ke

5、修改配置文件$KE_HOME/conf/system-config.properties，注：主要修改Zookeeper和数据库配置，使用MySQL

```
kafka.eagle.zk.cluster.alias=cluster1
cluster1.zk.list=localhost:2181 #zk部署机器ip


kafka.eagle.driver=com.mysql.cj.jdbc.Driver
kafka.eagle.url=jdbc:mysql://localhost:3306/ke?useUnicode=true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull
kafka.eagle.username=root
kafka.eagle.password=root

```

6、命令启动kafka-eagle
```
$KE_HOME/bin/ke.sh start

[2022-06-15 09:57:55] INFO: [Job done!]
Welcome to
    __ __    ___     ____    __ __    ___            ______    ___    ______    __     ______
   / //_/   /   |   / __/   / //_/   /   |          / ____/   /   |  / ____/   / /    / ____/
  / ,<     / /| |  / /_    / ,<     / /| |         / __/     / /| | / / __    / /    / __/   
 / /| |   / ___ | / __/   / /| |   / ___ |        / /___    / ___ |/ /_/ /   / /___ / /___   
/_/ |_|  /_/  |_|/_/     /_/ |_|  /_/  |_|       /_____/   /_/  |_|\____/   /_____//_____/   
                                                                                             

Version 2.0.5 -- Copyright 2016-2021
*******************************************************************
* Kafka Eagle Service has started success.
* Welcome, Now you can visit 'http://127.0.0.1:8048'
* Account:admin ,Password:123456
*******************************************************************
* <Usage> ke.sh [start|status|stop|restart|stats] </Usage>
* <Usage> https://www.kafka-eagle.org/ </Usage>
*******************************************************************

```

7、启动成功访问:http://ip:8048/,账号密码：admin:123456


# KafKa 可视化工具 kafka-map
