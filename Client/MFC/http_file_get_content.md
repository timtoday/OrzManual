HTTP抓取WEB内容
=============
头文件

	#pragma once

	class http_opt
	{
	public:
		http_opt(void);
		CString get_str(CString url);
	public:
		~http_opt(void);
	};


CPP文件


	#include "StdAfx.h"
	#include "http_opt.h"
	#include <afxinet.h>  

	http_opt::http_opt(void)
	{
	}

	http_opt::~http_opt(void)
	{
	}

	  
	CString http_opt::get_str(CString url)
	{
	 
		CString strHtml;
		CInternetSession session("HttpClient");

		//char * url = " http://www.imobile.com.cn/simcard.php?simcard=1392658";

		CHttpFile* pfile = (CHttpFile *)session.OpenURL(url);
		 

		DWORD dwStatusCode;

		pfile -> QueryInfoStatusCode(dwStatusCode);

		if(dwStatusCode == HTTP_STATUS_OK)

		{

		CString content;

		CString data;
	 
		while (pfile -> ReadString(data))
			
		{
		  
			content  += data + "\r\n";

		}	  
		content.TrimRight();
		  
		strHtml=content;
		  
		}
		  
		pfile -> Close();

		delete pfile;
		 
		session.Close();


		return strHtml.Trim();

	}
