---
layout: post
title:  "Python selenium —— 教你在Windows上搭建Python+Selenium环境"
date:   2016-10-21 17:00:13
categories: selenium
permalink: /archivers/teach-you-build-python-selenium-environment
---

发现很多人连环境都不会搭，虽然这个问题没有什么技术含量，但博主也决定写点东西给那些环境都不会搭建的小白。

关于selenium是什么的问题博主实在是懒得解释，直接上环境，小白学习一般需要以下一些东西：

1. 浏览器（Firefox/Chrome/IE..）
2. Python
3. Selenium
4. Selenium IDE（如果用Firefox）
5. FireBug、FirePath（如果用Firefox）
6. chromedriver、IEDriverServer
7. IDE（Pycharm/Sublime/Eclipse..）

接下来我们就一步步进行讲解：

## **1. 浏览器**

要搞selenium，浏览器是起码的，那么要选择哪个浏览器？选择哪个版本呢？博主建议用Firefox或Chrome，**千万不要用最新版本**，要用早两到三个版本的。

Firefox早期版本的下载，可以通过下面的链接：

> [http://ftp.mozilla.org/pub/firefox/releases/](http://ftp.mozilla.org/pub/firefox/releases/)

Chrome早期版本的下载，可以通过下面的链接（不过不是官方的版本，目前很难找到官方早期安装包）：

> [http://www.slimjet.com/chrome/google-chrome-old-version.php](http://www.slimjet.com/chrome/google-chrome-old-version.php)

浏览器的安装没有必要细说，但一定要注意安装完成之后关闭浏览器的自动更新功能。

Firefox 需通过“选项 - 高级 - 更新 - Firefox更新”修改为“不检查更新”，如图：

![firefox关闭更新](http://img.blog.csdn.net/20161021164114686)

Chrome 关闭更新可通过关闭服务中Google的两个更新服务（如下图），或者修改注册表 `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\Update\AutoUpdateCheckPeriodMinutes` 值为 `0`

![chrome关闭更新](http://img.blog.csdn.net/20161021164756564)

## **2. Python**

Windows下安装Python很简单，官网下包直接装就行，这里要建议一下，建议用`Python 2.7.x(10-12)`的版本，个人不建议用3，也不要用过低的版本。

> [Python下载传送门](https://www.python.org/ftp/python/) （去[官网](https://www.python.org/downloads/)下也可）

找好版本，下其中的`.msi`文件，64位系统下`.amd64.msi`，如：

![Python download](http://img.blog.csdn.net/20161021165616739)

...

## **3. Selenium**

Python装selenium很简单，直接pip就可以

    pip install selenium

默认装的就是最新的2.xx版本的selenium，这里也要建议一下，建议用`selenium 2.xx(53+)`版本，不要用`selenium 3.xx`，3.xx版本目前有一些功能还未稳定。

你也可以下载Python-selenium的包来安装

...

## **4. Selenium IDE**

如果想要学习Selenium IDE来录制回放，需要下载相应Firefox插件，可以到selenium官网：

> [http://seleniumhq.org/download/](http://seleniumhq.org/download/)

或者在Firefox里输入下面地址：

> [https://addons.mozilla.org/zh-CN/firefox/addon/selenium-ide/?src=search](https://addons.mozilla.org/zh-CN/firefox/addon/selenium-ide/?src=search)

也可以直接在Firefox的附加组件管理器中直接搜索`selenium ide`，直接搜索结果里应该是搜不到的，你拉到最下方，点开“查看全部的xx项结果”，在其中找到 `Selenium IDE` 项，如下图，这里需要注意，结果中有很多迷惑性的插件，如`Selenium IDE Button`（这个仅仅是一个浏览器按钮，而不是真正的IDE）等，要注意甄别。

![Selenium IDE](http://img.blog.csdn.net/20161021171955172)

`Selenium IDE`直接拖拽到Firefox中就可以安装，或者直接通过“添加到Firefox”链接添加。

安装完成之后就可在Firefox中找到一个`Se`的图标，如下图：

![ide button](http://img.blog.csdn.net/20161021172325909)

## **5. FireBug、FirePath**

如果要使用Firefox，必备的插件就是FireBug和FirePath，这俩都可以在附加组件管理器中搜到，如下图：

FireBug：

![FireBug](http://img.blog.csdn.net/20161021172114438)

FirePath：

![FirePath](http://img.blog.csdn.net/20161021172151158)

安装完成之后就能在Firefox找到一个虫子的图标，如下图：

![firebug button](http://img.blog.csdn.net/20161021172424394)

而FirePath在FireBug中，如下：

![firepath](http://img.blog.csdn.net/20161021172534651)

## **6. chromedriver、IEDriverServer**

如果需要使用Chrome浏览器或者IE浏览器，则需要对应的驱动，下载链接如下：

chromedriver，chromedriver没有64位版本，32即可驱动：

> [http://chromedriver.storage.googleapis.com/index.html](http://chromedriver.storage.googleapis.com/index.html)

IEDriverServer，下面链接能够下载所有版本的selenium以及IEDriverServer，IEDriverServer区分32位/64位：

> [http://selenium-release.storage.googleapis.com/index.html](http://selenium-release.storage.googleapis.com/index.html)

选择合适的版本并下载即可。

找个容易找到的文件夹放起来，在启动chrome浏览器以及IE时需要用到。

## **7. IDE（Pycharm/Sublime/...）**

...
