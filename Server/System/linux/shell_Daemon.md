linux 基于shell 的守护进程
==========================

偷懒监控mysql  apche  nginx之类的服务是否down掉。
自动恢复
 
	#!/bin/sh
	count=`ps -ef |grep "mysqld"  | wc -l`
	if [ $count -lt 3 ] ;then
		#echo "restart"
		nohup /root/xxxx.sh >/dev/null 2>&1 &
		########/etc/init.d/mysql restart
	fi
加到crontab 里面




用crontab每五分钟检查一次
--------------------------
	crontab -e
	*/5 * * * * root /root/xxx_deamon.sh
