# mysql
mysql安装
mysql源码地址：
https://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.38.tar.gz

yum install cmake
groupadd mysql 
useradd -g mysql mysql
vi /etc/security/limits.conf
mysql     soft          nproc            2047
mysql     hard          nproc            16384
mysql     soft          nofile          1024
mysql     hard         nofile           65536
保存退出wq
tar xzvf mysql-5.6.38.tar.gz
cd mysql-5.6.38
