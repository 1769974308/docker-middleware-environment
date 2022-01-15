## 文件目录位置
```
Redis
|-- docker-compose.yml
|-- README.md
```

## 启动注意

- docker-compose.yml包含redis(redis服务)、redis-commander(redis web命令控制台)、redis-exporter(redis监控插件、需要与Prometheus-Grafana使用)
- 以上三个按需要启动docker-compose -f docker-compose.yml up -d redis
 或者docker-compose -f docker-compose.yml up -d 
- 若启动redis-commander，则redis不能设置密码，否则报错NOAUTH Authentication required无法启动服务


## 运行结果
```
[root@VM-24-15-centos Redis]# docker-compose -f docker-compose.yml up -d
Starting redis                  ... done
Starting redis_redis-exporter_1 ... done
Starting redis-commander        ... done
[root@VM-24-15-centos Redis]# docker ps -a
CONTAINER ID   IMAGE                                   COMMAND                  CREATED              STATUS                        PORTS                                                     
                                       NAMESb18831d70f0d   rediscommander/redis-commander:latest   "/usr/bin/dumb-init …"   About a minute ago   Up About a minute (healthy)   0.0.0.0:8082->8081/tcp                                   
                                        redis-commandercca5f375a860   oliver006/redis_exporter                "/redis_exporter -re…"   About a minute ago   Up About a minute             0.0.0.0:9121->9121/tcp                                   
                                        redis_redis-exporter_1649b690af1c3   redis:5                                 "docker-entrypoint.s…"   2 minutes ago        Up About a minute             0.0.0.0:16379->6379/tcp                                  
                                        redisl
[root@VM-24-15-centos Redis]# 

```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1642226935678-9fb641bc-4ee8-4615-ae63-6bd51ae9f8be.png#clientId=u975c1a0b-0a41-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=987&id=u0da3d2b1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=987&originWidth=1868&originalType=binary&ratio=1&rotation=0&showTitle=false&size=172834&status=done&style=none&taskId=u6bde90ed-fbf8-4afe-8ec7-0c6b36ca8b5&title=&width=1868)