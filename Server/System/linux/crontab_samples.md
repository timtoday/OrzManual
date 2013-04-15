crontab实例
==============

方便拿来主义，放几个常用例子

  crontab -e


每5分钟
------------------------
  */5 * * * * /bin/sh /root/XXX_deamon.sh

每小时
------------------------
  0 * * * * /bin/sh /root/XXX_deamon.sh

每天
------------------------
  0 0 * * * /bin/sh /root/XXX_deamon.sh

每周
------------------------
  0 0 * * 0 /bin/sh /root/XXX_deamon.sh

每周
------------------------
  0 0 1 * * /bin/sh /root/XXX_deamon.sh

每年
------------------------
  0 0 1 1 * /bin/sh /root/XXX_deamon.sh

详细时间版
===================

  #每天早晨三点二十分执行
  
  20 3 * * * /bin/sh /root/XXX_deamon.sh

  #在每周一的下午3：00执行
  
  00 15 * * 1 /bin/sh /root/XXX_deamon.sh




