---
layout: post
title:  "Python selenium —— 用chrome的Mobile emulation模拟手机浏览器测试手机网页"
date:   2016-10-19 10:00:13
categories: selenium
permalink: /archivers/chrome-mobile-emulation-with-webdriver
---

很多人发现chrome有项功能，就是在开发者工具里能够模拟手机打开网页，便想能否用selenium对此进行自动化测试。答案当然是yes！

![chrome-emulation](http://img.blog.csdn.net/20161019082137801)

今天博主便给大家分享下如何用chrome的MobileEmulation实现手机网页自动化测试。

## **1. 第一种方法**

第一种方法是通过device name来确定我们要模拟的手机样式，示例代码如下：

```python
# -*- coding: utf-8 -*-
from selenium import webdriver
from time import sleep


mobileEmulation = {'deviceName': 'Apple iPhone 4'}
options = webdriver.ChromeOptions()
options.add_experimental_option('mobileEmulation', mobileEmulation)

driver = webdriver.Chrome(executable_path='chromedriver.exe', chrome_options=options)

driver.get('http://m.baidu.com')

sleep(3)
driver.close()
```

如上，可直接指定deviceName，具体deviceName应该怎么写，可以调出开发者工具，看看Device下拉框中的选项内容。

## **2. 第二种方法**

或者你可以直接指定分辨率以及UA标识，如下：

```python
# -*- coding: utf-8 -*-
from selenium import webdriver
from time import sleep

WIDTH = 320
HEIGHT = 640
PIXEL_RATIO = 3.0
UA = 'Mozilla/5.0 (Linux; Android 4.1.1; GT-N7100 Build/JRO03C) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/35.0.1916.138 Mobile Safari/537.36 T7/6.3'

mobileEmulation = {"deviceMetrics": {"width": WIDTH, "height": HEIGHT, "pixelRatio": PIXEL_RATIO}, "userAgent": UA}
options = webdriver.ChromeOptions()
options.add_experimental_option('mobileEmulation', mobileEmulation)

driver = webdriver.Chrome(executable_path='chromedriver.exe', chrome_options=options)
driver.get('http://m.baidu.com')

sleep(3)
driver.close()
```

上面这种方法直接指定了宽度、高度、分辨率以及ua标识，全部可以自定义。

你也可以配合 `driver.set_window_size(width,height)` 来将浏览器窗口设置为相同大小，这样看起来更舒服一些。

现在，你可以用chrome来模拟手机浏览器测试手机网页了。用touch_actions来模拟手指操作吧！

*****

> 更多关于python selenium的文章，请关注我的CSDN专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**
