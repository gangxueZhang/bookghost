# Docker命令介绍
## Docker维护命令

### docker logs查看日志

**功能说明:**查看docker日志

```shell
# containerId表示容器ID
# 查看容器日志信息
docker logs containerId
# 持续查看容器日志信息
docker logs -f containerId
# 查看容器详细的日志信息
docker logs --details containerId
```
### docker inspect基本信息

**功能说明:**查看Docker对象的基本信息

```shell
# containerId表示容器ID
# 查看Docker对象的基本信息
docker inspect containerId
```

## Docker常用命令
### docker ps查看可用容器

```shell
# 查看已经启动的容器
docker ps 
# 查看所有的容器，包括退出的容器
docker ps -a 
```
### docker search查找镜像

**功能说明:** 在`Docker hub`(公有仓库)中查找镜像

```shell
# 在公有仓库中查询centos进行
docker search centos  
```
### docker pull下载镜像

 **功能说明:** 在`Docker hub`(公有仓库)中下载镜像

```shell
# 从公有仓库下载centos镜像
docker pull centos  
```
### docker push上传镜像

**功能说明:** 上传个人镜像

```shell
#将centos镜像上传到公有仓库
docker push centos  
```
### docker image查看镜像

**功能说明:** 查看本地镜像

```shell
#列举本地镜像列表
docker image ls  
```
### docker images镜像列表
**功能说明:** 列举本地镜像列表  
**参数说明:**  

```shell
docker images [OPTIONS] [REPOSITORY[:TAG]]
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

### docker rmi删除镜像

**功能说明:** 删除本地一个或多少镜像。

**参数说明:**  

```shell
docker rmi [OPTIONS] IMAGE [IMAGE...]
-f :强制删除；
--no-prune :不移除该镜像的过程镜像，默认移除；
```

**案例说明:**  

```shell
# 删除runoob/ubuntu:v4镜像
docker rmi -f runoob/ubuntu:v4
```

### docker run运行镜像

**功能说明:** 在一个容器中运行命令，需指定镜像  
**参数说明:**

```shell
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
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

### docker rm删除容器

* 参数说明

```shell
docker rm [OPTIONS] CONTAINER [CONTAINER...]
-f :通过 SIGKILL 信号强制删除一个运行中的容器。
-l :移除容器间的网络连接，而非容器本身。
-v :删除与容器关联的卷。
```

* 案例说明

```shell
# 强制删除容器 db01、db02
docker rm -f db01 db02

# 移除容器 nginx01 对容器 db01 的连接，连接名 db：
docker rm -l db 

# 删除容器 nginx01, 并删除容器挂载的数据卷：
docker rm -v nginx01

# 删除所有已经停止的容器：
docker rm $(docker ps -a -q)
```

# Docker使用场景案例

## 将自己的镜像打包到私库

* 登录到私库

```shell
# 命令格式：docker login [OPTIONS] [SERVER]
# 登陆过程中需要输入用户名和密码
$ docker login http://demo.com:8843
username:
password:
```

* 登录到私有harbor仓库

  创建自己的项目，此处假设新建项目名为：test。

* 给本地镜像打tag

  假设本地镜像名为：`demo-service`，镜像版本为：`v1.0.1`，镜像ID为：`62557c09053a`。

```shell
# 命令格式：docker tag [OPTIONS] IMAGE[:TAG] [REGISTRYHOST/][USERNAME/]NAME[:TAG]
# 命令方式1：采用镜像名和版本
$ docker tag demo-service:v1.0.1 demo.com:8843/test/demo-service:v1.0.1
# 命令方式2：采用镜像ID
$ docker tag 62557c09053a demo.com:8843/test/demo-service:v1.0.1
```

* 推送进行到私有仓库

```shell
# 将之前打的tag推送到私有仓库
docker push demo.com:8843/test/demo-service:v1.0.1
```

