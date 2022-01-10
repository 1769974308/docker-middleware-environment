使用 promethues 和 grafana 搭建监控系统

- node-exporter：运行在主机上收集操作系统上各种数据的 agent，promethues 中称为 exporter
- prometheus：开源的时序数据库，作为数据存储和分析的中心
- grafana：数据展示分析界面，提供各种强大的 dashboard，可以从多个数据源读取数据，其中就包括 promethues

所有的服务都是通过 docker 启动的，需要安装 docker 和 docker-compose

## 文件目录位置
```
Prometheus-Grafana
|-- conf
|   `-- prometheus.yml
|   
|-- docker-compose.yml
|-- README.md
```


## 文档

 [prometheus-book](https://yunlzheng.gitbook.io/prometheus-book/)
 
  [Prometheus中文教程](https://www.orchome.com/Prometheus/index)

## 架构
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641626266901-3274fe31-948c-4d0c-963b-197814253b15.png#clientId=ua545f5d3-8edb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=813&id=u265b3a02&margin=%5Bobject%20Object%5D&name=image.png&originHeight=813&originWidth=1348&originalType=binary&ratio=1&rotation=0&showTitle=false&size=131013&status=done&style=none&taskId=u44e0cc82-6029-4678-9d17-48d183ba334&title=&width=1348)

- promethues server 位于中心，负责时序数据的收集、存储和查询
- 左边是数据来源，promethues 统一采用拉取的模式（pull mode）从兼容的 HTTP 接口处获取数据，数据源可以分为三种
  
  1、本身就暴露 promethues 可读取 metrics 数据的或者专门为某个组件编写的 exporter，称为 Jobs 或者 Exporters，比如 node exporter，HAProxy、Nginx、MySQL 等服务的 exporter
  
  2、通过 push gateway 可以把原来 push 类型的数据转换成 pull 类型
  
  3、其他 promethues server
 - 上面是目标自动发现机制。对于生产的很多情况，手动配置所有的 metrics 来源可能会非常繁琐，所以 promethues 支持 DNS、kubernetes、Consul 等服务发现机制来动态地获取目标源进行数据抓取
 - 右下方是数据输出，一般是用来进行 UI 展示，可以使用 grafana 等开源方案，也可以直接读取 promethues 的接口进行自主开发
 - 右上方是告警部分，用户需要配置告警规则，一旦 alertManager 发现监控数据匹配告警规则，就把告警信息通过邮件、社交账号发送出去
 
## 服务部署
| 服务名称 | IP | 部署方式 |
| --- | --- | --- |
| prometheus | 192.168.10.214 | docker |
| node-exporter | 192.168.10.214 | docker |
| grafana | 192.168.10.214 | docker |

## 启动异常处理
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641780635097-aab32789-a021-4e5a-821f-ad3e8d30e808.png#clientId=u7d4737cc-2fcc-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=419&id=uc447de96&margin=%5Bobject%20Object%5D&name=image.png&originHeight=419&originWidth=1586&originalType=binary&ratio=1&rotation=0&showTitle=false&size=82648&status=done&style=none&taskId=ud75b2df3-f735-451f-a6be-9d52eaa23e4&title=&width=1586)

原因是权限问题，prometheus的镜像中是使用的nobody这个用户，而pvc使用的是root权限

![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641781806207-e252d45b-e72b-48e5-b2b2-8acbfb85ba9f.png#clientId=u6fca222c-4c04-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=530&id=u47dd6c9a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=530&originWidth=1150&originalType=binary&ratio=1&rotation=0&showTitle=false&size=58812&status=done&style=none&taskId=u12058580-f1f6-41f6-8ecd-ea0621bcf6e&title=&width=1150)

## 启动并访问
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641782012028-2b22b40d-a8e1-4df9-be14-345d524b5c17.png#clientId=u0d3fee82-3c09-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=374&id=u1c508e52&margin=%5Bobject%20Object%5D&name=image.png&originHeight=374&originWidth=1587&originalType=binary&ratio=1&rotation=0&showTitle=false&size=58308&status=done&style=none&taskId=u349eda36-1c26-4565-b473-90781c4f5f8&title=&width=1587)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641782053202-4cfd6f1e-f3a5-4399-9cbd-f29ede1a06e5.png#clientId=u0d3fee82-3c09-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=483&id=u47070deb&margin=%5Bobject%20Object%5D&name=image.png&originHeight=483&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=55986&status=done&style=none&taskId=ub773d4db-1fa0-44b8-ad54-f79afd6b8c3&title=&width=1920)

## 使用Node Exporter采集主机数据
   在Prometheus的架构设计中，Prometheus Server并不直接服务监控特定的目标，其主要任务负责数据的收集，存储并且对外提供数据查询支持。如需要采集主机运行指标的CPU使用率、内存、磁盘等信息，需要使用到Node Exporter.Prometheus周期性的从Exporter暴露的HTTP服务地址拉取监控样本数据。
   
   docker方式安装Node Exporter
   ```
    node-exporter:
        image: prom/node-exporter:latest
        container_name: node-exporter
        restart: unless-stopped
        volumes:
          - /proc:/host/proc:ro
          - /sys:/host/sys:ro
          - /:/rootfs:ro
        command:
          - --path.procfs=/host/proc
          - --path.rootfs=/rootfs
          - --path.sysfs=/host/sys
          - --collector.filesystem.mount-points-exclude=^/(sys|proc|dev|host|etc)($$|/)
        ports:
          - 9100:9100
        networks:
          middleware-network:
            aliases:
              - nodeExporter
   ```

   启动node exporter并访问
   ![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641786878005-0047cdd2-56cb-46a3-ab77-14d31d94b569.png#clientId=uf38d42a9-5eb6-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=302&id=u928dbebc&margin=%5Bobject%20Object%5D&name=image.png&originHeight=302&originWidth=1342&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15778&status=done&style=none&taskId=uba154c0c-866a-4ea7-a72b-835bf954406&title=&width=1342)
   
   访问http://ip:9100/metrics,可以查看当前node exporter获取当前主机的所有监控数据如下：
   ![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641787190543-8f398be8-6d6f-45c3-ab28-d17df0557679.png#clientId=u8aeebb59-7544-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=671&id=u85e269e3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=671&originWidth=1059&originalType=binary&ratio=1&rotation=0&showTitle=false&size=63444&status=done&style=none&taskId=u61b915e7-7fc1-482c-b2fb-2b90f3b2989&title=&width=1059)
   
   每一个监控指标含义：
   ```
    # HELP node_disk_reads_completed_total The total number of reads completed successfully.
    # TYPE node_disk_reads_completed_total counter
    node_disk_reads_completed_total{device="dm-0"} 17090
    node_disk_reads_completed_total{device="dm-1"} 88
    node_disk_reads_completed_total{device="sda"} 18438
    node_disk_reads_completed_total{device="sr0"} 0
  ```
  HELP：用于解释当前指标的含义
  
  TYPE：则说明当前指标的数据类型
  
  当前页面中根据物理主机系统的不同，监控指标也不一样如下：
  ```
  node_boot_time：系统启动时间
  node_cpu：系统CPU使用量
  nodedisk*：磁盘IO
  nodefilesystem*：文件系统用量
  node_load1：系统负载
  nodememeory*：内存使用量
  nodenetwork*：网络带宽
  node_time：当前系统时间
  go_*：node exporter中go相关指标
  process_*：node exporter自身进程相关运行指标
  ```

  从Node Exporter收集监控数据，为了能够让Prometheus Server能够从当前的node exporter获取到监控数据，需要修改Prometheus配置文件并在scrape_configs节点下添加以下内容：
  ```
    scrape_configs:
    - job_name: "prometheus"
      scrape_interval: 5s
      static_configs:
      - targets: ["localhost:9090"]
    
    - job_name: "node"
      static_configs:
        - targets: ["192.168.10.214:9100"]
  ```
   static_configs.targets，表示要抓取任务的 HTTP 地址，默认会在 /metrics url 出进行抓取
，比如这里就是 http://localhost:9090/。 这是 prometheus 本身提供的监控数据，可以在浏览器中直接查看
  
  重新启动prometheus server,访问http://ip:9090,进入到prometheus server.如果输入“up”并且点击执行按钮以后，可以查看到如下结果：
  ![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641794882539-c1436aa6-3083-43cc-8220-f4ef000859ea.png#clientId=u808f7a93-94b8-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=442&id=u09dbddcf&margin=%5Bobject%20Object%5D&name=image.png&originHeight=442&originWidth=1905&originalType=binary&ratio=1&rotation=0&showTitle=false&size=59947&status=done&style=none&taskId=u9cd36eaf-6c77-4422-a722-3532ce485ba&title=&width=1905)
  
  ```
    up{instance="192.168.10.214:9090", job="prometheus"}    1
    up{instance="192.168.10.214:9100", job="node"}          1
    其中“1”表示正常，“0”则为异常
  ```


   
   使用PromQL查询监控数据，Prometheus UI是一个可视化管理界面，通过Graph面板，可以直接使用PromQL实时查询监控数据，使用关键字node_load1可以查询出Prometheus采集到的主机负载的样本数据如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641796499162-cf22d28a-f512-4207-bf09-bf18b9d847f1.png#clientId=u8949cf16-5fc1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=870&id=ud205bfb9&margin=%5Bobject%20Object%5D&name=image.png&originHeight=870&originWidth=1914&originalType=binary&ratio=1&rotation=0&showTitle=false&size=138533&status=done&style=none&taskId=uc2751a5d-27b3-49c2-9017-61aa9f1dee7&title=&width=1914)   

    


## 监控数据可视化

Prometheus UI提供了快速验证PromQL以及临时可视化支持的能力，而在大多数场景下引入监控系统通常还需要构建可以长期使用的监控数据可视化面板（Dashboard），
考虑使用第三方的可视化工具如Grafana，Grafana是一个开源的可视化平台，并且提供了对Prometheus的完整支持。

安装配置grafana
```
grafana:
    image: grafana/grafana:4.6.2
    container_name: grafana
    volumes:
      - ./grafana-storage:/var/lib/grafana
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=pass
    ports:
      - '3000:3000'
    networks:
      middleware-network:
        aliases:
          - grafana

