centos 集群文件自动同步配置
================================
自动同步可以使用很多方法，以下介绍inotify+rsync的配置(NFS的同学，文件量大于100G的直接跳过)

scp
-----
yum install openssh-clients

rsync
------
centos 直接yum安装

	yum install rsync -y

配置rsync用户
	useradd -m rsync
	passwd rsync
	
(以上操作每个节点都需要)	
	
	#su rsync
	[rsync@mynode1 ~]$ ssh-keygen -t rsa -P '' -f ~/.ssh/id_dsa
	Generating public/private rsa key pair.
	Your identification has been saved in /home/rsync/.ssh/id_dsa.
	Your public key has been saved in /home/rsync/.ssh/id_dsa.pub.
	The key fingerprint is:
	c9:6b:86:2e:00:c3:0e:89:e1:a8:80:97:33:25:cb:9e rsync@mynode1
	The key's randomart image is:
	+--[ RSA 2048]----+
	|                 |
	|. . .            |
	|*+ =             |
	|XoB    . .       |
	|== +    S        |
	|..E    . .       |
	|   .  . +        |
	|    .. o         |
	|     ..          |
	+-----------------+
	##拷贝到其他被访问节点去
	[rsync@mynode1 .ssh]$ scp ~/.ssh/id_dsa.pub rsync@192.168.6.198:~/.ssh/
	rsync@192.168.6.198's password: 
	id_dsa.pub                                                       100%  395     0.4KB/s   00:00    
		
	到其他主节点配置ssh自动登陆
	[rsync@mynode2 .ssh]$ mv id_dsa.pub authorized_keys
	#SElinux关闭过的同学跳过以下两行
	[rsync@mynode2 .ssh]$ restorecon -R -v ~/.ssh
	[rsync@mynode2 .ssh]$ setenforce 0
	
inotify-tools
--------------

代码目录
https://github.com/rvoicilas/inotify-tools


官方帮助
https://github.com/rvoicilas/inotify-tools/wiki#wiki-getting

下载安装
	wget -c http://github.com/downloads/rvoicilas/inotify-tools/inotify-tools-3.14.tar.gz
	
	tar zxvf inotify-tools-3.14.tar.gz
	 ./configure --prefix=/usr/local/intify-tools
	 make && make install
	 
配置监控同步脚本
	vi inotify_rsync.sh
	#内容
	
	#!/bin/sh

	# get the current path
	CURPATH=`pwd`
	src=/home/rsync/test/   
	des1=rsync@192.168.6.198:/home/rsync/test
	 
	/usr/local/intify-tools/bin/inotifywait -mrq --timefmt '%d/%m/%y %H:%M' --format '%T %w %f' \
	-e modify,delete,create,attrib,move  ${src} | while read date time dir file; do

		   FILECHANGE=${dir}${file} 
		   FILECHANGEREL=`echo "$FILECHANGE" | sed 's_'$CURPATH'/__'`	   
		   /usr/bin/rsync -vzrtopg --delete --progress ${src} ${des1}  && 
		   echo "[${date}][${time}]${FILECHANGE} was rsynced" >> /tmp/rsync.log 2>&1 
	done

			
相关参数：
inotify 可以监视的文件系统事件包括：
    
　　IN_ACCESS，即文件被访问<br/>
　　IN_MODIFY，文件被 write<br/>
　　IN_ATTRIB，文件属性被修改，如 chmod、chown、touch 等<br/>
　　IN_CLOSE_WRITE，可写文件被 close<br/>
　　IN_CLOSE_NOWRITE，不可写文件被 close<br/>
　　IN_OPEN，文件被 open<br/>
　　IN_MOVED_FROM，文件被移走,如 mv<br/>
　　IN_MOVED_TO，文件被移来，如 mv、cp<br/>
　　IN_CREATE，创建新文件<br/>
　　IN_DELETE，文件被删除，如 rm<br/>
　　IN_DELETE_SELF，自删除，即一个可执行文件在执行时删除自己<br/>
　　IN_MOVE_SELF，自移动，即一个可执行文件在执行时移动自己<br/>
　　IN_UNMOUNT，宿主文件系统被 umount<br/>
　　IN_CLOSE，文件被关闭，等同于(IN_CLOSE_WRITE | IN_CLOSE_NOWRITE)<br/>
　　IN_MOVE，文件被移动，等同于(IN_MOVED_FROM | IN_MOVED_TO)<br/>
　　注：上面所说的文件也包括目录。<br/>

    inotifywait 命令的常用参数包括：
    -m, --monitor      保持一直监听
    -r, --recursive    若有多级目录循环递归每一层。
    -q, --quiet        静默式运行
    -e <event>, --event <event>  create,move,delete,modify,move
	
	rsync -ahqzt --delete $SRC $DST
	-a 存档模式
	-h 保存硬连接
	-q 制止非错误信息
	-z 压缩文件数据在传输
	-t 维护修改时间
	-delete 删除于多余文件

	
要排除同步某个目录时，为rsync添加--exculde=PATTERN参数，注意，路径是相对路径，具体查看man rsync。
要排除某个目录的事件监听的处理时，为inotifywait添加--exclude或--excludei参数，具体查看man inotifywait。
增加排除功能的改进版：

	#!/bin/sh
	
	# get the current path
	CURPATH=`pwd`
	src=/opt/wwwroot/
	des1=root@192.168.1.102:/home/web/wwwroot
	#监听忽略
	exclude='(log|Runtime|Conf)'
	
	/usr/local/inotify-tools/bin/inotifywait -mrq --exclude ${exclude} --timefmt '%d/%m/%y %H:%M' --format '%T %w %f' \
	-e modify,delete,create,attrib,move  ${src} | while read date time dir file; do
	
	       FILECHANGE=${dir}${file}
	       FILECHANGEREL=`echo "$FILECHANGE" | sed 's_'$CURPATH'/__'`
	       /usr/bin/rsync -vzrtopg --delete --progress --exclude-from=/root/exclude.list  ${src} ${des1}  &&
	       # echo "[${date}][${time}]${FILECHANGE} was rsynced" >> /tmp/rsync.log 2>&1
	done
	
	增加rsync的忽略文件/root/exclude.list
	[root@node1 ~]# cat exclude.list 
	Runtime*
	*.log
	Conf*
	#一行一个，支持模糊*匹配
	

	#调整权限
	chmod +x inotify_rsync.sh
	#执行
	 ./inotify_rsync.sh &
	#自启动
	echo "/root/inotify_rsync.sh " >> /etc/rc.local

配置完成，到/home/rsync/test创建修改文件，再去子节点看看效果

	
