---
layout: post
title:  "[译]Selenium —— 怎样使用FireBug和FirePath"
date:   2016-10-25 11:00:13
categories: selenium
permalink: /archivers/how-to-use-firebug-firepath-in-firefox
---

> 译自techbeamers，**[原文链接](http://www.techbeamers.com/use-firebug-and-firepath-in-firefox/)**

****

## **怎样使用FireBug和FirePath寻找定位器**

### **什么是XPATH**

XPath是用于在网页上唯一的识别元素的技术，它就像HTML元素的地址，比如check boxes、text或div等。在Selenium里，我们认为XPath是最值得信赖的定位器之一。关于XPath更多的内容，请阅读 [W3School XPath教程](http://www.w3school.com.cn/xpath/) 。

### **什么是FireBug插件**

Firebug是Firefox浏览器最著名的附加组件。它跟Firefox的结合产生了大量的工具可用于web开发。它可以让你在网页上实时地修改、管理和监控CSS、HTML和JavaScript。

### **为什么FireBug在Selenium自动化中如此有用**

通常情况下，你可以用FireBug插件做四种类型的操作：

1. **显示源代码** - 它可以让你重新查看JavaScript引擎渲染之后的HTML网页。
2. **高亮显示变化** - 它可以检测并高亮（黄色）显示任何HTML页面的改变。这个特性可以立即获取你的注意，以确保你没有错过什么。
3. **快速检查** - FireBug有 `Inspect` 功能，能够让你快速查看你想要的web元素的定位器。
4. **复制HTML** - 你可以轻松地复制HTML网页的代码，或用 `innerHTML` 获取部分网页，或者某个元素的XPath表达式。

### **如何在Firefox安装FireBug**

你可以在Firefox插件商店轻松下载到FireBug插件。

**1-** 菜单选项 `Tools` >> `Web Developer` >> `Get More Tools`（中文： `工具` >> `Web开发者` >> `获取更多工具` ）

**2-** 上述动作将会加载一个网页。你会发现一个选项来下载/安装FireBug插件。你需要点击“添加到Firefox”按钮来启动该进程。

![download firebug](http://img.blog.csdn.net/20161025092153356)

**3-** 当点击添加链接，你会看到下面的弹出窗口。现在，单击“安装”按钮即可完成安装。

![install firebug](http://img.blog.csdn.net/20161025092304451)

**4-** 安装完成后，用 `F12` 快捷键可启动FireBug插件。如下图：

![use firebug](http://img.blog.csdn.net/20161025092428953)

### **学习如何使用FireBug寻找元素定位器**

使用FireBug很简单，只需要按照以下步骤：

**1-** 右键点击任何对象，并按下“用FireBug查看元素”选项，如下图，它会打开一个HTML代码窗口。

**2-** 从代码窗口，再单击右键，选择“复制XPath”选项来获取元素的XPath定位器，或者你可以尝试其他选项。

![copy xpath](http://img.blog.csdn.net/20161025092849491)

### **什么是FirePath附加组件**

这个插件扩展FireBug的功能。它带来能够修改、检查、生产XPath和CSS选择定位器的功能。

### **为什么FirePath在Selenium自动化中如此有用**

**1-** 你可以提供自定义的XPath值，并直接在网页上测试他们的正确性。

**2-** 它像FireBug一样返回你选择的元素的XPath

### **如何在Firefox安装FirePath**

我们上面说过FirePath仅仅是扩展FireBug的能力，所以，你应该在安装FireBug之后再安装FirePath。

**1-** 与安装FireBug的过程相同，只要到 `工具` >> `Web开发者` >> `获取更多工具` 。

**2-** 搜索FirePath插件并点击“添加到Firefox”按钮。

![add firepath](http://img.blog.csdn.net/20161025093715325)

**3-** 点击add选项之后，将打开FirePath安装对话框，你必须点击“安装”按钮完成安装。

![install firepath](http://img.blog.csdn.net/20161025093839029)

**4-** 现在，你可以按 `F12` 快捷键，点击 `FirePath` 选项打开FirePath工具栏。

![use firepath](http://img.blog.csdn.net/20161025094058842)

### **如何使用FirePath找到元素定位器**

FirePath使用起来甚至比FireBug更简单。按照下面的步骤操作：

**1-** 打开FireBug，然后点击 `FirePath` 标签。你可以看到一个XPath编辑框，它指向你选择的web元素。在这里你可以自己写XPath，然后用 `Highlight` 按钮验证它。

![highlight xpath](http://img.blog.csdn.net/20161025094435560)

**2-** FirePath插件很直接地显示XPath，你可以轻松地复制web元素的XPath，并将之用在你的自动化中。

### **动态 - 安装并使用FireBug和FirePath**

现在，是时候总结下你在这篇文章中学到了什么。看看下面的GIF动画，其中包含了插件的安装，以及上面介绍的部分使用流程。

![how to install and use firebug and firepath](http://img.blog.csdn.net/20161025094852721)

### **比较FireBug和FirePath**

两者之间的根本区别在于FireBug返回绝对XPath，但FirePath返回的是相对路径。看看下面的示例。虽然你也可以调整FirePath产生绝对XPath。

```
# 下面示例是FireBug产生的绝对XPath

html/body/div[2]/div/div/aside/div[5]/ul/li[1]/a
```

```
# 下面示例是FirePath生成的相对路径

.//*[@id='category-posts-5']/ul/li[2]/a
```

****

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
