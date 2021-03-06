# 网络配置

**vi \etc\sysconfig\network-scripts\ifcfg-eth0**

vi \etc\sysconfig\network-scripts\ifcfg-ens33(centos7)

DEVICE=eth0
TYPE=Ethernet
UUID=4ae3740f-d406-4dc5-868c-0c1fb5128c47
**ONBOOT=yes**
NM_CONTROLLED=yes
**BOOTPROTO=static**
**IPADDR=192.168.140.130**
**NETMASK=255.255.255.0**
**GATEWAY=192.168.140.2**
**DNS1=114.114.114.114**
**DNS2=8.8.8.8**s

------

```shell
#centos6
service network restart  #重启网络配置
ifconfig #查看网络配置是否生效
ping www.baidu.com #测试网络是否连通
```

```shell
#centos7
systemctl restart network #重启网络
ip addr #查看ip地址
ping www.baidu.com #测试网络是否连通
```



# 配置ssh

ssh-keygen



# 关闭防火墙

```shell
#centos6
service iptables status	 #查看防火墙状态
service iptables stop	#临时关闭防火墙
chkconfig iptables off	#永久关闭防火墙
```

```shell
#centos7
#临时关闭
systemctl stop firewalld
#禁止开机启动
systemctl disable firewalld
```



# notepad++安装nppFTP插件

安装nppPluginManager  7.5需要手动安装

https://github.com/bruderstein/nppPluginManager/releases

# .swap文件解决

原因：修改冲突

解决：删除   .aa.swap文件即可

# 修改yum源

```shell
#备份
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

#下载阿里云镜像
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo 

yum makecache 

yum -y update 
```

# 安装lrzsz

yum install -y lrzsz

# 安装mysql

```shell
yum list installed | grep mysql #查看是否安装mysql
yum -y remove mysql-libs.x86_64 #卸载已安装的mysql
find / -name mysql #查看是否有安装过mysql(非yum方式)
```

# 

```
use mysql
update user set host = '%' where user = 'root';
FLUSH PRIVILEGES;
```



# 安装jdk

```shell
tar -zxvf jdk-8u181-linux-x64.tar.gz  -C  /usr/local  #解压
```

```shell
#修改环境变量
JAVA_HOME=/usr/local/jdk1.8.0_181
JRE_HOME=$JAVA_HOME/jre
PATH=$PATH:$JAVA_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME
export JRE_HOME
export PATH
export CLASSPATH
```

```shell 
source /etc/profile #更新环境变量
```



# 虚拟机复制

## 修改网卡

vi /etc/udev/rules.d/70-persistent-net.rules 

```
里边会有两个网卡,删除重复的那个,记住新网卡的mac地址,并将网卡名改为eth0
```

/etc/sysconfig/network-scripts/ifcfg-eth0 

```
修改mac地址为新的网卡的mac地址
修改IP地址
```

#重启(必须的,重启网卡服务没效果)



## 修改主机名

vi /etc/sysconfig/network 

hostname #查看主机名

重启

## 修改hosts

```
192.168.140.130 vm01
192.168.140.131 vm02
192.168.140.132 vm03
```

注意是先ip后name,hosts的修改是立即生效的

# 配置ssh免密登陆

vm01上

```
ssh-keygen -t rsa  #生成公钥和私钥

ssh-copy-id root@vm02 #将公钥拷贝到vm02
```

vm01只要将公钥拷贝到vm02就能实现免密登陆vm02

# 安装ntpd服务

```shell
yum install -y ntp  #安装ntpd服务
systemctl is-enabled ntpd #查看是否开机启动
systemctl start ntpd #开机启动
date #查看当前时间
```

# 配置定时任务/时间同步

```shell
yum install -y vixie-cron #安装crontab

crontab -l #查看用户的定时任务(顺便可以检测crontab是否安装成功)

chkconfig --list crond  #查看是否是开机启动

chkconfig crond on #设置为开机启动

service crond start #开启crontab服务
```

定时同步时间

``` shell
crontab -e #编辑用户的定时任务

*/10 * * * * /usr/sbin/ntpdate us.pool.ntp.org #每隔10分钟同步一次时间
```

# 关闭SELinux

```
/usr/sbin/sestatus -v  #查看SElinux状态
```

```
vi /etc/selinux/config
SELINUX=enforcing改为SELINUX=disabled
```



# 查看系统版本

yum install redhat-lsb -y 

lsb_release -a 





# 安装ifconfig命令工具

yum install -y net-tools

