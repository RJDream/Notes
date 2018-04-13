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