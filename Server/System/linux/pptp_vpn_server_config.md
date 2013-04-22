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
