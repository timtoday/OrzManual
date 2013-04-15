linux 基于shell 的守护进程
==========================

偷懒监控mysql  apche  nginx之类的服务是否down掉。
自动恢复
 
	#!/bin/sh
	count=`ps -ef |grep "inotifywait"  | wc -l`
	if [ $count -lt 2 ] ;then
	    #echo "restart"
	   nohup /bin/sh /root/xxx.sh >/dev/null 2>&1 &
	fi
加到crontab 里面




用crontab每五分钟检查一次
--------------------------
	crontab -e

	*/5 * * * * /bin/sh /root/xxx_deamon.sh
