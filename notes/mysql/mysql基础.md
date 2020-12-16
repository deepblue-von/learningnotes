### mysql

| mysql基本使用                                 |                  |
| --------------------------------------------- | ---------------- |
| net start mysql                               | 启动数据库       |
| mysql -u root -p root                         | 登录数据库       |
| show databases;                               | 显示数据库列表   |
| use database;                                 | 进入数据库       |
| show tables;                                  | 显示数据库中的表 |
| dest + 表名                                   | 查询表结构       |
| show create table 表名                        | 查看字符集       |
| drop table if exists 表名                     | 删除表名         |
| alert table 表名 rename to 新表名             | 表重命名         |
| alert table stu gender varchar(10)            | 添加列           |
| alert table stu change gender sex varchar(20) | 修改列名         |
|                                               |                  |

| sql语句-dml                                       |            |
| ------------------------------------------------- | ---------- |
| insert into 表名 (列名1，列名n）values (值1，值n) | 添加数据   |
| insert into 表名 values（值1，值n）               |            |
| select * from stu                                 | 查询数据   |
| delete from 表名 [where 条件]                     | 删除数据   |
| SELECT * FROM 表名 WHERE name LIKE G%;            | 模糊查询   |
| SELECT * FROM  表名 WHERE name IN ("sy", "ym");   | 查询多个值 |
|                                                   |            |
|                                                   |            |
|                                                   |            |

```mysql
//创建表

create table stu(
	id int,
	name varchar(32),
	age int,
	score double(4,1),
	birthday date,
	insert_time TIMESTAMP
);

// 删除表
 drop table if exists 表名
 
 //复制表
 create table student like stu;
```

