# 一、对 jenkins_home 挂载目录chmod
对jenkins/jenkins_home 授权 chmod 777 jenkins_home

# 二、配置国内镜像加速

进入 cd jenkins/jenkins_home目录

修改 vim  hudson.model.UpdateCenter.xml里的内容

将 url 修改为 清华大学官方镜像：https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json

# 三、访问Jenkins页面地址：ip:8181
