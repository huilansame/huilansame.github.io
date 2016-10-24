---
layout: post
title:  "[译]Selenium Webdriver - 下载、安装稳定版本"
date:   2016-10-24 11:00:13
categories: selenium
permalink: /archivers/selenium-download-and-install-stable-versions
---

> 译自techbeamers，**[原文链接](http://www.techbeamers.com/selenium-webdriver-download-install/)**

****

> Selenium Webdriver正在持续地改进它的特性。最近，在Selenium Webdriver 3.0的官方release版本之后，我们又看到一些它的beta版本。
>
> 原因在于这个产品正在经历一个根本性的转变。所以每个使用Selenium的自动化测试工程师都应该关注它新版本的改变。

这就是我们写这篇文章的原因。在这里，我们向你提供最新的，最稳定的Selenium Webdriver的下载链接，并分享它的新特性。

下面，我们还有Selenium Webdriver的核心组件，如Standalone Webdriver以及浏览器驱动（如Firefox、Chrome、IE），以便于你专注于你的与自动化开发相关的模块。

伴随着每一个Selenium Webdriver下载链接，我们附上了一份重要特性表格，描述该模块的目的以便于你下载。

除了以上的东西，你能再我们的博客上发现一些非常好的Selenium WebDriver教程，可以帮助你安装、配置、并用Eclipse和Maven创建自动化测试项目。（PS：博主也有许多不错的Python + Selenium的博客）

但开始之前，让我们先看一下这个伟大的自动化工具的历史性事件：

1. **Jason Huggins**在**2004**年开始研发Selenium核心。
2. **Selenium RC**在**2006**年加入了进来。
3. **Selenium 2.0**（Webdriver支持）在**2011年替代了RC**。
4. **Selenium 3.0**在**2016年10月**出生。

## **Selenium Webdriver 下载 —— 官方发布**

在经历了四个beta版本之后，我们最终看到了Selenium 3.0的官方发布版。开发者在最新的Selenium Webdriver上做了很多了不起的工作。

所以我们从最新的发布版本Selenium Webdriver 3.0开始。在这个版本中有很多新特性，主要集中在把核心API跟客户端driver实现分离开。

下面，我们大概涵盖了Selenium Webdriver 3.0的主要变化。

### **Selenium 3.0 的新特性**

1. Selenium Webdriver核心API将作为一个接口，浏览器厂商将独立提供客户端驱动程序。

    甚至Firefox现在也有了GECKO驱动来实现Webdriver 3.0 API。GECKO驱动遵从W3C Webdriver spec文档。你可以在[这里](https://www.w3.org/TR/webdriver/)看到最新的API说明。

2. 支持Safari，通过Apple的Safari驱动。

    对于IE，Selenium使用Edge驱动。

3. 一些其他的改变如下：

    - 最低JRE版本要求8.0
    - 支持的IE版本>=9.0
    - 重新支持Firefox47.0.1以及早期版本。
        + 对于更新版本的Firefox，使用GECKO驱动。

在[change logs](https://raw.githubusercontent.com/SeleniumHQ/selenium/master/java/CHANGELOG)中你能看到一些其他的改变。

![selenium-webdriver-download](http://img.blog.csdn.net/20161024114343947)

### **Selenium Webdriver下载链接**

| 模块名称 | 模块描述 | Selenium Webdriver下载链接 |
| -- | -- | -- |
| **1. Selenium Standalone Server 3.0** | 这是Selenium Webdriver的最新稳定版本。你要执行remote Selenium Webdriver时需要它<br>同时，注意Selenium 3.0+不再支持RC API。你应该用一个[备用接口](http://www.seleniumhq.org/docs/appendix_migrating_from_rc_to_webdriver.jsp)来启动那些旧的东西 | **[Selenium Webdriver 3.0下载](https://goo.gl/Lyo36k)**（稳定版本） |
| **2. Selenium Java 包（3.0.1）**<br>**3. Selenium Python 包（3.0.0）** | 这些包包括了一系列的扩展Selenium功能的库 | **[Selenium Java 包 3.0.1](http://selenium-release.storage.googleapis.com/3.0/selenium-java-3.0.1.zip)**（稳定版本）<br>**[Selenium Python 包 3.0.0](http://pypi.python.org/pypi/selenium)**（稳定版本） |
| **4. IE Server Driver（2.53.1）** | 如果你想要启动IE来做网页测试，你必须有这两个驱动之一。根据你的系统架构来选择。 | **[32-位 IE Server Driver](http://selenium-release.storage.googleapis.com/2.53/IEDriverServer_Win32_2.53.1.zip)**（稳定版本）<br>**[64-位 IE Server Driver](http://selenium-release.storage.googleapis.com/2.53/IEDriverServer_x64_2.53.1.zip)**（稳定版本） |
| **5. GECKO Driver（最新版）** | 这个驱动是用来支持新版本的Firefox浏览器，从这里下载最新版 | **[Mozilla GECKO Driver](https://github.com/mozilla/geckodriver/releases)**（稳定版本） |
| **6. Google Chrome Driver（最新版）** | 从这里下载最新版本的Google Chrome驱动 | **Google Chrome Driver**（稳定版本）|

## **下载 Selenium 2.0（Webdriver）**

因为很多测试自动化开发者仍然用Selenium Webdriver 2.0，所以提供其相关的下载链接。我们分享最稳定的版本，质量保证专业人员在整个信息技术行业可以用来开发测试自动化的工件。

要下载Selenium Webdriver 2.0包，点击下面的链接

> **[+Selenium Webdriver Download 2.0](http://selenium-release.storage.googleapis.com/index.html?path=2.53/)**

![selenium 2.0 download](http://img.blog.csdn.net/20161024140736798)

## **阅读 Selenium Webdriver 教程**

最后，我们列出了一些非常有用的博客，很多我们的读者读过，对你也同样有用。

> **1. [Setup Your First Selenium Webdriver Project in Eclipse from Scratch.](http://www.techbeamers.com/six-steps-to-setup-selenium-webdriver-project-in-eclipse/)**
>
> **2. [Setup Selenium Webdriver Project Using Maven in 10 Minutes.](http://www.techbeamers.com/create-selenium-webdriver-maven-project/)**
>
> **3. [Setup a Selenium TestNG Project Using Maven and Eclipse.](http://www.techbeamers.com/running-webdriver-tests-using-maven-eclipse/)**
>
> **4. [Download and Setup Selenium IDE for Web Testing.](http://www.techbeamers.com/selenium-ide-download-and-installation-steps/)**


当然，最后博主还得推荐几篇自己的博客给各位品鉴，主要是对于学习Python + Selenium的同学，可以多看看博主的博客（当然用Java的同学也可借鉴）：

> **1. [Python selenium —— 教你在Windows上搭建Python+Selenium环境](http://blog.csdn.net/huilan_same/article/details/52888262)**，这篇博客主要讲了Python Selenium的Windows环境搭建。

> **2. [Python selenium —— webdriver cheat sheet(webdriver备忘单)](http://blog.csdn.net/huilan_same/article/details/52805034)**，这篇博客主要分享了Webdriver备忘单，用来对Webdriver中一些方法的速查，很有用。

> **3. [Python selenium —— XPath and CSS cheat sheet](http://blog.csdn.net/huilan_same/article/details/52806985)**，这篇博客主要分享了XPath和CSS的备忘单，放在手边，可以帮助你迅速找到定位某个元素的方法，极力推荐。

> **4. [Python selenium —— Webdriver Exception cheat sheet](http://blog.csdn.net/huilan_same/article/details/52815047)**，这篇博客主要分享了Webdriver异常备忘单，遇到异常怎么办，速查一下什么原因，方便你针对性解决问题。

****

> 还有很多，请关注博主的CSDN博客专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**

最后，博主最近新建了一个QQ群，用于Python Selenium的技术交流，不讨论其他技术，不讨论其他语言，专注于Python+Selenium的技术交流与分享，感兴趣的同学可以加群**（455478219）**，也可直接点击下面的图片加群：

<a target="_blank" href="http://shang.qq.com/wpa/qunwpa?idkey=90801105d449b59723f93b7c51173840de66567e43dcd78fc58f170698636538"><img border="0" src="http://img.blog.csdn.net/20161020130233897" alt="Python Selenium交流群" title="Python Selenium交流群"></a>

欢迎来跟博主讨论Python Selenium有关的问题。
