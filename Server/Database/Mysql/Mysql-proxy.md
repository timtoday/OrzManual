Mysql-prox简单反向代理
======================

    yum install mysql-proxy -y
  
    vi /etc/sysconfig/mysql-proxy
    
    #如果有多个mysql服务器，可以重复添加 "--proxy-backend-addresses=192.168.1.102:3306 --proxy-backend-addresses=192.168.1.103:3306"...来实现逐个服务器轮询查询
    PROXY_OPTIONS="--daemon --log-level=info --log-use-syslog --plugins=proxy --plugins=admin --proxy-backend-addresses=192.168.1.102:3306 --proxy-read-only-backend-addresses=192.168.1.105:3306 --proxy-lua-script=/usr/lib64/mysql-proxy/lua/proxy/rw-splitting.lua"
  
    service mysql-proxy start
  
