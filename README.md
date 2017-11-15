Mysql安装

一：卸载旧版本
rpm -qa | grep mysql
rpm -e mysql #普通删除模式 
rpm -e --nodeps xxx（xxx为刚才的显示的列表） # 强力删除模式，如果使用上面命令删除时，提示有依赖的其它文件，则用该命令可以对其进行强力删除
rm /etc/my.cnf #删除/etc/my.cnf (可以不删，后面覆盖就行)


二：安装编译代码需要的包
yum -y install make gcc gcc-c++ cmake bison-devel ncurses ncurses-devel perl openssl openssl-devel 


三：创建mysql用户（但是不能使用mysql账号登陆系统，创建安装文件夹 
groupadd mysql 
useradd -s /sbin/nologin -g mysql mysql
新建mysql安装目录
mkdir -p /usr/local/mysql  
新建mysql数据库数据文件目录
mkdir -p /data/mysqldata/3306
 
四：下载MySQL 源码
cd /usr/local/src
搜狐镜像下载
wget -c http://mirrors.sohu.com/mysql/MySQL-5.6/mysql-5.6.36.tar.gz


