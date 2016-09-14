# Github Pages和Hexo创建静态博客网站


## 安装Node.js ##

本人是window环境，所以下载window版。

下载地址：[https://nodejs.org/en/download/](https://nodejs.org/en/download/)

## 下载 msysgit  ##

下载地址：[https://git-for-windows.github.io/](https://git-for-windows.github.io/)

## Hexo ##

点击鼠标右键菜单Git Bash Here

	npm install hexo-cli -g
	npm install hexo --save
	#如果命令无法运行，可以尝试更换taobao的npm源
	npm install -g cnpm --registry=https://registry.npm.taobao.org

创建一个blog目录

执行命令：

	hexo init
	npm install

安装插件，可跳过此步骤：

	npm install hexo-generator-index --save
	npm install hexo-generator-archive --save
	npm install hexo-generator-category --save
	npm install hexo-generator-tag --save
	npm install hexo-server --save
	npm install hexo-deployer-git --save
	npm install hexo-deployer-heroku --save
	npm install hexo-deployer-rsync --save
	npm install hexo-deployer-openshift --save
	npm install hexo-renderer-marked@0.2 --save
	npm install hexo-renderer-stylus@0.2 --save
	npm install hexo-generator-feed@1 --save
	npm install hexo-generator-sitemap@1 --save

运行本地Hexo:

	hexo generate
	hexo server

浏览器查看效果：

	localhost:4000

## 上传到Github ##

找到主项目的https地址：https://xxx/github.io.git

打开blog文件夹下_config.yml文件修改：

	deploy:
	  type: git
	  repository: https://xxx/github.io.git
	  branch: master

执行命令：

	hexo g -d

## 换主题 ##

Hexo主题：[https://hexo.io/themes/](https://hexo.io/themes/)

执行命令下载主题：

git clone https://github.com/xxx.git(主题地址) themes/xxx(主题文件夹)

修改_config.yml文件：

	theme: xxx（主题名）

执行命令：

	hexo g
	hexo s

打开http://localhost:4000/即可看到效果。

## 部署到Github ##

执行命令：

	hexo clean   
	hexo g -d

## 写文章 ##

	hexo n "文章标题" #新建md文件
	hexo g -d #生成并部署


参考：

- [小白独立搭建博客–Github Pages和Hexo简明教程](http://blog.csdn.net/ljcitworld/article/details/50901626)
- [Hexo 靜態博客使用指南](http://www.jianshu.com/p/73779eacb494)
- [Hexo 中文版](http://wiki.jikexueyuan.com/project/hexo-document/)
