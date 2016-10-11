---
layout: post
title:  "Python selenium —— 文件下载，不弹出窗口，直接下载到指定路径"
date:   2016-10-11 16:45:13
categories: selenium
permalink: /archivers/how-to-download-files-with-selenium
---

今天谈谈文件下载吧，很多人不会处理弹出的文件下载框，其实跟上传类似，可以用autoit和win32api解决，方法类似，可以看博主之前的文章**[Python selenium —— 文件上传所有方法整理总结](https://huilansame.github.io/huilansame.github.io/archivers/file-upload-all-ways)**，今天这里博主主要想讲讲更漂亮的一种处理办法，那就是指定下载路径，不弹出弹框，直接下载到指定路径。

今天主要分享Firefox和Chrome的设置方法。

## **Firefox 文件下载**

对于Firefox，需要我们设置其Profile：

- `browser.download.dir`：指定下载路径
- `browser.download.folderList`：设置成 `2` 表示使用自定义下载路径；设置成 `0` 表示下载到桌面；设置成 `1` 表示下载到默认路径
- `browser.download.manager.showWhenStarting`：在开始下载时是否显示下载管理器
- `browser.helperApps.neverAsk.saveToDisk`：对所给出文件类型不再弹出框进行询问

下面来个示例：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver
from time import sleep

profile = webdriver.FirefoxProfile()
profile.set_preference('browser.download.dir', 'd:\\')
profile.set_preference('browser.download.folderList', 2)
profile.set_preference('browser.download.manager.showWhenStarting', False)
profile.set_preference('browser.helperApps.neverAsk.saveToDisk', 'application/zip')

driver = webdriver.Firefox(firefox_profile=profile)

driver.get('http://sahitest.com/demo/saveAs.htm')
driver.find_element_by_xpath('//a[text()="testsaveas.zip"]').click()
sleep(3)
driver.quit()
```

Firefox需要针对每种文件类型进行设置，这里需要我们查询对应文件的MIME类型，可以用以下链接进行查询：[MIME 参考手册](http://www.w3school.com.cn/media/media_mimeref.asp)

## **Chrome 文件下载**

Chrome浏览器类似，设置其options：

- `download.default_directory`：设置下载路径
- `profile.default_content_settings.popups`：设置为 `0` 禁止弹出窗口

它的设置就简单多了，看个示例：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver
from time import sleep


options = webdriver.ChromeOptions()
prefs = {'profile.default_content_settings.popups': 0, 'download.default_directory': 'd:\\'}
options.add_experimental_option('prefs', prefs)

driver = webdriver.Chrome(executable_path='D:\\chromedriver.exe', chrome_options=options)
driver.get('http://sahitest.com/demo/saveAs.htm')
driver.find_element_by_xpath('//a[text()="testsaveas.zip"]').click()
sleep(3)
driver.quit()
```

看，文件下载也很简单吧。

*****

> 更多关于python selenium的文章，请关注我的CSDN专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**
