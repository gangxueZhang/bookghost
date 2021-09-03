# Springboot集成Swagger

## Swagger2.x版本

### 依赖包引入配置

* **原装依赖配置**

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <scope>provided </scope>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <scope>provided </scope>
</dependency>
```

* **使用bootstrap-ui配置**

```xml
<!------------ 配置方式1 ---------------->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>swagger-bootstrap-ui</artifactId>
    <version>1.9.6</version>
</dependency>
<!------------  配置方式2 ------------->
<!------------ 后来bootstrap-ui更名为knife4j ------------->
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>2.0.4</version>
</dependency>
```

### Swagger配置类

```java
@Configuration//配置类
@EnableSwagger2 //swagger注解
public class SwaggerConfig {
    @Bean
    public Docket webApiConfig(){
        return new Docket(DocumentationType.SWAGGER_2)
                .groupName("webApi")
                .apiInfo(webApiInfo())
                .select()
                .paths(Predicates.not(PathSelectors.regex("/admin/.*")))
                .paths(Predicates.not(PathSelectors.regex("/error.*")))
                .build();
    }
    private ApiInfo webApiInfo(){
        return new ApiInfoBuilder()
                .title("Swagger 配置案例")
                .description("用于实践Swagger配置")
                .version("1.0")
                .contact(new Contact("java", "http://localhost", "test@qq.com"))
                .build();
    }
}
```

### 启动类添加扫描规则

```java
@SpringBootApplication
@ComponentScan(basePackages = "com.oscar")
public class NacosProviderDemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(NacosProviderDemoApplication.class, args);
    }
}
```

### 访问Swagger接口文档页面

* 原装依赖配置访问路径

```http
http://ip:port/swagger-ui.html
```

* bootstrap-ui依赖配置访问路径

```http
http://ip:port/doc.html
```

## Swagger3.x版本

### 依赖包引入配置

* **原装依赖配置**

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-boot-starter</artifactId>
    <version>3.0.0</version>
</dependency>
```

* **使用bootstrap-ui配置**

```xml
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>3.0.0</version>
</dependency>
```

### application.yml配置

```yaml
# ===== 自定义swagger配置 ===== #
swagger:
  enable: true
  application-name: ${spring.application.name}
  application-version: 1.0
  application-description: springfox swagger 3.0整合Demo
  try-host: http://localhost:${server.port}
```

### 启动类添加扫描规则

```java
@SpringBootApplication
@ComponentScan(basePackages = "com.oscar")
public class NacosProviderDemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(NacosProviderDemoApplication.class, args);
    }
}
```

### 访问Swagger接口文档页面

* 原装依赖配置访问路径

```http
http://ip:port/swagger-ui.html
```

* bootstrap-ui依赖配置访问路径

```http
http://ip:port/doc.html
```

## 