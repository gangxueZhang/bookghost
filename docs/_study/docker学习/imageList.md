# Docker获取远程仓库镜像列表
参考文档：https://blog.csdn.net/CodyGuo/article/details/86515354  

## 说明
* docker-tags 命令行获取docker远程仓库上指定镜像的tag列表  
* 用于命令行获取docker远程仓库上指定镜像的所有tag列表，支持版本号模糊搜索，可与docker search配合搜索。

## 脚本
下载 [docker-tags.sh](../files/docker-tags.sh)
```shell
#!/bin/bash
# 远程仓库地址，可设置为私有仓库
API="https://registry.hub.docker.com/v1/repositories"
# 默认查询Nginx镜像
DEFAULT_NAME="nginx"
DEFAULT_TIMEOUT=3

function Usage(){
cat << HELP

Usage: docker-tags NAME[:TAG]

docker-tags list all tags for docker image on a remote registry.

Example:
    docker-tags (default nginx)
    docker-tags nginx
    docker-tags nginx:1.15.8
    docker search nginx | docker-tags
    docker search nginx | docker-tags :1.15.8
    echo nginx | docker-tags
    echo nginx | docker-tags :1.15.8
HELP
}

ARG=$1
if [[ "$ARG" =~ "-h" ]];then
    Usage
    exit 0
fi

function ParseJson(){
    tr -d '[\[\]" ]' | tr '}' '\n' | awk -F: -v image=$1 '{if(NR!=NF && $3 != ""){printf("%s:%s\n",image,$3)}}'
}

function GetTags(){
    image=$1
    tag=$2
    ret=`curl -s ${API}/${image}/tags`
    tag_list=`echo $ret | ParseJson ${image}`
    if [ -z "$tag" ];then
        echo -e "$tag_list"
    else
        echo -e "$tag_list" | grep -w "$tag"
    fi
}

if [ -z $ARG ] || [[ ${ARG:0:1} == ":" ]];then
    if [ -x /usr/bin/timeout ];then
        images=`timeout $DEFAULT_TIMEOUT` awk '{print $1}' | grep -v "NAME" || echo $DEFAULT_NAME
    else
        images=`awk '{print $1}' | grep -v "NAME"`
    fi
else
    images=`echo $ARG | awk -F: '{print $1}'`
fi
tag=`echo $ARG | awk -F: '{print $2}'`

for i in ${images}
do
    tags=`GetTags $i $tag`
    count=`echo $tags | wc -w`
    if [[ $count -gt 0 ]];then
        echo -e "IMAGE [$i:$tag]:"
        echo -e "$tags"
        echo
    fi
done
```

# 使用案例
```shell
$ docker-tags nginx                                                                             [0]
IMAGE [nginx:]:
nginx:latest
nginx:1
nginx:1-alpine-perl
nginx:1-perl
nginx:1.10
nginx:1.10-alpine
......

$ docker-tags nginx:1.15.8                                                                      [1]
IMAGE [nginx:1.15.8]:
nginx:1.15.8
nginx:1.15.8-alpine
nginx:1.15.8-alpine-perl
nginx:1.15.8-perl

$ docker search nginx | docker-tags                                                             [0]
IMAGE [nginx:]:
nginx:latest
nginx:1
nginx:1-alpine-perl
nginx:1-perl
nginx:1.10
nginx:1.10-alpine
......
IMAGE [jwilder/nginx-proxy:]:
jwilder/nginx-proxy:latest
jwilder/nginx-proxy:0.1.0
jwilder/nginx-proxy:0.3.0
jwilder/nginx-proxy:0.4.0
jwilder/nginx-proxy:0.5.0
jwilder/nginx-proxy:0.6.0
jwilder/nginx-proxy:0.7.0
jwilder/nginx-proxy:alpine
jwilder/nginx-proxy:alpine-0.6.0
jwilder/nginx-proxy:alpine-0.7.0

IMAGE [richarvey/nginx-php-fpm:]:
richarvey/nginx-php-fpm:latest
richarvey/nginx-php-fpm:1.1.1
richarvey/nginx-php-fpm:1.1.3
richarvey/nginx-php-fpm:1.1.4
richarvey/nginx-php-fpm:1.1.5
richarvey/nginx-php-fpm:1.1.6
richarvey/nginx-php-fpm:1.2.0
richarvey/nginx-php-fpm:1.2.1
......

$ docker search nginx | docker-tags :1.15.8                                                   [130]
IMAGE [nginx:1.15.8]:
nginx:1.15.8
nginx:1.15.8-alpine
nginx:1.15.8-alpine-perl
nginx:1.15.8-perl

$ echo nginx | docker-tags                                                                      [0]
IMAGE [nginx:]:
nginx:latest
nginx:1
nginx:1-alpine-perl
nginx:1-perl
nginx:1.10
nginx:1.10-alpine

$ echo nginx | docker-tags :1.15.8                                                            [130]
IMAGE [nginx:1.15.8]:
nginx:1.15.8
nginx:1.15.8-alpine
nginx:1.15.8-alpine-perl
nginx:1.15.8-perl
```

