# Canal下发的MySQL日志数据格式说明

        `Canal`是阿里巴巴旗下的一款开源项目，纯`Java`开发。基于数据库增量日志解析，提供增量数据订阅&消费，目前主要支持了`MySQL`（也支持`mariaDB`）。

        `Canal`的工作原理就是把自己伪装成`MySQL slave`，模拟`MySQL slave`的交互协议向`MySQL Mater`发送` dump`协议，`MySQL mater`收到`Canal`发送过来的`dump`请求，开始推送`binary log`给`Canal`，然后`Canal`解析`Binary log`，再发送到存储目的地，比如`MySQL`，`Kafka`，`Elastic Search`等等。

        但是canal的数据同步**不是全量的，而是增量**。基于binary log增量订阅和消费，canal可以做：

- 数据库镜像
- 数据库实时备份
- 索引构建和实时维护
- 业务cache(缓存)刷新
- 带业务逻辑的增量数据处理

## 注意事项

1、目前微伴使用`Canal`将`Mysql`数据增量转发到`Kafka`中，供下游实时同步微伴数据变化，不支持定制数据格式。

2、`Canal`转发到`Kafka`的数据是以表为单位的，不能多表聚合同步，同步的是数据表中`Insert、Update、Delete`相关的数据。

3、目前微伴`Canal`同步数据的Kafka统一使用`库名.表名`的方式命名，比如：`d_weiban.staff_department_relation`。

## Insert数据格式说明

* binlog日志说明

```json
{
    "data": [{....}],               // 当前事件操作的数据最新状态，和操作后数据库里保存的值一致
    "database": "d_weiban",         // 数据库
    "es": 1648804115000,
    "id": 686344,
    "isDdl": false,
    "mysqlType": {.....},           // 所有字段的数据类型
    "old": null,
    "pkNames": [...],               // 主键
    "sql": "",
    "sqlType": {....},              // sql的数据类型
    "table": "staff_department_relation",     // 表名
    "ts": 1648804115758,
    "type": "INSERT"           			// 数据操作类型
}
```

* 完整binlog日志案例：

```json
{
    "data": [
        {
            "id": "17936128",
            "created_at": "2022-03-31 20:59:40",
            "corp_id": "1719376652447854593",
            "department_id": null,
            "department_ext_id": "204",
            "staff_id": "10804138",
            "order": "0",
            "is_leader_in_dept": "0",
            "deleted": "0",
            "staff_ext_id": "wansimeng",
            "updated_at": "2022-03-31 20:59:40"
        }
    ],
    "database": "d_weiban",
    "es": 1648804115000,
    "id": 686344,
    "isDdl": false,
    "mysqlType": {
        "id": "int(11)",
        "created_at": "timestamp",
        "corp_id": "bigint(20)",
        "department_id": "int(11)",
        "department_ext_id": "varchar(64)",
        "staff_id": "int(11)",
        "order": "bigint(20)",
        "is_leader_in_dept": "tinyint(1)",
        "deleted": "tinyint(1)",
        "staff_ext_id": "varchar(64)",
        "updated_at": "timestamp"
    },
    "old": null,
    "pkNames": [
        "id"
    ],
    "sql": "",
    "sqlType": {
        "id": 4,
        "created_at": 93,
        "corp_id": -5,
        "department_id": 4,
        "department_ext_id": 12,
        "staff_id": 4,
        "order": -5,
        "is_leader_in_dept": -6,
        "deleted": -6,
        "staff_ext_id": 12,
        "updated_at": 93
    },
    "table": "staff_department_relation",
    "ts": 1648804115758,
    "type": "INSERT"
}
```

## Update数据格式说明

* binlog日志说明

```json
{
    "data": [{....}],               // 当前事件操作的数据最新状态，和操作后数据库里保存的值一致
    "database": "d_weiban",         // 数据库
    "es": 1648804146000,
    "id": 686353,
    "isDdl": false,
    "mysqlType": {.....},           // 所有字段的数据类型
    "old": [{}],                    // 修改涉及的字段以及旧值
    "pkNames": [...],               // 主键
    "sql": "",
    "sqlType": {....},              // sql的数据类型
    "table": "staff_department_relation",     // 表名
    "ts": 1648804146507,
    "type": "UPDATE"         			  // 数据操作类型
}
```

