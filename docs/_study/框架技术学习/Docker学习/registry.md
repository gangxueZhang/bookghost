# Docker构建私有仓库
参考文档：https://www.cnblogs.com/rwxwsblog/p/5436739.html  
参考文档：https://www.cnblogs.com/kevingrace/p/6628062.html

## 前言
有时候使用Docker Hub这样的公共仓库可能不方便，这种情况下用户可以使用registry创建一个本地仓库供私人使用，这点跟Maven的管理类似。
使用私有仓库有许多优点：  
1. 节省网络带宽，针对于每个镜像不用每个人都去中央仓库上面去下载，只需要从私有仓库中下载即可；  
2. 提供镜像资源利用，针对于公司内部使用的镜像，推送到本地的私有仓库中，以供公司内部相关人员使用。

目前Docker Registry已经升级到了v2，最新版的Docker已不再支持v1。Registry v2使用Go语言编写，在性能和安全性上做了很多优化，重新设计了镜像的存储格式。如果需要安装registry v2，只需下载registry:2.2即可。Docker官方提供的工具docker-registry可以用于构建私有的镜像仓库。废话不多说了，下面记录下Docker私有仓库构建的过程：
## 镜像安装
选择一台安装了Docker环境的服务器作为注册服务器，用于搭建私有仓库。
```shell
#下载镜像
docker pull registry
#或者
docker pull registry:2.2

#查看镜像
docker images

#运行私有仓库，指定端口和数据卷
docker run -d -p 5000:5000 -v /opt/data/registry:/tmp/registry docker.io/registry
#-d表示后台运行 -p为端口映射 
#-v为数据卷挂载，宿主机的/opt/data/registry挂载到容器的/tmp/registry下

#访问私有仓库
curl 192.168.222.128:5000/v1/search

#给基础镜像打个标签(前提是mysql镜像存在)
docker tag mysql 192.168.222.128:5000/mysql 

#将镜像提交到私有仓库 
docker push 192.168.222.128:5000/mysql 

#查看镜像存储目录（宿主机上操作） 

tree /opt/data/registry/repositories/
```
## 配置私有仓库地址
可以解决一下问题：  
Get https://192.168.1.100:5000/v1/_ping: http: server gave HTTP response to HTTPS client
```shell 
# 方式一
# 修改Docker配置文件
$ vim /etc/sysconfig/docker
# 修改此行
OPTIONS='--selinux-enabled --log-driver=journald'
改为
OPTIONS='--selinux-enabled --log-driver=journald --insecure-registry=192.168.1.23:5000'

# 方式2
# mac修改[~/.docker/daemon.json]
vim /etc/docker/daemon.json
#设置内容如下
{
  #设置镜像源
  "registry-mirrors" : [
    "https://docker.mirrors.ustc.edu.cn",
    "https://hub-mirror.c.163.com"
  ],
  "experimental" : true,
  "debug" : true,
  # 设置私有仓库
  "insecure-registries" : [
    "172.16.1.25:5000"
  ]
}

#重启Docker服务
$ service docker restart
Redirecting to /bin/systemctl restart  docker.service
```

## 从私有仓库下载镜像
```shell
docker pull 192.168.222.128:5000/mysql
```
## 查看仓库镜像信息
```
$ curl -XGET http://registry_ip:5000/v2/search
$ curl -XGET http://registry_ip:5000/v2/_catalog
$ curl -XGET http://registry_ip:5000/v2/image_name/tags/list
```

