
# 检查部署情况

```
[root@hecs-74761 conf]# docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED          STATUS          PORTS                                                                                        
                                        NAMESe8f630170786   zookeeper                    "/docker-entrypoint.…"   52 minutes ago   Up 52 minutes   2888/tcp, 3888/tcp, 0.0.0.0:2181->2181/tcp, :::2181->2181/tcp, 8080/tcp                     
                                         zookeeper
[root@hecs-74761 conf]# docker exec -it zookeeper /bin/bash
root@e8f630170786:/apache-zookeeper-3.8.0-bin# zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /conf/zoo.cfg
Client port found: 2181. Client address: localhost. Client SSL: false.
Mode: standalone
root@e8f630170786:/apache-zookeeper-3.8.0-bin# 
```

# zkui可视化管理zookeeper

克隆zkui
```
git clone https://github.com/DeemOpen/zkui.git
```

使用maven打包
```
cd zkui/

mvn clean install

```

修改配置文件
```
cp config.cfg target/

cd target/

vim config.cfg


# 指定端口
serverPort=9090
# 以逗号分隔的所有zookeeper服务器列表
zkServer=localhost:2181
# 设置登录用户及其权限
userSet = {"users": [{ "username":"admin" , "password":"admin","role": "ADMIN" },{ "username":"appconfig" , "password":"appconfig","role": "USER" }]}

```

启动
```
nohup java -jar zkui-2.0-SNAPSHOT-jar-with-dependencies.jar > /uer/local/zk/zkui.log 2>&1 &
```

访问
```
http://ip:9090/
```
