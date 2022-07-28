对jenkins_home 授权 chmod 777 jenkins_home

配置镜像加速，进入 cd jenkins_home/ 目录

修改 vim  hudson.model.UpdateCenter.xml里的内容

将 url 修改为 清华大学官方镜像：https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json


访问Jenkins页面地址：ip:8181