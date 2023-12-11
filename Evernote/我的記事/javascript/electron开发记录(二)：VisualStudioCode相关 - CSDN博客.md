# electron开发记录(二)：VisualStudioCode相关 - CSDN博客

CSDN新首页上线啦，邀请你来立即体验！
[[#|立即体验]]

* <http://www.csdn.net/?ref=toolbar>

* [博客](http://blog.csdn.net/?ref=toolbar)
* [学院](http://edu.csdn.net/?ref=toolbar)
* [下载](http://download.csdn.net/?ref=toolbar)
* [[#|更多__]]
.

* <http://blog.csdn.net/u014595019/article/details/53401303#>

	[[#|__]]
	
* [__](http://write.blog.csdn.net/postedit?ref=toolbar)
* [__](http://gitbook.cn/new/gitchat/activity?utm_source=csdnblog1)
* [登录](https://passport.csdn.net/account/login?ref=toolbar)[注册](http://passport.csdn.net/account/mobileregister?ref=toolbar&action=mobileRegister)

# electron开发记录(二)：VisualStudioCode相关

原创 2016年11月29日 22:29:09

* 标签：

* [nodejs](http://so.csdn.net/so/search/s.do?q=nodejs&t=blog) /
* [electron](http://so.csdn.net/so/search/s.do?q=electron&t=blog) /
* [vscode](http://so.csdn.net/so/search/s.do?q=vscode&t=blog)
.

.

* * *

这篇文章主要写的是跟vscode有关的内容.毕竟这次是打算拿vscode来开发应用的

**主要内容:**
1.vscode的安装
2.使用vscode运行electron的demo
3.安装typings开启智能提示功能
4.vs的快捷键和其他设置

* * *

# 1.VSCode安装

VSCode的安装比较简单，去官网[https://code.visualstudio.com](https://code.visualstudio.com/) 下载deb包，然后

    $ sudo dpkg -i code_1.7.2-1479766213_amd64.deb

来进行安装. 注意下deb包的名字应该是你自己下载得到的deb文件的名字.
如果碰到dpkg安装失败的(我是没碰到,不过用dpkg安装软件有时候会出现这种情况),可能是因为缺少依赖包,可以尝试一下

    $ sudo apt-get -f install

安装一下缺失的依赖包,然后再运行上面的代码

vscode安装成功以后,可以在shell中通过`code`指令来打开vscode.

* * *

# 2.使用vscode运行electron

在之前的内容中已经实现了在shell中运行electron的demo.现在我们想办法能够在vscode中将electron程序运行起来.还是拿之前下好的demo工程为例.找到demo工程所在的文件夹,使用vscode打开文件夹

    $ code electron-quick-start

首先来看看vscode的侧边栏
![[选区_048.png]]
这边有5个按钮，一般箭头指的3个是比较常用的。
**文件管理**用来管理文件，比如新建，删除等。
**git操作**用来进行git的各项操作，可以将文件提交到本地，还可以对比查看
**debug**则用来运行、调试程序。我们现在就是要用debug功能。

按ctrl shift d或者点击左侧蜘蛛状按钮【1】，跳转到调试界面，再点击上面的齿轮按钮，打开launch.json【2】

![[选区_045.png]]

之后将如下launch.json文件中的configuration选项改成如下形式

    "configurations": [
            {
                "name": "Debug Main Process",
                "type": "node",
                "request": "launch",
                "program": "${workspaceRoot}/main.js",
                "cwd": "${workspaceRoot}",
                "runtimeExecutable": "${workspaceRoot}/node_modules/.bin/electron"
            }
        ]

其实主要是添加一条runtimeExecutable的参数，告诉vscode应该通过这个文件来运行。
保存后，点击上图中的绿色三角形，就能运行啦

* * *

# 3.安装typings开启智能提示功能

由于nodejs是弱类型语言，所以vscode自己是无法完成代码的智能提示的。最多只能根据当前文件中已经出现的方法来提示。但是这样又容易引起混淆。为了解决这个问题，我们需要安装typings。

typings全称是TypeScript Definition Manager(虽然我怎么看都觉得缩写跟全称不符)，用来取代tsd。
我是参照这里的：<http://www.cnblogs.com/IPrograming/p/VsCodeTypings.html>
操作流程如下：
假设你现在正在项目的根目录下。也就是当前目录下有node\_modules文件夹,package.json这些。然后输入如下代码

    npm install -g typings //　全局安装typings
    typings install dt~node --global --save　//　安装node的类型接口信息文件。其中dt表示源，node表示名称
    typings install dt~electron --global --save // 安装electron接口信息。

关于什么时候要加–global参数：

* 如果安装的包使用script标记来引用(如jQuery)(也就是在浏览器中使用)
* 这个包是属于环境的一部分(如node)时
* 该包没有使用 –global 安装失败时（或者提示你要加global参数时）

安装好以后，可以看到在根目录下会多出两样东西：typings.json文件和typings文件夹。前者类似于package.json，后者类似于node\_modules.　即typings.json是一个索引。多人协作时，只要获得typings.json，然后使用`typings install`就能够生成typings文件夹。此外，还可以使用`typings search packname`来看是否支持对某个包的智能提示

接下来，还需要开启vscode的智能提示功能。方法有两种：

1. 是在需要进行只能提示的文件最上行增加提示信息文件所在目录，格式如下:
	`/// <reference path="./typings/index.d.ts" />`
2. 在项目的根目录增加一个名为jsconfig.json的空文件,往其中写入：

    {
        "compilerOptions": {
            "target": "ES6"
        },
        "exclude": [
            "node_modules"
        ]
    }

更多jsconfig.json文件的内容可以参考：<https://code.visualstudio.com/docs/languages/javascript>

* * *

# 4.vscode的快捷键和其他设置

文件->首选项->更换主题 改变配色
ctrl+/ 注释当前行
ctrl+d 选中当前光标所在单词
ctrl+shift+k 删除当前行
ctrl+shift+i reformat代码
shift+alt+up/down 选中多行
shift+alt+鼠标拖动 选中一整块

## **个人体验**

优点：
1\. 可以直接在界面上进行git部分操作,可以存到本地。提交到github的话，还是要在shell中进行
2\. 可以直接运行node程序，这个点赞
3\. VS风格界面，很喜欢

缺点：
1\. 比之sublime，打开速度稍稍有些慢
2\. 写html的时候，自动补全不够智能(跟idea相比)
3\. 写nodejs补全不够智能，不过可以安装typings来弥补

版权声明：本文为博主原创文章，未经博主允许不得转载。

__目前您尚未登录，请 [登录](https://passport.csdn.net/account/login?from=http%3A%2F%2Fblog.csdn.net%2Fu014595019%2Farticle%2Fdetails%2F53401303%23t2) 或 [注册](http://passport.csdn.net/account/register?from=http%3A%2F%2Fblog.csdn.net%2Fu014595019%2Farticle%2Fdetails%2F53401303%23t2) 后进行评论

.

### 相关文章推荐

## [electron开发记录(一)：安装nodejs并运行demo](http://blog.csdn.net/u014595019/article/details/53349505)

一些个人的唠叨(着急的人可以跳过)很久之前就想写图形化界面应用了，正巧最近又有一项任务需要做这个，正好实现之前的愿望。挑挑拣拣的，看了看wpf，好像通用性不够啊，还不能跨平台；再看看qt，太丑了；看来...

* [![[2_u014595019.png]]](http://blog.csdn.net/u014595019)

* [u014595019](http://blog.csdn.net/u014595019)
* 2016-11-26 11:14
* __3624
.

## [electron开发记录(四)：electron中组件的一些用法](http://blog.csdn.net/u014595019/article/details/53516584)

之前讲了electron应用的基本框架和结构。现在来讲讲其中一些组件(BrowserWindow，Menu等)的一些用法。这些方法我是从electron-api-demo 以及electron官网上的...

* [![[2_u014595019.png]]](http://blog.csdn.net/u014595019)
* [u014595019](http://blog.csdn.net/u014595019)
* 2016-12-08 12:06
* __2567

.

## [electron开发记录(三)：应用基本框架解析](http://blog.csdn.net/u014595019/article/details/53436296)

这篇文章主要讲了electron应用的基本架构，并对之前下载的应用进行分析electron应用的基本架构在electron中，主要有两类进程。一类是主进程main，还有一类是渲染器进程renderer...

* [![[2_u014595019.png]]](http://blog.csdn.net/u014595019)
* [u014595019](http://blog.csdn.net/u014595019)
* 2016-12-02 16:44
* __2813

.

## [electron开发记录(五)：调用jquery,bootstrap并对其一些用法的总结](http://blog.csdn.net/u014595019/article/details/53610731)

本文主要分两个部分，分别是在electron中调用jquery和bootstrap的方法以及为什么要这么调用，以及对jquery一些用法的总结。electron中调用jquery及bootstrap的...

* [![[2_u014595019.png]]](http://blog.csdn.net/u014595019)
* [u014595019](http://blog.csdn.net/u014595019)
* 2016-12-13 16:05
* __3101

.

## [Electron实战：创建ELectron开发的window应用安装包](http://blog.csdn.net/w342916053/article/details/51323634)

前言：研究electron自动更新的时候，在electron的官方文档auto-updater 中，提到了在几个平台mac，linux，windows下electron 的自动更新方法，其中winds...

* [![[2_w342916053.png]]](http://blog.csdn.net/w342916053)
* [w342916053](http://blog.csdn.net/w342916053)
* 2016-05-05 15:51
* __18664

.

## [Angular+Electron+VSCode的桌面应用开发入门笔记（1）](http://blog.csdn.net/yr7942793/article/details/50986696)

第一部分 Electron开发入门笔记 GitHub 的 Electron 框架（以前叫做 Atom Shell）允许你使用 HTML, CSS 和 JavaScript 编写跨平台的桌面应用。它...

* [![2_yr7942793.jpg](http://avatar.csdn.net/A/0/B/2_yr7942793.jpg)](http://blog.csdn.net/yr7942793)
* [yr7942793](http://blog.csdn.net/yr7942793)
* 2016-03-26 14:22
* __9725

.

## [Electron-使用Electron开发第一个应用](http://blog.csdn.net/flyfish1986/article/details/50076701)

使用Electron开发第一个应用Electron 应用的目录结构如下：app/ ├── package.json ├── main.js └── index.html新建一个app文件夹 将这...

* [![2_flyfish1986.jpg](http://avatar.csdn.net/6/B/7/2_flyfish1986.jpg)](http://blog.csdn.net/flyfish1986)
* [flyfish1986](http://blog.csdn.net/flyfish1986)
* 2015-11-27 23:33
* __13952

.

## [【Electron】Electron开发入门（一）：开发环境搭建](http://blog.csdn.net/arvin0/article/details/52576427)

刚接触Electron+js开发PC端桌面应用程序的时候，简直一头雾水，搜了网上很多教程，有的要么讲的零零碎碎，要么就是版本太低，很多API语法都不能用了；现在我把一些有用的教程归纳一下，并把目前最新...

* [![[2_arvin0.png]]](http://blog.csdn.net/arvin0)
* [arvin0](http://blog.csdn.net/arvin0)
* 2016-09-18 17:41
* __6076

.

## [使用 AngularJS 和 Electron 构建桌面应用](http://blog.csdn.net/zhangzq86/article/details/73064884)

使用 AngularJS 和 Electron 构建桌面应用 GitHub 的 Electron 框架（以前叫做 Atom Shell）允许你使用 HTML, CSS 和 Java...

* [![2_i7thtool.jpg](http://avatar.csdn.net/8/D/D/2_i7thtool.jpg)](http://blog.csdn.net/i7thTool)
* [i7thTool](http://blog.csdn.net/i7thTool)
* 2017-06-11 19:53
* __547

.

## [Electron+React+Webpack+Vscode应用桌面开发平台搭建](http://blog.csdn.net/mingzznet/article/details/53510985)

https://my.oschina.net/xpbug/blog/637864 前提 先安装nodejs, NPM, vscode，electron 源代码 https://github.co...

* [![2_omingzi12345678.jpg](http://avatar.csdn.net/2/F/3/2_omingzi12345678.jpg)](http://blog.csdn.net/oMingZi12345678)
* [oMingZi12345678](http://blog.csdn.net/oMingZi12345678)
* 2016-12-07 22:00
* __2567

.

## [visualstudiocode 调试electron](http://blog.csdn.net/weishenhong/article/details/52074990)

1\. 相关链接 国外的人写的一篇博客：http://floatincode.net/post/debugging-electron-application-with-visual-studio-co...

* [![2_weishenhong.jpg](http://avatar.csdn.net/B/D/0/2_weishenhong.jpg)](http://blog.csdn.net/weishenhong)
* [weishenhong](http://blog.csdn.net/weishenhong)
* 2016-07-30 20:56
* __1304

.

<http://download.csdn.net/detail/bubifengyun/9551475>

## [AngularJS,Electron桌面开发](http://download.csdn.net/detail/bubifengyun/9551475)

* 2016-06-16 15:43

* 2.09MB
* [下载](http://download.csdn.net/detail/bubifengyun/9551475)
.

<http://download.csdn.net/detail/aurevoir1993/9858655>

## [electron开发-音乐播放器](http://download.csdn.net/detail/aurevoir1993/9858655)

* 2017-06-02 14:32

* 1010KB
* [下载](http://download.csdn.net/detail/aurevoir1993/9858655)
.

## [搭建electron开发环境](http://blog.csdn.net/bestfsq/article/details/65447288)

第一步、因为elctron需要使用npm安装，所以首先需要安装Node.js。 第二步、新建一个空文件夹，当作应用的根目录，给文件夹起一个名字，这里为app 第三步、点击上图窗口左上角“文件”，选...

* [![2_bestfsq.jpg](http://avatar.csdn.net/0/C/F/2_bestfsq.jpg)](http://blog.csdn.net/BestFSQ)
* [BestFSQ](http://blog.csdn.net/BestFSQ)
* 2017-03-23 19:43
* __1270

.

## [Electron + React + Node.js + ES6 开发桌面软件](http://blog.csdn.net/arnozhang12/article/details/51735815)

介绍了如何使用 Electron + React + Node.js + ES6 来开发桌面软件。

* [![2_zhangyafengcpp.jpg](http://avatar.csdn.net/9/1/D/2_zhangyafengcpp.jpg)](http://blog.csdn.net/ZhangYafengCPP)
* [ZhangYafengCPP](http://blog.csdn.net/ZhangYafengCPP)
* 2016-06-22 17:21
* __18444

.

## [Vue 全家桶 + Electron 开发的一个跨三端的应用](http://blog.csdn.net/qq_30513483/article/details/73603396)

GitHub Repo：vue-objccn Follow: halfrost · GitHub 项目地址：https://github.com/halfrost/vue-objccn...

* [![2_qq_30513483.jpg](http://avatar.csdn.net/3/F/4/2_qq_30513483.jpg)](http://blog.csdn.net/qq_30513483)
* [qq_30513483](http://blog.csdn.net/qq_30513483)
* 2017-06-22 13:41
* __401

.

## [一站式解决Electron框架模式的客户端软件开发模式](http://blog.csdn.net/jiyulonely/article/details/77031089)

\==引言==：Electron是目前非常流行的一种开发框架，它可以让你使用纯JS调用丰富的原生APIs来创造桌面应用。你可以把它看作是专注于桌面应用而不是web服务器。这不意味着 Electron是绑...

* [![2_jiyulonely.jpg](http://avatar.csdn.net/7/E/6/2_jiyulonely.jpg)](http://blog.csdn.net/jiyulonely)
* [jiyulonely](http://blog.csdn.net/jiyulonely)
* 2017-08-10 09:38
* __167

.

## [GitHub 发布了 Electron 1.0 版本，帮助开发者打造更强大的原生应用](http://blog.csdn.net/lewis_007/article/details/51544356)

在过去的两年里 Electron 已经帮助开发者构建了大量基于 HTML、CSS 和 JavaScript 的跨平台应用。如今 GitHub 发布了 Electron 1.0 版本。Electron ...

* [![2_lewis_007.jpg](http://avatar.csdn.net/D/2/A/2_lewis_007.jpg)](http://blog.csdn.net/lewis_007)
* [lewis_007](http://blog.csdn.net/lewis_007)
* 2016-05-31 11:34
* __269

.

## [javascript 开发多种类型的应用（Electron 跨平台开发）](http://blog.csdn.net/ejinxian/article/details/51179017)

Electron + Node.js + Javascript 做桌面应用  Ionic + JavaScript 做移动应用  Node.js + JavaScript 网站前后台  Javascr...

* [![2_ejinxian.jpg](http://avatar.csdn.net/2/9/1/2_ejinxian.jpg)](http://blog.csdn.net/ejinxian)
* [ejinxian](http://blog.csdn.net/ejinxian)
* 2016-04-18 13:48
* __1533

.

## [electron折腾记（二）——关于窗口](http://blog.csdn.net/sinat_25127047/article/details/51418682)

这里讲一些关于electron中窗口的设置，主要是对原API的解读创建一个新的窗口const BrowserWindow = require('electron').remote.BrowserWin...

* [![2_sinat_25127047.jpg](http://avatar.csdn.net/5/3/E/2_sinat_25127047.jpg)](http://blog.csdn.net/sinat_25127047)
* [sinat_25127047](http://blog.csdn.net/sinat_25127047)
* 2016-05-15 20:11
* __7659

.
.

[![[1_u014595019.png]]](http://blog.csdn.net/u014595019)

### [multiangle](http://blog.csdn.net/u014595019)

.

原创
87

粉丝
472

喜欢
0

码云
[未开通](https://gitee.com/?utm_source=csdn_blog)

.

### 他的最新文章

[更多文章](http://blog.csdn.net/u014595019) .

* [Jacobian矩阵，Hessian矩阵和牛顿法](http://blog.csdn.net/u014595019/article/details/58588383) .

* [tensorflow笔记:使用tf来实现word2vec](http://blog.csdn.net/u014595019/article/details/54093161) .
* [tensorflow笔记：模型的保存与训练过程可视化](http://blog.csdn.net/u014595019/article/details/53912710) .
* [ubuntu16.04下安装CUDA，cuDNN及tensorflow-gpu版本过程](http://blog.csdn.net/u014595019/article/details/53732015) .
.

### 博主专栏

	[![[./_resources/electron开发记录(二)：VisualStudioCode相关_-_CSDN博客.resources/20161111195652093.jpg]]
	15](http://blog.csdn.net/column/details/13461.html)
	
	#### [自然语言处理](http://blog.csdn.net/column/details/13461.html)
	
	__101860
	
	.

	[![[./_resources/electron开发记录(二)：VisualStudioCode相关_-_CSDN博客.resources/20161111195821919.png]]
	13](http://blog.csdn.net/column/details/13414.html)
	
	#### [深度学习&tensorflow笔记](http://blog.csdn.net/column/details/13414.html)
	
	__226220
	
	.

### _在线课程_

* [![[201711020956065261.png]]](http://edu.csdn.net/huiyiCourse/detail/594?utm_source=blog9)
	
	[C语言大型软件设计的面向对象](http://edu.csdn.net/huiyiCourse/detail/594?utm_source=blog9)
	
	讲师：宋宝华
	
* [![[201711020954527574.png]]](http://edu.csdn.net/huiyiCourse/detail/596?utm_source=blog9)
	
	[【免费直播】神经网络的原理及结构设计](http://edu.csdn.net/huiyiCourse/detail/596?utm_source=blog9)
	
	讲师：何宇健
	
.

### 热门文章

* [tensorflow笔记：多层LSTM代码分析](http://blog.csdn.net/u014595019/article/details/52759104)
	__48336
	
* [tensorflow笔记 ：常用函数说明](http://blog.csdn.net/u014595019/article/details/52805444)
	__35596
	
* [自己动手写word2vec (四):CBOW和skip-gram模型](http://blog.csdn.net/u014595019/article/details/51943428)
	__30069
	
* [tensorflow笔记：流程，概念和简单代码注释](http://blog.csdn.net/u014595019/article/details/52677412)
	__26840
	
* [深度学习笔记(三)：激活函数和损失函数](http://blog.csdn.net/u014595019/article/details/52562159)
	__19500
	

.
