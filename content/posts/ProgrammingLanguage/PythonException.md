---
title: "Python 异常处理 与 with 语句"
date: 2022-11-24T20:12:56+08:00
draft: false
toc: true
tags: ["Python"] 
categories: ["编程语言"] 
slug: ""
---

## Python 异常与处理方式
1. 捕获异常并处理`try except`:
~~~ python
try:
    code blocks may cause exception
except [(Error1, Error2... )[as e]]:
    process exception
except [(Error3, Error4... )[as e]]:
    process exception
except [Exception]
    process other exception
~~~
2. 捕获异常，若无异常则执行`else`：
~~~ python
try:
    code blocks may cause exception
except [Exception]
    process exception
else
    process code block
~~~
若发生异常，则不进入`else`。
3. 异常时资源回收工作`try except finally`：
~~~ python
try:
    code blocks may cause exception
except [Exception]
    process exception
else
    process code block
finally
    process code block
~~~
若发生异常，则不进入`else`；但一定进入`finally`。
4. 采用`raise`手动设置异常。

## with 语句
with 语句用于包装带有使用上下文管理器定义的方法的代码块的执行。 这允许对普通的 `try...except...finally` 使用模式进行封装以方便地重用。

`with`的执行过程如下：
1. 对上下文表达式（在 with_item 中给出的表达式）进行求值来获得上下文管理器。
2. 载入上下文管理器的 __enter__() 以便后续使用。
3. 载入上下文管理器的 __exit__() 以便后续使用。
4. 发起调用上下文管理器的 __enter__() 方法。
5. 如果 with 语句中包含一个目标，来自 __enter__() 的返回值将被赋值给它。
6. 执行语句体。
7. 发起调用上下文管理器的 __exit__() 方法。 如果语句体的退出是由异常导致的，则其类型、值和回溯信息将被作为参数传递给 __exit__()。 否则的话，将提供三个 None 参数。
以下代码：
~~~ python
with EXPRESSION as TARGET:
    SUITE
~~~
在语义上等价：
~~~ python
manager = (EXPRESSION)
enter = type(manager).__enter__
exit = type(manager).__exit__
value = enter(manager)
hit_except = False

try:
    TARGET = value
    SUITE
except:
    hit_except = True
    if not exit(manager, *sys.exc_info()):
        raise
finally:
    if not hit_except:
        exit(manager, None, None, None)
~~~