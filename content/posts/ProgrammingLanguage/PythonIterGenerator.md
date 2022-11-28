---
title: "Python 生成器、迭代器与可迭代对象"
date: 2022-11-25T15:40:46+08:00
draft: false
toc: true
tags: ["Python"] 
categories: ["编程语言"] 
slug: ""
---

## 生成器、迭代器与可迭代对象的关系
在Python中对象是否可迭代是一个十分重要的特性。如何判断一个对象是否是可迭代的，什是迭代器，以及什么是又生成器？
$$
生成器 \subset 迭代器 \subset 可迭代对象
$$


## 可迭代对象
一个实现了`__iter__()`方法的对象就是可迭代对象。通俗的讲指可以用for循环遍历的对象均为可迭代对象。判断一个对象是不是可迭代，有两种方法：
1. 通过`dir`获取某个对象所有的属性和方法，只要这个对象实现了`__iter__`方法，那么它就是可迭代对象。
2. 通过`isinstance()`函数，判断一个对象是否是`iterable`类型。
验证可迭代对象：
~~~ python
from collections import Iterable
class MyIter:
    def __iter__(self):
        pass
my_iter = MyIter()
print(isinstance(my_iter, Iterable))
# 输出 True
for i in my_iter:
    pass
# 输出 TypeError: iter() returned non-iterator of type 'NoneType'
~~~
上述代码证明不是所有的..可迭代对象..都可以被for循环遍历，前提是必须正确的实现`__iter__()`方法，让可迭代对象返回一个迭代器。

## 迭代器(Iterator)

官方定义如下：
> 1.迭代器是一个对象，该对象代表了一个数据流。
> 2.重复调用迭代器的__next__方法(或将迭代器对象当作参数传入內置函数next()中)将依次返回数据流中的元素。
> 3.当数据流中无可返回元素时，则抛出StopIteration异常。
> 4.迭代器必须拥有__iter__方法，该方法返回迭代器对象自身。

以列表`list`为例：
~~~ python
# for循环遍历list的代码如下：
my_list = [1, 2, 3]
for i in my_list:
    print(i)
# 输出 1， 2， 3
~~~
验证`list`是否未迭代器：
~~~ python
my_list = [1, 2, 3]
next(my_list)
# 输出 TypeError: 'list' object is not an iterator
~~~
由此观之，`list`并不是一个可迭代对象。那么为什么可以通过`for`对`list`遍历呢？
因为在遍历时，`Python`底层调用了`iter()`方法，获取对应的迭代器；随后在迭代器上操作`next`函数。
因此，`for`遍历`list`等价于：
~~~ python
my_iter = iter(my_list)
while True:
    try:
        print(next(my_iter))
    except StopIteration:
        break
~~~
迭代器在Python中最明显的是用于减少内存，以文件对象为例：
~~~ python
## 一次性加载
with open("test.txt") as f:
    data = f.readlines()
for line in data:
     pass
## 迭代器优化
with open("test.txt") as f:
  for line in f:
    pass
~~~
因为文件对象`f`是一个迭代器，所以可以对其使用循环遍历。而循环遍历时第一步就是调用`iter()`函数获取到f到迭代器对象(此处f到迭代器对象就是自身)，接下来就是每次循环的时候调用`__next__()`函数来获取下一行。所以利用迭代器可以很大程度的节省内存，只有在调用`next()`方法时才会去获取下一个元素，这样可以避免一次性的加载很大的对象导致占用内存过多，而是在需要时才进行..惰性计算..。
## 生成器(Generator)
生成器是特殊的迭代器。生成器在每次调用`__next__`方法生成一个元素，因此称之为生成器。
生成器在Python中，较之于迭代器更轻量，更优雅，主要有两种形式：
1. 通过函数创建
2. 通过推导式创建：`(i*2 for i in range(10))`
注：不同于列表生成器`[i*2 for i in range(10)]`一次性计算，推导式的生成器是..惰性计算..。

生成器对象是一种特殊的Iterator函数，它会在执行过程中保存执行的上下文环境，并在下次循环中从`yield`语句后继续执行，生成器的标志就是`yield`关键字。

## 示例：斐波那契数列
1. 传统方式：
~~~ python
def fibonacci_fun(num):
    numlist = [0,1]
    for i in range(num-1):
        numlist.append(numlist[-2]+numlist[-1])
    return numlist[1:]
res = fibonacci_fun(100000)
for i in res:
    print(i)
~~~
2. 迭代器：
~~~ python
class Fibonacci_Iterator:
    def __init__(self,counts):
        self.start = 0
        self.end = 1
        self.counts = counts
        self.time = 0
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.time >= self.counts:
            raise StopIteration
        else:
            self.time += 1
            self.start,self.end = self.end,self.start + self.end
            return self.start
          
f_iter = Fibonacci_Iterator(100000)
for i in f_iter:
     print(i)
~~~
3. 生成器
~~~ python
def fibonacci_generator(counts):
    start = 0
    end = 1
    for _ in range(counts):
        start,end = end,end+start
        yield start

f_gene = fibonacci_generator(100000)
for i in f_gene:
     print(i)
~~~
由上面实现可见[^1]：
- 传统方法求遍历解时需要一次性算出所有的元素，并将其保存在一个列表中返回；
- 迭代器求解遍历时只会在for循环每次隐式调用__next__()方法时才求解；
- 生成器的实现则最为优雅，代码执行到yield会暂停，把结果返回出来，再次隐式调用__next__()启动生成器的时候会在暂停的位置继续往下执行

---
[^1]: [如何更好地理解Python迭代器和生成器？](https://www.zhihu.com/question/20829330/answer/2320711618)https://www.zhihu.com/question/20829330/answer/2320711618