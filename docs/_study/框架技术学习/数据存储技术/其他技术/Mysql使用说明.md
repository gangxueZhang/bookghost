# Mysql使用说明

## Mysql更新时间(加上或减去一段时间)
```sql
# 添加一段时间函数语法
DATE_ADD(date,INTERVAL expr type)
# 减少一段时间函数语法
DATE_SUB(date,INTERVAL expr type)

# 参数说明：
date 参数是合法的日期表达式。
expr 参数是您希望添加的时间间隔。
type 参数可选值 MICROSECOND、SECOND、MINUTE、HOUR、DAY、WEEK、MONTH、QUARTER、YEAR、SECOND_MICROSECOND、MINUTE_MICROSECOND
MINUTE_SECOND、HOUR_MICROSECOND、HOUR_SECOND、HOUR_MINUTE、DAY_MICROSECOND、DAY_SECOND、DAY_MINUTE、DAY_HOUR、YEAR_MONTH

# sql语句示例
## 更新某个时间，每个时间加上一个星期
UPDATE comment t set t.time = DATE_ADD(t.time, INTERVAL 7 SECOND) ;
## 更新某个时间，每个时间减少一个星期
UPDATE comment t set t.time = DATE_SUB(t.time, INTERVAL 7 DAY) ;
## 更新某个时间，当前时间添加一个星期
UPDATE comment t set t.time = DATE_ADD(now(), INTERVAL 7 DAY) ;
```

## JDBC.URL常用参数说明
[JDBC完整请配置参考官方文档](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-reference-configuration-properties.html)
* JDBC URL格式说明

```sql
jdbc.url=jdbc:mysql://{host}:{port}/{dbName}
```

* 使用`sessionVariables`设`session`变量值

  有些场景使用过程中，需要设置`session`变量来改变`mysql的`原有配置，以Group by从句举例

  * 现象

  ```sql
  mysql> select *  from SC group by Sid;
  ERROR 1055 (42000): Expression #2 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'school_db.SC.Cid' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
  ```

  * 原因

    sql_mode=only_full_group_by要求所有查出来的非聚合字段必须要在GROUP BY从句中

  * 解决办法

    在sql_mode变量中去掉only_full_group_by选项

    ```sql
    # 查看sql_model参数命令
    mysql> SELECT @@GLOBAL.sql_mode;
    +-------------------------------------------------------------------------------------------------------------------------------------------+
    | @@GLOBAL.sql_mode                                                                                                                         |
    +-------------------------------------------------------------------------------------------------------------------------------------------+
    | ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION |
    +-------------------------------------------------------------------------------------------------------------------------------------------+
    1 row in set (0.00 sec)
    
    mysql> SELECT @@SESSION.sql_mode;
    +-------------------------------------------------------------------------------------------------------------------------------------------+
    | @@SESSION.sql_mode                                                                                                                        |
    +-------------------------------------------------------------------------------------------------------------------------------------------+
    | ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION |
    +-------------------------------------------------------------------------------------------------------------------------------------------+
    1 row in set (0.00 sec)
    
    # mysql命令配置
    SET GLOBAL sql_mode ='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
    SET SESSION sql_mode ='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
    
    # JDBC.URL中配置
    jdbc.url=jdbc:mysql://{host}:{port}/{dbName}?sessionVariables=sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'
    ```

