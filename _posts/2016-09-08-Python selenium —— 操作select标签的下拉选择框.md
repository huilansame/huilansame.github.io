---
layout: post
title:  "Python selenium —— 操作select标签的下拉选择框"
date:   2016-09-08 10:10:15
categories: selenium
permalink: /archivers/drop-down-select
---



今天总结下selenium的下拉选择框。我们通常会遇到两种下拉框，一种使用的是`html`的标签`select`，另一种是使用`input`标签做的假下拉框。

后者我们通常的处理方式与其他的元素类似，点击或使用JS等。而对于前者，selenium给了有力的支持，就是**Select**类。

**我们要进行试验的网站：[http://sahitest.com/demo/selectTest.htm](http://sahitest.com/demo/selectTest.htm)**

**网页与源码截图如下：**

![测试网页截图](http://img.blog.csdn.net/20160818235134405)

## **1.导入（`import`）**

你可以用以下方式导入：

```python
from selenium.webdriver.support.ui import Select
# 或者直接从select导入
# from selenium.webdriver.support.select import Select
```

这两种方法没有本质的区别，你如果去看`ui`库，你会发现，它也只是把`select` import进去。

## **2.选择（`select`）**

`Select`类提供了三种选择某一选项的方法：

```python
select_by_index(index)
select_by_value(value)
select_by_visible_text(text)
```

针对于示例网站中的第一个select框：

```html
<select id="s1Id">
<option></option>
<option value="o1" id="id1">o1</option>
<option value="o2" id="id2">o2</option>
<option value="o3" id="id3">o3</option>
</select>
```

我们可以这样定位：

```python
from selenium import webdriverd
from selenium.webdriver.support.ui import Select

driver = webdriver.Firefox()
driver.get('http://sahitest.com/demo/selectTest.htm')

s1 = Select(driver.find_element_by_id('s1Id'))  # 实例化Select

s1.select_by_index(1)  # 选择第二项选项：o1
s1.select_by_value("o2")  # 选择value="o2"的项
s1.select_by_visible_text("o3")  # 选择text="o3"的值，即在下拉时我们可以看到的文本

driver.quit()
```

以上是三种选择下拉框的方式，注意：

1. `index`从 **0** 开始
2. `value`是`option`标签的一个属性值，并不是显示在下拉框中的值
3. `visible_text`是在`option`标签中间的值，是显示在下拉框的值

## **3.反选（`deselect`）**

自然的，有选择必然有反选，即取消选择。`Select`提供了四个方法给我们取消原来的选择：

```
deselect_by_index(index)
deselect_by_value(value)
deselect_by_visible_text(text)
deselect_all()
```

前三种分别于`select`相对应，第四种是全部取消选择，是的，你没看错，是全部取消。有一种特殊的select标签，即设置了`multiple="multiple"`属性的`select`，这种`select`框是可以多选的，你可以通过多次`select`，选择多项选项，而通过`deselect_all()`来将他们全部取消。

全选？NO，不好意思，没有全选，不过我想这难不倒你，尤其是看了下面的这几个属性。

## **4.选项（`options`）**

当我们选择了选项之后，想要看看选择的是哪项，所选的是否是我想选的，怎么办？别担心，`Select`为你提供了相应的方法（或者应该说是属性了）：

```python
options
all_selected_options
first_selected_option
```

上面三个属性，分别返回这个`select`元素所有的`options`、所有被选中的`options`以及第一个被选中的`option`。

**1 想查看一个`select`所有的选项**

```python
...

s1 = Select(driver.find_element_by_id('s1Id'))

for select in s1.options:
	print select.text

...
```

结果：

```

o1
o2
o3
```
一共四项，第一项为空字符串。

**2 想查看我已选中的选项**

```python
...

s4 = Select(driver.find_element_by_id('s4Id'))

s4.select_by_index(1)
s4.select_by_value("o2val")
s4.select_by_visible_text("With spaces")
s4.select_by_visilbe_text("    With nbsp")

for select in s4.all_selected_options:
	print select.text

...
```

结果：

```
o1
o2
    With spaces
    With nbsp
```

输出所有被选中的选项，适合于能多选的框，仅能单选的下拉框有更合适的方法（当然用这种方法也可以）。这里需要注意的是两种不同空格的选择：

1. 空格`' '`，这种在以`visible_text`的方式选择时，不计空格，从第一个非空格字符开始
2. 网页空格`&nbsp;`，对于这种以`&nbsp;`为空格的选项，在以`visible_text`的方式选择时，需要考虑前面的空格，每一个`&nbsp;`是一个空格

**3 想要查看选择框的默认值，或者我以及选中的值**

```python
...

s2 = Select(driver.find_element_by_id('s2Id'))

print s2.first_selected_option.text

s2.select_by_value("o2")
print s2.first_selected_option.text

...
```

结果：

```

o2
```

第一行输出默认选项的文本——空字符串""；第二行输出选中的选择的文本——"o2"。

## **5.总结**

- `Select`提供了三种选择方法：
> select\_by\_index(index)  ——通过选项的顺序，第一个为 0
> 
> select\_by\_value(value)  ——通过value属性
> 
> select\_by\_visible\_text(text)  ——通过选项可见文本

- 同时，`Select`提供了四种方法取消选择：
> deselect\_by\_index(index)
> 
> deselect\_by\_value(value)
> 
> deselect\_by\_visible\_text(text)
> 
> deselect\_all()

- 此外，`Select`提供了三个属性方法给我们必要的信息：
> options  ——提供所有的选项的列表，其中都是选项的WebElement元素
> 
> all\_selected\_options  ——提供所有被选中的选项的列表，其中也均为选项的WebElement元素
> 
> first\_selected\_option  ——提供第一个被选中的选项，也是**下拉框的默认值**

- 通过`Select`提供的方法和属性，我们可以对标准`select`下拉框进行任何操作，但是对于非`select`标签的伪下拉框，就需要用其他的方法了，这个有机会再讨论。


****

> 更多关于python selenium的文章，请关注我的专栏：**[Python Selenium自动化测试详解](http://blog.csdn.net/column/details/12694.html)**

