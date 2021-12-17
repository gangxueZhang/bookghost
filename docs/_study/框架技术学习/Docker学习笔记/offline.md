# Docker镜像离线使用
由于公司产品不能上传到外网，客户主机无法与公司内网相连；或者客户主机不能连接外网，导致不能使用镜像仓库分发镜像，此时需要使用离线镜像。  
Docker提供了了两种方案，方便用户导入导出镜像
## 使用save和load导入导出镜像
### 查看本地镜像
* 使用`docker images`查看本地镜像列表

```shell
$ docker images
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
172.16.1.25:5000/compose   v1                  630a3cc7e612        6 days ago          691MB
mlamp/compose              v1.0                630a3cc7e612        6 days ago          691MB
redis                      latest              dcf9ec9265e0        5 weeks ago         98.2MB
golang                     1.11-alpine         e116d2efa2ab        4 months ago        312MB
java                       8u111               d23bdf5b1b1b        2 years ago         643MB
```
### 保存镜像
* 使用镜像ID导出镜像

```shell
# 经测试使用镜像ID导出可能会导致镜像名和镜像tag丢失
# 使用大于号写入
docker save dcf9ec9265e0 > redis.tar
# 使用[-o]选项,表示将镜像导出到指定的文件中
docker save -o redis.tar dcf9ec9265e0
```
* 使用镜像名导出镜像

```shell
# 如果不指定tag，会将本地同名的镜像都导出
docker save -o redis.tar redis:latest
```
* 指明保存多个镜像

```shell
docker save -o images.tar redis:latest java:8u111
```

### 载入镜像
```shell
# 使用小于号载入
docker load < redis.tar
# 使用[-i]选项,表示将指定的文件中镜像导入到本地
docker load -i redis.tar
```


## 使用export和import导入导出镜像
### 查看本机容器
* 使用`docker ps -a`查看本地容器列表

```
$ docker ps -a
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS                     PORTS                     NAMES
f4f56e4bec91        mlamp/compose:v1.0      "/bin/bash ./start.sh"   3 days ago          Exited (255) 2 days ago    0.0.0.0:8083->8083/tcp    docker_compose_1
177359744e69        redis:latest            "docker-entrypoint.s…"   3 days ago          Exited (255) 2 days ago    0.0.0.0:16379->6379/tcp   docker_redis_1
df3bc98dfc4f        java:8u111              "java -jar /usr/loca…"   7 weeks ago         Exited (255) 7 weeks ago   0.0.0.0:8849->8849/tcp    demo
```

### 导出镜像
* 使用镜像ID导出镜像

```shell
# 经测试使用镜像ID保存可能会导致镜像名和镜像tag丢失
# 使用大于号写入
docker export 177359744e69 > redis.tar
# 使用[-o]选项,表示将镜像输出到指定的文件中
docker export -o redis.tar 177359744e69
```

### 载入镜像
```shell
# 从tar文件中导入镜像，并命名为user/redis:latest
docker import  redis.tar user/redis:latest 
```

## 附录A

### 两种方案的差别

```
特别注意：两种方法不可混用。
如果使用 import 导入 save 产生的文件，虽然导入不提示错误，但是启动容器时会提示失败，会出现类似"docker: Error response from daemon: Container command not found or does not exist"的错误。
```

1. 文件大小不同  
    * export 导出的镜像文件体积小于 save 保存的镜像

2. 是否可以对镜像重命名  
    * docker import 可以为镜像指定新名称  
    * docker load 不能对载入的镜像重命名  

3. 是否可以同时将多个镜像打包到一个文件中  
    * docker export 不支持  
    * docker save 支持

4. 是否包含镜像历史
    * export 导出（import 导入）是根据容器拿到的镜像，再导入时会丢失镜像所有的历史记录和元数据信息（即仅保存容器当时的快照状态），所以无法进行回滚操作。
    * 而 save 保存（load 加载）的镜像，没有丢失镜像的历史，可以回滚到之前的层（layer）。

5. 应用场景不同
    * docker export 的应用场景：主要用来制作基础镜像，比如我们从一个 ubuntu 镜像启动一个容器，然后安装一些软件和进行一些设置后，使用 docker export 保存为一个基础镜像。然后，把这个镜像分发给其他人使用，比如作为基础的开发环境。
    * docker save 的应用场景：如果我们的应用是使用 docker-compose.yml 编排的多个镜像组合，但我们要部署的客户服务器并不能连外网。这时就可以使用 docker save 将用到的镜像打个包，然后拷贝到客户服务器上使用 docker load 载入。

### 批量操作镜像

* **镜像源列表**

**文件名：**`image_list/name_images.txt`

```shell
harbor.com:8843/public/image_1:latest
harbor.com:8843/public/image_2:latest
harbor.com:8843/public/image_3:latest
harbor.com:8843/public/image_4:latest
```

* **下载镜像**

**文件名：**`down_image.sh`

```shell
#/bin/bash
cd `dirname $0`;
echo `pwd`
dir_name=${1}

if [ ! -d "${dir_name}" ]; then
        mkdir ${dir_name}
fi

docker login harbor.com:8843

echo "读取镜像列表"
cat image_list/${dir_name}_images.txt | while read line
do
    if [[ ${line} =~ ^\#.* ]] || [[ -z "${line}" ]]; then
        continue
    fi
    echo "加载镜像："${line}
    #打包镜像
    #对IFS变量进⾏替换处理
    OLD_IFS="$IFS"
    IFS="/"
    array=(${line})
    IFS="$OLD_IFS"

    if [ ${#array[@]} -gt 1 ] ;then
        docker pull ${line}

        index=${#array[@]}-1
        var=${array[index]}
        echo "保存镜像: "${var}.tar
        docker save -o ${dir_name}/${var}".tar" $line
    fi
done
echo "压缩镜像包"
tar -czvf ${dir_name}.tar.gz ${dir_name}/
```

* **安装上传镜像**

**文件名：**`image_install.sh`

```shell
#/bin/bash
#进⼊⽬录
cd `dirname $0`;
echo `pwd`
dir_name=${1}

if [ ! -d "${dir_name}" ]; then
    echo "解压镜像包"
    tar -zxf ${dir_name}.tar.gz
fi

cp image_list/${dir_name}_images.txt ${dir_name}/
cd ${dir_name}
echo "读取镜像列表"
cat ${dir_name}_images.txt | while read line
do
    if [[ ${line} =~ ^\#.* ]] || [[ -z "${line}" ]]; then
        continue
    fi
    echo "加载镜像: " ${line}
    #对IFS变量进⾏替换处理
    OLD_IFS="$IFS"
    IFS="/"
    array=(${line})
    IFS="$OLD_IFS"

    if [ ${#array[@]} -gt 1 ] ;then
        index=${#array[@]}-1
        var=${array[index]}
        echo "加载压缩文件: "${var}.tar
        docker load -i ${var}.tar
    fi
done

echo "上传镜像列表"
#登陆新的harbor
docker login harbor.com:8843
cat ${dir_name}_images.txt | while read line
do
    if [[ ${line} =~ ^\#.* ]]; then
        continue
    fi
    echo "上传镜像: "${line}
    docker push $line
    echo "—————--------------------"
    sleep 1
done
```

* **执行脚本说明**

```shell
# 下载镜像
$ ./down_image.sh name
# 安装上传镜像
$ ./image_install.sh name
```

