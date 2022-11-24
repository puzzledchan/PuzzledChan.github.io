---
title: "λ 演算的组合子(三)"
date: 2022-11-23T21:14:23+08:00
draft: false
toc: true
mathjax: true
dropCap: true
tags: ["lambda演算"] 
series: ["lambda 演算"]
categories: ["计算机科学"] 
---

## $\lambda$ 高级操作
λ 演算与图灵机等价，然而 λ 演算中并无函数的概念，也可理解为所有函数都是匿名的。众所周知，递归，是指在函数定义中调用函数自身的方法。那么在无函数名的 λ 演算中是如何实现递归的呢？
首先介绍一下递归：
>递归是指在函数的定义中使用函数自身的方法。从数学角度上，对应着归纳法，将解决的原问题转化为解决它的子问题，而它的子问题又变成子问题的子问题，同时存在相同的逻辑归纳处理项。

递归程序算法的设计要素：
- 明确递归的终止条件；
- 给出递归终止时的处理方法；
- 提取重复的逻辑，缩小问题的规模。

尾递归[^1]：
> 如果一个函数中所有递归形式的调用都出现在函数的末尾，我们称这个递归函数是尾递归的。即递归调用后返回的结果直接返回。
传统递归若递归链过长会导致运行效率低，甚至栈溢出。尾递归优化了传统递归的栈内存的问题。

## 重复 与 Y 组合子
### 问题的引入
首先看一个简单地阶乘递归函数：
~~~ sh
factorial(n) = 1 if n = 0 
factorial(n) = n * factorial(n-1) if n > 0 
~~~
那如何通过 $\lambda$ 演算实现呢？
可以先假定存在一个函数：
~~~ sh
let fact = lambda n . IsZero n 1 (Mult n ( something (Pred n))) 
~~~
该函数接受参数`n`，若 `n == 0` 则返回1，否则返回乘法的结果；从原有的方案上看，乘法为`n` * `fact n-1`的结果。如何获取`something`使其递归呢？

### 解决方案
答案是通过 $Y$ 组合子：
~~~
let Y = lambda y . (lambda x . y (x x)) (lambda x . y (x x))
~~~
$Y$ 组合子的形状像一个`Y`，如下图所示：
![组合子](/images/ComputerScience/combinators.jpg)
若用`javascript`表示[^2]：
~~~ javascript
λf.λx.(f(x x))(λx.(f(x x)))
// 写成js函数
(f => (x => f(x(x)))(x => f(x(x))))
~~~
$Y$ 组合子的特殊之处在与应用自身来创造自身：$(Y Y) = Y(Y Y) = Y (Y (Y Y))$，具体变化如下：
- Y Y
- 展开第一个Y：`(lambda y . (lambda x . y (x x)) (lambda x . y (x x))) Y`
- 现在，beta规约：`(lambda x . Y (x x)) (lambda x . Y (x x))`
- alpha[x/z]变换第二个lambda：`(lambda x . Y (x x)) (lambda z. Y (z z))`
- beta规约：`Y ((lambda z. Y (z z)) (lambda z. Y (z z)))`
- 展开前面的Y，并alpha[y/a][x/b]变换：`(lambda a . (lambda b . a (b b)) (lambda b . a (b b))) ((lambda z . Y (z z)) ( lambda z . Y (z z)))`
- beta规约：`(lambda b . ((lambda z. Y (z z)) (lambda z. Y (z z))) (b b)) (lambda b . ((lambda z. Y (z z)) (lambda z. Y (z z))) (b b))`
- 此刻可得：Y Y = Y (Y Y)
  

## 关于 Y 组合子在定义递归函数的思考
首先，在lambda演算中，函数名不是不可缺少的，没有函数名的函数称为「匿名函数」。lambda符号的引入就是为了去掉函数名这个冗余，使定义匿名函数成为可能。但是当需要定义的函数含有递归时，比如阶乘factorial，也就是函数的定义部分需要引用函数自身的时候，没有函数名意味着用lambda演算无法直接引用函数自身。怎么办呢？

一种办法是设计另一个函数G，它接受一个函数作为参数，返回值也是一个函数（这种参数是函数的函数称为高阶函数）。然后，我们把factorial当做参数传给G，如果G返回的函数也是factorial的话，就圆满了。也就是说，这个G需要满足两个特征：
G的定义中不会出现factorial，但是它可以接受factorial作为参数。回想一下一阶函数f(x) = x * x，它的定义里没有出现数字「1」，但是「1」可以传给它进行计算。而在构造G时，factorial就相当于数字「1」。
方程G(f)=f的解是factorial。这样我们就不用直接定义factorial，求解这个关于G的方程就可以得到factorial的定义了。
于是，我们需要干两件事：找到G，和找到求解G(f)=f的办法。寻找G很简单，既然我们想让G(factorial)=factorial，那么把factorial定义中关于factorial的引用参数化就可以了，即：
~~~ sh
let metafact = lambda f . lambda n . IsZero n 1 (Mult n ( f (Pred n))) 
~~~
这就是上面的「G」函数。这种构造方法可以用于构造任意递归函数的「G」。
~~~ sh
f = Y (g) 
    = (lambda y . (lambda x . y (x x)) (lambda x . y (x x))) g
    = (lambda x . g (x x)) (lambda x . g (x x)) 
    = (lambda x . f (x x)) (lambda a . g (a a))
    = g ((lambda a . g (a a)) (lambda a . g (a a)))
    = g ((lambda x . g (x x)) (lambda x . g (x x)))
    = g (Y(g))
~~~
于是，`factorial` 的定义可以写为：
``` sh
factorial n = (Y metafact) n 
            = {[lambda y . (lambda t . y (t t)) (lambda t . y (t t))]
               [lambda f . lambda n . IsZero n 1 (Mult n ( f (Pred n)))]} n
```
在`metafact`中不存在对自身的引用，只有未知的递归函数参数与递归函数入口的变量，通过求解`metafact`中的不动点，
得到了最终的函数结果。


---
[^1]: [高阶函数、柯里化、回调函数、递归](https://zhuanlan.zhihu.com/p/523100559)https://zhuanlan.zhihu.com/p/523100559
[^2]: [Lambda 演算基础](https://zhuanlan.zhihu.com/p/447398364)https://zhuanlan.zhihu.com/p/447398364