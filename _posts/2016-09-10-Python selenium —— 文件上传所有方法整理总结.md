---
layout: post
title:  "Python selenium —— 文件上传所有方法整理总结"
date:   2016-09-10 10:50:15
categories: selenium
permalink: /archivers/file-upload-all-ways
---


文件上传是所有UI自动化测试都要面对的一个头疼问题，今天博主在这里给大家分享下自己处理文件上传的经验，希望能够帮助到广大被文件上传坑住的seleniumer。

**首先，我们要区分出上传按钮的种类，大体上可以分为两种，一种是input框，另外一种就比较复杂，通过js、flash等实现，标签非input**

我们分别对这两种进行分析：

## **1.input标签**

众所周知，`<input>`标签是可以直接`send_keys`的，这里也不例外，来看代码示例：

> 示例网址:[http://www.sahitest.com/demo/php/fileUpload.htm](http://www.sahitest.com/demo/php/fileUpload.htm)

代码：

```python
# -*- coding: utf-8 -*-
from selenium import webdriver

driver = webdriver.Firefox()
driver.get('http://sahitest.com/demo/php/fileUpload.htm')
upload = driver.find_element_by_id('file')
upload.send_keys('d:\\baidu.py')  # send_keys
print upload.get_attribute('value')  # check value

driver.quit()
```

结果：

```
baidu.py
```

很明显，**对于input上传，直接send_keys是最简单的解决方案。**

## **2.非input型上传**

接下来难度要升级了，对于那些不是input框实现的上传怎么办，这种上传千奇百怪，有用a标签的，有用div的，有用button的，有用object的，我们没有办法通过直接在网页上处理掉这些上传，唯一的办法就是打开`OS弹框`，去处理弹框。

问题又来了，OS弹框涉及的层面已经不是selenium能解决的了，怎么办？很简单，用OS层面的操作去处理呗，到这里我们基本找到了问题的处理方法。

大体上有以下几种解决方案：

1. `autoIT`，借助外力，我们去调用其生成的au3或exe文件。
2. Python `pywin32`库，识别对话框句柄，进而操作
3. `SendKeys`库
4. `keybd_event`，跟3类似，不过是模拟按键，ctrl+a，ctrl+c， ctrl+v...

目前我只知道以上四种办法，有其他方法的请留言告诉我，让我学习一下。

我们依次看一下：

**1. autoIT**

关于autoIT上传以及参数化的方法我已经在另一篇博文中讲过了，请参见*[selenium之 autoit命令行参数 ](http://blog.csdn.net/huilan_same/article/details/52208363)*。这里不再赘述。

**2.win32gui**

废话不多说，上代码先：

> 示例网址:[http://www.sahitest.com/demo/php/fileUpload.htm](http://www.sahitest.com/demo/php/fileUpload.htm)

代码：

```python
# -*- coding: utf-8 -*-
from selenium import webdriver
import win32gui
import win32con
import time

dr = webdriver.Firefox()
dr.get('http://sahitest.com/demo/php/fileUpload.htm')
upload = dr.find_element_by_id('file')
upload.click()
time.sleep(1)

# win32gui
dialog = win32gui.FindWindow('#32770', u'文件上传')  # 对话框
ComboBoxEx32 = win32gui.FindWindowEx(dialog, 0, 'ComboBoxEx32', None) 
ComboBox = win32gui.FindWindowEx(ComboBoxEx32, 0, 'ComboBox', None)
Edit = win32gui.FindWindowEx(ComboBox, 0, 'Edit', None)  # 上面三句依次寻找对象，直到找到输入框Edit对象的句柄
button = win32gui.FindWindowEx(dialog, 0, 'Button', None)  # 确定按钮Button

win32gui.SendMessage(Edit, win32con.WM_SETTEXT, None, 'd:\\baidu.py')  # 往输入框输入绝对地址
win32gui.SendMessage(dialog, win32con.WM_COMMAND, 1, button)  # 按button

print upload.get_attribute('value')
dr.quit()
```

结果：

```
baidu.py
```

在这里你需要一个非常重要的小工具：`Spy++`，百度一下有很多，当然你也可以用autoIT自带的工具，不过没有这个好用，建议去下一个吧。

而且，你得安装**pywin32**的库，你可以到**[这里](https://sourceforge.net/projects/pywin32/files/pywin32/)**找到对应你Python版本的库，注意32位还是64位一定要和你安装的Python版本对应。

安装完成之后在【开始菜单Python的文件夹】里看到PyWin32的文档【Python for Windows Documentation】，你能从中找到对应的方法API。

简单介绍几个用到的：

> win32gui.FindWindow(lpClassName=None, lpWindowName=None):

- 自顶层窗口开始寻找匹配条件的窗口，并返回这个窗口的句柄。
- lpClassName：类名，在Spy++里能够看到
- lpWindowName：窗口名，标题栏上能看到的名字
- 代码示例里我们用来寻找上传窗口，你可以只用其中的一个，用classname定位容易被其他东西干扰，用windowname定位不稳定，不同的上传对话框可能window_name不同，怎么定位取决于你的情况。

> win32gui.FindWindowEx(hwndParent=0, hwndChildAfter=0, lpszClass=None, lpszWindow=None)

- 搜索类名和窗体名匹配的窗体，并返回这个窗体的句柄。找不到就返回0。
- hwndParent：若不为0，则搜索句柄为hwndParent窗体的子窗体。
- hwndChildAfter：若不为0，则按照z-index的顺序从hwndChildAfter向后开始搜索子窗体，否则从第一个子窗体开始搜索。
- lpClassName：字符型，是窗体的类名，这个可以在Spy++里找到。
- lpWindowName：字符型，是窗口名，也就是标题栏上你能看见的那个标题。
- 代码示例里我们用来层层寻找输入框和寻找确定按钮

> win32gui.SendMessage(hWnd, Msg, wParam, lParam)

- hWnd：整型，接收消息的窗体句柄
- Msg：整型，要发送的消息，这些消息都是windows预先定义好的，可以参见[系统定义消息（System-Defined Messages）](https://msdn.microsoft.com/en-us/library/windows/desktop/ms644927(v=vs.85).aspx#system_defined)
- wParam：整型，消息的wParam参数
- lParam：整型，消息的lParam参数
- 代码示例里我们用来向输入框输入文件地址以及点击确定按钮

至于`win32ui`模块以及其他的方法，这里不进行更多描述，想要了解的自行百度或看pywin32文档。

**3.SendKeys**

首先要安装`SendKeys`库，可以用pip安装

> pip install SendKeys

代码示例：

> 示例网址:[http://www.sahitest.com/demo/php/fileUpload.htm](http://www.sahitest.com/demo/php/fileUpload.htm)

代码：

```python
# -*- coding: utf-8 -*-
from selenium import webdriver
import win32gui
import win32con
import time

dr = webdriver.Firefox()
dr.get('http://sahitest.com/demo/php/fileUpload.htm')
upload = dr.find_element_by_id('file')
upload.click()
time.sleep(1)

# SendKeys
SendKeys.SendKeys('D:\\baidu.py')  # 发送文件地址
SendKeys.SendKeys("{ENTER}") # 发送回车键

print upload.get_attribute('value')
dr.quit()
```
结果：
```
baidu.py
```

通过`SendKeys`库可以直接向焦点里输入信息，不过要注意在打开窗口是略微加一点等待时间，否则容易第一个字母send不进去（或者你可以在地址之前加一个无用字符），不过我觉得这种方法很不稳定，不推荐。


**4.keybd_event**

`win32api`提供了一个`keybd_event()`方法模拟按键，不过此方法比较麻烦，也不稳定，所以很不推荐，下面给出部分代码示例，如果想要研究，自己百度去学习吧。

```python
...

# 先找一个input框，输入想要上传的文件的地址，剪切到剪贴板 
video.send_keys('C:\\Users\\Administrator\\Pictures\\04b20919fc78baf41fc993fd8ee2c5c9.jpg')
video.send_keys(Keys.CONTROL, 'a')  # selenium的send_keys（ctrl+a）
video.send_keys(Keys.CONTROL, 'x')  # (ctrl+x)
driver.find_element_by_id('uploadImage').click()  # 点击上传按钮，打开上传框

# 粘贴（ctrl + v）
win32api.keybd_event(17, 0, 0, 0)  # 按下按键 ctrl
win32api.keybd_event(86, 0, 0, 0)  # 按下按键 v
win32api.keybd_event(86, 0, win32con.KEYEVENTF_KEYUP, 0)  # 升起按键 v
win32api.keybd_event(17, 0, win32con.KEYEVENTF_KEYUP, 0)  # 升起按键 ctrl
time.sleep(1)

# 回车（enter）
win32api.keybd_event(13, 0, 0, 0)  # 按下按键 enter
win32api.keybd_event(13, 0, win32con.KEYEVENTF_KEYUP, 0)  # 升起按键 enter

...
```

是不是很麻烦，当然，你甚至可以用按键把整个路径输入进去，不过，我想没人愿意这么做的。而且在此过程中你不能随意移动鼠标，不能使用剪贴板，太不稳定了，所以非常不建议你用这种办法。。

## **3.多文件上传**

接下来还有一种情况值得我们考虑，那就是多文件上传。如何上传多个文件，当然我们还是往输入框里输入文件路径，所以唯一要搞清楚的就是多文件上传时，文件路径是怎么写的。

> 我来告诉你吧，多文件上传就是在文件路径框里用引号括起单个路径，然后用逗号隔开多个路径，就是这么简单，例如：
> 
	 "D:\\a.txt" "D:\\b.txt"
> 但需要注意的是：只有多个文件在同一路径下，才能这样用，否则是会失败的（下面的写法是不可以的）：
> 
	 "C:\\a.txt" "D:\\b.txt"

接下里找一个例子试试：

> 示例网址：[http://www.sucaijiayuan.com/api/demo.php?url=/demo/20150128-1](http://www.sucaijiayuan.com/api/demo.php?url=/demo/20150128-1)

代码：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver
import win32gui
import win32con
import time

dr = webdriver.Firefox()
dr.get('http://www.sucaijiayuan.com/api/demo.php?url=/demo/20150128-1')

dr.switch_to.frame('iframe')  # 一定要注意frame
dr.find_element_by_class_name('filePicker').click()
time.sleep(1)

dialog = win32gui.FindWindow('#32770', None)
ComboBoxEx32 = win32gui.FindWindowEx(dialog, 0, 'ComboBoxEx32', None)
ComboBox = win32gui.FindWindowEx(ComboBoxEx32, 0, 'ComboBox', None)
Edit = win32gui.FindWindowEx(ComboBox, 0, 'Edit', None)
button = win32gui.FindWindowEx(dialog, 0, 'Button', None)

# 跟上面示例的代码是一样的，只是这里传入的参数不同，如果愿意可以写一个上传函数把上传功能封装起来
win32gui.SendMessage(Edit, win32con.WM_SETTEXT, 0, '"d:\\baidu.py" "d:\\upload.py" "d:\\1.html"')
win32gui.SendMessage(dialog, win32con.WM_COMMAND, 1, button)


print dr.find_element_by_id('status_info').text
dr.quit()
```

结果：

```
选中3张文件，共1.17KB。
```

可见，多文件上传并没有那么复杂，也很简单，唯一的区别就是输入的参数不同而已。autoIT也可以实现，有兴趣可以自己试试。

而且我们可以发现一点，就是上面的这个窗口的代码跟之前示例中的基本是一样，说明我们可以把上传的部分抽出来，写一个函数，这样每次要上传，直接去调用函数，传入参数即可。

> 看，上传其实很好处理，你有什么好的办法也可以给博主留言，共同交流。

*****

> 更多关于python selenium的文章，请关注我的专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**
