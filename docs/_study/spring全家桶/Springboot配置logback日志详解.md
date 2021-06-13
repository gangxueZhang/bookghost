# Springboot配置logback日志详解

## 前言
   以前项目中日志管理不够规范，导致重要日志和一般日志混合在一起，不好定位问题。所以趁着新项目需要配置日志，特意整理一个规范的日志配置，方便以后的项目中可以复用。

## 相关技术版本

| 序号 | 技术名称                    | 技术版本      |
| ---- | --------------------------- | ------------- |
| 1    | spring-boot-starter-parent  | 2.3.4.RELEASE |
| 2    | spring-boot-starter-web     | 2.3.4.RELEASE |
| 3    | mybatis-spring-boot-starter | 2.1.4         |

## 开发包依赖

```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>2.3.4.RELEASE</version>
  <relativePath/> <!-- lookup parent from repository -->
</parent>

<dependencies>
  <!-- spring-boot-starter-web已经包含了logback相关依赖，不用额外添加依赖 -->
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  <dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.4</version>
  </dependency>
</dependencies>
```

## 项目中参考日志配置

### `logback-spring.xml`配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="false">
    <!-- 日志文件拆分限制，到达限制会对文件进行拆分 -->
    <property name="LOG_MAX" value="20MB"/>
    <!-- 日志文件总体大小限制，到达限制会清理，即使未到达文件保存时间限制 -->
    <property name="LOG_TOTAL" value="2GB"/>
    <!-- 日志文件保存时间限制，单位天，到达时间会被删除 -->
    <property name="LOG_HISTORY" value="5"/>
    <property name="LOG_PATH" value="${user.dir}"/>
    <!-- 日志打印级别控制变量，从application.properties配置中获取值 -->
    <springProperty scope="context" name="LOG_ROOT" source="spring.logback.root" defaultValue="info"/>
    <springProperty scope="context" name="LOG_MYBATIS" source="spring.logback.mybatis" defaultValue="info"/>
    <!-- appender是configuration的子节点，是负责写日志的组件。 -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} %highlight(%-5level) --- [%15.15(%thread)] %cyan(%-40.40(%logger{40})) : %msg%n</pattern>
            <charset>UTF-8</charset>
        </encoder>
    </appender>
    <appender name="debug_log" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <File>${LOG_PATH}/logs/monitor_debug.log</File>
        <append>true</append>
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>DEBUG</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
        <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <fileNamePattern>${LOG_PATH}/logs/monitor_debug.%d.%i.log</fileNamePattern>
            <maxHistory>${LOG_HISTORY}</maxHistory>
            <totalSizeCap>${LOG_TOTAL}</totalSizeCap>
            <maxFileSize>${LOG_MAX}</maxFileSize>
            <!-- 设置这个启动时MaxHistory才生效，才会删日志 -->
            <cleanHistoryOnStart>true</cleanHistoryOnStart>
        </rollingPolicy>
        <!--编码器-->
        <encoder>
            <!-- pattern节点，用来设置日志的输入格式 ps:日志文件中没有设置颜色,否则颜色部分会有ESC[0:39em等乱码-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} %-5level --- [%15.15(%thread)] %-40.40(%logger{40}) : %msg%n</pattern>
            <!-- 记录日志的编码:此处设置字符集 - -->
            <charset>UTF-8</charset>
        </encoder>
    </appender>
    <appender name="info_log" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <File>${LOG_PATH}/logs/monitor_info.log</File>
        <append>true</append>
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>INFO</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
        <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <fileNamePattern>${LOG_PATH}/logs/monitor_info.%d.%i.log</fileNamePattern>
            <maxHistory>${LOG_HISTORY}</maxHistory>
            <totalSizeCap>${LOG_TOTAL}</totalSizeCap>
            <maxFileSize>${LOG_MAX}</maxFileSize>
            <cleanHistoryOnStart>true</cleanHistoryOnStart>
        </rollingPolicy>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} %-5level --- [%15.15(%thread)] %-40.40(%logger{40}) : %msg%n</pattern>
            <charset>UTF-8</charset>
        </encoder>
    </appender>
		<!-- 配置mybatis中sql语句输出级别 -->
    <logger name="java.sql" level="${LOG_MYBATIS}" />
    <logger name="org.apache.ibatis" level="${LOG_MYBATIS}" />
    <logger name="com.mlamp.monitor.mapper" level="${LOG_MYBATIS}" />

    <root level="${LOG_ROOT}">
        <appender-ref ref="STDOUT" />
        <appender-ref ref="debug_log" />
        <appender-ref ref="info_log" />
    </root>
</configuration>
```

### `mybatis-config.xml`配置

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" 
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<settings>
		<setting name="logImpl" value="SLF4J" />
	</settings>
	<typeAliases>
	</typeAliases>
	<mappers>
	</mappers>
</configuration>
```

### `application.properties`配置

```xml
spring.logback.root=info
spring.logback.mybatis=debug
```

## logback详细配置说明

待补充



















