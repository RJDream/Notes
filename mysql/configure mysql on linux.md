

# Configure MySQL on Linux



## 1.编码设置

```shell
[root@acrlnx002 etc]# vim /etc/my.cnf

------------------------------------------------------------------------------------------------

# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html

[mysqld]
character-set-server=utf8
init_connect='SET NAMES utf8'
collation-server=utf8_general_ci
```



## 2.Mysql关闭表名大小写敏感

```shell
[root@acrlnx002 etc]# vim /etc/my.cnf

------------------------------------------------------------------------------------------------

# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html

[mysqld]
lower_case_table_names=1
```



## 3.sql_mode

sql 错误信息

```log
ERROR 1055 (42000): Expression #7 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'postscan.verifyDelayLog.auditor' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
```

如果在MySQL的sql_mode变量中包含ONLY_FULL_GROUP_BY这个值，那么在group by的时候，在select 中的字段必须使用分组函数或在group by子句中。

```shell
#查询原始的sql_mode
mysql> select @@sql_mode;
+-------------------------------------------------------------------------------------------------------------------------------------------+
| @@sql_mode                                                                                                                                |
+-------------------------------------------------------------------------------------------------------------------------------------------+
| ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION |
+-------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)

#修改之

[root@acrlnx002 etc]# vim /etc/my.cnf

------------------------------------------------------------------------------------------------

# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html

[mysqld]
sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'
```

