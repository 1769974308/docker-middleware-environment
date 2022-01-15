## 文件目录位置
```
Nginx
|-- conf
|   `-- nginx.conf
|-- docker-compose.yml
|-- data
|-- README.md

```

## 运行结果
```
[root@VM-24-15-centos Nginx]# docker-compose -f docker-compose.yml up -d
Creating network "nginx_middleware-network" with driver "bridge"
Creating volume "nginx_middleware_data" with default driver
Creating nginx ... done
[root@VM-24-15-centos Nginx]# docker ps -a
CONTAINER ID   IMAGE                                   COMMAND                  CREATED          STATUS                    PORTS                                                             
                               NAMES544fa966ba25   nginx:1.10                              "nginx -g 'daemon of…"   5 seconds ago    Up 5 seconds              0.0.0.0:80->80/tcp, 443/tcp                                      
                                nginx
```