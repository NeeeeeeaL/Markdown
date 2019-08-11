## MySQL Learning Note 

查看数据库：` show databases; `

选中数据库：` use + name `

查询选中数据库中内容：` select * from + name(选中的数据库中包含的内容的名字) `

` select * from student; `


查询指定内容：` select * from student where 学号 = 1; `

在数据库服务器中创建数据库：` create database + name `

创建数据表：
```
create table pet(
    name VARCHAR(20),
    owner VARCHAR(20),
    species VARCHAR(20),
    sex CHAR(1),
    birth DATE,
    death DATE
);
```

查看数据表是否创建成功：` show tables `

查看创建好的数据表结构：` describe + name `

往数据表中添加数据记录：
```
insert into pet
values ('Puffball', 'Diane', 'hamster', 'f', '1993-03-30', NULL);
```

