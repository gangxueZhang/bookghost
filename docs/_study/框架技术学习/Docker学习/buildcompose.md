# 基于compose构建镜像
详情请参考模块`docker-compose`.
## 配置详情
* Dockerfile配置详情
```shell
#基础镜像
FROM java:8u111
#维护者信息
MAINTAINER zhanggangxue<zhanggangxue@mininglamp.com>
#设置环境变量
ENV PATH /usr/local/docker-compose:$PATH
#拷贝文件到安装目录下，压缩文件会自动解压
ADD target/docker-compose.tar.gz /usr/local/
#执行指定命令
RUN chmod -R 777 /usr/local/docker-compose
#移动到指定目录，相当于cd
WORKDIR /usr/local/docker-compose
#对外开放端口
EXPOSE 8083
#CMD 运行以下命令
CMD ["/bin/bash","./start.sh"]
```
* docker-compose.yml配置详情
```yml
# docker compose 版本号
version: "3.7"
# 注册相关服务
services:
  # 测试服务配置
  compose:
    # 从本地DockerFile构建镜像
    build:
      context: .
    # 设置镜像名和版本
    image: mlamp/compose:v1.0
    # 映射容器8082端口到主机的8082端口
    ports:
      - "8083:8083"
    # 链接到另一个服务中的容器。 指定服务名称和链接别名（SERVICE:ALIAS），或者仅指定服务名称。
    links:
      - redis
    # 解决容器的依赖、启动先后的问题。此处会先启动容器 redis 再启动 compose
    depends_on:
      - redis
    # 环境变量，在项目中也需要配置，否则程序报错，这里的配置会覆盖程序内的配置。
    environment:
      spring.redis.host: 192.168.62.60
      spring.redis.port: 16379
      spring.redis.password:
    # 设置容器名称
    container_name: docker_compose_1
  # redis服务
  redis:
    # 镜像源(镜像名称和版本，本地没有的话会到远程仓库去下载)
    image: redis:latest
    # 映射容器6379端口到主机的16379端口
    ports:
      - "16379:6379"
    # 设置容器名称
    container_name: docker_redis_1
```

## 构建镜像命令
```shell
# -d 表示在后台构建
docker-compose up -d
```

## 构建镜像现象(会自动发布)
```shell
$ docker-compose up -d
Building compose
Step 1/8 : FROM java:8u111
 ---> d23bdf5b1b1b
Step 2/8 : MAINTAINER zhanggangxue<zhanggangxue@mininglamp.com>
 ---> Using cache
 ---> 6d981f68578f
Step 3/8 : ENV PATH /usr/local/docker-compose:$PATH
 ---> Running in 245171a682bf
Removing intermediate container 245171a682bf
 ---> f190ac389eb1
Step 4/8 : ADD target/docker-compose.tar.gz /usr/local/
 ---> 2c59fe2e4db4
Step 5/8 : RUN chmod -R 777 /usr/local/docker-compose
 ---> Running in 578e4109f9ba
Removing intermediate container 578e4109f9ba
 ---> 01080925d315
Step 6/8 : WORKDIR /usr/local/docker-compose
 ---> Running in 70c035f55e9c
Removing intermediate container 70c035f55e9c
 ---> 240be9a814e8
Step 7/8 : EXPOSE 8083
 ---> Running in 0a5a692ba126
Removing intermediate container 0a5a692ba126
 ---> e91a9518d81d
Step 8/8 : CMD ["/bin/bash","./start.sh"]
 ---> Running in 7e954eacd923
Removing intermediate container 7e954eacd923
 ---> 36709a0afc98

Successfully built 36709a0afc98
Successfully tagged mlamp/compose:v1.0
WARNING: Image for service compose was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating docker_redis_1 ... done
Creating docker_compose_1 ... done
$ 
```

