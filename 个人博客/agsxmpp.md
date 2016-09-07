# agsxmpp

# agsXMPP使用 #

agsXMPP中的例子已经有注册、登录、添加好友、接收好友添加请求、发送消息、接收消息等功能。

## 修改用户密码 ##

登录后可用以下方法修改密码

	IQ iq = new IQ(IqType.set);
    Register riq = new Register();
    riq.Username = "Username";
    riq.Password = "Password";
    iq.Query = riq;
    XmppClientConnection.Send(iq);

此代码生成的xml为：

	<query xmlns="jabber:iq:register"><username>yhcao</username><password>yhcao1</password></query>

## 程序例子 ##

[http://files.cnblogs.com/files/yhcao/IM.rar](http://files.cnblogs.com/files/yhcao/IM.rar "http://files.cnblogs.com/files/yhcao/IM.rar")