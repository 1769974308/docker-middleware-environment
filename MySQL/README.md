## 文件目录位置
```
RockertMQ
|-- conf
|   `-- broker.conf
|-- docker-compose.yml
|-- logs
|-- README.md
`-- store
```

## 启动RocketMQ步骤

- 修改broker.conf中的brokerIP1参数，修改为本机IP
- 进入docker-compose.yml文件所在路径，执行docker-compose up 命令即可
- 启动成功后浏览器输入http://ip:8080/,即可看到管理页面，表示搭建成功

## 运行结果
```
[root@VM-24-15-centos RockertMQ]# docker-compose -f docker-compose.yml up -d
Creating network "rockertmq_middleware-network" with driver "bridge"
Creating rmqnamesrv ... done
Creating rmqbroker  ... done
Creating rmqconsole ... done
[root@VM-24-15-centos RockertMQ]# docker ps -a
CONTAINER ID   IMAGE                           COMMAND                  CREATED          STATUS         PORTS                                                                     NAMES
43113d828c0d   foxiswho/rocketmq:broker        "mqbroker -c /etc/ro…"   10 seconds ago   Up 8 seconds   0.0.0.0:10909->10909/tcp, 9876/tcp, 10912/tcp, 0.0.0.0:10911->10911/tcp   rmqbroker
ecf1ff915e77   styletang/rocketmq-console-ng   "sh -c 'java $JAVA_O…"   10 seconds ago   Up 8 seconds   0.0.0.0:8080->8080/tcp                                                    rmqconsole
5eff88323b85   foxiswho/rocketmq:server        "/bin/sh -c 'cd ${RO…"   11 seconds ago   Up 9 seconds   10909/tcp, 0.0.0.0:9876->9876/tcp, 10911-10912/tcp                        rmqnamesrv
[root@VM-24-15-centos RockertMQ]# 
```