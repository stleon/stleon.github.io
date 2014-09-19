---
layout: post
title:  "Линейный поиск / Linear search на Python"
description: "Линейный поиск на Python"
date:   2015-03-28 15:32:00
categories: python algorithms
permalink: /blog/linear-search
---

Представим себе обыкновенную ситуацию: у нас есть список, содержащий в себе множество элементов, например, строки. 

Нам требуется найти индекс какого-нибудь элемента, и мы ничего не знаем о порядке элементов в списке.

Для начала предлагаю написать свое решение, а потом сравнить. Так будет интереснее.
<!--more-->

В качестве примера возьмем часть ряда клавиш с клавиатуры.

{% highlight python %}
a = ['q', 'w', 'e', 'r', 't', 'y', 'u', 'i', 'o', 'p']
{% endhighlight %}

Создадим функцию, которая будет принимать список и искомое значение, индекс которого нужно будет вернуть. 

{% highlight python %}
def linear_search(lst, x):
	pass
{% endhighlight %}

Условимся, что будем двигаться по списку слева-направо, как и в реальной жизни, если бы искали книги на полке.

Самый простой алгоритм (и самый неправильный), который приходит в голову - это создать переменную **result**, которой заранее присвоим значение **None**, а далее в цикле **for** пройтись по всем индексам и сравнить элементы с искомым значением (**lst[index] = x**), и если данное условие выполняется, то **result = index**.

{% highlight python %}
def linear_search_1(lst, x):
	result = None
	for index, string in enumerate(lst):
		print(index, string)
		if string == x:
			result = index
	return result

print('='*10, linear_search_1(a, 'y'), sep='\n')
{% endhighlight %}

Процедура всегда возвращает правильный ответ, так почему она неэффективна? 

Как многие догадались, поиск в массиве будет продолжаться, даже после нахождения искомого индекса. Если запустим скрипт, то *output* будет следующим:
{% highlight python %}
0 q
1 w
2 e
3 r
4 t
5 y
6 u
7 i
8 o
9 p
==========
5
{% endhighlight %}

Не самое элегентное решение, тем более, если элементов в списке будет очень много. 

Теперь сделаем так, чтобы поиск прекращался, как только мы найдем значение **x**. 

{% highlight python %}
def linear_search_2(lst, x):
	i = 0
	while i < len(lst) and lst[i] != x:
		print(i)
		i += 1
	return i if i < len(lst) else None

print('='*10, linear_search_2(a, 'y'), sep='\n')
{% endhighlight %}

Если мы находим **x**, то цикл сразу прерывается, если же мы ничего не находим, то получаем **None**:

{% highlight python %}
0
1
2
3
4
==========
5
{% endhighlight %}

Но смущает то, что процедура выполняет по 2 проверки на каждой итерации - не вышли ли мы за границы списка и проверка равенства. 

Опять же вернемся к книжным полкам, вы каждый раз смотрите на книгу, проверяете ее название и то, не дошли ли вы до конца полки? <s>У всех умные девайсы и читалки</s>

Проще говоря, условие **i < len(lst)** из **while** надо бы убрать. 

Поместим элемент **x** в конец списка и будем точно уверены, что найдем искомый индекс. Если мы найдем "заменитель" (его еще называют ограничителем или барьером), то **x** в списке нет.

{% highlight python %}
def linear_search_3(lst, x):
	lst.append(x)
	i = 0
	while lst[i] != x:
		print(i)
		i += 1
	return i if i < len(lst) - 1 else None

print('='*10, linear_search_3(a, 'y'), sep='\n')
{% endhighlight %}

Ну и в конце напишем тесты, чтобы спать спокойно. Пишите свои предложения, если я что-то пропустил.

{% highlight python %}
class TestLinearSearch(unittest.TestCase):
	
	def setUp(self):
		self.lst = ['q', 'w', 'e', 'r', 't', 'y', 'u', 'i', 'o', 'p']
	
	def assertFunctions(self, *args, **kwargs):
		for func in args:
			result = func(self.lst, kwargs['str2find'])
			self.assertEqual(result, kwargs['index2find'], 'Problem with %s function' % func.__name__)

	def testMiddle(self):
		self.assertFunctions(linear_search_1, linear_search_2, linear_search_3, str2find='t', index2find=4)

	def testNotIn(self):
		self.assertFunctions(linear_search_1, linear_search_2, linear_search_3, str2find='1', index2find=None)

	def testEnd(self):
		self.assertFunctions(linear_search_1, linear_search_2, linear_search_3, str2find='p', index2find=9)
{% endhighlight %}

Код можно найти [здесь](https://github.com/stleon/algorithms).

Интересно? См. [Бинарный (двоичный) поиск](/blog/binary-search)