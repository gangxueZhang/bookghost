# Docker安装教程
## centos系统安装
前提条件
目前，CentOS 仅发行版本中的内核支持 Docker。  
Docker 运行在 CentOS 7 上，要求系统为64位、系统内核版本为 3.10 以上。  
Docker 运行在 CentOS-6.5 或更高的版本的 CentOS 上，要求系统为64位、系统内核版本为 2.6.32-431 或者更高版本。  
### 简易安装：使用`yum安装`(CentOS 7下)
Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker 。
```shell
#查看你当前的内核版本
uname -r

#安装 Docker
yum -y install docker

#启动 Docker 后台服务
service docker start

#测试运行 hello-world,由于本地没有hello-world这个镜像，所以会下载一个hello-world的镜像，并在容器内运行。
docker run hello-world
```
### 使用脚本安装 Docker
* 使用 sudo 或 root 权限登录 Centos。
* 确保 yum 包更新到最新。

```shell
#确保 yum 包更新到最新
sudo yum update

#执行 Docker 安装脚本,执行这个脚本会添加 docker.repo 源并安装 Docker。
curl -fsSL https://get.docker.com/ | sh

#启动 Docker 进程
sudo service docker start

#验证 docker 是否安装成功并在容器中执行一个测试的镜像
sudo docker r
```
### 官网安装说明
Docker 支持以下的 64 位 CentOS 版本：  
* CentOS 7
* CentOS 8
* 更高版本...  

该 centos-extras 库必须启用。默认情况下，此仓库是启用的，但是如果已禁用它，则需要重新启用它。  
建议使用 overlay2 存储驱动程序。

#### 卸载旧版本
较旧的 Docker 版本称为 docker 或 docker-engine 。如果已安装这些程序，请卸载它们以及相关的依赖项。
```shell
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```
#### 安装 Docker Engine-Community
##### 使用 Docker 仓库进行安装
在新主机上首次安装 `Docker Engine-Community`之前，需要设置`Docker`仓库。之后，您可以从仓库安装和更新`Docker`。
##### 设置仓库

1. 安装所需的软件包`yum-utils`提供了 yum-config-manager ，并且`device mapper`存储驱动程序需要`device-mapper-persistent-data`和`lvm2`。  

```shell
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```
2. 使用以下命令来设置稳定的仓库。

```shell
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```
##### 可选操作：启用`nightly`和`test`存储库
这些存储库包含在上面的`docker-ce.repo`文件中，但默认情况下处于禁用状态。 您可以和稳定存储库一起启用它们。   
**Stable**：稳定版可为您提供最新版本，以提供一般可用性。  
**Test**： 测试提供了预发布的版本，这些版本可以在正式发布之前进行测试。  
**Nightly**：Nightly为您提供了下一个主要版本的最新工作进展。  

* 启用`Nightly`仓库

```shell
$ sudo yum-config-manager --enable docker-ce-nightly
```
* 启用`test`仓库

```shell
sudo yum-config-manager --enable docker-ce-test
```
* 禁用`Nightly`仓库

```shell
$ sudo yum-config-manager --disable docker-ce-nightly
```
##### 安装 Docker Engine-Community
安装最新版本的`Docker Engine-Community`和`containerd`，或者转到下一步安装特定版本：
```shell
$ sudo yum install docker-ce docker-ce-cli containerd.io
```
如果提示您接受 GPG 密钥，请选是。
```txt
有多个 Docker 仓库吗？

如果启用了多个 Docker 仓库，则在未在 yum install 或 yum update 命令中指定版本的情况下，进行的安装或更新将始终安装最高版本，这可能不适合您的稳定性需求。
```

Docker 安装完默认未启动。并且已经创建好 docker 用户组，但该用户组下没有用户。  

**要安装特定版本的 Docker Engine-Community，请在存储库中列出可用版本，然后选择并安装：**

1. 列出并排序您存储库中可用的版本。此示例按版本号（从高到低）对结果进行排序。

```shell
$ yum list docker-ce --showduplicates | sort -r
docker-ce.x86_64  3:18.09.1-3.el7                     docker-ce-stable
docker-ce.x86_64  3:18.09.0-3.el7                     docker-ce-stable
docker-ce.x86_64  18.06.1.ce-3.el7                    docker-ce-stable
docker-ce.x86_64  18.06.0.ce-3.el7                    docker-ce-stable
```
2. 通过其完整的软件包名称安装特定版本，该软件包名称是软件包名称（docker-ce）加上版本字符串（第二列），从第一个冒号（:）一直到第一个连字符，并用连字符（-）分隔。例如：docker-ce-18.09.1。

```shell
$ sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
```
启动 Docker。
```shell
$ sudo systemctl start docker
```
通过运行 hello-world 映像来验证是否正确安装了 Docker Engine-Community 。
```shell
$ sudo docker run hello-world
```