# MySql 

## 1.常用语句

### 设置用户

**create user **	

```sql
use mysql;
insert into user(host,user,password,select_priv,insert_priv,update_priv,)
values('localhost','test_user',PASSWORD('123456'),'Y','Y','Y');
flush privileges; -- 重新加载授权表
```

**为用户开通特定数据库的权限**

```sql
grant select,insert,update,delete,create,drop on dbname.* to 'username'@'localhost' identified by 'password';
```



```sql
grant all on db_name.* to 'username'@'client_host';
```



权限:

| Privileges             | Type          | Null | key  | default | extra |
| ---------------------- | ------------- | ---- | ---- | ------- | ----- |
| Select_priv            | enum('N','Y') | NO   |      | N       |       |
| Insert_priv            | enum('N','Y') | NO   |      | N       |       |
| Update_priv            | enum('N','Y') | NO   |      | N       |       |
| Delete_priv            | enum('N','Y') | NO   |      | N       |       |
| Create_priv            | enum('N','Y') | NO   |      | N       |       |
| Drop_priv              | enum('N','Y') | NO   |      | N       |       |
| Reload_priv            | enum('N','Y') | NO   |      | N       |       |
| Shutdown_priv          | enum('N','Y') | NO   |      | N       |       |
| Process_priv           | enum('N','Y') | NO   |      | N       |       |
| File_priv              | enum('N','Y') | NO   |      | N       |       |
| Grant_priv             | enum('N','Y') | NO   |      | N       |       |
| References_priv        | enum('N','Y') | NO   |      | N       |       |
| Index_priv             | enum('N','Y') | NO   |      | N       |       |
| Alter_priv             | enum('N','Y') | NO   |      | N       |       |
| Show_db_priv           | enum('N','Y') | NO   |      | N       |       |
| Super_priv             | enum('N','Y') | NO   |      | N       |       |
| Create_tmp_table_priv  | enum('N','Y') | NO   |      | N       |       |
| Lock_tables_priv       | enum('N','Y') | NO   |      | N       |       |
| Execute_priv           | enum('N','Y') | NO   |      | N       |       |
| Repl_slave_priv        | enum('N','Y') | NO   |      | N       |       |
| Repl_client_priv       | enum('N','Y') | NO   |      | N       |       |
| Create_view_priv       | enum('N','Y') | NO   |      | N       |       |
| Show_view_priv         | enum('N','Y') | NO   |      | N       |       |
| Create_routine_priv    | enum('N','Y') | NO   |      | N       |       |
| Alter_routine_priv     | enum('N','Y') | NO   |      | N       |       |
| Create_user_priv       | enum('N','Y') | NO   |      | N       |       |
| Event_priv             | enum('N','Y') | NO   |      | N       |       |
| Trigger_priv           | enum('N','Y') | NO   |      | N       |       |
| Create_tablespace_priv | enum('N','Y') | NO   |      | N       |       |





### Show

show databases;

show tables;

show columns from tb_name;//显示表结构

describe tb_name;//显示表结构









### 使用文件导入数据

```sql
load data local infile 'path/tb.txt' into table tb_name;
```

tb.txt 中每一行表示一条数据，每个字段的值按照表声明的顺序排列，使用tab分割多个值，使用\N插入Null值。

如果在Windows的编辑器中，使用\r\n作为行分界线，那么在sql中应该指定行分界符：

```sql
load data local infile 'path/tb.txt' into table tb_name lines terminated by '\r\n';
```

你可以在load data语句中明确指定值分割符和行分隔符，默认的值分隔符是tab，行分隔符是换行符\n



如果执行失败，那么load data local 可能需要进行安全设置。see<mysql manaul 6.16>





### 日期

####curdate/now

```sql
mysql> select curdate();
+------------+
| curdate()  |
+------------+
| 2018-04-11 |
+------------+
1 row in set (0.00 sec)

mysql> select now();
+---------------------+
| now()               |
+---------------------+
| 2018-04-11 14:06:40 |
+---------------------+
1 row in set (0.00 sec)


```



####timestampdiff

```sql
mysql> select timestampdiff(year,'2007-01-01',curdate()) as years;
+-------+
| years |	
+-------+
|    11 |
+-------+
1 row in set (0.00 sec)

mysql> select timestampdiff(month,'2007-01-01',curdate()) as years;
+-------+
| years |
+-------+
|   135 |
+-------+
1 row in set (0.00 sec)

mysql> select timestampdiff(day,'2007-01-01',curdate()) as years;
+-------+
| years |
+-------+
|  4118 |
+-------+
1 row in set (0.00 sec)
```



#### extract parts of dates

