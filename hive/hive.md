

![hive-hadoop兼容性](D:\note\assets\hive-hadoop兼容性.png)

udaf(行转列)

collect_set  可以将某列的值转为 一个数组,作为一个数组值

|  id  | value |
| :--: | :---: |
|  1   | cxs1  |
|  2   | cxs2  |
|  3   | cxs3  |



set hive.exec.mode.local.auto=true;



udtf(列转行)

可以将某列的值,分为多行





