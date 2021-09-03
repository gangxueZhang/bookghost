# 前言

**[Jackson开源项目地址](https://github.com/FasterXML/jackson)**

Jackson被称为“Java Json 库”或“Java的最佳Json解析器”。或者简单地称为“Java的Json”。

    不仅如此，Jackson是一套适用于Java(和 JVM平台)的数据处理工具，包括旗舰流式JSON解析器/生成器库、匹配数据绑定库(POJO与 JSON)和附加数据格式模块以处理以Avro、BSON、CBOR、CSV、Smile、(Java) Properties、Protobuf、TOML、XML 或 YAML 编码的数据；甚至是支持广泛使用的数据类型的大量数据格式模块，例如 Guava、Joda、PCollections 等等。

    Jackson的核心组件存在于他们自己的项目中——包括三个核心包(流、数据绑定、注解)；数据格式库；数据类型库； JAX-RS 提供者；和一组其他扩展模块，Jackson项目充当将所有部分链接在一起的中心枢纽。

# 环境版本说明

```properties
jackson-annotations: 2.11.2
spring-boot: 2.3.4.RELEASE
```

# Jackson的流使用说明

# Jackson数据绑定说明

# Jackson通用注解说明

    该项目包含 Jackson 数据处理器的通用注解，用于值和处理程序类型。 唯一未包含的注解是需要依赖于 Databind 包的注解。 请注意，仅包含注解本身（和相关的值类），但不包含使用注解的功能。

## @JacksonAnnotation

    这个注解经常用于**Jackson**自定义注解中，用来标记这是一个**Jackson**注解

## @JacksonAnnotationsInside

     这个注解用来标记**Jackson**复合注解,当你使用多个**Jackson**注解组合成一个自定义注解时会用到它。

```java
/** 非空以及忽略未知属性 **/
@Retention(RetentionPolicy.RUNTIME)
@JacksonAnnotationsInside
@JsonInclude(Include.NON_NULL)
@JsonIgnoreProperties(ignoreUnknown = true)
public @interface NotNullAndIgnoreAnnotation {}
```

## @JacksonInject

    Jackson特定的注解用于指示被注解属性的值将被"注入"，根据 <code>ObjectMapper</code> 配置的值设置）。尽管可以注入默认值也允许从Json进行可选覆盖，但是通常该属性不会从 JSON反序列化。

* 使用具体关键字的方式注入

```java
//userInput为FALSE的时候是属性值不覆盖默认值,如果只是为了值为空的时候注入默认的值，按如下配置即可(目前测试没有效果)。
@JacksonInject(value= "username", useInput= OptBoolean.FALSE)
private String username; 

// 测试代码
public static void main(String[] args) throws IOException {
    ObjectMapper mapper = new ObjectMapper();
    InjectableValues.Std injectableValues = new InjectableValues.Std();
    //如果username字段为空，那么就默认是小明
    injectableValues.addValue("username","小明");
    mapper.setInjectableValues(injectableValues);
    String jsonValue = "{}";
    User user = mapper.readValue(jsonValue, User.class);
    System.out.println(JSONObject.toJSONString(user));
}

// 打印结果
{username='小明', password='null', age=null}
```

* 按数据类型进行注入

```java
// 使用类型的方式，不需要指定字段名
@JacksonInject
private String username; 

// 如下配置值就可以将@JacksonInject注解的String类型的字段默认值设置成"--""
InjectableValues.Std injectableValues = new InjectableValues.Std();
injectableValues.addValue(String.class,"--");
```

## @JsonAlias

    在反序列化的时候来对Java Bean的属性进行名称绑定，可以绑定多个**json**的键名

```java
public class User {
  @JsonAlias({"name","user"})
  private String username;
  private Integer age;
}

// 测试代码
public static void main(String[] args) throws JsonProcessingException {
    ObjectMapper mapper = new ObjectMapper();
    String jsonValue = "{\"name\":\"小明\",\"age\":15}";
    User user = mapper.readValue(jsonValue, User.class);
    System.out.println(JSONObject.toJSONString(user));
}

// 打印结果
{"age":15,"username":"小明"}
```

<font color="red">**注意:** 序列化的时候仍然是原始的属性名称，只是在反序列化的时候别名有用，如果多个字段对应了Bean里面的同一个属性，依然会报Json解析异常错误的。</font>

## @JsonAnyGetter

    通常和`@JsonAnySetter`一起使用，主要用来处理反序列时未匹配上的字段，该注解使用有以下限制：

* 只能用于非静态、无参数方法
* 带注解的方法的返回类型<b>必须</b>是{@link java.util.Map}）
* 在一个实体类中有且只能用在一个方法上
* 序列化的时候会将Map中的键值对打平，提升到上一级json中。

```java
public class User {
    private String username;
    private Map<String,String> map = new HashMap<>();
    @JsonAnyGetter
    public Map<String,String> getMap() {
        return map;
    }
}  

// 测试代码
public static void main(String[] args) throws IOException {
    User user = new User();
    user.setUsername("oscar");
    Map<String, String> map = user.getMap();
    map.put("test1","testOne");
    map.put("test2","testTwo");
    ObjectMapper objectMapper = new ObjectMapper();
    String value = objectMapper.writeValueAsString(user);
    System.out.println(value);
}

// 测试结果
{"username":"oscar","test2":"testTwo","test1":"testOne"}
```

可以看出加上这个注解以后，序列化的时候就会将Map里面的值，也相当于实体类里面的字段给显示出来。

## @JsonAnySetter

 通常和`@JsonAnyGetter`一起使用，主要用来处理反序列时未匹配上的字段，该注解使用有以下限制：

* 用在非静态方法上，注解的方法必须有两个参数，第一个是属性，第二个是值
* 用在对象的Map类型属性上面
* 反序列化的时候将对应不上的字段全部放到Map里面

```java
public class User {
   	private String username;
    @JsonAnySetter  //方法和属性上只需要一个就可以
    private Map<String,String> map = new HashMap<>();
    /** @JsonAnySetter
    public void setMap(String key,String value) {
        this.map.put(key,value);
    }*/
}

// 测试代码
public static void main(String[] args) throws IOException {
    String jsonStr = "{\"username\":\"oscar\",\"test2\":\"testTwo\",\"test1\":\"testOne\"}";
    ObjectMapper objectMapper = new ObjectMapper();
    Test user = objectMapper.readValue(jsonStr, Test.class);
    System.out.println(user);
}

// 测试结果
Test(username=oscar, map={test2=testTwo, test1=testOne})
```

## @JsonAutoDetect

    类注解，用于定义自动检测可见限制，通过方法种类和访问级别进行控制。

* 方法种类
  * **getterVisibility：**自动检测常规 getter 方法所需的最低可见性。
  * **isGetterVisibility：**自动检测 is-getter 方法所需的最低可见性。
  * **setterVisibility：**自动检测 setter 方法所需的最低可见性。
  * **creatorVisibility：**自动检测Creator方法所需的最低可见性，除了无参数构造函数（无论如何总是被检测到）。
  * **fieldVisibility：**自动检测成员字段所需的最低可见性。

* 访问级别
  * **Visibility.ANY：**所有类型的访问修饰符都是可以接受的，从私有到公共。
  * **Visibility.NON_PRIVATE：**除`private`之外的任何其他访问修饰符都被认为是可自动检测的。
  * **Visibility.PROTECTED_AND_PUBLIC：**访问修饰符`protected`和`public`是可自动检测的。
  * **Visibility.PUBLIC_ONLY：**只有`public`访问修饰符被视为可自动检测的值。
  * **Visibility.NONE：**没有访问修饰符可自动检测的：这可用于显式禁用指定类型的自动检测。
  * **Visibility.DEFAULT：**使用的默认可见性级别（取决于上下文）的值。 这通常意味着要使用继承的值（来自父可见性设置）。

```java
@JsonAutoDetect(fieldVisibility = JsonAutoDetect.Visibility.PROTECTED_AND_PUBLIC)
public class User {
    private String username = "oscar";
    public String password = "123456";
    protected String phone = "13762876376";
    Integer age = 15;
}  

// 测试代码
public static void main(String[] args) throws JsonProcessingException {
    Test user = new Test();
    ObjectMapper mapper = new ObjectMapper();
    String jsonString = mapper.writerWithDefaultPrettyPrinter().writeValueAsString(user);
    System.out.println(jsonString);
}

// 测试结果
{ "password" : "123456", "phone" : "13762876376"}
```

## @JsonBackReference

用于指示关联属性是字段之间双向链接的一部分的注解；并且它的作用是“子”（或“后”）链接。 属性的值类型必须是 bean：不能是Collection、Map、Array 或枚举。







## JacksonAnnotationValue

     标记接口，由 {@link JsonFormat.Value} 等值类使用的标记接口，用于包含来自 Jackson 注释之一的信息，并且可以直接从这些注注解例化，以及以编程方式构造和可能合并。 使用此类标记的原因是允许对某些注解进行通用处理，并允许从注解以外的来源更轻松地注入配置。

```java
public interface JacksonAnnotationValue<A extends Annotation>
{
    /**
     * 可用于查找可以用作值实例源的实际注释的自我检查方法。 
     * @return 为创建此值类实例的注解类
     */
    public Class<A> valueFor();
}
```





# 附录A

## 参考文档地址

[jackSon中@JacksonInject 注解](https://blog.csdn.net/weixin_44130081/article/details/89675657)

[上篇|Jackson注解的用法和场景，不看巨亏](https://cloud.tencent.com/developer/article/1851260)

[jackSon中@JsonAnySetter @JsonAnyGetter注解详解](http://www.manongjc.com/article/114528.html)

