---
layout: post
title:  "Python selenium —— 如何获取当前焦点元素（switch_to.active_element）"
date:   2016-09-08 09:21:15
categories: selenium
permalink: /archivers/switch-to-active-element
---


今天我们讲讲`switch_to`家中的一个异类：`switch_to.active_element`，当然，我们先普及一下其他的家族成员。

## **1.`switch_to`**

selenium做自动化的过程中，经常会遇到`alert`、`frame`和新的`window`，这是经常是`switch_to`家族大展拳脚的时候，先看看`switch_to`家族的成员：

> **alert**  ——返回浏览器的Alert对象，可对浏览器alert、confirm、prompt框操作
> 
> **default_content**()  ——切到主文档
> 
> **frame**(*frame_reference*)  ——切到某个frame
> 
> **parent\_frame**() ——切到父frame，这个方法也不常被人所知，但有多层frame的时候很有用，不过这里要提一句，一般这种嵌套多层的frame都是有问题的，会影响到性能，可以提给开发，让其改进（内部软件除外）
> 
> **window**(*window_name*)  ——切到某个浏览器窗口
> 
> **active_element** ——返回当前焦点的WebElement对象

其中：

- `alert`和`window`的操作在之前的博客**[Python selenium —— 教你分辨alert、window、div模态框，以及操作](https://huilansame.github.io/huilansame.github.io/archivers/switch-to-alert-window-div)**中已经讲到
- 关于`frame`的三个方法也在之前的博客**[Python selenium —— 深刻解析及操作frame、iframe](https://huilansame.github.io/huilansame.github.io/archivers/switch-to-frame)**中已经讲到

唯一没有说到的就是`switch_to`家族中的`active_element`，这个家伙不常被用到，所以也容易被人遗忘，在网上也很少找到关于它的介绍。但是，存在的即是合理的，它也必然有它存在的意义。

在这里需要特别说明一下的是不支持的用法~~`switch_to_alert()`~~，如果你在pycharm或者eclipse上编辑，会发现这样的方法被划上了删除线，意思是告诉你这个用法已经不被支持，今后很有可能被废除，不建议继续使用。所以，如果你不想你的脚本哪天升级后不能运行，那就不要用这种old style了，用`switch_to.alert()`类的方法吧。

## **2.`switch_to.active_element`**

> **switch_to.active_element**返回的是当前**焦点**的对象，即返回WebElement对象。

那么**焦点**是什么？大概可以这样理解：即网页上当前操作的对象（也就是你网页上光标的位置），比如，你鼠标点击到了一个input框，你可以在这个input框里输入信息，这时这个input框即焦点。

说了半天，到底什么时候会需要返回当前的焦点呢？下面这个例子可以更直观一些：

> 今天群里有人遇到这样的问题，一个网页上的新建文件夹的功能，右键-新建之后，在页面上有个输入文件夹名的input框，但这个框一旦失去焦点而且没有内容的话，就会消失、取消掉新建文件夹的操作。如图：

![右键新建](http://img.blog.csdn.net/20160827235644622)

![填写文件夹名](http://img.blog.csdn.net/20160827235718279)

**最初的代码**如下（部分）：

```python
...
l = driver.find_element_by_id('pm_treeRoom_1_span')
ActionChains(driver).context_click(l).perform()
driver.find_element_by_class_name('fnew').click()
time.sleep(2)
driver.find_element_by_xpath('//*[@id="pm_treeRoom_1_ul"]/li[...]').send_keys('filename')
time.sleep(2)
...
```

结果这种操作总会导致输入框失去焦点，直接消失，更不能`send_keys`进去了，直接报错。

我提醒用`ActionChains`的`send_keys`发送，不去重新定位元素，就用默认的焦点元素。**修改后的代码**如下（部分）：

```python
...
driver.find_element_by_class_name('fnew').click()
time.sleep(2)
ActionChains(driver).send_keys('filename')
time.sleep(2)
...
```

结果仍是失败，代码执行成功了。但是光标仍卡在输入框，输入框也没有输入任何信息。

没办法，只好祭出我的大招，用`switch_to.active_element`，看**代码**（部分）：

```python
driver.find_element_by_class_name('fnew').click()
time.sleep(2)
driver.switch_to.active_element.send_keys('filename')
time.sleep(2)
```

结果：

![成功](http://img.blog.csdn.net/20160828001156898)

成功添加上了新的文件夹！

**注意：`active_element`后面没有括号。**

有上面的示例我想大家也大概明白了`active_element`的用法。当你想要获取当前焦点元素时，你就可以用它了。

> **(附)API : Returns the element with focus, or BODY if nothing has focus.**

****


> 更多关于python selenium的文章，请关注我的CSDN专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**

