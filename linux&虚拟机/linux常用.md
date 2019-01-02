# rpm,deb,tar.gz的区别?

rpm:	redhat系列专属,可以使用rpm工具管理

deb:	debian系列专属,使用dpkg工具管理

tar.gz:	所有的linux发行版都可以使用,但是需要编译麻烦



/  #进入查找模式

n  #下一个

N  #上一个



```
f_v_bxdr_city_grid_imsi_d_render.sql
```

# 源码编译安装



 yum install install autoconf automake libtool





sudo yum install -y cmake gcc gcc-c++ doxygen



sudo yum install epel-release

sudo yum install python34



```
yum install man-pages-zh-CN
```

# 命令

cd -   回到上次的目录

awk

head

xagrs

free -m

du -sh *

df -h

top

# 磁盘



/dev/sd[a-]  实体机

/dev/vd[a-p]  虚拟机

# 用户以及权限

groupadd 组名

useradd 用户名 -g 组名 -p 密码

chown -R 用户名：组名 文件夹

groups  查看组内其他成员

groups username  查看用户所在的组以及组内成员



su username  切换用户





_  xxx xxx xxx  

_  文件或目录

第一个xxx   用户权限

第二个xxx   组权限

第三个xxx   其他人权限



# 交互式shell  VS  非交互式shell

交互式shell:  打开一个shell后,输入各种命令,返回结果,这就是交互式shell

非交互式shell:执行脚本,脚本的运行环境就是非交互式shell

# 登录shell和非登录shell

 图形界面下  直接创建一个shell  这就是非登录shell