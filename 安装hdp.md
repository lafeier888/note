网络配置

​	开机启动，静态ip，网关，dns，子网掩码

重启网卡

 ping 网络检查

关闭防火墙开机启动

hosts

主机名

时间同步服务 

​	yum install ntp

修改yum源

安装jdk

关闭selinux

关闭THP



克隆机器  

1. 要复制的机器删除rule那个文件

2. 复制后的机器网卡配置删除HWADADR

3. 复制后的机器删除rule文件

   ​

yum安装mysql（看需要）

yum list mysql-server

yum install mysql-server

service mysqld start

/usr/bin/mysqladmin -u root password '123456'

chkconfig mysqld on



vi /etc/my.cnf

[client]

default-character-set=utf8

[mysqld]

default-stroge-engine=INNODB

collation_server=utf8_general_ci

character_set_server=utf8



service mysqld restart



创建ambari，hive，oozie库

安装mysql驱动包

​	yum install mysql-connector-java

​	ls /usr/share/java #查看jar包是否下载下来了



本地yum源

yum install yum-utils -y

yum repolist

yum install createrepo -y

yum install httpd -y



/var/www/html下创建hdp和ambari目录

cd /var/www/htmI

```
mkdir /var/www/html/ambari[root@yum~]# mkdir /var/www/html/hdp
mkdir /var/www/html/hdp/HDP-UTlLS-11.0.21
tar -zxvf ambari-2.4. 1.0-centos6.tar.g -C /var/www/html/ambari/
tar -zxvf HDP-2.5.o.o-centos6-rpm.tag -C /var/www/html/hdp/
tar -zxvf HDP-UTlLS-11.o.21-centos6 -C /var/www/htmI/hdp/HDP-UTlLS-.1.0.21
```

service httpd start

chkconfig httpd on

```
wget O /etc/yum.repos.d/ambari.repo http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.4.1.0/ambari.repo
```

