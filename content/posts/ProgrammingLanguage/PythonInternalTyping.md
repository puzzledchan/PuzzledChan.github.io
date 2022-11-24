---
title: "Python 内置类型"
date: 2022-11-24T19:44:24+08:00
draft: false
toc: true
tags: ["Python"] 
categories: ["编程语言"] 
slug: ""
---

## 数字类型 --- int/float/complex

## 迭代器类型
Python 支持在容器中进行迭代的概念。 这是通过使用两个单独方法来实现的；它们被用于允许用户自定义类对迭代的支持。

容器对象要提供 iterable 支持，必须定义一个方法:
``` python
container.__iter__()
```
返回一个 iterator 对象。 该对象需要支持下文所述的迭代器协议。 如果容器支持不同的迭代类型，则可以提供额外的方法来专门地请求不同迭代类型的迭代器。(支持多种迭代形式的对象的例子有同时支持广度优先和深度优先遍历的树结果。)此方法对应于 Python/C API 中 Python 对象类型结构体的 tp_iter 槽位。

迭代器对象自身需要支持以下两个方法，它们共同组成了 迭代器协议:
``` Python
iterator.__iter__()
```
返回 `iterator` 对象本身。 这是同时允许容器和迭代器配合 `for` 和 `in` 语句使用所必须的。 此方法对应于 Python/C API 中 Python 对象类型结构体的 tp_iter 槽位。

``` Python
iterator.__next__()
```
`iterator` 中返回下一项。 如果已经没有可返回的项，则会引发 `StopIteration` 异常。 此方法对应于 Python/C API 中 Python 对象类型结构体的 tp_iternext 槽位。

## 生成器类型
Python 的 generator 提供了一种实现迭代器协议的便捷方式。 如果容器对象 `__iter__()` 方法被实现为一个生成器，它将自动返回一个迭代器对象（从技术上说是一个生成器对象），该对象提供 `__iter__()` 和 `__next__()` 方法。

## 序列类型 --- list/tuple/range
有三种基本序列类型：`list`, `tuple` 和 `range` 对象。

通用的序列操作如下：
|运算|结果|
|:-|:-|
|x in s||