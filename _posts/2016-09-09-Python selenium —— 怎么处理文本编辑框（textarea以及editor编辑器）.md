---
layout: post
title:  "Python selenium —— 怎么处理文本编辑框（textarea以及editor编辑器）"
date:   2016-09-09 15:50:15
categories: selenium
permalink: /archivers/input-textarea-editor
---


在群里经常会遇到有人问文本框的处理，今天有时间，便写一点这方面的经验。

一般输入框有三种：

1. 短的`input`框，如下：

	![input](http://img.blog.csdn.net/20160831153441946)

		<input id="zenInput2" class="zenInputDemo" type="text" style="position: static;">

2. `textarea`框，如下：

	![textarea](http://img.blog.csdn.net/20160831153559170)

		<textarea id="message1" name="message1"></textarea>

3. `div`式的`editor`框，如下：
	![editor1](http://img.blog.csdn.net/20160831154341946)

	代码见**[网页源码](http://www.sucaijiayuan.com/api/demo.php?url=/demo/%E5%9F%BA%E4%BA%8Ebootstrap%E7%9A%84%E8%BD%BB%E9%87%8F%E7%BA%A7jQuery%E6%96%87%E6%9C%AC%E7%BC%96%E8%BE%91%E5%99%A8%E6%8F%92%E4%BB%B6%20LineControl/index.html)**

4. 也可能是更复杂的`iframe`的`editor`，如下：
	![editor2](http://img.blog.csdn.net/20160831154801139)

	代码见**[网页源码](http://ueditor.baidu.com/website/examples/completeDemo.html)**

下面依次看看这几种输入框该怎么解决：

## **1. input**

其实这个只是列在这里，input该如何处理，我想懂点selenium的都知道怎么办。

## **2.textarea**

很简单，定位到元素，直接`send_keys`就行。

> 示例网址:[http://www.sucaijiayuan.com/api/demo.php?url=/demo/20150325-1](http://www.sucaijiayuan.com/api/demo.php?url=/demo/20150325-1)

代码：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver
from time import sleep

driver = webdriver.Firefox()
driver.get('http://www.sucaijiayuan.com/api/demo.php?url=/demo/20150325-1')
driver.maximize_window()

driver.switch_to.frame('iframe')

driver.find_element_by_id('message1').send_keys('Hello world!')  # 很简单，直接send_keys就行
sleep(2)

print driver.find_element_by_id('message1').get_attribute('value')

driver.quit()
```

结果：

```
Hello world!
```

## **3.div式的editor**

这种一样，定位到元素`div`，直接`send_keys`就行，不过这个`send_keys`不是到了`value`属性中，而是在`text`中。

> 示例网址:[http://www.sucaijiayuan.com/api/demo.php?url=/demo/基于bootstrap的轻量级jQuery文本编辑器插件%20LineControl/index.html](http://www.sucaijiayuan.com/api/demo.php?url=/demo/%E5%9F%BA%E4%BA%8Ebootstrap%E7%9A%84%E8%BD%BB%E9%87%8F%E7%BA%A7jQuery%E6%96%87%E6%9C%AC%E7%BC%96%E8%BE%91%E5%99%A8%E6%8F%92%E4%BB%B6%20LineControl/index.html)

代码：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver
from time import sleep


driver = webdriver.Firefox()
driver.get('http://www.sucaijiayuan.com/api/demo.php?url=/demo/%E5%9F%BA%E4%BA%8Ebootstrap%E7%9A%84%E8%BD%BB%E9%87%8F%E7%BA%A7jQuery%E6%96%87%E6%9C%AC%E7%BC%96%E8%BE%91%E5%99%A8%E6%8F%92%E4%BB%B6%20LineControl/index.html')
driver.maximize_window()

driver.switch_to.frame('iframe')

driver.find_element_by_class_name('Editor-editor').send_keys('Hello world again!')  # 没什么区别，也是直接send_keys
sleep(2)

print driver.find_element_by_class_name('Editor-editor').text

driver.quit()
```

结果：

```
Hello world again!
```

## **4.iframe中的editor**

这种是最复杂的一种，但要搞明白了，其实也很简单。

> 示例网址：[http://ueditor.baidu.com/website/examples/completeDemo.html](http://ueditor.baidu.com/website/examples/completeDemo.html)

代码：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver

driver = webdriver.Chrome(executable_path='D:\py\AutoTestFramework\drivers\chromedriver.exe')
driver.get('http://ueditor.baidu.com/website/examples/completeDemo.html')

driver.switch_to.frame('ueditor_0')  # 注意，这种editor一定有frame，一定要切frame

body_string = """Hello world again again!
Hello world again again!
Hello world again again!

Hello world again again!"""

driver.find_element_by_tag_name('body').send_keys(body_string)  # 直接往frame里的body里填内容，是不是很简单粗暴
print driver.find_element_by_tag_name('body').text
driver.quit()
```

结果：

```
Hello world again again!
```

其实`frame editor`的内容一般都是写在里面的`<body>`里，最重要的就是切到`frame`中去，关于`frame`的定位于switch，见我的博客：
**[Python selenium —— 深刻解析及操作frame、iframe ](https://huilansame.github.io/huilansame.github.io/archivers/switch-to-frame)**

frame中一般是一个空的html，其中显示的内容即是body中的内容。

> 关于输入框、富文本框、editor编辑器的处理，大概就这些。如果有什么问题或者特殊的情况，可以在博客评论中给我留言。

****

> 更多关于python selenium的文章，请关注我的CSDN专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**
