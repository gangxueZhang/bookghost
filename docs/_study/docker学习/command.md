# Docker命令介绍
## Docker维护命令

* docker logs ：查看docker日志

```shell
# containerId表示容器ID
# 查看容器日志信息
docker logs containerId
# 持续查看容器日志信息
docker logs -f containerId
# 查看容器详细的日志信息
docker logs --details containerId
```
* docker inspect ：查看Docker对象的基本信息

```shell
# containerId表示容器ID
# 查看Docker对象的基本信息
docker inspect containerId
```

## Docker常用命令
* docker ps ：查看可用容器

```shell
# 查看已经启动的容器
docker ps 
# 查看所有的容器，包括退出的容器
docker ps -a 
```
* docker search ：在`Docker hub`(公有仓库)中查找镜像

```shell
# 在公有仓库中查询centos进行
docker search centos  
```
* docker pull ：在`Docker hub`(公有仓库)中下载镜像

```shell
# 从公有仓库下载centos镜像
docker pull centos  
```
* docker push ：上传个人镜像

```shell
#将centos镜像上传到公有仓库
docker push centos  
```
* docker image ：查看本地镜像

```shell
#列举本地镜像列表
docker image ls  
```
## docker images  
**功能说明:** 列举本地镜像列表  
**参数说明:**  
```shell
-a :列出本地所有的镜像（含中间映像层，默认情况下，过滤掉中间映像层）；
--digests :显示镜像的摘要信息；
-f :显示满足条件的镜像；
--format :指定返回值的模板文件；
--no-trunc :显示完整的镜像信息；
-q :只显示镜像ID。
```
**案例说明:**  
```shell
# 列举本地所有镜像列表
docker images
# 列举本地跟java相关的镜像列表
docker images java
```


## docker run
**功能说明:** 在一个容器中运行命令，需指定镜像  
**参数说明:**
```shell
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
OPTIONS说明：
-a stdin: 指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；
-d: 后台运行容器，并返回容器ID；
-i: 以交互模式运行容器，通常与 -t 同时使用；
-P: 随机端口映射，容器内部端口随机映射到主机的高端口
-p: 指定端口映射，格式为：主机(宿主)端口:容器端口
-t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
--name="nginx-lb": 为容器指定一个名称；
--dns 8.8.8.8: 指定容器使用的DNS服务器，默认和宿主一致；
--dns-search example.com: 指定容器DNS搜索域名，默认和宿主一致；
-h "mars": 指定容器的hostname；
-e username="ritchie": 设置环境变量；
--env-file=[]: 从指定文件读入环境变量；
--cpuset="0-2" or --cpuset="0,1,2": 绑定容器到指定CPU运行；
-m :设置容器使用内存最大值；
--net="bridge": 指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型；
--link=[]: 添加链接到另一个容器；
--expose=[]: 开放一个端口或一组端口；
--volume , -v: 绑定一个卷
```
**案例说明:**
```shell
# 使用docker镜像nginx:latest以后台模式启动一个容器,并将容器命名为mynginx。
$ docker run --name mynginx -d nginx:latest

#使用镜像nginx:latest以后台模式启动一个容器,并将容器的80端口映射到主机随机端口。
$ docker run -P -d nginx:latest

#使用镜像 nginx:latest，以后台模式启动一个容器,将容器的 80 端口映射到主机的 80 端口,主机的目录 /data 映射到容器的 /data。
$ docker run -p 80:80 -v /data:/data -d nginx:latest

#绑定容器的 8080 端口，并将其映射到本地主机 127.0.0.1 的 80 端口上。
$ docker run -p 127.0.0.1:80:8080/tcp ubuntu bash

#使用镜像nginx:latest以交互模式启动一个容器,在容器内执行/bin/bash命令。
$ docker run -it nginx:latest /bin/bash
```

