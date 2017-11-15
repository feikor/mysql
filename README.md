Mysql安装（阿里云centos6.8下）

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
tar zxvf mysql-5.6.36.tar.gz
cd mysql-5.6.36

五：安装
cmake
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql               #安装目录
-DMYSQL_DATADIR=/data/mysqldata/3306                  #数据库存放目录
-DSYSCONFDIR=/etc                                     #系统配置目录
-DMYSQL_UNIX_ADDR=/usr/local/mysql/data/mysql.sock    #Unix中socket文件路径
-DWITH_MYISAM_STORAGE_ENGINE=1                        #安装 myisam 存储引擎
-DWITH_INNOBASE_STORAGE_ENGINE=1                      #安装 innodb 存储引擎
-DWITH_ARCHIVE_STORAGE_ENGINE=1                       #安装 archive 存储引擎
-DWITH_BLACKHOLE_STORAGE_ENGINE=1                     #安装 blackhole 存储引擎
-DWITH_MEMORY_STORAGE_ENGINE=1                        #安装 blackhole 存储引擎
-DWITH_PARTITION_STORAGE_ENGINE=1                     #安装数据库分区
-DENABLED_LOCAL_INFILE=1                              #允许从本地导入数据
-DWITH_READLINE=1                                     #快捷键功能readline库支持（提供可编辑的命令行）
-DWITH_SSL=system                                     #启用ssl库支持（安全套接层）
-DWITH_ZLIB=system                                    #启用libz库支持（zib、gzib相关
-DDEFAULT_CHARSET=utf8                                #使用 utf8 字符
-DDEFAULT_COLLATION=utf8_general_ci                   #校验字符
-DEXTRA_CHARSETS=all                                  #安装所有扩展字符集
-DMYSQL_TCP_PORT=3306                                 #MySQL 监听端口

--------------------------------------------------------------------
cmake
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql
-DMYSQL_DATADIR=/data/mysqldata/3306 
-DSYSCONFDIR=/etc 
-DMYSQL_UNIX_ADDR=/usr/local/mysql/data/mysql.sock 
-DWITH_MYISAM_STORAGE_ENGINE=1   
-DWITH_INNOBASE_STORAGE_ENGINE=1  
-DWITH_ARCHIVE_STORAGE_ENGINE=1  
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 
-DWITH_MEMORY_STORAGE_ENGINE=1  
-DWITH_PARTITION_STORAGE_ENGINE=1  
-DENABLED_LOCAL_INFILE=1  
-DWITH_READLINE=1 
-DWITH_SSL=system 
-DWITH_ZLIB=system 
-DDEFAULT_CHARSET=utf8 
-DDEFAULT_COLLATION=utf8_general_ci  
-DEXTRA_CHARSETS=all  
-DMYSQL_TCP_PORT=3306  

-DWITH_SSL=yes                                        #支持 SSL
--------------------------------------------------------------------

下面可以直接复制到linux中执行
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/data/mysqldata/3306 -DSYSCONFDIR=/etc -DMYSQL_UNIX_ADDR=/usr/local/mysql/data/mysql.sock -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_ARCHIVE_STORAGE_ENGINE=1 -DWITH_BLACKHOLE_STORAGE_ENGINE=1 -DWITH_MEMORY_STORAGE_ENGINE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DENABLED_LOCAL_INFILE=1 -DWITH_READLINE=1 -DWITH_SSL=system -DWITH_ZLIB=system -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DEXTRA_CHARSETS=all -DMYSQL_TCP_PORT=3306 

编译并安装
make && make install

六、修改mysql目录所有者和组

修改mysql安装目录
chown -R mysql:mysql /usr/local/mysql
cd /usr/local/mysql   
chown -R mysql:mysql .

修改mysql数据库文件目录
chown -R mysql:mysql /data/mysqldata/3306
cd /data/mysqldb  
chown -R mysql:mysql .

七、初始化mysql数据库
cd /usr/local/mysql   
scripts/mysql_install_db --user=mysql --datadir=/data/mysqldata/3306 

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
mysql -uroot
执行下面的命令修改root密码 
mysql> SET PASSWORD = PASSWORD('123456');
若要设置root用户可以远程访问，执行
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
使授权立即生效
FLUSH PRIVILEGES;
红色的password为远程访问时，root用户的密码，可以和本地不同。





