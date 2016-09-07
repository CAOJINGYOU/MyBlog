# xmpp

# xmpp学习 #

---

## 下载： ##

[Openfire](http://www.igniterealtime.org/downloads/index.jsp)

服务器：Openfire 4.0.2

客户端：Spark 2.7.7

## 安装 ##

### Openfire安装： ###

根据提示一直下一步，服务器域名设置为：localhost(ps:如果使用gloox，还是直接用机器名，因为gloox不识别ip)；数据库使用内嵌数据库或别的标准数据库。

使用mysql的时候需要注意要用管理员权限打开Openfire,否则会出现如下错误：

	HTTP ERROR 500
	Problem accessing /setup/setup-profile-settings.jsp. Reason: 
	
	    Server Error
	
	Caused by:


安装完成后浏览器登录：http://127.0.0.1:9090

### Spark ###

分别在两台电脑上安装Spark，创建各自用户，互添加好友，然后就可以通信了。

## 自己实现客户端 ##

### 使用agsXMPP ###

很好用，直接到官网下载即可，有详细例子，但是程序运行时调试输出信息中会有

    在 System.Net.Sockets.SocketException 中第一次偶然出现的“System.dll”类型的异常

的提示

网上说可能是服务器的问题，暂时没找。

### 使用gloox ###

#### 下载gloox ####

下载地址：[https://camaya.net/gloox/download/](https://camaya.net/gloox/download/ "https://camaya.net/gloox/download/")

gloox 0.9.9.12：[http://camaya.net/download/gloox-0.9.9.12.tar.bz2](http://camaya.net/download/gloox-0.9.9.12.tar.bz2 "http://camaya.net/download/gloox-0.9.9.12.tar.bz2")

直接用vs打开gloox.vcxproj即可运行

gloox 1.0.15：[http://camaya.net/download/gloox-1.0.15.tar.bz2](http://camaya.net/download/gloox-1.0.15.tar.bz2 "http://camaya.net/download/gloox-1.0.15.tar.bz2")

直接用vs打开gloox.vcxproj运行会有问题，需要修改一下文件。
以下提供一个可在vs2013中直接运行的gloox

[gloox_lib_with_vs2013.rar](http://files.cnblogs.com/files/yhcao/gloox_lib_with_vs2013.rar)


svn地址： 
gloox-1.0：svn co svn://svn.camaya.net/gloox/branches/1.0


#### 调试例子 ####

1. 新建一个win32控制台空项目,字符集为多字节
1. 新建筛选器gloox
1. 把gloox中src文件夹与config.h.win文件复制到新项目中
1. 修改src文件夹为gloox,并把其中文件(不包括子目录)添加到筛选器gloox中
1. 加载gloox文件夹下的examples中例子进行调试

例如：MyGloox2015下MyGloox项目

[MyGloox.rar](http://files.cnblogs.com/files/yhcao/MyGloox.rar "MyGloox.rar")

#### gloox内存泄露 ####

本人发现不管是直接使用gloox还是自己封装gloox，都会有内存泄露，找了几个版本以及别人写的程序，发现都有内存泄露的问题。

### 使用libstrophe ###

#### libstrophe编译 ####

1. 下载libstrophe-master.zip

1. 解压，看看各目录，expat是空的，再下载expat，解压，不需要编译，把.h和.c放到expat\lib下面。

1. 然后先编译expat，顺利编译出 lib文件、

1. 再编译libstrophe工程，会提示没有parser.c。看说明文档，libstrophe缺省用的是expat,可选用libxml2，进入src下，看到有parser_libxml2.c和parser_expat.c，很明显，把parser_expat.c改名成parser.c就可以了。编译通过。

1. 再编译其他例子工程，提示没有va_copy。vc2008时好象没有兼容它，不过也没关系，这问题肯定很多人碰到过，果然随便一google，就有答案了， #define一下就行了。

		#ifndef va_copy
		# ifdef __va_copy
		# define va_copy(DEST,SRC) __va_copy((DEST),(SRC))
		# else
		# define va_copy(DEST, SRC) memcpy((&DEST), (&SRC), sizeof(va_list))
		# endif
		#endif 

或者

		#ifndef va_copy
		#define va_copy(d,s) ((d) = (s))
		#endif

1. 全部编译通过，测试登录和发消息，api简洁明了，是目前见过最好用的xmpp库。当然gloox和qxmpp也都很好用，还有libjingle功能更强，各取所需。

个人又遇到一个错误：
	
	1>libstrophe.lib(sasl.obj) : error LNK2019: 无法解析的外部符号 _SCRAM_SHA1_ClientSignature，该符号在函数 _sasl_scram_sha1 中被引用
	1>libstrophe.lib(sasl.obj) : error LNK2019: 无法解析的外部符号 _SCRAM_SHA1_ClientKey，该符号在函数 _sasl_scram_sha1 中被引用
	1>E:\code\C\Local\xmpp\libstrophe-0.8.8\libstrophe-0.8.8\vs2008\Debug\roster example.exe : fatal error LNK1120: 2 个无法解析的外部命令

此问题需要把sasl.c与auth.c文件

如果还不行请用这个：

[libstrophe(可用).rar](http://files.cnblogs.com/files/yhcao/libstrophe%28%E5%8F%AF%E7%94%A8%29.rar "http://files.cnblogs.com/files/yhcao/libstrophe%28%E5%8F%AF%E7%94%A8%29.rar")

可用vs2008与vs2012直接打开

#### libstrophe文档 ####

[http://strophe.im/libstrophe/doc/0.8-snapshot/](http://strophe.im/libstrophe/doc/0.8-snapshot/ "http://strophe.im/libstrophe/doc/0.8-snapshot/")


