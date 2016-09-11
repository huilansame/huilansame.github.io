---
layout: post
title:  "Python selenium —— 怎么处理时间、日期控件"
date:   2016-09-09 14:50:15
categories: selenium
permalink: /archivers/time-date-widget
---


很多人问时间日期的控件怎么处理，但是时间日期控件各种各样，你可能遇到正常点的像这样：

![正常点的](http://img.blog.csdn.net/20160831142857143)

当然也可能遇到难点的，像这样：

![困难的](http://img.blog.csdn.net/20160831143120887)

当然，也不排除会遇到变态的，像这样：

![变态的](http://img.blog.csdn.net/20160831143142371)

呵呵，真要一个个想着怎么去选择，简直是非人类干的事！

那么该怎么办？

其实很简单，我们不去搞时间日期空间，我们把它当成一个普通的`input`框处理就好了！

但是，很多此类型`input`框都是禁止手动输入的，怎么办？

很简单，用js把禁止输入的`readonly`属性干掉就好了。

来吧，思路明确了，看一下示例：

> 示例网址：[http://www.sucaijiayuan.com/api/demo.php?url=/demo/20141108-1/](http://www.sucaijiayuan.com/api/demo.php?url=/demo/20141108-1/)

代码：

```python
# -*- coding: utf-8 -*-

from selenium import webdriver
from time import sleep

driver = webdriver.Firefox()
driver.get('http://www.sucaijiayuan.com/api/demo.php?url=/demo/20141108-1/')

driver.switch_to.frame('iframe')

# js = "document.getElementById('txtBeginDate').removeAttribute('readonly')"  # 1.原生js，移除属性
# js = "$('input[id=txtBeginDate]').removeAttr('readonly')"  # 2.jQuery，移除属性
# js = "$('input[id=txtBeginDate]').attr('readonly',false)"  # 3.jQuery，设置为false
js = "$('input[id=txtBeginDate]').attr('readonly','')"  # 4.jQuery，设置为空（同3）

driver.execute_script(js)

driver.find_element_by_id('txtBeginDate').send_keys('2016-08-24')
sleep(2)
print driver.find_element_by_id('txtBeginDate').get_attribute('value')

driver.quit()
```

结果：

```
2016-08-24
```

看，我一下子给了你四种写法。当然你去选也没有问题，就是一堆`div`、`ul`、`li`，但是我想说对于日期时间控件，认真你就输了，绕过去才是真理，就像验证码一样，这个东西去深究，一点意义也没有嘛，所以这里就不推荐也不写这种方法的示例了。

当然，这里还得说一下，有人说他遇到的日期时间框没有id、name，这里提醒一下，这就是普通的input框，定位方法太多了，class、tag name、xpath、css，总有一种适合你。同时友情提醒一下：注意**时刻提防`iframe`！**

> 有什么问题或者遇到什么特殊的控件无法这样处理，可以通过评论告诉我！

****

> 更多关于python selenium的文章，请关注我的专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**