```sql

mysql> select year('2017-05-31') as year;
+------+
| year |
+------+
| 2017 |
+------+
1 row in set (0.00 sec)

mysql> select month('2017-05-31') as month;
+-------+
| month |
+-------+
|     5 |
+-------+
1 row in set (0.00 sec)

mysql> select dayofmonth('2017-05-31') as dayofmonth;
+------------+
| dayofmonth |
+------------+
|         31 |
+------------+
1 row in set (0.00 sec)
```





#### date_add

```sql
mysql> select date_add(curdate(),interval 1 month) as nextmonth;
+------------+
| nextmonth  |
+------------+
| 2018-05-11 |
+------------+
1 row in set (0.00 sec)

mysql> select date_add(curdate(),interval 1 year) as nextyear;
+------------+
| nextyear   |
+------------+
| 2019-04-11 |
+------------+
1 row in set (0.00 sec)

mysql> select date_add(curdate(),interval 1 day) as tomorrow;
+------------+
| tomorrow   |
+------------+
| 2018-04-12 |
+------------+
1 row in set (0.00 sec)
```





#### Pattern Matching

**To find name containing a w**

```sql
mysql> select * from pet where name like '%w%';
+----+----------+-------+---------+------+------------+------------+
| id | name     | owner | species | sex  | birth      | death      |
+----+----------+-------+---------+------+------------+------------+
|  2 | Clasws   | Gwen  | cat     | m    | 1994-03-17 | NULL       |
|  5 | Bowser   | Diane | dog     | m    | 1979-08-31 | 1995-07-29 |
|  7 | Whistler | Gwen  | bird    | NULL | 1997-12-09 | NULL       |
+----+----------+-------+---------+------+------------+------------+
3 rows in set (0.03 sec)
```

**To find names containing exactly five chars**

```sql
mysql> select * from pet where name like '_____';
+----+-------+--------+---------+------+------------+-------+
| id | name  | owner  | species | sex  | birth      | death |
+----+-------+--------+---------+------+------------+-------+
|  3 | Buffy | Harold | dog     | f    | 1989-05-13 | NULL  |
+----+-------+--------+---------+------+------------+-------+
1 row in set (0.00 sec)
```



MySQL 支持扩展正则表达式

##### Regexp/Not Regexp  == RLike /Not RLike

| regexp | means                           |
| ------ | ------------------------------- |
| .      | 匹配任意单个字符                        |
| [...]  | 匹配括号内的任意一个字符                    |
| *      | 匹配0-n个前面指定的字符， eg:[0-9]* 匹配任意字符 |
| ^      | 以给定的字符开始^ab                     |
| $      | 以给定的字符结尾st$                     |

```sql
mysql> select * from pet where name regexp '^b';
+----+--------+--------+---------+------+------------+------------+
| id | name   | owner  | species | sex  | birth      | death      |
+----+--------+--------+---------+------+------------+------------+
|  3 | Buffy  | Harold | dog     | f    | 1989-05-13 | NULL       |
|  5 | Bowser | Diane  | dog     | m    | 1979-08-31 | 1995-07-29 |
+----+--------+--------+---------+------+------------+------------+
2 rows in set (0.01 sec)

mysql> select * from pet where name regexp 'w';
+----+----------+-------+---------+------+------------+------------+
| id | name     | owner | species | sex  | birth      | death      |
+----+----------+-------+---------+------+------------+------------+
|  2 | Clasws   | Gwen  | cat     | m    | 1994-03-17 | NULL       |
|  5 | Bowser   | Diane | dog     | m    | 1979-08-31 | 1995-07-29 |
|  7 | Whistler | Gwen  | bird    | NULL | 1997-12-09 | NULL       |
+----+----------+-------+---------+------+------------+------------+
3 rows in set (0.00 sec)

mysql> select * from pet where name regexp 'fy$';
+----+-------+--------+---------+------+------------+-------+
| id | name  | owner  | species | sex  | birth      | death |
+----+-------+--------+---------+------+------------+-------+
|  3 | Buffy | Harold | dog     | f    | 1989-05-13 | NULL  |
+----+-------+--------+---------+------+------------+-------+
1 row in set (0.00 sec)
```



### Combine tables

#### inner join

inner join 只会将在两张表中都符合on子句条件的数据加载出来。

```SQL
select * from tb_a a inner join tb_b b on a.name = b.name where a.name = 'lisi';
```

该语句只会将两张表中name 是lisi的数据加载出来，如果只有一张表中存在lisi，那么该数据就不会出现在结果集











# Storage Engines

