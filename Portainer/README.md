## 文件目录位置
```
Portainer
|-- docker-compose.yml
|-- README.md

```
## 安装文件

https://youmeek.gitbooks.io/linux-tutorial/content/markdown-file/Portainer-Install-And-Settings.html

## 运行结果
```
[root@VM-24-15-centos Portainer]# docker-compose -f docker-compose.yml up -d
Creating network "portainer_middleware-network" with driver "bridge"
Creating volume "portainer_middleware_data" with default driver
Pulling portainer (portainer/portainer:)...
latest: Pulling from portainer/portainer
94cfa856b2b1: Pull complete
49d59ee0881a: Pull complete
a2300fd28637: Pull complete
Digest: sha256:fb45b43738646048a0a0cc74fcee2865b69efde857e710126084ee5de9be0f3f
Status: Downloaded newer image for portainer/portainer:latest
Creating portainer ... done
[root@VM-24-15-centos Portainer]# docker ps 
CONTAINER ID   IMAGE                                   COMMAND                  CREATED         STATUS                 PORTS                                                                 
                           NAMES2e77e2a21fa8   portainer/portainer                     "/portainer"             9 seconds ago   Up 8 seconds           0.0.0.0:9000->9000/tcp                                                
                           portainer
```

## 对于本地监控配置

- Portainer 镜像构建的时候已经配置了：/var/run/docker.sock:/var/run/docker.sock，所以对于跟 Portainer 同一台机器的其他容器都可以被直接监控

- 浏览器访问访问：http://ip:9000

- 第一次启动需要创建用户名和密码。第二步就是配置管理哪里的 docker 容器，选择：local