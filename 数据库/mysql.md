# 导出数据

SELECT * from user    where date='2018-08-30' into OUTFILE 'E:\\1.csv' fields terminated by ',' ;

# 创建临时表

create TEMPORARY table  newTableName as select * from log where id=1;

select @@basedir

select @@datadir