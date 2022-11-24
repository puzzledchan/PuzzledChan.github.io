---
title: "λ 演算中组合子的启发式推导(四)"
date: 2022-11-24T12:26:06+08:00
draft: false
toc: true
mathjax: true
dropCap: true
tags: ["lambda演算"] 
series: ["lambda 演算"]
categories: ["计算机科学"]
---

## $\lambda$ 演算组合子
λ 演算可以通过组合子进行各种复杂的逻辑运算与代数运算，包括重复、递归等等。然而组合子的推导
过于复杂，因此参考了一些启发式推导的方法[^1]。

## Y 组合子启发式推导
递归函数的基本形式：
~~~ js
let f = x => (函数体中用f表示递归函数)
// 阶乘
let fact = n => n < 2 ? 1 : n * fact(n-1)
~~~
递归函数的实现体中通过函数名引用了自身。若希望消除对自身的引用，则必须将其转化为参数，得到：
~~~ js
let g = f => x => (函数体中用f表示递归函数)
~~~
函数$g$在$f$的基础上添加一层，成为高阶函数；当函数$g$作用在$f$上，则有：
~~~ js
g(f) = x => (函数体中的f通过闭包变量引用了参数f)
~~~
显然，希望$g(f) = f$时，得到递归函数。
指定构造匿名递归函数的标准程序：
1. 根据命名函数f定义辅助函数g
2. 求解辅助函数g的不动点

假设存在标准解法得到$g$的不动点，记解法为$Y$：
~~~ 
f = Y g ==> Y g = g(Y g)
~~~
迭代则产生：
~~~
f = g(f) =g(g(g...))
~~~
启发式的构造：
~~~
f = g(g(g...))=G(G)=g(f)=g(G(G))
~~~
通过`G(G)=g(G(G))`构造，构造并不唯一：
~~~ sh
G(G) = g(G(G)) ==> G = λG. g(G(G)) = λx. g(x x)
~~~
那么有：
~~~ sh
Y g = f = G(G) = (λx.g (x x)) (λx.g (x x))  
Y = λg. (λx.g (x x)) (λx.g (x x)) 
Y = λf. (λx.f (x x)) (λx.f (x x))          
~~~

## 递归的本质 G(G)
递归函数，内部必然存在着两种结构：递归结构和计算结构。如：
~~~ js
let fact = n => n < 2 ? 1 : n * fact(n-1)
// 或者写成函数声明的形式
function fact(n){
    return n < 2 ? 1: n*fact(n-1)
}
~~~
试图将递归结构与计算结构分离：
~~~ js
function metafact(fact, n){
    return n < 2 ? 1 : n * fact(n-1)
}
// 或者使用lambda表达式形式
const metafact = fact => n => n < 2 ? 1 : n * fact(n-1)
~~~
若递归结构可被抽象到算子$Y$中，则可通过如下形式为`metafact`增加递归结构：
~~~ sh
Y(metafact)(n) = fact(n), fact = Y(metafact)
~~~
`metafact`是已知的结构，现在需要求取`fact`使得其成立。
~~~ js
const factAss = factAss => n => n < 2 ? 1 : n * factAss(factAss)(n-1)
~~~
验证：
~~~ sh
Y (metafact)(n) = fact(n) = factAss(factAss)(n) 
~~~

## Y 组合子 vs. Z 组合子
Y 组合子表达式为 $\lambda f.(\lambda x.f (x x)) (\lambda x.f (x x))$，在 `JavaScript`中，表示如下：
~~~ js
// Y组合子：λf.(λx.f (x x)) (λx.f (x x))
const Y = f => (x=> f(x(x)))(x=> f(x(x)))
~~~
然而在`JavaScript`无法直接使用，会报堆栈溢出的错误，因为`JavaScript`采用`Call By Value`的调用约定，参数传入前会先计算值；因此考虑采用形式为$\lambda f.(\lambda x.f (\lambda y. (x x) y)) (\lambda x. f (\lambda y. (x x) y))$的 Z 组合子，具体如下[^2]：
~~~ js
// Z组合子：λf.(λx.f (λy. (x x) y)) (λx. f (λy. (x x) y))
const Z = f => (x => f(y => x(x)(y)))(x => f(y => x(x)(y)))

g = f => n => {
  if (n === 0) return 0
  return n + f(n - 1)
}
Z(g)(10) // => 55
~~~

---
[^1]: [Y组合子的一个启发式推导](https://zhuanlan.zhihu.com/p/547191928)https://zhuanlan.zhihu.com/p/547191928
[^2]: [十分钟速通 Y Combinator](https://zhuanlan.zhihu.com/p/51856257)https://zhuanlan.zhihu.com/p/51856257