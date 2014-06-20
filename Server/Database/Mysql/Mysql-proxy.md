Mysql-prox简单反向代理
======================

    yum install mysql-proxy -y
  
    vi /etc/sysconfig/mysql-proxy
    
    #如果有多个mysql服务器，可以重复添加 "--proxy-backend-addresses=192.168.1.102:3306 --proxy-backend-addresses=192.168.1.103:3306"...来实现逐个服务器轮询查询
    PROXY_OPTIONS="--daemon --log-level=info --log-use-syslog --plugins=proxy --plugins=admin --proxy-backend-addresses=192.168.1.102:3306 --proxy-read-only-backend-addresses=192.168.1.105:3306 --proxy-lua-script=/usr/lib64/mysql-proxy/lua/proxy/rw-splitting.lua"
  
    service mysql-proxy start


如何测试负载均衡是否执行？
--------------------------

增加一个Lua检测脚本 test-conn.lua:

    function read_query( packet )
        print("read_query: connection.backend_ndx: ", proxy.connection.backend_ndx)
    end

执行：

    /usr/local/sbin/mysql-proxy --proxy-address=192.168.0.2:3306 --proxy-backend-addresses=127.0.0.1:3000 --proxy-backend-addresses=192.168.0.2:4000 --proxy-lua-script=./test-conn.lua

在windows上调用mysql客户端连接过去，打开多个cmd窗口进行连接：mysql -u root -h 192.168.0.2 -P 3306

mysql-proxy服务器端输出结果：

    read_query: connection.backend_ndx:     1
    read_query: connection.backend_ndx:     2
    read_query: connection.backend_ndx:     1
    read_query: connection.backend_ndx:     2
    read_query: connection.backend_ndx:     1
    read_query: connection.backend_ndx:     2


说明轮询生效了！



mysql-proxy 0.8X版本配置稍微有些修改？
-------------------------------------

启动方式：
    /usr/local/mysql-proxy/bin/mysql-proxy  --defaults-file=/usr/local/mysql-proxy/mysql-proxy.cnf  
        
cnf配置文件：

    [mysql-proxy]
    admin-username=root
    admin-password=123456
    admin-lua-script=/usr/local/mysql-proxy/share/admin.lua
    proxy-read-only-backend-addresses=192.168.1.131
    proxy-backend-addresses=192.168.1.132
    proxy-lua-script=/usr/local/mysql-proxy/share/rw-splitting.lua
    log-file=/var/log/mysql-proxy.log
    log-level=debug
    daemon = true
    keepalive=true
        