* 完整binlog日志案例：

```json
{
    "data": [
        {
            "id": "17936128",
            "created_at": "2022-03-31 20:59:40",
            "corp_id": "1719376652447854593",
            "department_id": null,
            "department_ext_id": "204",
            "staff_id": "10804138",
            "order": "0",
            "is_leader_in_dept": "0",
            "deleted": "0",
            "staff_ext_id": "wansimeng",
            "updated_at": "2022-03-31 21:59:40"
        }
    ],
    "database": "d_weiban",
    "es": 1648804146000,
    "id": 686353,
    "isDdl": false,
    "mysqlType": {
        "id": "int(11)",
        "created_at": "timestamp",
        "corp_id": "bigint(20)",
        "department_id": "int(11)",
        "department_ext_id": "varchar(64)",
        "staff_id": "int(11)",
        "order": "bigint(20)",
        "is_leader_in_dept": "tinyint(1)",
        "deleted": "tinyint(1)",
        "staff_ext_id": "varchar(64)",
        "updated_at": "timestamp"
    },
    "old": [
        {
            "updated_at": "2022-03-31 20:59:40"
        }
    ],
    "pkNames": [
        "id"
    ],
    "sql": "",
    "sqlType": {
        "id": 4,
        "created_at": 93,
        "corp_id": -5,
        "department_id": 4,
        "department_ext_id": 12,
        "staff_id": 4,
        "order": -5,
        "is_leader_in_dept": -6,
        "deleted": -6,
        "staff_ext_id": 12,
        "updated_at": 93
    },
    "table": "staff_department_relation",
    "ts": 1648804146507,
    "type": "UPDATE"
}
```

## Delete数据格式说明

* binlog日志说明

```json
{
    "data": [{....}],               // 当前事件操作的数据最新状态，和操作后数据库里保存的值一致
    "database": "d_weiban",         // 数据库
    "es": 1648804155000,
    "id": 686356,
    "isDdl": false,
    "mysqlType": {.....},           // 所有字段的数据类型
    "old": null,
    "pkNames": [...],               // 主键
    "sql": "",
    "sqlType": {....},              // sql的数据类型
    "table": "staff_department_relation",        // 表名
    "ts": 1648804155625,
    "type": "DELETE"           			// 数据操作类型
}
```

* 完整binlog日志案例：

```json
{
    "data": [
        {
            "id": "17936128",
            "created_at": "2022-03-31 20:59:40",
            "corp_id": "1719376652447854593",
            "department_id": null,
            "department_ext_id": "204",
            "staff_id": "10804138",
            "order": "0",
            "is_leader_in_dept": "0",
            "deleted": "0",
            "staff_ext_id": "wansimeng",
            "updated_at": "2022-03-31 21:59:40"
        }
    ],
    "database": "d_weiban",
    "es": 1648804155000,
    "id": 686356,
    "isDdl": false,
    "mysqlType": {
        "id": "int(11)",
        "created_at": "timestamp",
        "corp_id": "bigint(20)",
        "department_id": "int(11)",
        "department_ext_id": "varchar(64)",
        "staff_id": "int(11)",
        "order": "bigint(20)",
        "is_leader_in_dept": "tinyint(1)",
        "deleted": "tinyint(1)",
        "staff_ext_id": "varchar(64)",
        "updated_at": "timestamp"
    },
    "old": null,
    "pkNames": [
        "id"
    ],
    "sql": "",
    "sqlType": {
        "id": 4,
        "created_at": 93,
        "corp_id": -5,
        "department_id": 4,
        "department_ext_id": 12,
        "staff_id": 4,
        "order": -5,
        "is_leader_in_dept": -6,
        "deleted": -6,
        "staff_ext_id": 12,
        "updated_at": 93
    },
    "table": "staff_department_relation",
    "ts": 1648804155625,
    "type": "DELETE"
}
```





