---
title: "λ 演算的基本规则(一)"
date: 2022-11-23T15:01:07+08:00
draft: false
toc: true
mathjax: true
dropCap: true
tags: ["lambda演算"] 
series: ["lambda 演算"]
categories: ["计算机科学"] 
slug: ""
---

##  $\lambda$ 演算介绍
λ 演算是一套从数学逻辑中发展，以变量绑定和替换的规则，来研究函数如何抽象化定义、函数如何被应用以及递归的..形式系统..。 它由数学家阿隆佐·邱奇在20世纪30年代首次发表。Lambda演算作为一种广泛用途的计算模型，可以清晰地定义可计算函数，而任何可计算函数都能以这种形式表达和求值，它能模拟单一磁带图灵机的计算过程；尽管如此，$\lambda$ 演算强调的是变换规则的运用，而非实现它们的具体机器。$\lambda$ 演算伟大的原因有很多[^1]，诸如：
- 简单；
- 图灵完备；
- 易于读写；
- 语义强大；
- 具备良好的实体模型；
- 容易创建变种，以便探索各种构建计算或语义方式的属性。

$\lambda$ 演算建立在函数的概念的基础上。纯粹的 $\lambda$  演算中，一切都是函数，连值的概念都没有。但是，我们可以用函数构建任何我们需要的东西。

### $\lambda$ 表达式的语法
1. 函数定义：$\lambda$ 演算中的函数是表达式，形如：`lambda para. res`，表示“一个参数为`para`的函数，返回值为`res`的结果”，即 $\lambda$ 表达式绑定了参数`para`。
2. 标志符引用(Identifier reference)：标识符引用即一个名字，用于匹配函数表达式中的某个参数。
3. 函数应用(Function application)：函数应用将函数值放置在参数前面，如(lambda x. plus x x)y。
形式化表达如下：
~~~ bash
    <expr> ::= λ <identifier> . <expr>
    <expr> ::= <identifier> 
    <expr> ::= <expr> <expr>
~~~
注意[^2]：
- 加括号可以避免歧义；
- 未加括号时，函数应用的参数满足左结合；
- 未加括号时，函数体部分满足右结合，即尽可能向右扩展。


### 柯里化(currying)
Converting a function that takes multiple arguments into a single-argument higher-order function.

将带多个参数的函数转换为单参数高阶函数。currying来源于Haskell Curry逻辑学家，也是Haskell的命名来源currying可以说是高阶函数来将多个参数的函数变换成接收单一参数的函数，嵌套返回直到所有参数都被使用并返回最终结果。

> 把函数的层层调用和封装，变成了状态的转换。函数每调用一次，生成一个新的函数，即相当于迁移至了一个新的状态上。于是程序代码更加接近状态机，也会有助于程序结构的梳理。

### 自由标识符 vs. 绑定标识符
自由标识符即是不属于任何闭合的 $\lambda$ 演算表达式的参数；绑定标识符即属于闭合表达式的参数；类比形参和实参的定义。

一个 $\lambda$ 表达式只有在其所有变量都是绑定的时候才完全合法。但是，当我们脱开上下文，关注于一个复杂表达式的子表达式时，自由变量是允许存在的——这时候搞清楚子表达式中的哪些变量是自由的就显得非常重要了。


## $\lambda$ 演算的运算法则
$\lambda$ 演算的只有两条真正的法则：称为Alpha和Beta。Alpha也被称为「转换」，Beta也被称为「规约」。
1. $\alpha$ 转换：将函数定义中绑定的标识符进行命名的转换，如$\lambda x.x \leftrightarrow \lambda y.y$。
2. $\beta$ 规约：在函数应用中，将参数代入函数体中，如：$(\lambda x.x) a \rightarrow a$ 。
3. $\eta$ 代换：若两个函数对所有的输入都有相同的输出，可以进行 $\eta$ 转换。
  
---
[^1]: [λ 演算](https://cgnail.github.io/academic/)https://cgnail.github.io/academic/
[^2]: [函数构建抽象](https://zhuanlan.zhihu.com/p/527572849)https://zhuanlan.zhihu.com/p/527572849 


