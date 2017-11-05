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
-DMYSQL_DATADIR=/usr/local/mysql/data \
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
-DWITH_DEBUG=1

php
http://blog.csdn.net/u010861514/article/details/51926575
