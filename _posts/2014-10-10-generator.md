---
layout: post
title: python--生成器、迭代器
---

###1.迭代器：遵循迭代协议的对象

迭代器是含有.next()方法的对象。调用next()时，返回序列中的下一个项目，当无项目可返回时，raise StopIteration异常.
用户自定义的类，实现next()方法并__iter__()返回self，就是一个迭代器。如下：

		class Reverse:
		    """Iterator for looping over a sequence backwards."""
		    def __init__(self, data):
		        self.data = data
		        self.index = len(data)
		    def __iter__(self):
		        return self
		    def next(self):
		        if self.index == 0:
		            raise StopIteration
		        self.index = self.index - 1
		        return self.data[self.index]
		        
		        
		>>> rev = Reverse('spam')
		>>> for char in rev:
		...     print char
		...
		...
		m
		a
		p
		s
		>>>
用iter()可以将任何序列编程一个迭代器。

###2.生成器：

生成器是这样一个函数，它记住上一次返回时在函数体中的位置。对生成器函数的第二次（或第 n 次）调用跳转至该函数中间，而上次调用的所有局部变量都保持不变。

定义：如果一个函数使用了yield语句，那么它就是一个生成器函数。生成器可以用普通函数来实现，但使用yield语句会更简单。

		>>> def fib(n):
		...     a, b = 1, 1
		...     while a < n:
		...         yield a
		...         a, b = b, a + b
		...
		...
		...
		>>> fib(10)
		<generator object fib at 0x101196d20>

调用fib(10)会返回一个生成器对象，它的函数体并不执行，而是等到第一次调用next()时才执行。
		
		>>> dir(f)
		['__class__', '__delattr__', '__doc__', '__format__', 
		'__getattribute__', '__hash__', '__init__', '__iter__', 
		'__name__', '__new__', '__reduce__', '__reduce_ex__', 
		'__repr__', '__setattr__', '__sizeof__', '__str__', 
		'__subclasshook__', 'close', 'gi_code', 'gi_frame', 
		'gi_running', 'next', 'send', 'throw']

除了有yield的函数以外，产生生成器的另一种方法是通过列表推导的方式，只不过将其放到括号中。如(i for i in num)
这个对象带有__iter__()和next()方法，所以它同时也是一个迭代器。

###3.生成器和迭代器的关系：

迭代器不是生成器；但是生成器可以实现迭代协议，在一定程度上可以看做是迭代器。

###4.生成器的作用:

1. 快速产生迭代器
2. 实现with语句的上下文管理器协议, 利用的是调用生成器函数时函数体并不执行，第一次调用next()方法才执行，并执行到yield后中止，，直到下一次调用next()方法才执行 
3. 实现协程，利用的是send(),throw(),close()等特性



---


