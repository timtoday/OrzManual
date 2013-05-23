c++简单线程操作
=============

	UINT __cdecl CxxDlg::_thread( LPVOID pParam )
	{
	 CSfPlatformDlg *dlg = (CSfPlatformDlg *)pParam;
	 dlg->threadFunction();
	 return 0;
	}

	void CxxDlg::threadFunction()
	{

			Sleep(1000);
			//TODO something...

	}

 
创建调用

	void CxxDlg::OnBnClickedButton1()
	{
		AfxBeginThread(_thread, (void *)this);

	}
	 
