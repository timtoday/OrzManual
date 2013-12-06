MySql Master/Slave配置
===================================

Master/Slave方式配置较简单，只需要在mysql默认配置中加入简单设置即可，应用场景较多。
Master负责写入、读取数据功能，Slave只有读取功能(这不算是读写分离)。
以下2台服务器做主从配置：

	Master : 192.168.1.101
	Slave  : 192.168.1.102

注意事项
-------------------
	1.Master/Slave的mysql-server版本必须相同。
	2.配置前Master/Slave的my.cnf必须相同。
	3.Slave需要同步的数据库不能有写入操作。
	4.同步操作前Master必须先锁表(FLUSH TABLES WITH READ LOCK)，再导出数据到Slave库，保证Master/Slave数据绝对相同,Slave启动正常后再解锁（UNLOCK TABLES）。

Master配置
---------------------
	#修改my.cnf
	server-id        = 1  			#Master/Slave不能使用相同ID
	log-bin			 = mysql-bin	#开启日志
	binlog-do-db	 = dbname       #这里写要同步的库名字
	binlog-do-db	 = xxx       	#如果要同步多个库，请重复写这一行	
    	binlog-ignore-db = mysql,test	#这里写忽略同步的库名字
	
	#修改完成后重启一下mysql服务器
	#service mysqld restart
	
	
	#增加slave访问账户
	GRANT REPLICATION SLAVE ON *.* to slave_user@192.168.1.102  identified by '123456';
	#格式:GRANT REPLICATION SLAVE ON *.* TO '帐号'@'从服务器IP或主机名' IDENTIFIED BY '密码';
	
	#刷新权限
	flush privileges;	
 


Slave配置
-------------------
	#修改my.cnf
	server-id			= 2
	master-host    			= 192.168.1.101
	master-user     		= slave_user
	master-password 		= 123456
	master-port     		= 3306
	master-connect-retry		= 60
	replicate-do-db			= dbname   			#要同步的数据库 
	replicate-do-db	    		= xxx				#如果有多个库需要同步，请重复这一行 
	log-slave-updates
	slave-skip-errors		= all 				#忽略错误
	
	#修改完成后重启一下mysql服务器
	#service mysqld restart
		
	
	
数据配置
---------------------
	#先到slave服务器上停止slave
	stop slave;
	
	#到master服务器上锁定表数据
	FLUSH TABLES WITH READ LOCK;
	
	#记录master服务器的状态
	show master status;
	
File	          | Position    | Binlog_Do_DB | Binlog_Ignore_DB
:-----------------|------------:|-------------:|----------------:
 mysql-bin.000001 | 123         | dbname       | mysql     

	
	#记录File,Position的2个值。
	
	#导出Master中需要同步的数据库
	mysqldump -uroot -p dbname>dbname.sql
	
	#再到Slave服务器倒入
	mysql -uroot -p dbname<dbname.sql
	
	#设定File,Position 值
	change master to master_host='192.168.1.101',master_user='slave_user',master_password='123456', master_log_file='mysql-bin.000001' ,master_log_pos=123;
	
	#执行成功后再启动slave
	start slave
	
	#查看slave 状态
	show slave status
	#如果状态中“file,position”值均正确，并且
 
Master_Log_File	  | Read_Master_Log_Pos | Slave_SQL_Running | Slave_IO_Running
:-----------------|--------------------:|------------------:|-----------------:
 mysql-bin.000001 | 123                 | YES       	    | YES     	
	
	#说明Master/Slave设定成功
	
	#再到Master服务器解除数据库锁定，完成所有配置
	UNLOCK TABLES
	
	
	
	
	
异常处理
---------------------
	#如果在Master/Slave 运行过程中，发现数据库不同步，slave status中 Slave_SQL_Running/Replicate_DO_DB 有出现NO的情况，说明出现异常，需要修复数据。
	#具体操作如下。
	#到Slave服务器停止Slave
	stop slave
	
	#重置slave配置
	reset slave
	
	#到Master服务器锁定数据
	FLUSH TABLES WITH READ LOCK;
	#导出数据库
	
	#再重置master状态
	reset master
	
	#再使用新配置的方法，把数据导入到slave然后按新配置一样操作即可
	

	
	
	
	
	
	
	
	
	
 



