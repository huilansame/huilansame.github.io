---
layout: post
title:  "Python selenium —— Webdriver Exception cheat sheet"
date:   2016-10-14 13:00:13
categories: selenium
permalink: /archivers/webdriver-exception-cheat-sheet
---

之前整理了Python版webdriver的备忘单与xpath、css的备忘单，今天把Python webdriver的exception清单整理一下。

## Python Webdriver Exception Cheat Sheet

![webdriver exception cheat sheet](http://img.blog.csdn.net/20161014125841929)

上表大概罗列了Python Webdriver 中的Exception以及出现该问题的原因，具体的代码解析与代码示例博主改天再给大家分享。

HTML版如下：

| 异常 | 描述 |
| -- | -- |
|WebDriverException | 所有webdriver异常的基类，当有异常且不属于下列异常时抛出|
|InvalidSwitchToTargetException | 下面两个异常的父类，当要switch的目标不存在时抛出|
|NoSuchFrameException  |  当你想要用switch_to.frame()切入某个不存在的frame时抛出|
|NoSuchWindowException |  当你想要用switch_to.window()切入某个不存在的window时抛出|
|NoSuchElementException | 元素不存在，一般由find_element与find_elements抛出|
|NoSuchAttributeException |   一般你获取不存在的元素属性时抛出，要注意有些属性在不同浏览器里是有不同的属性名的|
|StaleElementReferenceException | 指定的元素过时了，不在现在的DOM树里了，可能是被删除了或者是页面或iframe刷新了|
|UnexpectedAlertPresentException| 出现了意料之外的alert，阻碍了指令的执行时抛出|
|NoAlertPresentException| 你想要获取alert，但实际没有alert出现时抛出|
|InvalidElementStateException |   下面两个异常的父类，当元素状态不能进行想要的操作时抛出|
|ElementNotVisibleException | 元素存在，但是不可见，不可以与之交互|
|ElementNotSelectableException |  当你想要选择一个不可被选择的元素时抛出|
|InvalidSelectorException  |  一般当你xpath语法错误的时候抛出这个错|
|InvalidCookieDomainException |   当你想要在非当前url的域里添加cookie时抛出|
|UnableToSetCookieException | 当driver无法添加一个cookie时抛出|
|TimeoutException  |  当一个指令在足够的时间内没有完成时抛出|
|MoveTargetOutOfBoundsException | actions的move操作时抛出，将目标移动出了window之外|
|UnexpectedTagNameException | 获取到的元素标签不符合要求时抛出，比如实例化Select，你传入了非select标签的元素时|
|ImeNotAvailableException  |  输入法不支持的时候抛出，这里两个异常不常见，ime引擎据说是仅用于linux下对中文/日文支持的时候|
|ImeActivationFailedException |   激活输入法失败时抛出|
|ErrorInResponseException   | 不常见，server端出错时可能会抛|
|RemoteDriverServerException |不常见，好像是在某些情况下驱动启动浏览器失败的时候会报这个错|



如果它对你有帮助，或者你有什么好的建议，请告诉我。

pdf版本可在此下载 **[Python Webdriver Exceptions Cheat Sheet By 灰蓝](http://download.csdn.net/detail/huilan_same/9653782)**

*****

> 更多关于python selenium的文章，请关注我的CSDN专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**

