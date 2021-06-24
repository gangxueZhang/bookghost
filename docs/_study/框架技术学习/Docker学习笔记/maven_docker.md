# Maven整合Docker插件

​	在我们持续集成过程中，项目工程一般使用Maven编译打包，然后生成镜像，通过镜像上线，能够大大提供上线效率，同时能够快速动态扩容，快速回滚。docker-maven-plugin插件就是为了帮助我们在Maven工程中，通过简单的配置，自动生成镜像并推送到仓库中

## 环境及软件版本说明

* 系统： macOS Big Sur
* SpringBoot：2.4.1(版本需要在2.3以上)
* Java：version 1.8.0_212
* Maven：3.6.1
* docker-maven-plugin：0.36.0

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
  <docker.skip>false</docker.skip>
  <docker.namespace>test</docker.namespace>
  <docker.username>username</docker.username>
  <docker.password>password</docker.password>
  <docker.host>tcp://127.0.0.1:2375</docker.host>
  <docker.registry>registry:8843</docker.registry>
  <docker.plugin.version>0.36.0</docker.plugin.version>
</properties>

<plugins>
    <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <version>${spring-boot.version}</version>
        <configuration>
            <finalName>${project.build.finalName}</finalName>
            <layers>
                <!-- 构建多层写入镜像内容，会增加一个文件清单layers.idx -->
                <enabled>true</enabled>
            </layers>
        </configuration>
        <executions>
            <execution>
                <goals>
                    <goal>repackage</goal>
                </goals>
            </execution>
        </executions>
    </plugin>
    <plugin>
        <groupId>io.fabric8</groupId>
        <artifactId>docker-maven-plugin</artifactId>
        <version>${docker.plugin.version}</version>
        <configuration>
            <!-- 是否跳过步骤 -->
            <skip>${docker.skip}</skip>
            <!--这一部分是为了实现对远程docker容器的控制-->
            <!--docker主机地址,用于完成docker各项功能,注意是tcp不是http!-->
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
            <!--镜像相关配置,支持多镜像-->
            <images>
                <!-- 单个镜像配置 -->
                <image>
                    <!--镜像全限定名： 命名空间/镜像名:镜像版本 -->
                    <name>${docker.namespace}/${project.name}:${project.version}</name>
                    <!--别名:用于容器命名和在docker-compose.yml文件只能找到对应名字的配置-->
                    <alias>${project.name}</alias>
                    <build>
                        <!--使用dockerFile文件-->
                        <dockerFile>${project.basedir}/Dockerfile</dockerFile>
                    </build>
                    <!--容器run相关配置-->
                    <run>
                        <!--配置运行时容器命名策略为:别名,如果不指定则默认为none,即使用随机分配名称-->
                        <namingStrategy>alias</namingStrategy>
                    </run>
                </image>
            </images>
        </configuration>
    </plugin>
</plugins>
```

## Dockerfile配置参考

[体验SpringBoot(2.3)应用制作Docker镜像(官方方案)](https://blog.csdn.net/boling_cavalry/article/details/106597358)

```shell
# 指定基础镜像，这是分阶段构建的前期阶段
FROM openjdk:8-alpine as builder
# 执行工作目录
WORKDIR /build

# 配置参数
ARG JAR_FILE=nacos-provider/target/nacos-provider.jar
# 将编译构建得到的jar文件复制到镜像空间中
COPY ${JAR_FILE} app.jar

# 根据layer.idx从jar中提取要写入镜像的文件
# 通过工具spring-boot-jarmode-layertools从nacos-provider.jar中提取拆分后的构建结果
RUN java -Djarmode=layertools -jar app.jar extract && rm app.jar

# 正式构建镜像
FROM openjdk:8-alpine
# 使用"-Djava.security.egd=file:/dev/./urandom"加快随机数产生过程
ENV TZ=Asia/Shanghai JAVA_OPTS="-Xms256m -Xmx512m -Djava.security.egd=file:/dev/./urandom"
WORKDIR pig-upms

# 声明运行时容器提供服务端口，这只是一个声明运行时不会启这个端口的服务。
EXPOSE 8081

# 前一阶段从jar中提取除了多个文件，这里分别执行COPY命令复制到镜像空间中，每次COPY都是一个layer
COPY --from=builder /build/dependencies/ ./
COPY --from=builder /build/snapshot-dependencies/ ./
COPY --from=builder /build/spring-boot-loader/ ./
COPY --from=builder /build/application/ ./

CMD java $JAVA_OPTS org.springframework.boot.loader.JarLauncher
```

## 执行打包命令

```shell
# 只执行docker:build操作
$ mvn clean package docker:build -DskipTests
# 执行docker:push推送镜像
$ mvn docker:push
```

## 参考文件地址

[Maven 插件之 docker-maven-plugin 的使用](https://www.cnblogs.com/jpfss/p/10945324.html)

[fabric8io/docker-maven-plugin说明文档](fabric8io/docker-maven-plugin)

