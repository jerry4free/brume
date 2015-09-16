---
layout: post
title: python--协程
---

###概念
协程就是协作的例程。
根线程有点不同，线程是操作系统级别的，是由操作系统去调度的，抢占式的；
而程是存在于用户空间，协程之间通过yield方式转移执行权，它们由用户去调度的，是协作式的，通常无法利用多核。

###基于生成器的协程

        def echo(value=None):
            print "execution starts when 'next()' is called "
            try:
                while True:
                    try:
                        value = (yield value)
                    except Exception, e:
                        value = e
            finally:
                print "Don't forget to clean up when 'closed()' is called"


        >>> from generator1 import echo
        >>> a = echo(2)
        >>> a
        <generator object echo at 0x10d62f640>
        >>> a.next()
        execution starts when 'next()' is called
        2
        >>> a.next()
        >>> print a.next()
        None
        >>> print a.next()
        None
        >>> print a.send(3)
        3
        >>> print a.send(4)
        4
        >>> a.throw(TypeError, 'spam')
        TypeError('spam',)
        >>> print a.next()
        None
        >>> a.close()
        Dont forget to clean up when 'closed()' is called
        >>> a
        <generator object echo at 0x10d62f640>
        >>> a.next()
        Traceback (most recent call last):
          File "<stdin>", line 1, in <module>
        StopIteration

value = (yield value)语句中，只是执行了yield value这个表达式，赋值操作并未执行。
所以第二次调用next()时，value被赋值为yield表达式的值，yield表达式的值就是None。
所以后续value的值就是None.

send()方法的作用是控制yield表达式的返回值，使其返回值就是它的实参，next()就相当于send(None).

当调用close()方法时，yield表达式会抛出GeneratorExit异常。


        def consumer():
            while True:
                line = yield
                print line.upper()

        def producter(filenam ):
        with open(filename, 'r') as f:
            for i, line in enumerate(f):
                yield line
                print 'processed line %d' % i


        filename = './property1.py'
        c = consumer()
        c.next()
        for line in producter(filename):
            c.send(line)


###greenlet
greenle是一个用C语言编写的库，与yield没有关系。但是它实现协程。

###优点、应用场景
协程虽然不能利用多核，但它根异步IO结合编写I/O密集型应用非常容易。代表是greenlet与libevent/libev结合的gevent库，性能提升很高。
