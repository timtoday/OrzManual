nginx的upstream配置
=====================
upstream是最简单常见的一种分布式的负载均衡工具，适用于相对小型的集群环境，简单配置即可实现web服务单点故障自动处理效果。<br/>
upstream常见有以下几种策略 :</br/>

轮询
--------------
	upstream php_node{
		server 192.168.6.198:80;
		server 192.168.6.199:81;
	}
	#自动轮询访问198.199两台机器
	

权重
---------------
	upstream php_node{
		server 192.168.6.198:80 weight=2;
		server 192.168.6.199:81 weight=3;
	}
	#weight越大越有机会被访问到

	
ip_hash
---------------
	upstream php_node{
		ip_hash;
		server 192.168.6.198:80;
		server 192.168.6.199:81;
	}
	#根据访问者IP分配服务器


fair
----------
	upstream php_node{
		ip_hash;
		server 192.168.6.198:80;
		server 192.168.6.199:81;
		fair;
	}
	#可以根据相应效率来分配服务器，不过要装个模块支持 http://wiki.nginx.org/HttpUpstreamFairModule

使用方法
--------------
直接在server里面添加对应的代码：<br/>
	server{
		listen 80;
		server_name .abc.com;
		charset utf-8;
		
		location / {
			proxy_pass http://php_node/;
		} 
	}

 
	 