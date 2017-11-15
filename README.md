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

五：安装
tar zxvf mysql-5.6.36.tar.gz
cd mysql-5.6.36
cmake
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql               #安装目录
-DMYSQL_DATADIR=/data/mysql/data                      #数据库存放目录
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
-DMYSQL_DATADIR=/data/mysql/data 
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
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/data/mysqldata/3306 -DSYSCONFDIR=/etc -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_MEMORY_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DMYSQL_UNIX_ADDR=/usr/local/mysql/data/mysql.sock  -DMYSQL_TCP_PORT=3306 -DENABLED_LOCAL_INFILE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DEXTRA_CHARSETS=all -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci




