---
layout: post
title:  "Python selenium —— 分析一个异常（StaleElementReferenceException）"
date:   2016-09-09 18:50:15
categories: selenium
permalink: /archivers/exceptions-StaleElementReferenceException
---



今天给大家分享一个selenium中经常会有人遇到的坑：

> **selenium.common.exceptions.StaleElementReferenceException: Message: Element not found in the cache - perhaps the page has changed since it was looked up**

群里经常会有人问，“我循环去点击一列链接，但是只能点到第一个，第二个就失败了，为什么？”。原因就在这里：**你点击第二个时已经是新页面，当然找不到之前页面的元素**。这时，他会问“可是明明元素就在那里，没有变，甚至我是回退回来的，页面都没有变，怎么会说是新页面？”。这个就需要你明白页面长得一样不代表就是同一张页面，就像两个人长得一样不一定是同一个人，他们的身份证号不同。**页面，甚至页面上的元素都是有自己的身份证号（id）的。**

我们来试试看：

代码：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver

driver = webdriver.Firefox()
driver.get('http://www.baidu.com')

print driver.find_element_by_id('kw')  # kw before refresh

driver.refresh()  # refresh

print driver.find_element_by_id('kw')  # kw after refresh

driver.quit()
```

结果：

```
<selenium.webdriver.remote.webelement.WebElement (session="6c251157-6d81-435c-9100-97696a46ab9c", element="{f74ae41d-a557-4d5c-9029-3c122e3d0744}")>
<selenium.webdriver.remote.webelement.WebElement (session="6c251157-6d81-435c-9100-97696a46ab9c", element="{d7bd4320-31f2-4708-824f-f1a8dba3e79b}")>
```

我们发现，仅仅是刷新了一下页面，两次的element id是不同的，也就是说**这是两个不同的元素**，如果你用以下的方式来定位，自然会因为找不到而报错：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver

driver = webdriver.Firefox()
driver.get('http://www.baidu.com')

kw = driver.find_element_by_id('kw')  # 先定位并获得kw元素
kw.click()

driver.refresh()  # refresh

kw.click()  # 刷新后，仍用原来的kw元素操作，这时会报错

driver.quit()
```

结果：

```
Traceback (most recent call last):
  File "D:/Code/py/AutoTestFramework/src/others/tryrefreshpage.py", line 16, in <module>
    kw.click()
  File "C:\APP\Python2.7.10\lib\site-packages\selenium\webdriver\remote\webelement.py", line 75, in click
    self._execute(Command.CLICK_ELEMENT)
  File "C:\APP\Python2.7.10\lib\site-packages\selenium\webdriver\remote\webelement.py", line 469, in _execute
    return self._parent.execute(command, params)
  File "C:\APP\Python2.7.10\lib\site-packages\selenium\webdriver\remote\webdriver.py", line 201, in execute
    self.error_handler.check_response(response)
  File "C:\APP\Python2.7.10\lib\site-packages\selenium\webdriver\remote\errorhandler.py", line 194, in check_response
    raise exception_class(message, screen, stacktrace)
selenium.common.exceptions.StaleElementReferenceException: Message: Element not found in the cache - perhaps the page has changed since it was looked up
Stacktrace:
    ....
```

原因很明显，你用别人的身份证id去找现在的人，哪怕这两个人长得很像，他也会告诉你：**对不起，你找错人了。**

当然，不仅仅这一种情况，如果你执行以下的操作，都有可能找错人：

- `refresh`，不论你是主动刷新还是页面自动刷新
- `back`，已经跳转到了其他页面，然后你用`driver.back()`跳回来，这也是一张新的页面了
- 跳转到了新的页面，但这张新页面上有一些元素跟之前页面是长得一样的，这也是一张新的页面了。比如：一排分页按钮，你点击下一页跳转到了第二页，想要还用原来的元素操作到下一页，那也是不可能的了。

除此之外可能还有其他的原因，总之你看到这类型长得差不多，但是对页面有了操作的情况，就应该想想这种可能性了。

那遇到这种情况该怎么办？

很简单：

只要刷新页面之后重新获取元素就行，不要提前获取一组元素，然后去循环操作每一个元素，这种情况还是获取元素的个数，然后在循环中获取相应位置的元素，在用的时候才去获取，这样你就获取到最新的`id`了，也不会出现找错人的尴尬了。

总之一句话，**遇到页面有变化的情况，不要去循环元素，去循环个数或者定位方式，在循环中获取元素。**

****

> 更多关于python selenium的文章，请关注我的CSDN专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**

