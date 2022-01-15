## 文件目录位置
```
Rabbitmq
|-- docker-compose.yml
|-- README.md
```



## 运行结果
```
[root@VM-24-15-centos Rabbitmq]# docker-compose -f docker-compose.yml up -d
Creating network "rabbitmq_middleware-network" with driver "bridge"
Recreating rabbitmq ... done
[root@VM-24-15-centos Rabbitmq]# docker ps -a
CONTAINER ID   IMAGE                                   COMMAND                  CREATED          STATUS                    PORTS                                                             
                               NAMESb7bb4a0a0a32   rabbitmq:3.7.15-management              "docker-entrypoint.s…"   19 seconds ago   Up 18 seconds             4369/tcp, 5671/tcp, 0.0.0.0:5672->5672/tcp, 15671/tcp, 25672/tcp,
 0.0.0.0:15672->15672/tcp       rabbitmq
```
## 访问
  - 访问管理页面地址:http://ip:15672/
  - 输入账号密码并登录：guest  / guest