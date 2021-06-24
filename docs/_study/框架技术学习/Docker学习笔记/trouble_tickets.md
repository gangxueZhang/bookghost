# Docker使用问题记录

## 子模块长不到Dockerfile

​		在一个多模块的maven项目中，只有父模块有Dockerfile文件，子模块不用打镜像，所以没有编写Dockerfile，导致项目打包时项目报错。

* **项目打包命令**

```
mvn clean package docker:build -DskipTests
```

* **报错日志信息**

```
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO] 
[INFO] demo-service                                                    [pom]
[INFO] demo-service-api                                                [jar]
[INFO] demo-service-biz                                                [jar]
[INFO] 
[INFO] --------------------< oscar.cn:demo-service >--------------------
[INFO] Building demo-service 1.0.0                                     [1/3]
[INFO] --------------------------------[ pom ]---------------------------------
[INFO] 
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ demo-service ---
[INFO] Deleting ${path}/demo-service/target
[INFO] 
[INFO] --- docker-maven-plugin:0.32.0:build (default-cli) @ demo-service ---
[INFO] Building tar: ${path}/demo-service/target/docker/demo-service/1.0.0/tmp/docker-build.tar
[INFO] DOCKER> [demo-service:1.0.0]: Created docker-build.tar in 1 second 
[INFO] DOCKER> [demo-service:1.0.0]: Built image sha256:62557
[INFO] DOCKER> [demo-service:1.0.0]: Removed old image sha256:9af64
[INFO] 
[INFO] ------------------< oscar.cn:demo-service-api >------------------
[INFO] Building demo-service-api 1.0.0                                 [2/3]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ demo-service-api ---
[INFO] Deleting ${path}/demo-service/demo-service-api/target
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ demo-service-api ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory ${path}/demo-service/demo-service-api/src/main/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ demo-service-api ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 92 source files to ${path}/demo-service/demo-service-api/target/classes
[INFO] 
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ demo-service-api ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory ${path}/demo-service/demo-service-api/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ demo-service-api ---
[INFO] No sources to compile
[INFO] 
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ demo-service-api ---
[INFO] Tests are skipped.
[INFO] 
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ demo-service-api ---
[INFO] Building jar: ${path}/demo-service/demo-service-api/target/demo-service-api.jar
[INFO] 
[INFO] --- docker-maven-plugin:0.32.0:build (default-cli) @ demo-service-api ---
[ERROR] DOCKER> Configured Dockerfile "${path}/demo-service/demo-service-api/Dockerfile" (resolved to "${path}/demo-service/demo-service-api/Dockerfile") doesn't exist
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for demo-service 1.0.0:
[INFO] 
[INFO] demo-service .................................... SUCCESS [ 12.313 s]
[INFO] demo-service-api ................................ FAILURE [  5.545 s]
[INFO] demo-service-biz ................................ SKIPPED
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  18.288 s
[INFO] Finished at: 2021-06-16T14:00:32+08:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal io.fabric8:docker-maven-plugin:0.32.0:build (default-cli) on project demo-service-api: Configured Dockerfile "${path}/demo-service/demo-service-api/Dockerfile" (resolved to "${path}/demo-service/demo-service-api/Dockerfile") doesn't exist -> [Help 1]
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException
[ERROR] 
[ERROR] After correcting the problems, you can resume the build with the command
[ERROR]   mvn <goals> -rf :demo-service-api
```

* **异常原因分析**

        由于docker打包插件配置在父模块中，子模块继承了父模块的配置，导致执行docker:build子模块也进行了build，因此需要在子模块中设置跳过build过程

* **异常解决方案**

        `docker-maven-plugin`插件只需要在`configuration`标签下配置`skipBuild`标签，当`skipBuild`的值为`true`时，项目会自动跳过`build`步骤。

> 有的版本中关键字可能不是`skipBuild`，在Idea中配置时，标签名输入`skip`会有提示，可以根据提示选择正确的标签名

* **打包插件修改后配置**

```xml
<!-- 父模块POM文件配置 -->
<properties>
    <skip.build>false</skip.build>
</properties>

<plugin>
    <groupId>io.fabric8</groupId>
    <artifactId>docker-maven-plugin</artifactId>
    <version>${docker.plugin.version}</version>
    <configuration>
        <!-- 是否跳过build -->
        <skipBuild>${skip.build}</skipBuild>
        <!--docker主机地址，用于完成docker各项功能-->
        <dockerHost>${docker.host}</dockerHost>
        <!--registry地址，用于推送，拉取镜像-->
        <registry>${docker.registry}</registry>
        <!--认证配置-->
        <authConfig>
            <push>
                <username>${docker.username}</username>
                <password>${docker.password}</password>
            </push>
        </authConfig>
        <images>
            <!-- 单个镜像配置 -->
            <image>
                <!--镜像名-->
                <name>${project.name}:${project.version}</name>
                <build>
                    <!--使用dockerFile文件-->
                    <dockerFile>${project.basedir}/Dockerfile</dockerFile>
                </build>
            </image>
        </images>
    </configuration>
</plugin>

<!-- 子模块POM文件配置 -->
<properties>
    <skip.build>true</skip.build>
</properties>
```

## Mac中Docker监听2375

* 第一种方式

```shell
# 启动容器并映射1234端口
$ docker run -d -v /var/run/docker.sock:/var/run/docker.sock -p 127.0.0.1:1234:1234 bobrik/socat TCP-LISTEN:1234,fork UNIX-CONNECT:/var/run/docker.sock
# 声明Docker主页面地址端口
$ export DOCKER_HOST=tcp://localhost:1234
```

* 第二种方式

```shell
$ docker run -d -v /var/run/docker.sock:/var/run/docker.sock -p 2375:2375 bobrik/socat TCP4-LISTEN:2375,fork,reuseaddr UNIX-CONNECT:/var/run/docker.sock
```

