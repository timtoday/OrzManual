网络连接状态的监控
=====================

服务器响应速度慢，可以用vnstat来查看流量情况，判断是否是访问量过大造成的负载<br/>
vnstat官方网站 http://humdi.net/vnstat/


安装
---------

    wget -c http://humdi.net/vnstat/vnstat-1.11.tar.gz
    tar zxvf vnstat-1.11.tar.gz
    cd vnstat-1.11
    make && make install


常用命令
-----------------
    #实时监控
    vnstat -l -i eth0 
    #自带帮助
    vnstat -help
    -q,  --query          query database
    -h,  --hours          show hours
    -d,  --days           show days
    -m,  --months         show months
    -w,  --weeks          show weeks
    -t,  --top10          show top10
    -s,  --short          use short output
    -u,  --update         update database
    -i,  --iface          select interface (default: eth0)
    -?,  --help           short help
    -v,  --version        show version
    -tr, --traffic        calculate traffic
    -ru, --rateunit       swap configured rate unit
    -l,  --live           show transfer rate in real time


扩展使用
-----------------
    vnstat PHP frontend 是一个基于php的vnstat状态查看工具，详细配置见官网
    http://www.sqweek.com/sqweek/index.php?p=1

