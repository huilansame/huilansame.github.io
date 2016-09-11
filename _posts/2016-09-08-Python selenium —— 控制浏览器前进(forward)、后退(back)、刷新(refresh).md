---
layout: post
title:  "Python selenium —— 控制浏览器前进(forward)、后退(back)、刷新(refresh)"
date:   2016-09-08 09:30:15
categories: selenium
permalink: /archivers/forward-back-refresh
---


今天这几个方法非常简单，就是我们能看到的浏览器导航栏的三个按钮 —— 后退、前进、刷新

![导航栏按钮](http://img.blog.csdn.net/20160828092036944)

> driver.back()
> 
> driver.forward()
> 
> driver.refresh()

不多说，have a try

```python
# -*- coding: utf-8 -*-

from selenium import webdriver
from time import sleep

driver = webdriver.Firefox()
driver.maximize_window()
driver.get('http://www.baidu.com')
print 'base_url: ', driver.current_url

driver.find_element_by_id('kw').send_keys(u'selenium 灰蓝')
driver.find_element_by_id('su').click()
sleep(2)
print 'after search: ', driver.current_url

driver.back()
print 'back to: ', driver.current_url

driver.forward()
print 'forward to: ', driver.current_url

sleep(2)
driver.refresh()
print 'refresh: ', driver.current_url

sleep(2)
driver.quit()
```

结果：明显能够看到浏览器的后退、前进与刷新的动作。

```
https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=0&rsv_idx=1&tn=baidu&wd=selenium%20%E7%81%B0%E8%93%9D&rsv_pq=f88ee279000319b4&rsv_t=3537ECuJsvn8biOOxAemTJhRgJA9fRB%2FgYyaBy3sdcQagRhU5jOqEDICoQI&rqlang=cn&rsv_enter=0&rsv_sug3=11&inputT=266&rsv_sug4=266
https://www.baidu.com/
https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=0&rsv_idx=1&tn=baidu&wd=selenium%20%E7%81%B0%E8%93%9D&rsv_pq=f88ee279000319b4&rsv_t=3537ECuJsvn8biOOxAemTJhRgJA9fRB%2FgYyaBy3sdcQagRhU5jOqEDICoQI&rqlang=cn&rsv_enter=0&rsv_sug3=11&inputT=266&rsv_sug4=266
https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=0&rsv_idx=1&tn=baidu&wd=selenium%20%E7%81%B0%E8%93%9D&rsv_pq=f88ee279000319b4&rsv_t=3537ECuJsvn8biOOxAemTJhRgJA9fRB%2FgYyaBy3sdcQagRhU5jOqEDICoQI&rqlang=cn&rsv_enter=0&rsv_sug3=11&inputT=266&rsv_sug4=266
```

那么什么时候会用呢？

一种情况就是，当你从一个父页面跳转到子页面进行操作，操作完之后没有“返回”之类的按钮或链接，重新进入父页面又很麻烦，back()可以帮你。forward()与此类似，相对没有back()那么常用。

当你修改了页面信息但是没有即时刷新时，可以手动refresh()。

**不过需要注意：**

> **当你前进并退回原来的页面或刷新页面之后，页面的元素id是改变了的。不要妄图用原来定位好的WebElement去操作现在的页面元素！否则会出现如下报错：**
> selenium.common.exceptions.StaleElementReferenceException: Message: Element not found in the cache - perhaps the page has changed since it was looked up

代码示例下：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver

driver = webdriver.Firefox()
driver.maximize_window()
driver.get('http://www.baidu.com')


kw = driver.find_element_by_id('kw')
print kw  # 原页面元素ID

driver.refresh()

print driver.find_element_by_id('kw')  # 刷新之后的kw元素ID
kw.send_keys('csdn')  # 再用原来的元素ID去操作就会抛出异常

driver.quit()
```

来看看输出结果：

```
<selenium.webdriver.remote.webelement.WebElement (session="19a16be7-f885-4454-aa44-065b35d8377f", element="{307bfd7c-bcb8-4bed-a0f0-164924e9f648}")>
<selenium.webdriver.remote.webelement.WebElement (session="19a16be7-f885-4454-aa44-065b35d8377f", element="{8eac4fb5-4c8f-4141-b9d5-2ced9b412388}")>
Traceback (most recent call last):
  File "D:/Code/py files/zhigou/zhigou_framework/test/UI_test/case/testnavigation.py", line 16, in <module>
    kw.send_keys('csdn')  # 再用原来的元素ID去操作就会抛出异常
  File "C:\APP\Python2.7.10\lib\site-packages\selenium\webdriver\remote\webelement.py", line 334, in send_keys
    self._execute(Command.SEND_KEYS_TO_ELEMENT, {'value': typing})
  File "C:\APP\Python2.7.10\lib\site-packages\selenium\webdriver\remote\webelement.py", line 469, in _execute
    return self._parent.execute(command, params)
  File "C:\APP\Python2.7.10\lib\site-packages\selenium\webdriver\remote\webdriver.py", line 201, in execute
    self.error_handler.check_response(response)
  File "C:\APP\Python2.7.10\lib\site-packages\selenium\webdriver\remote\errorhandler.py", line 194, in check_response
    raise exception_class(message, screen, stacktrace)
selenium.common.exceptions.StaleElementReferenceException: Message: Element not found in the cache - perhaps the page has changed since it was looked up
Stacktrace:
    at fxdriver.cache.getElementAt (resource://fxdriver/modules/web-element-cache.js:9407)
    at Utils.getElementAt (file:///c:/users/zx/appdata/local/temp/tmpsorvob/extensions/fxdriver@googlecode.com/components/command-processor.js:8992)
    at fxdriver.preconditions.visible (file:///c:/users/zx/appdata/local/temp/tmpsorvob/extensions/fxdriver@googlecode.com/components/command-processor.js:10043)
    at DelayedCommand.prototype.checkPreconditions_ (file:///c:/users/zx/appdata/local/temp/tmpsorvob/extensions/fxdriver@googlecode.com/components/command-processor.js:12597)
    at DelayedCommand.prototype.executeInternal_/h (file:///c:/users/zx/appdata/local/temp/tmpsorvob/extensions/fxdriver@googlecode.com/components/command-processor.js:12614)
    at DelayedCommand.prototype.executeInternal_ (file:///c:/users/zx/appdata/local/temp/tmpsorvob/extensions/fxdriver@googlecode.com/components/command-processor.js:12619)
    at DelayedCommand.prototype.execute/< (file:///c:/users/zx/appdata/local/temp/tmpsorvob/extensions/fxdriver@googlecode.com/components/command-processor.js:12561)

```

能看到**两次输出的WebElement是完全不同的**，还用原来的元素去操作当然是不行的：

![两次WebElement不同](http://img.blog.csdn.net/20160828095936324)

所以在有可能有页面刷新的时候，**不要用这种获取到元素，然后一直操作这个元素的写法；要在刷新之后重新获取一下元素进行操作。**


****

> 更多关于python selenium的文章，请关注我的CSDN专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**
