---
layout: post
title:  "Рекурсивный импорт в Python"
description: "Рекурсивный импорт в Python"
date:   2015-05-27 09:40:00
categories: python 
permalink: /blog/python-recursive-import
---

На [27 Django Moscow Meetup](/blog/27-moscow-django-meetup) Николай Чабановский рассказывал про Stackoverflow на русском. 

Написал ответ на [вопрос](http://ru.stackoverflow.com/questions/426298/), ответ получился слишком обширным, на правах автора забираю сюда.

Вопрос был про рекурсивный импорт в Python.

<!--more-->

**recur1.py**:

{% highlight python %}
print('x = 1, hi from recur1! We are in recur1 file')
x = 1

print('import recur2... We are in recur1 file')
import recur2

print('y = 2, hi from recur1! We are in recur1 file')
y = 2
{% endhighlight %}

**recur2.py**:

{% highlight python %}
print('import x from recur1... We are in recur2 file')
from recur1 import x

print('import y from recur1... We are in recur2 file')
from recur1 import y

print('hi from recur2! We are in recur2 file')
{% endhighlight %}

В ходе импортирования инструкции в файле выполняются сначала и до конца.

Если выполним следующее:

{% highlight python %}
>>> import recur1
{% endhighlight %}

То получим ошибку:

{% highlight python %}
ImportError: cannot import name 'y'
{% endhighlight %}

Это происходит по очень простой причине. В модуле **recur1** сначала создается переменная **x**, а потом сразу импортируется **recur2**, в котором мы используем **from**.

Таким образом, у нас будет доступ только к тем именам, которые уже были определены в этом модуле. То есть доступ только к **x**, y не будет доступен, так как к нему будет присвоено значение после импорта в **recur1**.

{% highlight python %}
from recur1 import y # а у еще нет!
{% endhighlight %}

Лучше не использовать при рекурсивном импорте инструкцию **from**. При рекурсивном импорте интерпретатор не будет повторно выполнять инструкции.

Это легко можно проверить:

{% highlight python %}
>>> import recur2
import x from recur1... We are in recur2 file
x = 1, hi from recur1! We are in recur1 file
import recur2... We are in recur1 file
y = 2, hi from recur1! We are in recur1 file
import y from recur1... We are in recur2 file
hi from recur2! We are in recur2 file
{% endhighlight %}

Ошибки не происходит, потому что при первом импорте мы получаем **x**, потом снова импортируем из **recur1** **recur2**, повторного определения переменной **х** происходить не будет (см. выше), поэтому сразу получим **y**, и когда уже захотим импортнуть его из **recur2**, также повторно инструкции не будут выполняться.

Идем дальше:

{% highlight python %}
>>> from sys import modules
>>> 'recur1' in modules.keys()
False
>>> 'recur2' in modules.keys()
False
>>> import recur2
# вывод см. выше
>>> 'recur2' in modules.keys()
True
>>> 'recur1' in modules.keys()
True
{% endhighlight %}

И когда после этого мы захотим сразу сделать:

{% highlight python %}
>>> import recur1
{% endhighlight %}

То ошибки в 

{% highlight python %}
from recur1 import y
{% endhighlight %}

не будет, так как y уже будет объявлен в **recur1**:

{% highlight python %}
>>> help(modules['recur1'])
{% endhighlight %}

Вернет:

{% highlight python %}
NAME
    recur1

DATA
    x = 1
    y = 2
{% endhighlight %}

Если же запустим

{% highlight python %}
python recur1.py
{% endhighlight %}

Получим:

{% highlight python %}
x = 1, hi from recur1! We are in recur1 file
import recur2... We are in recur1 file
import x from recur1... We are in recur2 file
x = 1, hi from recur1! We are in recur1 file
import recur2... We are in recur1 file
y = 2, hi from recur1! We are in recur1 file
import y from recur1... We are in recur2 file
hi from recur2! We are in recur2 file
y = 2, hi from recur1! We are in recur1 file
{% endhighlight %}

Как пишет Лутц:

>Когда модули запускаются как самостоятельные программы, они не импортируются, поэтому здесь возникает тот же эффект, как и при импортировании recur2 в интерактивной оболочке, – recur2 является первым импортируемым модулем.

А при

{% highlight python %}
python recur2.py
{% endhighlight %}

В выводе увидим:

{% highlight python %}
import x from recur1... We are in recur2 file
x = 1, hi from recur1! We are in recur1 file
import recur2... We are in recur1 file
import x from recur1... We are in recur2 file
import y from recur1... We are in recur2 file
ImportError: cannot import name 'y'
{% endhighlight %}

То есть мы еще не объявили в **recur1** переменную **y**, поэтому ее и нет.