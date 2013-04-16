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
直接在server里面添加对应的代码：

	server{
		listen 80;
		server_name .abc.com;
		charset utf-8;
		
		location / {
			proxy_pass http://php_node/;
		} 
	}


附3个常见问题
----------------
1)分布式部署的ip获取问题。

	proxy_set_header Host $host;
	proxy_set_header X-Real-IP $remote_addr;
	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	#nginx里面把ip扔到x_forwarded_for里面。
 
2）session共享问题

	 session.save_handler = "memcache"  
	 session.save_path = "tcp://192.168.1.104:11211"  
	 
	 #phpini里面也的相应修改。注意是修改不是增加。

3)大文件上传问题
	
	#反向代理可能会导致大文件上传失败，可以修改nginx.conf
	#server 里面添加
	proxy_connect_timeout 300;
	proxy_read_timeout 300;
	proxy_send_timeout 300;
	#http里面添加
	proxy_buffers 8 16k;
	proxy_buffer_size 32k;
	#如果有fastcgi
	fastcgi_buffers 8 16k;
	fastcgi_buffer_size 32k;