> Storage engines are MySQL components that handle the SQL operations for different table types. [`InnoDB`](https://dev.mysql.com/doc/refman/5.7/en/innodb-storage-engine.html) is the default and most general-purpose storage engine, and Oracle recommends using it for tables except for specialized use cases.  



存储引擎是MySQL的底层组件，用于在不同的表类型中进行sql语句处理（crud）. `InnoDB`是默认的存储引擎，且最为通用，Oracle推荐使用`InnoDB`除非有特殊的应用场景。

==Note==

在Mysql 5.7 中 `CREATE TABLE` 语句默认使用`InnoDB`作为存储引擎。



MySql 使用可插拔的存储引擎结构，可以在运行时加载或卸载存储引擎

==show engines==

![1545622058729](mysql.assets\1545622058729.png)





## mysql 支持的存储引擎

- InnoDB : 

  MySql 5.7 的默认存储引擎，它是事物安全型的（遵从ACID），它提供了`提交，回滚，故障恢复`的能力，用于保护用户的数据。InnoDB行级锁和一致性非锁定读提高了多用户并发和性能。

- MyISAM

  

- Memory

- CSV

- Archive

- Blackhole

- NDB

- Merge

- Federated

- Example





## InnoDB

最常用的存储引擎，他平衡了高可靠性和高性能。 是MySql 5.7的默认存储引擎。除非配置了不同的默认存储引擎，在使用`CREATE TABLE`语句创建表时，不明确指定`ENGINE=`语句，默认创建一个InnoDB 表。



### InnoDB的关键优势

- DML操作遵从**ACID模型**，具有事务特性的提交、回滚和故障恢复的能力保障用户的数据。
- **行级锁**和**一致性读**提高多用户并发以及性能。
- InnoDB 会在磁盘上对数据进行排列，用于优化基于主键的查询。每一个InnoDB表都有一个聚簇索引的主键索引，该索引会组织数据以减少基于主键查询的I/O操作。
- InnoDB支持`Foreign Key`约束以维持数据的完整性。使用外键时，`insert`,`update`,`delete`操作将会检查数据以确保它们不会导致在不同的表之间的数据不一致。

| Feature                                                      | Support                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **B-tree indexes**                                           | Yes                                                          |
| **Backup/point-in-time recovery**(Implemented in the server, rather than in the storage engine.) | Yes                                                          |
| **Cluster database support**                                 | No                                                           |
| **Clustered indexes**                                        | Yes                                                          |
| **Compressed data**                                          | Yes                                                          |
| **Data caches**                                              | Yes                                                          |
| **Encrypted data**                                           | Yes (Implemented in the server via encryption functions; In MySQL 5.7 and later, data-at-rest tablespace encryption is supported.) |
| **Foreign key support**                                      | Yes                                                          |
| **Full-text search indexes**                                 | Yes (InnoDB support for FULLTEXT indexes is available in MySQL 5.6 and later.) |
| **Geospatial data type support**                             | Yes                                                          |
| **Geospatial indexing support**                              | Yes (InnoDB support for geospatial indexing is available in MySQL 5.7 and later.) |
| **Hash indexes**                                             | No (InnoDB utilizes hash indexes internally for its Adaptive Hash Index feature.) |
| **Index caches**                                             | Yes                                                          |
| **Locking granularity**                                      | Row                                                          |
| **MVCC**                                                     | Yes                                                          |
| **Replication support** (Implemented in the server, rather than in the storage engine.) | Yes                                                          |
| **Storage limits**                                           | 64TB                                                         |
| **T-tree indexes**                                           | No                                                           |
| **Transactions**                                             | Yes                                                          |
| **Update statistics for data dictionary**                    | Yes                                                          |



### 使用InnoDB的好处

- 如果DB服务器因为硬件或软件的原因导致故障，在重启数据库后不需要做任何特殊的操作，InnoDB的`Crash Recovery` 会自动完成在故障发生前提交的任何修改，并取消在正在进行但未提交的修改。你只需要重启数据库并从之前的地方重新开始。
- InnoDB 存储引擎维持着一个自己的缓冲池，当访问数据时它在主存中缓存表和索引数据。频繁使用的数据直接在内存中进行处理。这个缓存应用于多种类型的信息并提高数据的处理速度。在专用的数据库服务器，最高80%的物理内存被分配给缓存池。
- 如果将有关联的数据拆分到不同的表中，可以设置外键关联确保数据 **参照完整性**。更新或删除数据，在其他表中关联的数据也会自动更新或删除。如果在主表中没有相应数据的情况下，将数据插入到附表中，脏数据将自动被剔除。
- 如果数据在内存或磁盘中损坏，在使用该数据前**检查和**机制会发出警告。
- 如果在设计表的时候使用了适当的主键列，涉及到这些列的操作会自动优化。在`where`，`Order By` ,`Group By`子句和 `join`操作中引用主键列会非常快。
- `insert，updates，deletes` 通过**更改缓存**机制自动优化。InnoDB 不只是允许对同一个表进行并发的读写访问，同时还会缓存修改后的数据简化磁盘I/O操作。
- 性能优势不仅限于在大型表中长时间运行的的查询。当相同行的数据反复的被查询时，**自适应哈希索引** 会自动接管这些查询使其更快，仿佛这些数据来自哈希表中一样。
- 可以压缩表和相关的索引
- 可以创建和删除索引，对性能和可用性的影响很小。
- 