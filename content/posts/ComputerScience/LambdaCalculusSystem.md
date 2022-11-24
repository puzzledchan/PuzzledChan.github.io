---
title: "λ 演算构建基本的系统(二)"
date: 2022-11-23T16:19:13+08:00
draft: false
toc: true
mathjax: true
dropCap: true
tags: ["lambda演算"] 
series: ["lambda 演算"]
categories: ["计算机科学"] 
slug: ""
---


## $\lambda$ 演算构造系统
λ 演算与图灵计算等价，也可以构造相应的数字运算、逻辑运算等等。


## $\lambda$ 演算中的数字
### 皮亚诺自然数
皮亚诺自然数公理基于自然数集合 $N$ 和后继函数 $S$：
1. $0 \in N$
2. $x \in N \rightarrow S(x) \in N$
3. $x \in N \rightarrow S(x) \neq N$
4. $x \in N \land y \in N \land S(x)=S(y) \rightarrow x = y$
5. 对于任意的属性 $M$，通过归纳法以下性质成立：
$$
0 \in M \land \forall x(x\in M \rightarrow S(x)\in M)\rightarrow N \subseteq M
$$ 
### 丘奇数
所有的丘奇数都是带有..两个参数..的函数：
- 0 是`lambda s z . z`。
- 1 是`lambda s z . s z`。
- 2 是`lambda s z . s (s z) `。
- 对于任何数`n`，丘奇数表达为第一个参数应用到第二个参数上`n`次的函数。
  
那么如何做一个`x + y`的加法计算呢？现在就是见证奇迹的时候。
函数需要存在四个参数——两个相加的数字；两个用于推导的数字：
```
let add = lambda s z x y . x s (y s z)
```
将其进行柯理化：
```
let add = lambda x y . (lambda s z. (x s (y s z)))
```
该式表明，为了进行加法计算，首先利用`s`和`z`计算正则化的`y`，随后在应用`x`到`y`上，从而得到`x + y`的结果；该式接受两个参数，与此同时，这两个参数为丘奇数——接受两个参数的 $\lambda$ 表达式。
若计算 `2+3`，则：
``` sh
add (lambda s2 z2 . s2 (s2 z2)) (lambda s3 z3 . s3 (s3 (s3 z3))) 
```
基于定义替换：
``` sh
(lambda x y .(lambda s z. (x s y s z))) (lambda s2 z2 . s2 (s2 z2)) (lambda s3 z3 . s3 (s3 (s3 z3))) 
``` 
进行 $\beta$ 规约：
``` sh
lambda s z . (lambda s2 z2 . s2 (s2 z2)) s (lambda s3 z3 . s3 (s3 (s3 z3)) s z) 
```
对丘奇数`3`进行 $\beta$ 规约：
``` sh
lambda s z . (lambda s2 z2 . s2 (s2 z2)) s (s (s (s z))) 
```
最终，得到丘奇数`5`：
``` sh
lambda s z . s (s (s (s (s z)))) 
```

## $\lambda$ 演算中的布尔值与选择
在编程语句中，表达式`if/then/else`必不可少。因此，除了将数字表示为函数外，构造`TRUE`和`FALSE`的$\lambda$表达式也极为重要[^1]。
~~~ sh
let TRUE = lambda x y . x
let FALSE = lambda x y . y
~~~
`TRUE`表示接受两个参数，返回第一个参数的表达式；`FALSE`表示接受两个参数，返回第二个参数的表达式。
那么可以定义`if`函数：
~~~ sh
let IfThenElse = lambda cond true_expr false_expr . cond true_expr false_expr
IfThenElse(TRUE expr1 expr2)=TRUE expr1 expr2 = expr1
IfThenElse(FALSE expr1 expr2)=FALSE expr1 expr2 = expr2
~~~
定义常用的逻辑运算：
~~~ sh
let BoolAnd = lambda x y . x y FALSE 
let BoolOr = lambda x y. x TRUE y 
let BoolNot = lambda x . x FALSE TRUE 
~~~
具体例子：
~~~ sh
BoolAnd TRUE FALSE
≡ (λx y.x y FALSE)(TRUE FALSE)
≡ TRUE FALSE FALSE
≡ (λx y.x)(FALSE FALSE)
≡ FALSE

BoolOr FALSE TRUE
≡(λx y.x TRUE y)(FALSE TRUE)
≡ FALSE TRUE TRUE
≡ (λx y.x)(TRUE FALSE)
≡ TRUE

BoolNot TRUE
≡ (λx.x FALSE TRUE)(TRUE)
≡ TRUE FALSE TRUE
≡ (λx y.x)(FALSE TRUE)
≡ FALSE

BoolNot FALSE
≡ (λx.x FALSE TRUE)(FALSE)
≡ FALSE FALSE TRUE
≡ (λx y.y)(FALSE TRUE)
≡ TRUE
~~~
通过如上的方式，定了常用的条件选择。个人理解可以类比为`C++`中的模板操作，通过演算可以简化代码编写的量。


---
[^1]: [Lambda 演算](https://cgnail.github.io/academic/)https://cgnail.github.io/academic/