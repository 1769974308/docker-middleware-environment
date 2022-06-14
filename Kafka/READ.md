
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