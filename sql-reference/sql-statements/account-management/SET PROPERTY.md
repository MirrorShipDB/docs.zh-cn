# SET PROPERTY

## description

### Syntax

```SQL
SET PROPERTY [FOR 'user'] 'key' = 'value' [, 'key' = 'value']
```

设置用户的属性，包括分配给用户的资源、导入cluster等。这里设置的用户属性，是针对 user 的，而不是 user_identity。即假设通过 CREATE USER 语句创建了两个用户 'jack'@'%' 和 'jack'@'192.%'，则使用 SET PROPERTY 语句，只能针对 jack 这个用户，而不是 'jack'@'%' 或 'jack'@'192.%'

导入 cluster 仅适用于百度内部用户。

key:

超级用户权限:

```plain text
max_user_connections: 最大连接数。
resource.cpu_share: cpu资源分配。
load_cluster.{cluster_name}.priority: 为指定的cluster分配优先级，可以为 HIGH 或 NORMAL
```

普通用户权限：

```plain text
quota.normal: normal级别的资源分配。
quota.high: high级别的资源分配。
quota.low: low级别的资源分配。
```

load_cluster.{cluster_name}.hadoop_palo_path: palo使用的hadoop目录，需要存放etl程序及etl生成的中间数据供palo导入。导入完成后会自动清理中间数据，etl程序自动保留下次使用。

```plain text
load_cluster.{cluster_name}.hadoop_configs: hadoop的配置，其中fs.default.name、mapred.job.tracker、hadoop.job.ugi必须填写。
load_cluster.{cluster_name}.hadoop_http_port: hadoop hdfs name node http端口。其中 hdfs 默认为8070，afs 默认 8010。
default_load_cluster: 默认的导入cluster。
```

## example

1. 修改用户 jack 最大连接数为1000

    ```SQL
    SET PROPERTY FOR 'jack' 'max_user_connections' = '1000';
    ```

2. 修改用户 jack 的cpu_share为1000

    ```SQL
    SET PROPERTY FOR 'jack' 'resource.cpu_share' = '1000';
    ```

3. 修改 jack 用户的normal组的权重

    ```SQL
    SET PROPERTY FOR 'jack' 'quota.normal' = '400';
    ```

4. 为用户 jack 添加导入cluster

    ```SQL
    SET PROPERTY FOR 'jack'
    'load_cluster.{cluster_name}.hadoop_palo_path' = '/user/palo/palo_path',
    'load_cluster.{cluster_name}.hadoop_configs' = 'fs.default.name=hdfs://dpp.cluster.com:port;mapred.job.tracker=dpp.cluster.com:port;hadoop.job.ugi=user,password;mapred.job.queue.name=job_queue_name_in_hadoop;mapred.job.priority=HIGH;';
    ```

5. 删除用户 jack 下的导入cluster。

    ```SQL
    SET PROPERTY FOR 'jack' 'load_cluster.{cluster_name}' = '';
    ```

6. 修改用户 jack 默认的导入cluster

    ```SQL
    SET PROPERTY FOR 'jack' 'default_load_cluster' = '{cluster_name}';
    ```

7. 修改用户 jack 的集群优先级为 HIGH

    ```SQL
    SET PROPERTY FOR 'jack' 'load_cluster.{cluster_name}.priority' = 'HIGH';
    ```

## keyword

SET, PROPERTY
