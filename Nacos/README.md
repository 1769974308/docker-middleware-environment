## 文件目录位置
```
Nacos
|-- conf
|   `-- mysql
|   `    `-- init
|   `        `-- nacos_init.sql
|   `-- nocos
|        `-- custom.properties
         `-- nacos-standlone-mysql.env
|-- docker-compose.yml
|-- README.md
```

## Nacos文档
[quick-start](https://nacos.io/zh-cn/docs/quick-start.html)

## 数据库表
[官方创建库表sql](https://github.com/alibaba/nacos/blob/master/distribution/conf/nacos-mysql.sql)

![image.png](https://cdn.nlark.com/yuque/0/2021/png/1429632/1640676947907-f1268303-aa14-4c7c-83f5-099478d15bbb.png#clientId=u9cf80081-9929-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=233&id=u2141bec4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=233&originWidth=451&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7998&status=done&style=none&taskId=u42120588-3f02-48b0-8d93-69883931ee9&title=&width=451)


## 配置
[nacos-standlone-mysql](https://github.com/nacos-group/nacos-docker/blob/master/example/standalone-mysql-5.7.yaml)
```
nacos-standlone-mysql.env 配置文件内容

PREFER_HOST_MODE=hostname
MODE=standalone
SPRING_DATASOURCE_PLATFORM=mysql
MYSQL_SERVICE_HOST=替换具体数据库实例
MYSQL_SERVICE_DB_NAME=替换具体数据库名
MYSQL_SERVICE_PORT=替换具体数据库端口
MYSQL_SERVICE_USER=替换具体数据库用户名
MYSQL_SERVICE_PASSWORD=替换具体数据库密码
MYSQL_SERVICE_DB_PARAM=characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useSSL=false&serverTimezone=UTC

```

## 访问地址
```
http://ip:8848/nacos/
账号/密码 ： nacos/nacos
```
![nacos_login.png](https://cdn.nlark.com/yuque/0/2021/png/1429632/1640745235169-816181a1-8952-4a5d-bfea-dd45319b3891.png#clientId=ueb3b92f2-9d66-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=uf5a9ebb6&margin=%5Bobject%20Object%5D&name=nacos_login.png&originHeight=751&originWidth=1902&originalType=binary&ratio=1&rotation=0&showTitle=false&size=73313&status=done&style=none&taskId=u1074f85e-f5c2-431a-aeda-1e02db46ece&title=)