## 文件目录位置
```
Sentinel
|-- conf 
|-- docker-compose.yml
|-- README.md

```


## 运行结果
```
[root@localhost Sentinel]# docker-compose -f docker-compose.yml up -d 
Creating network "sentinel_middleware-network" with driver "bridge"
Creating volume "sentinel_middleware_data" with default driver
Creating sentinel ... done
[root@localhost Sentinel]# docker ps
CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS              PORTS                                       
       NAMES736e8c2ac542        bladex/sentinel-dashboard   "java -Djava.securit…"   7 seconds ago       Up 6 seconds        8719/tcp, 0.0.0.0:8858->8858/tcp           
        sentinel
```

## 访问
```
http://192.168.10.214:8858/

用户名：sentinel
密码：sentinel
```
![sentinel_login.png](https://cdn.nlark.com/yuque/0/2021/png/1429632/1640675337746-b5161454-f332-4f01-bd24-636b86778db5.png#clientId=u7a42fef3-4cb9-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=503&id=uab9009d3&margin=%5Bobject%20Object%5D&name=sentinel_login.png&originHeight=503&originWidth=821&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26806&status=done&style=none&taskId=uc0356acc-a851-48cc-a217-9e3cd5690f2&title=&width=821)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1429632/1640675477073-af07b6ea-db84-470b-b76c-714482630dcd.png#clientId=u0feeb98d-ec3c-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=689&id=u7de4f22b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=689&originWidth=1897&originalType=binary&ratio=1&rotation=0&showTitle=false&size=154449&status=done&style=none&taskId=ud0baae40-a5f9-4941-82db-e03a02db25e&title=&width=1897)