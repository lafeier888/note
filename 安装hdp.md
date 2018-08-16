

# 查看系统版本

cat /etc/redhat-release

# 网络配置

```shell
vi /etc/sysconfig/network-scripts/ifcfg-ens33
```

```shell
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=af051b79-6169-4bca-89e8-38ab0f6157f6
DEVICE=ens33
ONBOOT=yes
IPADDR=192.168.140.150
NETMASK=255.255.255.0
GATEWAY=192.168.140.2
DNS1=114.114.114.114
DNS2=8.8.8.8
```

```shell
systemctl restart network #重启网络服务
```

```shell
reboot #重启

ping www.baidu.com #网络检测
```
# 安装ifconfig命令工具
yum install net-tools 
# 安装vim
yum install -y vim
# 安装lrzsz
yum install lrzsz	
# 安装ntp服务
yum install ntp

 vi /etc/ntp.conf 

server配置

```
restrict 192.168.0.0 mask 255.255.255.0   #添加此行
```

client配置（同步server）

```
server 192.168.0.104     #添加此行

# server [0.centos.pool.ntp.org](http://0.centos.pool.ntp.org/) iburst           #以下四行注释掉

# server [1.centos.pool.ntp.org](http://1.centos.pool.ntp.org/) iburst

# server [2.centos.pool.ntp.org](http://2.centos.pool.ntp.org/) iburst

# server [3.centos.pool.ntp.org](http://3.centos.pool.ntp.org/) iburst
```

systemctl start ntpd.service #启动

systemctl enable ntpd.service #开机启动

ntpstat #查看ntpd服务状态

systemctl status ntpd.service #查看服务状态

# 安装wget

yum install -y wget

# 防火墙
```shell
systemctl status firewalld.service #查看防火墙状态
systemctl stop firewalld #停止防火墙
systemctl start firewalld #开启防火墙
systemctl disable firewalld #关闭防火墙开机自启
systemctl enable firewalld #关闭防火墙开机自启
chkconfig --list|grep network #查看防火墙开机自启状态

```



# 关闭SELinux

vim /etc/selinux/config

```
SELINUX=disabled
```

关闭THP ==>暂时没弄

# 修改hosts

 vi /etc/hosts

# 修改主机名

 vi /etc/hostname 

# 修改yum镜像源

```
cd /etc/yum.repos.d
mv CentOS-Base.repo CentOS-Base.repo.bak	
wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
yum clean all
yum makecache
yum -y update
yum repolist
```

# 安装jdk

tar -zxvf jdk-8u181-linux-x64.tar.gz -C  /usr/local

vi /etc/profile

```
JAVA_HOME=/usr/local/jdk1.8.0_181
JRE_HOME=$JAVA_HOME/jre
PATH=$PATH:$JAVA_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME
export JRE_HOME
export PATH
export CLASSPATH
```

source /etc/profile #更新环境变量

# 克隆机器  

1. 要复制的机器删除rule那个文件

2. 复制后的机器网卡配置删除HWADADR

3. 复制后的机器删除rule文件

   ​

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
grant all privileges  on *.* to root@'%' identified by "@Aa123456";
#密码复杂度修改
set global validate_password_policy=0;
set global validate_password_length=4;
```



# 安装mysql驱动包

​	yum install mysql-connector-java

​	ls /usr/share/java #查看jar包是否下载下来了

# hdp本地yum源

```
yum install yum-utils -y

yum repolist

yum install createrepo -y

yum install httpd -y
```



```
#/var/www/html下创建hdp和ambari目录
cd /var/www/htmI
mkdir /var/www/html/ambari
mkdir /var/www/html/hdp
mkdir /var/www/html/hdp/HDP-UTlLS-11.0.21

```
```
wget http://public-repo-1.hortonworks.com/ambari/centos7/2.x/updates/2.6.1.5/ambari-2.6.1.5-centos7.tar.gz
wget http://public-repo-1.hortonworks.com/HDP/centos7/2.x/updates/2.6.4.0/HDP-2.6.4.0-centos7-rpm.tar.gz
wget http://public-repo-1.hortonworks.com/HDP-UTILS-1.1.0.22/repos/centos7/HDP-UTILS-1.1.0.22-centos7.tar.gz
```

```
tar -zxvf ambari-2.6.1.5-centos7.tar.gz -C /var/www/html/
tar -zxvf HDP-2.6.4.0-centos7-rpm.tar.gz -C /var/www/html/
tar -zxvf HDP-UTlLS-11.o.21-centos6 -C /var/www/htmI/hdp/HDP-UTlLS-.1.0.21
```

```
systemctl start httpd.service #启动http服务,访问测试(关闭防火墙)
systemctl enable httpd.service
```

```
yum install -y wget
```
```
wget O /etc/yum.repos.d/ambari.repo http://public-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.4.1.0/ambari.repo
```

# 查看磁盘余量

df -hl

# 安装ambari

安装ambari-server

```
yum install ambari-server 
```

配置ambari-server

```
ambari-server setup 
```

启动

```
ambari-server start 
```

访问

```
http://localhost:8080 默认admin/admina:accept:
```

:jack_o_lantern:

如果mysql配置那里出问题了,可以找到var/lib/ambari/resource/Ambari-DDL-MySQL-CREATE.sql文件,自己去mysql中选择ambari库执行这个sql脚本

