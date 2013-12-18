Mysql-prox简单反向代理
======================

  yum install mysql-proxy -y
  
  vi /etc/sysconfig/mysql-proxy
  
  PROXY_OPTIONS="--daemon --log-level=info --log-use-syslog --plugins=proxy --plugins=admin --proxy-backend-addresses=192.168.1.102:3306 --proxy-read-only-backend-addresses=192.168.1.105:3306 --proxy-lua-script=/usr/lib/mysql-proxy/lua/proxy/rw-splitting.lua"
  
  service mysql-proxy start
  
