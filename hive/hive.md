# hive和hadoop的兼容性

![hive-hadoop兼容性](D:\note\assets\hive-hadoop兼容性.png)



# hive使用本地模式

set hive.exec.mode.local.auto=true;



# UDF

## 内置函数

数学函数

round/bround 舍入

floor/ceil	上下限

rand	随机

ln/log2/log10/log	对数

pow/power	幂

sqrt	开方

bin/hex/unhex/conv 进制转换

abs	绝对值

sin/asin/cos/acos/tan/atan 三角函数

degrees/radians 角度/弧度









## 内置udaf函数（聚合函数）

## 内置UDTF（表生成函数）





# Hive CLI



hive -e "sql"

hive -f xxx.sql



## Hive CLI 交互式模式(不加参数进入)

quit/exit   



set 输出所有可用的属性

set -v

set <key>=<value>

reset 重置所有属性为默认值



!<command>  执行终端命令

dfs <cmmand> 指定dfs命令

source FILE <filepath> 执行脚本



add file|archive|jar <path>

add files|archives|jars <paths*>



list file|archive|jar 

list files|archives|jars

