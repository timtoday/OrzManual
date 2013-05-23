常见Form操作
=============

动态改变控件属性
----------------

	GetDlgItem(IDC_STATIC)->SetWindowText(msg);     //修改caption
	GetDlgItem(IDC_GIFFIRST)->ShowWindow(SW_HIDE);  //隐藏 SW_HIDE  ，显示  SW_SHOW
	
调用外部浏览器
----------------

	CString url;
	url="http://www.163.com"; 
	ShellExecute(NULL,_T("open"),url,NULL,NULL,SW_SHOW);

 

 
