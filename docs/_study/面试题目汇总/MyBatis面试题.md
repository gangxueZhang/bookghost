# MyBatis面试题

## 1、什么是Mybatis

 1、Mybatis 是一个半 ORM（ 对象关系映射）框架，它内部封装了JDBC，开发时只需 要关注 SQL 语句本身， 不需要花费精力去处理加载驱动、创建连接、创建 statement 等繁杂的过程。程序员直接编写原生态 sql，可以严格控制 sql 执行性能， 灵活度高。

2、MyBatis 可以使用 XML 或注解来配置和映射原生信息， 将 POJO映射成数据库中的 记录， 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。

3、通过 xml 文件或注解的方式将要执行的各种 statement 配置起来， 并通过java 对象 和 statement 中 sql 的动态参数进行映射生成最终执行的 sql 语句，最后由 mybatis 框 架执行 sql 并将结果映射为 java 对象并返回。（ 从执行 sql 到返回 result 的过程）。

## 2、Mybaits的优点

1、基于 SQL 语句编程，相当灵活，不会对应用程序或者数据库的现有设计造成任何影 响，SQL 写在 XML 里，解除 sql 与程序代码的耦合，便于统一管理；提供 XML 标签， 支持编写动态 SQL 语句， 并可重用。 

2、与 JDBC 相比，减少了 50% 以上的代码量，消除了 JDBC 大量冗余的代码，不需要手 动开关连接；

3、很好的与各种数据库兼容（ 因为 MyBatis 使用 JDBC 来连接数据库，所以只要JDBC 支持的数据库 MyBatis 都支持）。 

4、能够与 Spring 很好的集成； 5、提供映射标签， 支持对象与数据库的 ORM 字段关系映射； 提供对象关系映射 标签， 支持对象关系组件维护。

## 3、MyBatis 框架的缺点

1、SQL 语句的编写工作量较大， 尤其当字段多、关联表多时， 对开发人员编写SQL 语句的功底有一定要求。 

2、SQL 语句依赖于数据库， 导致数据库移植性差， 不能随意更换数据库。

## 4、MyBatis 框架适用场合

1、Mybatis专注于 SQL 本身， 是一个足够灵活的 DAO 层解决方案。

2、对性能的要求很高，或者需求变化较多的项目，如互联网项目， MyBatis 将是不错的 选择。

## 5、MyBatis 与Hibernate 有哪些不同

1、Mybatis和hibernate不同，它不完全是一个ORM框架，因为MyBatis需要程序员自己编写Sql语句。

2、Mybatis直接编写原生态 sql， 可以严格控制sql执行性能， 灵活度高， 非常适合对关系数据模型要求不高的软件开发， 因为这类软件需求变化频繁， 一但需求变化要求迅速输出成果。但是灵活的前提是mybatis无法做到数据库无关性， 如果需要实现支持多种数据库的软件，则需要自定义多套sql映射文件，工作量大。 

3、Hibernate 对象/关系映射能力强， 数据库无关性好， 对于关系模型要求高的软件， 如果用 hibernate 开发可以节省很多代码， 提高效率

## 6、#{}和${}的区别是什么

1、#{}是预编译处理，\${}是字符串替换。 

2、Mybatis在处理#{}时，会将 sql 中的#{}替换为?号，调用 PreparedStatement 的set 方 法来赋值； 

3、Mybatis在处理${}时， 就是把${}替换成变量的值。使用#{}可以有效的防止SQL注入， 提高系统安全性。

## 7、实体类属性名和表字段名不一样 ，如何处理

第1种： 通过在查询的sql语句中定义字段名的别名， 让字段名的别名和实体类的属性名一致。

```sql
<select id=”selectorder” parametertype=”int” resultetype=” me.gacl.domain.order”> 
	select order_id id, order_no orderno ,order_price price form orders where order_id=#{id}; 
</select>
```

第2种： 通过\<resultMap>来映射字段名和实体类属性名的一一对应的关系。

```
<select id="getOrder" parameterType="int" resultMap="orderresultmap">
	select order_id,order_no,order_price from orders where order_id=#{id} 
</select> 
<resultMap type=”me.gacl.domain.order” id=”orderresultmap”> 
	<id property=”id” column=”order_id”> 
	<result property = “orderno” column =”order_no”/> 
	<result property=”price” column=”order_price” /> 
</resultMap>
```

## 8、模糊查询like语句该怎么写

第1种：在Java代码中添加sql通配符。

```sql
string wildcardname = “%smi%”; 
list<name> names = mapper.selectlike(wildcardname);

<select id=”selectlike”> 
	select * from foo where bar like #{value} 
</select>
```

第2种：在sql语句中拼接通配符，会引起 sql 注入

```sql
string wildcardname = “smi”; 
list<name> names = mapper.selectlike(wildcardname);

<select id=”selectlike”> 
	select * from foo where bar like "%"#{value}"%" 
</select>
```

## 9、和Xml映射的接口中，方法能重载吗

1、Dao接口即Mapper接口。接口的全限名，就是映射文件中的namespace的值； 接口的方法名， 就是映射文件中Mapper的 Statement的id值； 接口方法内的参数， 就是传递给sql的参数。 Mapper接口是没有实现类的，当调用接口方法时，接口全限名+方法名拼接字符串作为key值， 可唯一定位一个MapperStatement。在Mybatis中， 每一个 \<select>、\<insert>、\<update>、\<delete>标签，都会被解析为一个 MapperStatement 对象。

2、举例：com.demo.mapper.StudentDao.findStudentById， 可以唯一找到namespace为com.demo.mapper..StudentDao下面id为findStudentById的MapperStatement 。 Mapper接口里的方法，是不能重载的，因为是使用全限名+方法名的保存和寻找策略。Mapper接口的工作原理是JDK动态代理， Mybatis运行时会使用JDK动态代理为Mapper接口生成代理对象proxy， 代理对象会拦截接口方法， 转而执行MapperStatement所代表的sql， 然后将sql执行结果返回。

## 10、Mybatis如何进行分页，分页插件的原理是什么

1、Mybatis使用RowBounds对象进行分页， 它是针对ResultSet结果集执行的内存分页，而非物理分页。可以在sql内直接书写带有物理分页的参数来完成物理分页功能， 也可以使用分页插件来完成物理分页。 

2、分页插件的基本原理是使用Mybatis提供的插件接口， 实现自定义插件， 在插件的拦截方法内拦截待执行的sql，然后重写sql，根据 dialect方言，添加对应的物理分页语句和物理分页参数。

## 11、Mybatis如何将sql执行结果封装为目标对象并返回， 都有哪些映射形式

1、第一种使用\<resultMap>标签， 逐一定义数据库列名和对象属性名之间的映射关系。

2、第二种是使用sql列的别名功能， 将列的别名书写为对象属性名。

​    有了列名与属性名的映射关系后， Mybatis通过反射创建对象， 同时使用反射给对象的属性逐一赋值并返回， 那些找不到映射关系的属性， 是无法完成赋值的。

## 12、如何执行批量插入

