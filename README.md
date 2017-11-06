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

编译参数
cmake . -DCMAKE_INSTALL_PREFIX=/usr/local/mysql \ 
-DMYSQL_DATADIR=/var/lib/mysql \
-DSYSCONFDIR=/etc \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DWITH_PERFSCHEMA_STORAGE_ENGINE=1 \
-DWITHOUT_EXAMPLE_STORAGE_ENGINE=1 \
-DWITHOUT_FEDERATED_STORAGE_ENGINE=1 \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_EXTRA_CHARSETS=all \
-DENABLED_LOCAL_INFILE=1 \
-DWITH_READLINE=1 \
-DMYSQL_UNIX_ADDR=/usr/local/mysql/mysql.sock \
-DMYSQL_TCP_PORT=3306 \
-DCOMPILATION_COMMENT="lq-edition" \
-DENABLE_DTRACE=1 \
-DWITH_SSL=yes \
-DWITH_DEBUG=1

php
http://blog.csdn.net/u010861514/article/details/51926575
http://blog.csdn.net/zero_295813128/article/details/51223209

------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------
一、编译安装MySQL前的准备工作
安装编译源码所需的工具和库
yum install gcc gcc-c++ ncurses-devel perl

安装cmake，从http://www.cmake.org下载源码并编译安装
wget http://www.cmake.org/files/v2.8/cmake-2.8.10.2.tar.gz   
tar -xzvf cmake-2.8.10.2.tar.gz   
cd cmake-2.8.10.2   
./bootstrap ; make ; make install   
cd ~
 
二、设置MySQL用户和组
新增mysql用户组
groupadd mysql
新增mysql用户
useradd -r -g mysql mysql 

三、新建MySQL所需要的目录

新建mysql安装目录
mkdir -p /usr/local/mysql  
新建mysql数据库数据文件目录
mkdir -p /data/mysqldb 

四、下载MySQL源码包并解压
从http://dev.mysql.com/downloads/mysql/直接下载源码，解压mysql-5.6.16.tar.gz
wget http://dev.mysql.com/downloads/mysql/mysql-5.6.16.tar.gz  
tar -zxv -f mysql-5.6.16.tar.gz  
cd mysql-5.6.16 

五、编译安装MySQL

从mysql5.5起，mysql源码安装开始使用cmake了，设置源码编译配置脚本。
设置编译参数
cmake \   
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \   
-DMYSQL_UNIX_ADDR=/usr/local/mysql/mysql.sock \   
-DDEFAULT_CHARSET=utf8 \   
-DDEFAULT_COLLATION=utf8_general_ci \   
-DWITH_INNOBASE_STORAGE_ENGINE=1 \   
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \   
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \   
-DMYSQL_DATADIR=/data/mysqldb \   
-DMYSQL_TCP_PORT=3306 \   
-DENABLE_DOWNLOADS=1  

注：重新运行配置，需要删除CMakeCache.txt文件
rm CMakeCache.txt  

make  
make install

六、修改mysql目录所有者和组

修改mysql安装目录
cd /usr/local/mysql   
chown -R mysql:mysql . 

修改mysql数据库文件目录
cd /data/mysqldb  
chown -R mysql:mysql .

七、初始化mysql数据库
cd /usr/local/mysql   
scripts/mysql_install_db --user=mysql --datadir=/data/mysqldb 

八、复制mysql服务启动配置文件
cp /usr/local/mysql/support-files/my-default.cnf /etc/my.cnf
注：如果/etc/my.cnf文件存在，则覆盖。

九、复制mysql服务启动脚本及加入PATH路径
cp support-files/mysql.server /etc/init.d/mysqld   
  
vim /etc/profile   
  
      PATH=/usr/local/mysql/bin:/usr/local/mysql/lib:$PATH  
  
      export PATH  
  
source /etc/profile  

十、启动mysql服务并加入开机自启动(可选这个步骤，以后可以自己启动的)

service mysqld start 

十一、检查mysql服务是否启动
netstat -tulnp | grep 3306   
mysql -u root -p 
密码为空，如果能登陆上，则安装成功。

十二、修改MySQL用户root的密码
mysqladmin -u root password '123456' 
注：也可运行安全设置脚本，修改MySQL用户root的密码，同时可禁止root远程连接，移除test数据库和匿名用户
/usr/local/mysql/bin/mysql_secure_installation 


十三、可能会出现的错误
问题：   
Starting MySQL..The server quit without updating PID file ([FAILED]/mysql/Server03.mylinux.com.pid).   
解决：   
修改/etc/my.cnf 中datadir,指向正确的mysql数据库文件目录

问题：   
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)   
解决：   
新建一个链接或在mysql中加入-S参数，直接指出mysql.sock位置。   
ln -s /usr/local/mysql/data/mysql.sock /tmp/mysql.sock   
  
/usr/local/mysql/bin/mysql -u root -S /usr/local/mysql/data/mysql.sock

MySQL问题解决：-bash:mysql:command not found  
因为mysql命令的路径在/usr/local/mysql/bin下面,所以你直接使用mysql命令时,  
系统在/usr/bin下面查此命令,所以找不到了   
   解决办法是：  
 ln -s /usr/local/mysql/bin/mysql /usr/bin　做个链接即可



