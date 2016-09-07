# electron之Windows下使用 html js css 开发桌面应用程序

# 1.atom/electron #

## github: ##
[https://github.com/atom/electron](https://github.com/atom/electron)

## 中文文档： ##

[https://github.com/atom/electron/tree/master/docs-translations/zh-CN](https://github.com/atom/electron/tree/master/docs-translations/zh-CN)

# 2.下载 electron-v0.36.5-win32-x64 #

[https://github.com/atom/electron/releases/download/v0.36.5/electron-v0.36.5-win32-x64.zip](https://github.com/atom/electron/releases/download/v0.36.5/electron-v0.36.5-win32-x64.zip)

# 3.新建一个项目-快速入门： #

[https://github.com/atom/electron/blob/master/docs-translations/zh-CN/tutorial/quick-start.md](https://github.com/atom/electron/blob/master/docs-translations/zh-CN/tutorial/quick-start.md)

大体上，一个 Electron 应用的目录结构如下：

    your-app/
    ├── package.json
    ├── main.js
    └── index.html

## you-app: ##

[electron之Windows下使用html,js,css,开发桌面应用程序_you-app.rar](http://files.cnblogs.com/files/yhcao/electron%E4%B9%8BWindows%E4%B8%8B%E4%BD%BF%E7%94%A8html%2Cjs%2Ccss%2C%E5%BC%80%E5%8F%91%E6%A1%8C%E9%9D%A2%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F_you-app.rar)

### package.json： ###
    
    {
      "name": "your-app",
      "version" : "0.1.0",
      "main": "main.js"
    }
    
### main.js： ###

    var app = require('app');  // 控制应用生命周期的模块。
    var BrowserWindow = require('browser-window');  // 创建原生浏览器窗口的模块
    // 保持一个对于 window 对象的全局引用，不然，当 JavaScript 被 GC，
    // window 会被自动地关闭
    var mainWindow = null;
    // 当所有窗口被关闭了，退出。
    app.on('window-all-closed', function() {
      // 在 OS X 上，通常用户在明确地按下 Cmd + Q 之前
      // 应用会保持活动状态
      if (process.platform != 'darwin') {
    app.quit();
      }
    });
    // 当 Electron 完成了初始化并且准备创建浏览器窗口的时候
    // 这个方法就被调用
    app.on('ready', function() {
      // 创建浏览器窗口。
      mainWindow = new BrowserWindow({width: 800, height: 600});
      // 加载应用的 index.html
      mainWindow.loadURL('file://' + __dirname + '/index.html');
      // 打开开发工具
      mainWindow.openDevTools();
      // 当 window 被关闭，这个事件会被发出
      mainWindow.on('closed', function() {
    // 取消引用 window 对象，如果你的应用支持多窗口的话，
    // 通常会把多个 window 对象存放在一个数组里面，
    // 但这次不是。
    mainWindow = null;
      });
    });
    

### index.html： ###

    <!DOCTYPE html>
    <html>
      <head>
    <title>Hello World!</title>
      </head>
      <body>
    <h1>Hello World!</h1>
    We are using io.js <script>document.write(process.version)</script>
    and Electron <script>document.write(process.versions['electron'])</script>.
      </body>
    </html>

# 4.应用部署: #

[https://github.com/atom/electron/blob/master/docs-translations/zh-CN/tutorial/application-distribution.md](https://github.com/atom/electron/blob/master/docs-translations/zh-CN/tutorial/application-distribution.md)

为了使用Electron部署你的应用程序，你存放应用程序的文件夹需要叫做 app 并且需要放在 Electron 的资源文件夹下（在 OS X 中是指 Electron.app/Contents/Resources/，在 Linux 和 Windows 中是指 resources/） 就像这样：

在 Windows 和 Linux 中:

    electron/resources/app
    ├── package.json
    ├── main.js
    └── index.html

## Windows环境下的NodeJS+NPM+Bower安装配置 ##

[http://jingyan.baidu.com/article/2d5afd69e243cc85a2e28efa.html](http://jingyan.baidu.com/article/2d5afd69e243cc85a2e28efa.html)

## 下载并安装node-v5.5.0-x64.msi ##

[https://nodejs.org/dist/v5.5.0/node-v5.5.0-x64.msi](https://nodejs.org/dist/v5.5.0/node-v5.5.0-x64.msi)

### 检验是否安装成功： ###

    C:\Users\yhcao>node -v
    v5.5.0
    
    C:\Users\yhcao>npm -v
    3.3.12

### 用nmp打包成asar: ###

第一步：安装asar

    npm install -g asar

第二步：打包

    asar pack your-app app.asar

例如：asar pack F:\atom_project\myatom_1 F:\atom_project\app.asar
这样就会把myatom_1打包成app.asar

[electron之Windows下使用html,js,css,开发桌面应用程序_app.rar](http://files.cnblogs.com/files/yhcao/electron%E4%B9%8BWindows%E4%B8%8B%E4%BD%BF%E7%94%A8html%2Cjs%2Ccss%2C%E5%BC%80%E5%8F%91%E6%A1%8C%E9%9D%A2%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F_app.rar)

# 5.更改Electron名称 #
你可以将 electron.exe 改成任意你喜欢的名字，然后可以使用像 [rcedit](https://github.com/atom/rcedit) 或者[ResEdit](http://www.resedit.net/) 编辑它的icon和其他信息。

## ResEdit: ##

[http://www.cr173.com/soft/12721.html](http://www.cr173.com/soft/12721.html)