```
访问http://ip:3000就可以进入到Grafana的界面中，默认情况下使用账户admin/admin进行登录。使用GF_SECURITY_ADMIN_PASSWORD=pass修改默认密码
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641799633995-c11c7634-9367-4f01-b7b5-f5dc6c7dca4c.png#clientId=u51af6635-36b4-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=640&id=u9d914980&margin=%5Bobject%20Object%5D&name=image.png&originHeight=640&originWidth=1913&originalType=binary&ratio=1&rotation=0&showTitle=false&size=89526&status=done&style=none&taskId=uab3be6bd-10e9-44cd-8330-eac5ca5ef21&title=&width=1913)

从时序数据库中获取数据进行展示，比如使用promethues,所以在正式配置界面之前，需要先添加数据源，点击grafana左上角按钮找到"Data Sources"页面或者直接http://host-ip:3000/datasources 地址，会进入对应页面。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641800066604-7aa8d4c3-7723-44fe-b56c-58890a8e2fca.png#clientId=ue0610048-069b-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=970&id=u3bf87b0d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=970&originWidth=1908&originalType=binary&ratio=1&rotation=0&showTitle=false&size=287800&status=done&style=none&taskId=u633c69d5-3488-455f-a626-c0a2fe0f6dc&title=&width=1908)

创建一个Dashboard，并添加graph,在graph中添加一个panel,使用panel展示系统的load数据。编辑panel数据，选择data source为之前添加的promethues,然后填写query,系统node比较简单，一共是node_load1、node_load5和node_load15，分别是系统最近一分钟、五分钟、十五分钟的load数值，输入完成后点击输入框之外，grafana会自动更新图表：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641800689991-0f2ba977-1acb-4bed-86bd-91b9a84a3a27.png#clientId=u6f10dd20-ad4a-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=629&id=udb7cbb1f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=629&originWidth=1904&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91544&status=done&style=none&taskId=u8ef93400-4e65-4303-8267-b0ba670d729&title=&width=1904)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641800917340-d6e11714-224e-4f61-a889-efd5a0da906e.png#clientId=u33c74fbe-2e96-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=350&id=u9f8ef692&margin=%5Bobject%20Object%5D&name=image.png&originHeight=350&originWidth=1910&originalType=binary&ratio=1&rotation=0&showTitle=false&size=69600&status=done&style=none&taskId=u40ef8621-21b7-4992-b28a-1572a5d4fb5&title=&width=1910)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641801384347-2e02da1c-c9be-4824-8526-05f2331139ee.png#clientId=u33c74fbe-2e96-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=974&id=ub964a898&margin=%5Bobject%20Object%5D&name=image.png&originHeight=974&originWidth=1907&originalType=binary&ratio=1&rotation=0&showTitle=false&size=273268&status=done&style=none&taskId=uedbdb5fe-9cdf-4f30-8688-5e2ec33756e&title=&width=1907)

类似的，可以添加其他的 panel，展示系统方方面的监控数据，比如 CPU、memory、IO、网络等。grafana 更多配置可以参考[官方文档](https://grafana.com/docs/grafana/v7.5/panels/visualizations/graph-panel/)，选择合适的图表来展示想要的结果.


可以手动通过界面对grafana可以很灵活地创建出很强大的图表，grafana支持导出和导出配置，并提供[官方社区](https://grafana.com/grafana/dashboards/)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641802011536-246fe8e3-d517-455d-9f86-612ab043794c.png#clientId=u67eefdb5-281a-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=740&id=udafabe2b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=740&originWidth=1909&originalType=binary&ratio=1&rotation=0&showTitle=false&size=316153&status=done&style=none&taskId=u40dd55cd-8e17-4888-84fd-1f65ea1ae84&title=&width=1909)


每个 dashboard 都有一个编号，比如[编号22的dashboard](https://grafana.com/grafana/dashboards/22) 就是专门为 node-exporter 设计的展示图表。在 grafana 中点击导入 dashboard，添加编号选择数据源，就能得到已经配置完整的图表：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641802458361-b0643079-2b11-4350-a860-647e07821dbf.png#clientId=u0bc9584d-9cd3-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=645&id=u5f70ff20&margin=%5Bobject%20Object%5D&name=image.png&originHeight=645&originWidth=1912&originalType=binary&ratio=1&rotation=0&showTitle=false&size=139263&status=done&style=none&taskId=u91c94fd9-a272-4872-a9f5-ca99b4260a8&title=&width=1912)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641802498990-48cc617c-aeef-4f9a-a928-a8891f921c93.png#clientId=u0bc9584d-9cd3-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=586&id=u1ec4b98c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=586&originWidth=1910&originalType=binary&ratio=1&rotation=0&showTitle=false&size=140468&status=done&style=none&taskId=u3cbe59f1-ff6e-4177-9201-cb4032897cc&title=&width=1910)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1429632/1641802954427-fec38806-9006-4aeb-a4c0-5ff947cc62a1.png#clientId=u0bc9584d-9cd3-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=949&id=u1ee5ad70&margin=%5Bobject%20Object%5D&name=image.png&originHeight=949&originWidth=1904&originalType=binary&ratio=1&rotation=0&showTitle=false&size=161200&status=done&style=none&taskId=u9b180df7-1417-4901-b0c9-3e6ffcf8c02&title=&width=1904)

## 配置告警
通过 grafana 图表我们可以知道系统各种指标随着时间的变化，方便轻松判断系统某个资源是否异常。但是我们不能一直盯着 dashboard，还需要系统发生异常的时候能立即通过邮件或者其他方式通知我们，这就是告警的功能。

定义两个简单的告警规则：当主机 download 掉，或者 CPU load 持续过高一段时间就发送告警。对此，需要新建一个 alert.rules 文件，用来保存告警规则，按照需求对应的内容如下：
```
#alert.rules内容如下

groups:
- name: node-alert
  rules:
  - alert: service_down
    expr: up == 0
    for: 2m
  - alert: high_load
    expr: node_load1 > 0.5
    for: 5m

```
在启动 promethues 服务的时候把告警规则文件 mount 到 pod 中，如下添加一个 volume
```
services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    restart: unless-stopped
    user: root
    volumes:
      - ./config/prometheus.yml:/etc/prometheus/prometheus.yml
      - ./data/prometheus_data:/prometheus
      - ./config/alert.rules:/etc/prometheus/alert.rules
```

告知 promethues 加载这些规则,配置文件prometheus.yml添加如下：
```
rule_files:
  - 'alert.rules'
```

promethues 还提供了 alertmanager 可以自动化根据告警规则触发对应的动作，一般是各种方式通知用户和管理员

## 其他组件监控接入

- Prometheus监控MySQL
- Prometheus监控Redis
- Prometheus监控Elasticsearch
- Prometheus监控MongoDB 
- Prometheus监控Kafka
- Prometheus监控RabbitMQ
- Prometheus监控RocketMQ
- Prometheus监控PostgreSQL
- Prometheus监控ZooKeeper
- Prometheus监控Nginx
- Prometheus监控JVM