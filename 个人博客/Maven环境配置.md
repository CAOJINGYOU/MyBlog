# Maven环境配置

# 1.下载Maven: #

下载地址：http://maven.apache.org/


# 2.安装 Maven  #

如果需要使用到 Maven ，必须首先安装 Maven ， Maven 的下载地址在 Apache Maven 中有，您也可以点击这里下载 zip ,tar.gz。

下载好 Maven 后，需要简单安装下。将下载的  zip  或者  tar.gz  包解压到需要安装到的目录。 接下简单配置下环境变量：

1. 新建环境变量  M2_HOME  ,输入值为 Maven 的安装目录。
1. 新建环境变量  M2  ，输入值为:  %M2_HOME%\bin  。
1. 将 M2 环境变量加入  Path  的最后，如：  ;%M2%  ;。
1. 环境变量就这么简单配置下就可以了。打开命令行窗口输入  mvn -version  。可以看到如下输出：

 ![](http://images2015.cnblogs.com/blog/792271/201511/792271-20151103151936539-1813106675.png)


看到以上输出，您的 Maven 环境就已经搭建好了。

来源： <http://maven.oschina.net/help.html>
 
# 3.配置eclipse: #

![](http://images2015.cnblogs.com/blog/792271/201511/792271-20151103152011211-772620241.png)

![](http://images2015.cnblogs.com/blog/792271/201511/792271-20151103152029711-1972974655.png)


# 4.设置JRE: #

![](http://images2015.cnblogs.com/blog/792271/201511/792271-20151103152055383-1622076243.png)

    -Dmaven.multiModuleProjectDirectory=$M2_HOME

网址：http://www.cnblogs.com/yhcao/p/4901815.html

# 5.为项目安装Maven: #
选中选项右键

![](http://images2015.cnblogs.com/blog/792271/201511/792271-20151103152154649-802120497.png)


如果出现错误，请检查网络。（公司网络不能访问外网，出错了，最后让网管开通外网才成功）
