---
layout: post
title:  "Сортировка выбором / Selection Sort на Python"
description: "Сортировка выбором на Python"
date:   2015-05-06 10:40:00
categories: python algorithms
permalink: /blog/selection-sort
---

В предыдуших частях мы говорили о [линейном поиске](/blog/linear-search) и [бинарном поиске](/blog/binary-search).

Сейчас речь пойдет о сортировке выбором, задача которой состоит в следующем: надо так поменять местами элементы массива, чтобы каждый элемент был меньше или равен последующему за ним.

<!--more-->

Предположим, у нас есть массив:

{% highlight python %}
[4, 2, 0, 1, 5, 3]
{% endhighlight %}

Нам надо привести его к виду:

{% highlight python %}
[0, 1, 2, 3, 4, 5]
{% endhighlight %}

Сначала мы проходимся по всему массиву и находим самый наименьший элемент. Далее мы меняем этот элемент местами с элементом, занимающим индекс 0. 

Теперь в массиве первый элемент - наименьшее число.

{% highlight python %}
[0, 2, 4, 1, 5, 3]
{% endhighlight %}

Затем мы снова проходимся по массиву, начиная уже с 1ого индекса, опять находим наименьшее число и меняем его местами с элементом под индексом 1.

{% highlight python %}
[0, 1, 4, 2, 5, 3]
{% endhighlight %}

Тоже самое делается для индекса 2, 3 и тд. 

Код на Python выглядит следующим образом:

{% highlight python %}
def selection_sort(lst):
	n = len(lst)
	i = 0

	while i < n - 1:
		smallest = i
		j = i + 1
		
		while j < n:
			if lst[j] < lst[smallest]:
				smallest = j # находим наименьшее число
			j+=1
		
		lst[i], lst[smallest] = lst[smallest], lst[i] # меняем местами, наименьшее "вначало"

		#print(lst)

		i+=1
	
	return lst
{% endhighlight %}

Надеюсь, что вы помните про метод **sort()**, который и надо использовать для таких задач.

Если раскоментировать принт, то в выводе будет полный пример того, как сортируется наш массив:

{% highlight python %}
[0, 2, 4, 1, 5, 3]
[0, 1, 4, 2, 5, 3]
[0, 1, 2, 4, 5, 3]
[0, 1, 2, 3, 5, 4]
[0, 1, 2, 3, 4, 5]
{% endhighlight %}

По традиции, напишем тесты, чтобы удостовериться, что код хоть немного работает:

{% highlight python %}
class TestSelectionSort(unittest.TestCase):

	def setUp(self):
		self.lst = [100, 10, 1000, 1]
		self.lst_sorted = [1, 10, 100, 1000]

	def testResult(self):
		self.assertEqual(selection_sort(self.lst), self.lst_sorted, 'Problem with sorting')

	def testSortedResult(self):
		self.assertEqual(selection_sort(self.lst_sorted), self.lst_sorted, 'Problem with sorting')
{% endhighlight %}
