FreeBsd System Install 操作系统安装
===================================

严格来说[FreeBSD](http://www.freebsd.org)不是linux系统，属于原始的uninx分支版本。之前项目中用过FreeBSD 5.4很长时间，由于之前的版本对JAVA的支不好，所以都转向到Linux阵营。<br />
但是，如果项目中不使用JAVA ORACLE之类的东西，或者单纯作为前端的服务器，FreeBSD还是有很大优势的，（具体优势暂不追究，有兴趣的同学百度一下）。<br/>



版本
---------
当前官方提供的生产适用版本是9.1,而多数服务器都是采用64位操作系统,下面所有内容之保证对FreeBSD 9.1 x64 适用。<br/>
采用最小化网络安装（ftp://ftp.cn.freebsd.org/pub/FreeBSD/releases/ISO-IMAGES/9.1/FreeBSD-9.1-RELEASE-amd64-bootonly.iso)<br/>
这种方式可以减少多余文件下载。但是安装时间较长，真实IDC环境几乎可以忽略

 
