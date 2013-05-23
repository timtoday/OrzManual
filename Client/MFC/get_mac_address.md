检测网络，并获取当前上网卡的MAC地址
==================================
头文件

	#pragma once

	class pc_info
	{
	public:
		pc_info(void);
		CString get_mac(void);
	public:
		~pc_info(void);
	};

CPP文件
	#include "StdAfx.h"
	#include "pc_info.h"
	#include <WinSock2.h>
	 
	#include <iostream>
	#include "Nb30.h"
	#pragma comment(lib, "Netapi32.lib")
	typedef struct _ASTAT_
	{
	 ADAPTER_STATUS adapt;
	 NAME_BUFFER    NameBuff[30];
	}ASTAT, * PASTAT;


	 
	pc_info::pc_info(void)
	{
	}

	pc_info::~pc_info(void)
	{
	}


	UCHAR GetAddressByIndex(int lana_num,ASTAT & Adapter)
	{
	 UCHAR uRetCode;
	 //-------------------------------------------------------------------
	 NCB ncb;
	 memset(&ncb, 0, sizeof(ncb) );
	 ncb.ncb_command = NCBRESET;
	 ncb.ncb_lana_num = lana_num;
	 //指定网卡号,首先对选定的网卡发送一个NCBRESET命令,以便进行初始化
	 uRetCode = Netbios(&ncb );
	 memset(&ncb, 0, sizeof(ncb) );
	 ncb.ncb_command = NCBASTAT;
	 ncb.ncb_lana_num = lana_num;//指定网卡号
	 //strcpy((char *)ncb.ncb_callname,"*");
	 //strcpy(char *strDestination,const char *strSource)
	 //strcpy_s(destination, sizeof (destination) / sizeof (destination[0]), source); 
	 strcpy_s((char *)ncb.ncb_callname,sizeof (ncb.ncb_callname)/2,"*");
	 ncb.ncb_buffer = (unsigned char *)&Adapter;
	 //指定返回的信息存放的变量
	 ncb.ncb_length = sizeof(Adapter);
	 //接着,可以发送NCBASTAT命令以获取网卡的信息
	 uRetCode = Netbios(&ncb );
	 //-------------------------------------------------------------------
	 return uRetCode;
	}


	CString pc_info::get_mac(void)
	{
	 CString strMacAddress;
	 //-------------------------------------------------------------------
	 NCB ncb;
	 UCHAR uRetCode;
	 int num = 0;
	 LANA_ENUM lana_enum;
	 memset(&ncb, 0, sizeof(ncb) );
	 ncb.ncb_command = NCBENUM;
	 ncb.ncb_buffer = (unsigned char *)&lana_enum;
	 ncb.ncb_length = sizeof(lana_enum);
	 //向网卡发送NCBENUM命令,以获取当前机器的网卡信息,如有多少个网卡
	 //每张网卡的编号等
	 uRetCode = Netbios(&ncb);
	 if (uRetCode == 0)
	 {
	  num = lana_enum.length;
	  //对每一张网卡,以其网卡编号为输入编号,获取其MAC地址
	  for (int i = 0; i < num; i++)
	  {
	   ASTAT Adapter;
	   if(GetAddressByIndex(lana_enum.lana[i],Adapter) == 0)
	   {
		   strMacAddress.Format(_T( "%02X:%02X:%02X:%02X:%02X:%02X "),
		 Adapter.adapt.adapter_address[0],
		 Adapter.adapt.adapter_address[1],
		 Adapter.adapt.adapter_address[2],
		 Adapter.adapt.adapter_address[3],
		 Adapter.adapt.adapter_address[4],
		 Adapter.adapt.adapter_address[5]);

	   }
	  }
	 }
	 //-------------------------------------------------------------------
	 return strMacAddress;

	}
