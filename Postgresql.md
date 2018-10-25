# Postgresql

# 与mysql对比:

## text字段限制

MySQL 的各种 text 字段有不同的限制, 要手动区分 small text, middle text, large text... Pg 没有这个限制, text 能支持各种大小.

## null判断

 SQL 标准, 做 null 判断不能用 = null, 只能用 is null

但 pg 可以设置 transform_null_equals 把 = null 翻译成 is null 避免踩坑

## emoji

不少人应该遇到过 MySQL 里需要 utf8mb4 才能显示 emoji 的坑, Pg 就没这个坑.



## OVER()

## 事务隔离级别

## 文档数据类型

json,xml,jsonb



# 模型

c/s架构

服务端运行服务程序postgres



# 交互式终端psql

可能需要环境变量

```
/usr/local/pgsql/bin/createdb mydb
```



创建数据库

```
createdb mydb
```

```
dropdb mydb
```

### 账号权限

PostgreSQL用户名与操作系统用户帐户是分开的。连接到数据库时，可以选择要连接的 PostgreSQL用户名; 如果不这样做，它将默认使用与当前操作系统帐户相同的名称。碰巧的是，总会有一个 PostgreSQL用户帐户与启动服务器的操作系统用户同名，并且该用户也总是有权创建数据库。您可以在`-U`任何地方指定选项，而不是以该用户身份登录，以选择要连接的 PostgreSQL用户名。



### 终端语法

```
psql mydb
```

```
SELECT version();
SELECT current_date;
```



\h  sql语法帮助

\q 退出

\?  终端帮助

### 

```
\i basics.sql 执行脚本
```

```
CREATE TABLE weather (
    city            varchar(80),
    temp_lo         int,           -- low temperature
    temp_hi         int,           -- high temperature
    prcp            real,          -- precipitation
    date            date
);
```

```
DROP TABLE tablename;
```

```
INSERT INTO weather VALUES ('San Francisco', 46, 50, 0.25, '1994-11-27');
```

```
INSERT INTO weather (city, temp_lo, temp_hi, prcp, date)
    VALUES ('San Francisco', 43, 57, 0.0, '1994-11-29');
```

```
COPY weather FROM '/home/user/weather.txt';
```

```
SELECT * FROM weather;
```

```
SELECT city, (temp_hi+temp_lo)/2 AS temp_avg, date FROM weather;
```

```
SELECT DISTINCT city
    FROM weather
    ORDER BY city;
```

```
SELECT *
    FROM weather, cities
    WHERE city = name;
```




```
UPDATE weather
    SET temp_hi = temp_hi - 2,  temp_lo = temp_lo - 2
    WHERE date > '1994-11-28';
```

```
DELETE FROM weather WHERE city = 'Hayward';
```

```
DELETE FROM tablename;
```



psql的连接查询  可以使用where代替on关键字

```
SELECT *
    FROM weather, cities
    WHERE city = name;
```

