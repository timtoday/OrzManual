只使用Myisam的例子
===================================

请根据配置对号入座。
源文件采用my-huge.cnf参考

方案A
---------

32 bit system
2GB of memory
Dedicated DB Box
All MyISAM
[mysqld]
user = mysql
pid-file = /var/run/mysqld/mysqld.pid
socket = /var/run/mysqld/mysqld.sock
port = 3306
basedir = /mysql/
datadir = /data01/data
tmpdir = /tmp
thread_cache_size = 128
table_cache = 512
key_buffer = 768M
sort_buffer_size = 256K
read_buffer_size = 256K
read_rnd_buffer_size = 256K
max_allowed_packet = 1M
tmp_table_size=16M
max_heap_table_size=16M
query_cache_size=64M
query_cache_type=1
log_output=FILE
slow_query_log_file=/mysql/slow1.log
slow_query_log=1
long_query_time=3
log-error=/mysql/error.log
myisam_recover = force,backup
myisam_sort_buffer_size=128M
skip-innodb 


方案B
---------

32 bit system
4GB of memory
Dedicated DB Box
All MyISAM
[mysqld]
user = mysql
pid-file = /var/run/mysqld/mysqld.pid
socket = /var/run/mysqld/mysqld.sock
port = 3306
basedir = /mysql/
datadir = /data01/data
tmpdir = /tmp
thread_cache_size = 256
table_cache = 1024
key_buffer = 1500M
sort_buffer_size = 256K
read_buffer_size = 256K
read_rnd_buffer_size = 256K
max_allowed_packet = 1M
tmp_table_size=32M
max_heap_table_size=32M
query_cache_size=128M
query_cache_type=1
log_output=FILE
slow_query_log_file=/mysql/slow1.log
slow_query_log=1
long_query_time=3
log-error=/mysql/error.log
myisam_recover = force,backup
myisam_sort_buffer_size=256M
skip-innodb 


方案C
---------

64 bit system
8GB+ of memory
Dedicated DB Box
All MyISAM
[mysqld]
user = mysql
pid-file = /var/run/mysqld/mysqld.pid
socket = /var/run/mysqld/mysqld.sock
port = 3306
basedir = /mysql/
datadir = /data01/data
tmpdir = /tmp
thread_cache_size = 256
table_cache = 1024
key_buffer = 4000M
sort_buffer_size = 256K
read_buffer_size = 256K
read_rnd_buffer_size = 256K
max_allowed_packet = 1M
tmp_table_size=64M
max_heap_table_size=64M
query_cache_size=128M
query_cache_type=1
log_output=FILE
slow_query_log_file=/mysql/slow1.log
slow_query_log=1
long_query_time=3
log-error=/mysql/error.log
myisam_recover = force,backup
myisam_sort_buffer_size=512M
skip-innodb
