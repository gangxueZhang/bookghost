# Maven整合Docker插件

​		在我们持续集成过程中，项目工程一般使用Maven编译打包，然后生成镜像，通过镜像上线，能够大大提供上线效率，同时能够快速动态扩容，快速回滚。docker-maven-plugin插件就是为了帮助我们在Maven工程中，通过简单的配置，自动生成镜像并推送到仓库中

## 环境及软件版本说明

* 系统： macOS Big Sur
* SpringBoot：2.2.6.RELEASE
* Java：version 1.8.0_212
* Maven：3.6.1
* docker-maven-plugin：0.32.0

## 配置DOCKER_HOST

docker-maven-plugin插件默认连接本地Docker地址为：localhost:2375，所以我们需要在本地配置docker监听2375端口。

* mac设置docker监听2375端口方式1

```shell
# 启动容器并映射1234端口
$ docker run -d -v /var/run/docker.sock:/var/run/docker.sock -p 127.0.0.1:1234:1234 bobrik/socat TCP-LISTEN:1234,fork UNIX-CONNECT:/var/run/docker.sock
# 声明Docker主页面地址端口
$ export DOCKER_HOST=tcp://localhost:1234
```

* mac设置docker监听2375端口方式2

```shell
$ docker run -d -v /var/run/docker.sock:/var/run/docker.sock -p 2375:2375 bobrik/socat TCP4-LISTEN:2375,fork,reuseaddr UNIX-CONNECT:/var/run/docker.sock
```

* 如果不配置DOCKER_HOST可以在命令行指定

```shell
# 将DOCKER_HOST指定到本机
DOCKER_HOST=unix:///var/run/docker.sock mvn clean package docker:build
```

## Pom文件中插件配置

```xml
<properties>
  <skip.build>false</skip.build>
  <docker.namespace>test</docker.namespace>
  <docker.username>username</docker.username>
  <docker.password>password</docker.password>
  <docker.host>tcp://127.0.0.1:2375</docker.host>
  <docker.registry>registry:8843</docker.registry>
  <docker.plugin.version>0.32.0</docker.plugin.version>
</properties>

<plugin>
	<groupId>io.fabric8</groupId>
	<artifactId>docker-maven-plugin</artifactId>
	<version>${docker.plugin.version}</version>
	<configuration>
    <!-- 是否跳过build阶段，不build设置成false即可 -->
    <skipBuild>${skip.build}</skipBuild>
		<!--docker主机地址，用于完成docker各项功能-->
		<dockerHost>${docker.host}</dockerHost>
		<!--registry地址，用于推送，拉取镜像，这里用的是阿里的registry-->
		<registry>${docker.registry}</registry>
		<!--认证配置，用于私有registry认证，如果忘记了可以去阿里的registry查看-->
		<authConfig>
			<push>
				<username>${docker.username}</username>
				<password>${docker.password}</password>
			</push>
		</authConfig>
		<images>
			<!-- 单个镜像配置 -->
			<image>
				<!--镜像名(含版本号)-->
				<name>${project.name}:${project.version}</name>
				<build>
					<!--使用dockerFile文件-->
					<dockerFile>${project.basedir}/Dockerfile</dockerFile>
				</build>
			</image>
		</images>
	</configuration>
</plugin>
```

## Dockerfile配置参考

```shell
FROM openjdk:8-alpine as builder
WORKDIR /build

ARG JAR_FILE=nacos-provider/target/nacos-provider.jar
COPY ${JAR_FILE} app.jar
RUN java -Djarmode=layertools -jar app.jar extract && rm app.jar

FROM openjdk:8-alpine
ENV TZ=Asia/Shanghai JAVA_OPTS="-Xms256m -Xmx512m -Djava.security.egd=file:/dev/./urandom"
WORKDIR pig-upms

EXPOSE 8081

COPY --from=builder /build/dependencies/ ./
COPY --from=builder /build/snapshot-dependencies/ ./
COPY --from=builder /build/spring-boot-loader/ ./
COPY --from=builder /build/application/ ./

CMD java $JAVA_OPTS org.springframework.boot.loader.JarLauncher
```

## 执行打包命令

```shell
# 只执行 build 操作
$ mvn clean package docker:build -DskipTests
# 执行 build 完成后 push 镜像
$ mvn clean package docker:build -DpushImage -DskipTests
# 执行 build 并 push 指定 tag 的镜像
mvn clean package docker:build -DpushImageTags -DdockerImageTags=imageTag_1 -DskipTests
```

* POM文件中指定配置镜像

```xml
<!-- docker-maven-plugin插件中配置 -->
<configuration>
  <imageTags>
     <imageTag>imageTag_1</imageTag>
  </imageTags>
</configuration>
```



## 参考文件地址

[Maven 插件之 docker-maven-plugin 的使用](https://www.cnblogs.com/jpfss/p/10945324.html)