# 设置免密登陆

```
ssh-keygen
```

# 创建ambari账户

# 开始时钟同步服务

```
yum install -y ntp
systemctl enable ntpd
```

# 网络

## 编辑hosts

vi /etc/hosts

## 设置主机名

vi /etc/sysconfig/network

## ip配置

vi /etc/sysconfig/network-scrips/ifcfg-ens33

# 配置防火墙

```
systemctl disable firewalld
service firewalld stop
```

# 关闭SELinux

# 下载数据库连接驱动

# 

# 安装mysql

```shell
rpm -qa | grep mysql #查看是否已经安装mysql	
yum list installed | grep mysql #查看是否已经安装mysql
yum -y remove mysql-libs.x86_64 #卸载已安装的mysql
```

```
wget http://repo.mysql.com/mysql57-community-release-el7-8.noarch.rpm #下载mysql安装包
rpm -ivh mysql57-community-release-el7-8.noarch.rpm #安装
yum install mysql-server
```

```shell
service mysqld start #启动mysql服务
```

```shell
#查看安装后默认密码
/var/log/mysqld.log 这个文件保存着安装之后的临时密码

grep 'password' /var/log/mysqld.log |head -n 1 #查看yum安装后的临时密码

	内容:A temporary password is generated for root@localhost: **H+JdAAnx/2ie** 加粗的就是密码
```

```shell
#更改密码复杂度设置
set global validate_password_policy=0;
set global validate_password_length=1;
```

```shell
#修改密码

alter user 'root'@'localhost' identified by '123456';   
```

vi /etc/my.cnf

```
[client]

default-character-set=utf8

[mysqld]

default-stroge-engine=INNODB

collation_server=utf8_general_ci

character_set_server=utf8
```

添加开机启动

```
systemctl restart mysqld.service #重启服务

systemctl enable mysqld.service #开启启动
```

创建ambari，hive，oozie库

```
 create database ambari character set utf8 ;  

 create database hive character set utf8 ; 

 create database oozie character set utf8 ;   
```

开启远程访问

```
use mysql;
update user set host = '%' where user = 'root';
grant all privileges  on *.* to root@'%' identified by "123456";
#密码复杂度修改
set global validate_password_policy=0;
set global validate_password_length=4;
FLUSH PRIVILEGES;
```



# 创建本地yum源

```
yum install yum-utils createrepo
```

创建http服务器

	开启
	
	保证网络可以访问

创建镜像web目录

```
mkdir -p /var/www/html/
```

下载资源

```
ambari
hdp
hdp-utils
hdp-gpl
```

下载ambari.repo 编辑







yum install ambari-server

创建ambari数据库

执行ddl语句

# 解决confirm hosts阶段ssl问题

vi /etc/ambari-agent/conf/ambari-agent.ini

	在[security节中添加force_https_protocol=PROTOCOL_TLSv1_2

vi /etc/python/cert-verification.cfg 

	在https节中设置verify=disable

# 安装mysql问题(组件安装失败)

rpm -Uvh [http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm](http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm%EF%BC%8C%E6%9C%80%E5%90%8E%E4%BD%BF%E7%94%A8%E5%83%8F%E5%AE%89%E8%A3%85MySQL%E7%9A%84%E5%B8%B8%E8%A7%84%E6%96%B9%E6%B3%95%E4%B8%80%E6%A0%B7%E5%AE%89%E8%A3%85mysql)