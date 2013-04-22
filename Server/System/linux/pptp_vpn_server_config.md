Centos6 下基于PPTP的VPN服务器搭建
=================================
服务器环境是centos6_x64,其他版本稍微自己调整下。

相关资源
------------
    wget -c https://github.com/timtoday/OrzManual/blob/master/Server/System/linux/resource/vpn/dkms-2.0.17.5-1.noarch.rpm?raw=true
    wget -c https://github.com/timtoday/OrzManual/blob/master/Server/System/linux/resource/vpn/kernel_ppp_mppe-1.0.2-3dkms.noarch.rpm?raw=true
    wget -c https://github.com/timtoday/OrzManual/blob/master/Server/System/linux/resource/vpn/ppp-2.4.5-17.0.rhel6.x86_64.rpm?raw=true
    wget -c https://github.com/timtoday/OrzManual/blob/master/Server/System/linux/resource/vpn/pptpd-1.3.4-2.el6.x86_64.rpm?raw=true


    yum -y install make libpcap iptables gcc-c++ logrotate tar cpio perl pam tcp_wrappers
    rpm -ivh dkms-2.0.17.5-1.noarch.rpm
  	rpm -ivh kernel_ppp_mppe-1.0.2-3dkms.noarch.rpm


    rpm -qa kernel_ppp_mppe
  	rpm -Uvh ppp-2.4.5-17.0.rhel6.x86_64.rpm	
  	rpm -ivh pptpd-1.3.4-2.el6.x86_64.rpm

    mknod /dev/ppp c 108 0 
  	echo 1 > /proc/sys/net/ipv4/ip_forward 
  	echo "mknod /dev/ppp c 108 0" >> /etc/rc.local
  	echo "echo 1 > /proc/sys/net/ipv4/ip_forward" >> /etc/rc.local
    #当前机器IP
  	echo "localip 10.0.0.24" >> /etc/pptpd.conf

    #远程接入者的IP
  	echo "remoteip 10.0.0.200-254" >> /etc/pptpd.conf
  	echo "ms-dns 8.8.8.8" >> /etc/ppp/options.pptpd
  	echo "ms-dns 8.8.4.4" >> /etc/ppp/options.pptpd

    #服务启动
    service pptpd start

账户配置
------------
    #账户密码配置文件在/etc/ppp/chap-secrets
    #格式如下
    username pptpd password *
    #用户名 pptpd 密码 *
    
服务修复
--------------

    mknod /dev/ppp c 108 0
    service pptpd start
    
相关问题
------------------
1）service pptpd start 成功了链接不上？<br/>
    IPtables问题,service iptables stop 后尝试。<br/>
2）windows链接后本地上网变慢了？<br/>
    网络链接-〉属性-〉网络-〉TCP/IPV4 ->高级-〉IP设置-〉在远程网络上使用默认网关（不选）<br/>
    
    




