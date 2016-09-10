---
layout: post
title:  "Python selenium —— 使用命令行参数向autoIT生成的exe中传参"
date:   2016-09-07 09:10:15
categories: jekyll update
permalink: /archivers/cmd-params-to-autoit-exe
---


> 更多关于python selenium的文章，请关注我的CSDN专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**

****

selenium对网页进行UI自动化时经常会遇到OS弹框，比如上传、下载框，对这种弹框，selenium无法处理，常常我们会选择autoit这个工具。

想要参数化传入的参数，可以通过autoit的命令行参数：

> myProg.exe param1 "This is a string parameter" 99 

在脚本中，可用以下变量获取命令行参数：

	$CmdLine[0] ; = 3
	$CmdLine[1] ; = param1
	$CmdLine[2] ; = "This is a string parameter"
	$CmdLine[3] ; = 99
	$CmdLineRaw ; = 'param1 "This is a string parameter" 99'

**\$CmdLine[0]** 获取的是命令行参数的总数，在上例中\$CmdLine[0]=3
**\$CmdLine[1]~\$CmdLine[63]** 获取的是命令行参数第1到第63位，这个方式最多只能获取63个参数，不过正常情况下是足够用的
**\$CmdLineRaw** 获取的是未拆分的所有参数，是一个长字符串，这种情况下不局限与63个参数

下面我们小小实践一下：

> [示例网址：http://www.sahitest.com/demo/php/fileUpload.htm](http://www.sahitest.com/demo/php/fileUpload.htm)

通过autoit的获取对象并编辑脚本：

```autoit
ControlFocus("文件上传", "", "Edit1")
WinWait("[CLASS:#32770]", "", 10)
ControlSetText("文件上传" ,"", "Edit1", $CmdLine[1])
Sleep(2000)
ControlClick("文件上传", "","Button1");
```

通过Aut2Exe工具将脚本转成exe文件（upfile.exe）

我们先通过命令行试试，打开网页上传弹框，然后在cmd中执行该脚本：

```batch
> D:\upfile.exe "D:\1.html"
```

成功！

接下来就是用Python用os模块来调用该文件了：

```python
# -*- coding: utf-8 -*-
from selenium import webdriver
import os
import time

driver = webdriver.Firefox()
driver.get('http://www.sahitest.com/demo/php/fileUpload.htm')
driver.find_element_by_id('file').click()
time.sleep(1)

os.system('D:\\upfile.exe "D:\\1.html"')  # 这里可以对传参进行参数化，我们可以通过py脚本来控制所要上传的文件了

time.sleep(3)
driver.quit()
```

执行，成功！

当然，这里只是个示例，实际上对于这种input标签，我们直接send_keys就可以了。今后再专门讨论上传的处理。

