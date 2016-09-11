---
layout: post
title:  "Python selenium —— 区分关闭窗口的两个方法 close、quit"
date:   2016-09-08 09:20:15
categories: selenium
permalink: /archivers/close-and-quit
---


selenium关闭窗口有两个方法，`close`与`quit`，我们稍作研究便知道这两个方法的区别。

## **1.读读API**

这是`close()`的说明：

> Closes the current window.
> 
> 关闭当前窗口。

这是`quit()`的说明：

> Quits the driver and closes every associated window.
> 
> 退出驱动并关闭所有关联的窗口。

区别：一个关闭当前窗口，一个关闭所有窗口，下面写一小段代码测试一下。


## **2.代码试验**

代码：

```python
# -*- coding: utf-8 -*-
from selenium import webdriver
from time import sleep

driver = webdriver.Firefox()
driver.get('http://sahitest.com/demo/index.htm')
print driver.current_window_handle  # 查看当前window handle

driver.find_element_by_link_text('Window Open Test').click()  # 打开新window1
driver.find_element_by_link_text('Window Open Test With Title').click()  # 打开新window2
print driver.window_handles  # 查看所有window handles

driver.close()
print driver.window_handles  # 查看现在的所有window handles

driver.quit()  # 看到所有window都被关闭
```

结果：

```
{b030dd54-3cbd-4d7b-800a-2ff296f03f5b}
[u'{b030dd54-3cbd-4d7b-800a-2ff296f03f5b}', u'{7fdacf2e-0c34-4f0d-9a7a-ae34f3af932c}', u'{f2d79121-8cc2-47ea-bd7d-2035e305ba2f}']
[u'{7fdacf2e-0c34-4f0d-9a7a-ae34f3af932c}', u'{f2d79121-8cc2-47ea-bd7d-2035e305ba2f}']
```

我们一共打开了三个window，使用`close`方法仅仅关闭了当前window，其他两个window还在，当我们使用`quit`方法，所有window都被关闭。

当仅有一个window时，两种方法区别不大，但当有多个window时，就要注意使用的方法了。


****

> 更多关于python selenium的文章，请关注我的CSDN专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**

