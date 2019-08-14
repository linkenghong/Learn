# mysql_learning

1. 登陆

   ```
   mysql -u root -p
   ```

   -u 后面是用户名，-p即输入密码，敲下回车后就需要输入密码了。
   
1. 查看有哪些数据库

   ```
   show databases;
   ```

1. 创建数据库

   ```
   create database XXX
   ```

1. 删除数据库

   ```
   drop database XXX
   ```

1. 创建管理用户

   ```
   use mysql；
   INSERT INTO user 
   (host, user, password, select_priv, insert_priv, update_priv) 
   VALUES ('localhost', 'guest', PASSWORD('guest123'), 'Y', 'Y', 'Y');
   ```
   密码最好用MySQL提供的 PASSWORD() 函数进行加密，否则会明文显示。

1. 进入某数据库查看有哪些表

   ```
   use XXX;    --XXX为要进入的数据库名称
   show tables;
   ```

1. 创建及删除数据表

   ```
   create table XXX；
   drop table XXX；
   ```

1. 查看数据表结构

   ```
   desc 表名;
   ```

1. 查询数据

   ```
   SELECT column_name,column_name
   FROM table_name [WHERE Clause][LIMIT N]
   ```
   查询语句中你可以使用一个或者多个表，表之间使用逗号(,)分割，并使用WHERE语句来设定查询条件。
 
   SELECT 命令可以读取一条或者多条记录。

   你可以使用星号（*）来代替其他字段，SELECT语句会返回表的所有字段数据

   你可以使用 WHERE 语句来包含任何条件。
    你可以使用 LIMIT 属性来设定返回的记录数

1. 插入数据

   ```
   INSERT INTO table_name ( field1, field2,...fieldN )
   VALUES ( value1, value2,...valueN );
   ```

1. 修改更新数据

   ```
   UPDATE table_name SET field1=new-value1, field2=new-value2 [WHERE Clause]
   ```

1. 删除数据
   
   ```
   DELETE FROM table_name [WHERE Clause]
   ```
    如果没有指定 WHERE 子句，MySQL表中的所有记录将被删除。

1. 表里增加或删除一列

   ```
   alter table 表名 add column 列名 格式 not null;
   alter table 表名 add column 列名 格式 not null after 指定行;
   alter table 表名 add column 列名 格式 not null first;
   alter table 表名 change 原列名 新列名 char;
   alter table 表名 drop column 列名;
   ```
   第一行为直接在表最后加一列，第二行为在指定行后面加一列，第三行为在表最前面加一列；第四行为修改列名称；第五行为删除该列。

1. 排序

   ```
   ORDER BY field1, [field2...] [ASC [DESC]]
   ```
   以ORDER BY 后字段为排序条件，可按一个或多个字段排序。ASC 或 DESC 关键字来设置查询结果是按升序或降序排列。默认情况下，它是按升排列。

1. 相似（LIKE）

   可以在`where`语句中使用`like`，通常会与`%`一起使用表示匹配任意数量任意字符，如果只有`like`则与`=`作用一样。

1. 连接(INNER JOIN）

   ```
   INNER JOIN（内连接,或等值连接）：获取两个表中字段匹配关系的记录。
   SELECT a.x, b.y FROM table_1 a INNER JOIN table_2 b ON a.n = b.m;
   ```
   上诉语句的意思是返回满足条件的表table_1的x字段和table_2的y字段，条件是表table_1中n字段在表table_2的m字段中存在的。

   table_1、table_2后跟的a和b是指用a、b来代替这两个表，这样写起来就简便很多。

1. 分组

   GROUP BY 语句根据一个或多个列对结果集进行分组。
   在分组的列上我们可以使用 COUNT, SUM, AVG,等函数。

   ```
   SELECT column_name, function(column_name)
   FROM table_name
   WHERE column_name operator value
   GROUP BY column_name  [[ASC | DESC] [WITH ROLLUP]];
   ```
   以上语句意思为按column_name为分组，统计各分组function(columu_name)的值，function可以是求和或者平均数等。

   WITH ROLLUP则是对整体数据做一次function统计。

