---
layout: post
title:  "Почему при объявлении класса в Python 2 используется унаследование его от object?"
description: "Унаследование класса python от object"
date:   2015-05-26 15:57:00
categories: python 
permalink: /blog/python-class-inherits-object
---

Ситуация достаточно стандартная. Наверное, многие замечали в **Django** примерно следующую конструкцию:

{% highlight python %}
from django.db import models

class Ox(models.Model):
	horn_length = models.IntegerField()

	class Meta:
		ordering = ["horn_length"]
		verbose_name_plural = "oxen"
{% endhighlight %}

Мы видим класс **Meta**, но после него нет наследования от **object**.

Объясняется все очень просто. 

<!--more-->

До Python 2.2 классы объявлялись так:

{% highlight python %}
class OldStyle():
	pass
{% endhighlight %}

Но в новой версии появились фичи, такие как дескрипторы, свойства и [многое другое](https://docs.python.org/release/2.2.3/whatsnew/sect-rellinks.html).

Таким образом, в Python 2.2, чтобы использовать для классов новинки, требовалось унаследоваться от **object**:

{% highlight python %}
class NewStyle(object):
	pass
{% endhighlight %}

**object** - это встроенный класс и объект. Экземпляры типа или класса **object** - это объекты. [Более подробно](http://habrahabr.ru/post/114585/).

В Python 3 старый стиль объявления классов был убран, так что наследоваться от **object** надо только в Python 2.

