## 文件目录位置
```
Rabbitmq
|-- docker-compose.yml
|-- rabbitmq_delayed_message_exchange-3.8.0.ez
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
  
  部署在云服务器上，需在安全组开放端口
  
 ## 延时队列安装 
 
<a name="hMJEZ"></a>
### 一、下载（文件下目录已经下载好rabbitmq_delayed_message_exchange-3.8.0.ez）
[rabbitmq-delayed-message-exchange](https://github.com/rabbitmq/rabbitmq-delayed-message-exchange/releases/tag/v3.8.0)<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1648685728111-21fa9aff-32d7-4b54-bac9-c493f98b9513.png#clientId=u13a0233c-5df1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=519&id=uc7a0bd05&margin=%5Bobject%20Object%5D&name=image.png&originHeight=519&originWidth=931&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47608&status=done&style=none&taskId=uf7e57302-591e-4c58-8b15-55701dd041d&title=&width=931)<br />因为安装的rabbitmq版本是3.7.15-management，所以选择这个版本的延时队列插件。

<a name="DkNhq"></a>
### 二、安装
上传到服务器，然后进行如下操作:
```
[root@hecs-74761 ~]# rz

[root@hecs-74761 ~]# ll
total 44
-rw-r--r-- 1 root root 43377 Mar 30 22:31 rabbitmq_delayed_message_exchange-3.8.0.ez

```

```
[root@hecs-74761 ~]# docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED        STATUS        PORTS           
                                                                                                                     NAMES2b7519e93a35   rabbitmq:3.7.15-management   "docker-entrypoint.s…"   10 hours ago   Up 10 hours   4369/tcp, 5671/
tcp, 0.0.0.0:5672->5672/tcp, :::5672->5672/tcp, 15671/tcp, 25672/tcp, 0.0.0.0:15672->15672/tcp, :::15672->15672/tcp   rabbitmq3c6cac8a7b22   mysql:5.7                    "docker-entrypoint.s…"   44 hours ago   Up 44 hours   33060/tcp, 0.0.
0.0:13306->3306/tcp, :::13306->3306/tcp                                                                               mysql[root@hecs-74761 ~]# docker cp rabbitmq_delayed_message_exchange-3.8.0.ez rabbitmq:/plugins
[root@hecs-74761 ~]# docker exec -it rabbitmq /bin/bash
root@2b7519e93a35:/# 

```
```
root@2b7519e93a35:/# rabbitmq-plugins enable rabbitmq_delayed_message_exchange
Enabling plugins on node rabbit@2b7519e93a35:
rabbitmq_delayed_message_exchange
The following plugins have been configured:
  rabbitmq_delayed_message_exchange
  rabbitmq_management
  rabbitmq_management_agent
  rabbitmq_web_dispatch
Applying plugin configuration to rabbit@2b7519e93a35...
The following plugins have been enabled:
  rabbitmq_delayed_message_exchange

started 1 plugins.
root@2b7519e93a35:/# rabbitmq-plugins list
Listing plugins with pattern ".*" ...
 Configured: E = explicitly enabled; e = implicitly enabled
 | Status: * = running on rabbit@2b7519e93a35
 |/
[  ] rabbitmq_amqp1_0                  3.7.15
[  ] rabbitmq_auth_backend_cache       3.7.15
[  ] rabbitmq_auth_backend_http        3.7.15
[  ] rabbitmq_auth_backend_ldap        3.7.15
[  ] rabbitmq_auth_mechanism_ssl       3.7.15
[  ] rabbitmq_consistent_hash_exchange 3.7.15
[E*] rabbitmq_delayed_message_exchange 3.8.0
[  ] rabbitmq_event_exchange           3.7.15
[  ] rabbitmq_federation               3.7.15
[  ] rabbitmq_federation_management    3.7.15
[  ] rabbitmq_jms_topic_exchange       3.7.15
[E*] rabbitmq_management               3.7.15
[e*] rabbitmq_management_agent         3.7.15
 

```
```
root@2b7519e93a35:/# exit
exit
[root@hecs-74761 ~]# docker restart rabbitmq
rabbitmq
[root@hecs-74761 ~]# docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED        STATUS         PORTS          
                                                                                                                      NAMES2b7519e93a35   rabbitmq:3.7.15-management   "docker-entrypoint.s…"   10 hours ago   Up 7 seconds   4369/tcp, 5671
/tcp, 0.0.0.0:5672->5672/tcp, :::5672->5672/tcp, 15671/tcp, 25672/tcp, 0.0.0.0:15672->15672/tcp, :::15672->15672/tcp   rabbitmq
```
未安装延时插件时Exchanges中Type类型如下：<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1648686773116-7e3381fa-7d96-4cd0-92d6-644a7a9eb401.png#clientId=u13a0233c-5df1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=742&id=u99954be0&margin=%5Bobject%20Object%5D&name=image.png&originHeight=742&originWidth=766&originalType=binary&ratio=1&rotation=0&showTitle=false&size=51584&status=done&style=none&taskId=ufbb51fa3-8526-4f1c-8a06-a15568cbe38&title=&width=766)<br />
安装延时插件时Exchanges中Type类型如下：<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1648687100909-4831a32a-b229-4a1d-9dd6-05ec04c28947.png#clientId=u13a0233c-5df1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=698&id=u43fff605&margin=%5Bobject%20Object%5D&name=image.png&originHeight=698&originWidth=613&originalType=binary&ratio=1&rotation=0&showTitle=false&size=54677&status=done&style=none&taskId=u6fa72eda-ea58-40b5-9b97-00b1ab30363&title=&width=613)
