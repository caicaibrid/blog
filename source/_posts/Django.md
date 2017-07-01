---
title: Django
date: 2017-04-13 20:50:33
categories: Python
tags:
     - Django
---

# Django 学习
练习项目：[python django mysql 个人博客](https://github.com/caicaibrid/django_blog_test)
## django 基本命令

> 1.新建一个django project
    django-admin.py startproject project-name //project-name项目名称

>2.新建app (模块)
   python manage.py startapp app-name //一般一个项目有多个app, 当然通用的app也可以在多个项目中使用。

把 app-name 加入到 settings.INSTALLED_APPS中
django就会默认去寻找该模块下面的templates文件夹，可以通过render(request,'templates里的html')直接返回

## django 模板

```
from django.shortcuts import render //返回模板render()
{% block title%} {% endblock %}

{% extends 'base.html'%}

{% include "header.html" %}

{% url "add2" 4 5 %} //add2 为路由指定的name 4 5 为参数

模板上得到视图的网址 {% url 'add2' 4 5 %}
<br>
获取当前的地址 {{ request.path }}
<br>
获取当前的GET参数 {{ request.GET.urlencode }}
<br>
获取当前的用户 {{ request.user }}

```
## django models

```
python manage.py startapp modelStudy //创建一个模块modelStudy
然后在该模块下面编辑models.py添加一个Peopele类

class People(models.Model):
    name = models.CharField(max_length=30)
    age = models.IntegerField()
    def __unicode__(self):
        return self.name

同步一下数据库（我们使用默认的数据库 SQLite3，无需配置）
python manage.py syncdb # 进入 manage.py 所在的那个文件夹下输入这个命令
注意：Django 1.7 及以上的版本需要用以下命令
python manage.py makemigrations
python manage.py migrate

上面命令Django生成了一系列的表，也生成了我们新建的modelStudy_people这个表

Django提供了丰富的API, 下面演示如何使用它。
$ python manage.py shell
>>> from people.models import Person
>>> Person.objects.create(name="WeizhongTu", age=24)
<Person: Person object>

新建一个对象的方法有以下几种：

1.People.objects.create(name=name,age=age)
2.p = People(name="WZ", age=23)
  p.save()
3.p = People(name="TWZ")
  p.age = 23
  p.save()
4.People.objects.get_or_create(name="WZT", age=23)

这种方法是防止重复很好的方法，但是速度要相对慢些，返回一个元组，第一个为People对象，第二个为True或False, 新建时返回的是True, 已经存在时返回False.

### 获取对象有以下方法：

Person.objects.all()

Person.objects.all()[:10] 切片操作，获取10个人，不支持负索引，切片可以节约内存

Person.objects.get(name=name)

get是用来获取一个对象的，如果需要获取满足条件的一些人，就要用到filter

Person.objects.filter(name="abc") # 等于Person.objects.filter(name__exact="abc") 名称严格等于 "abc" 的人

Person.objects.filter(name__iexact="abc") # 名称为 abc 但是不区分大小写，可以找到 ABC, Abc, aBC，这些都符合条件

Person.objects.filter(name__contains="abc") # 名称中包含 "abc"的人

Person.objects.filter(name__icontains="abc") #名称中包含 "abc"，且abc不区分大小写

Person.objects.filter(name__regex="^abc") # 正则表达式查询

Person.objects.filter(name__iregex="^abc")# 正则表达式不区分大小写

filter是找出满足条件的，当然也有排除符合某条件的

Person.objects.exclude(name__contains="WZ") # 排除包含 WZ 的Person对象

Person.objects.filter(name__contains="abc").exclude(age=23) # 找出名称含有abc, 但是排除年龄是23岁的
```

# django 模板过滤器

```
一、形式：小写：  {{ name | lower }}

二、串联：先转义文本到HTML，再转换每行到 <p> 标签： {{ my_text|escape|linebreaks }}

三、过滤器的参数

显示前30个字：{{ bio | truncatewords:"30" }}

格式化：{{ pub_date | date:"F j, Y" }}

过滤器列表：{{ 123|add:"5" }} 给value加上一个数值

{{ "AB'CD"|addslashes }}： 单引号加上转义号，一般用于输出到javascript中

{{ "abcd"|capfirst }}： 第一个字母大写

{{ "abcd"|center:"50" }}： 输出指定长度的字符串，并把值对中

{{ "123spam456spam789"|cut:"spam" }}： 查找删除指定字符串

{{ value|date:"F j, Y" }}： 格式化日期

{{ value|default:"(N/A)" }}： 值不存在，使用指定值

{{ value|default_if_none:"(N/A)" }}： 值是None，使用指定值

{{ 列表变量|dictsort:"数字" }} ：排序从小到大

{{ 列表变量|dictsortreversed:"数字" }} ：排序从大到小

{% if 92|divisibleby:"2" %} ：判断是否整除指定数字

{{ string|escape }} ：转换为html实体

{{ 21984124|filesizeformat }} ：以1024为基数，计算最大值，保留1位小数，增加可读性

{{ list|first }} ：返回列表第一个元素

{{ "ik23hr&jqwh"|fix_ampersands }}： &转为&amp;

{{ 13.414121241|floatformat }} ：保留1位小数，可为负数，几种形式

{{ 13.414121241|floatformat:"2" }}： 保留2位小数

{{ 23456 |get_digit:"1" }} ：从个位数开始截取指定位置的1个数字

{{ list|join:", " }} ：用指定分隔符连接列表

{{ list|length }} ：返回列表个数

{% if 列表|length_is:"3" %} ：列表个数是否指定数值

{{ "ABCD"|linebreaks }}： 用新行用<p> 、 <br /> 标记包裹

{{ "ABCD"|linebreaksbr }}： 用新行用<br /> 标记包裹

{{ 变量|linenumbers }}： 为变量中每一行加上行号

{{ "abcd"|ljust:"50" }}： 把字符串在指定宽度中对左，其它用空格填充

{{ "ABCD"|lower }}： 小写

{% for i in "1abc1"|make_list %}ABCDE,{% endfor %}： 把字符串或数字的字符个数作为一个列表

{{ "abcdefghijklmnopqrstuvwxyz"|phone2numeric }}： 把字符转为可以对应的数字？？

{{ 列表或数字|pluralize }}： 单词的复数形式，如列表字符串个数大于1，返回s，否则返回空串

{{ 列表或数字|pluralize:"es" }}： 指定es

{{ 列表或数字|pluralize:"y,ies" }}： 指定ies替换为y

{{ object|pprint }}： 显示一个对象的值

{{ 列表|random }}： 返回列表的随机一项

{{ string|removetags:"br p div" }}： 删除字符串中指定html标记

{{ string|rjust:"50" }}： 把字符串在指定宽度中对右，其它用空格填充

{{ 列表|slice:":2" }}： 切片

{{ string|slugify }}： 字符串中留下减号和下划线，其它符号删除，空格用减号替换

{{ 3|stringformat:"02i" }}： 字符串格式，使用Python的字符串格式语法

{{ "E<A>A</A>B<C>C</C>D"|striptags }}： 剥去[X]HTML语法标记

{{ 时间变量|time:"P" }}： 日期的时间部分格式

{{ datetime|timesince }}： 给定日期到现在过去了多少时间

{{ datetime|timesince:"other_datetime" }}： 两日期间过去了多少时间

{{ datetime|timeuntil }}： 给定日期到现在过去了多少时间，与上面的区别在于2日期的前后位置。

{{ datetime|timeuntil:"other_datetime" }}： 两日期间过去了多少时间

{{ "abdsadf"|title }}： 首字母大写

{{ "A B C D E F"|truncatewords:"3" }}： 截取指定个数的单词

{{ "<a>1<a>1<a>1</a></a></a>22<a>1</a>"|truncatewords_html:"2" }}： 截取指定个数的html标记，并补完整

{{ list|unordered_list }}：//ul 多重嵌套列表展现为html的无序列表

{{ string|upper }} ：全部大写

<a href="{{ link|urlencode }}">linkage</a>： url编码

{{ string|urlize }}： 将URLs由纯文本变为可点击的链接。（没有实验成功）

{{ string|urlizetrunc:"30" }}： 同上，多个截取字符数。（同样没有实验成功）

{{ "B C D E F"|wordcount }}： 单词数

{{ "a b c d e f g h i j k"|wordwrap:"5" }}： 每指定数量的字符就插入回车符

{{ boolean|yesno:"Yes,No,Perhaps" }}： 对三种值的返回字符串，对应是 非空,空,None

加法
{{value|add:10}}
note:value=5,则结果返回15

减法
{{value|add:-10}}
note:value=5,则结果返回-5，加一个负数就是减法了

乘法
{% widthratio 5 1 100%}
note:等同于：(5 / 1) * 100 ，结果返回500，withratio需要三个参数，它会使用参数1/参数2*参数3的方式进行运算，进行乘法运算，使「参数2」=1

除法
{% widthratio 5 100 1%}
note:等同于：(5 / 100) * 1,则结果返回0.05,和乘法一样，使「参数3」= 1就是除法了。

### 关闭自动转义
{% autoescape off %}
    {{ ele.content |truncatewords_html:"2" }}
{% endautoescape %}
```