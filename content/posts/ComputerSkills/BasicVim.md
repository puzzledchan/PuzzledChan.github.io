---
title: "Vim 设计哲学与基本操作"
date: 2022-11-21T18:30:15+08:00
draft: false
toc: true
tags: ["vim"] 
categories: ["计算机工程技术"] 
slug: ""
---
                
## Vim 基础教程
### Vim键位图
![cheat sheet](/images/ComputerSkills/vi-vim-tutorial-svg/vi-vim-cheat-sheet-sch.jpg)

### 基本编辑指令
![cheat sheet](/images/ComputerSkills/vi-vim-tutorial-svg/vi-vim-tutorial-1.svg)

### 操作和重复
![cheat sheet](/images/ComputerSkills/vi-vim-tutorial-svg/vi-vim-tutorial-2.svg)

### 拷贝和粘贴
![cheat sheet](/images/ComputerSkills/vi-vim-tutorial-svg/vi-vim-tutorial-3.svg)

### 搜索指令
![cheat sheet](/images/ComputerSkills/vi-vim-tutorial-svg/vi-vim-tutorial-4.svg)

### 标记和宏
![cheat sheet](/images/ComputerSkills/vi-vim-tutorial-svg/vi-vim-tutorial-5.svg)

### 各种移动
![cheat sheet](/images/ComputerSkills/vi-vim-tutorial-svg/vi-vim-tutorial-6.svg)

### 各种命令
![cheat sheet](/images/ComputerSkills/vi-vim-tutorial-svg/vi-vim-tutorial-7.svg)

## Vim 即语言
Vim 在某种意义上可以理解为语言，拥有名词、动词、副词等属性。


### 动词
动词值执行在对象上的动作，可施加于名词之上，常见动作如下：
|动词|含义|
|:-:|:-:|
|d|删除|
|c|修改|
|y|拷贝|
|v|可视化|

### 修饰语
修饰语用在名词之前，表明执行动作的方式，常见修饰语如下：
|定语|含义|
|:-:|:-:|
|i|内部|
|a|周围|
|Num|数字|
|t|查找指定字符，并跳转到字符前|
|f|查找指定字符，并跳转到字符位置|
|/|查找字符串|

### 名词
在语言中，名词表示操作对象，均为客体。常见的名词如下：
|名词|含义|
|:-:|:-:|
|w|单词|
|s|句子|
|)|句子，另一种操作方式|
|p|段落|
|}|段落，另一种操作方式|
|t|标签(HTML/XML)|
|b|块(编程语言)|
> Nouns as motion：名词也可视作移动动作。

### 文本对象总结
|文本|表示|
|:-:|:-:|
|单词|iw、aw|
|句子|is、as|
|段落|ip、ap|
|单引号|i'、a'|
|双引号|i"、a"|
|圆括号|i(、a(|
|方括号|i[、a[|
|大括号|i{、a{|
|标签|it、at|

## Vim 命令

### 移动命令
1. 基本移动
|光标移动|含义| 
|:-:|:-:|
|h|左移一个字符|
|j|下移一行|
|k|上移一行|
|l|右移一个字符|

2. 行内移动
|光标移动|含义| 
|:-:|:-:|
|0|移动到行首|
|$|移动到行末|
|^|移动到行首非空|
|t{character}|移动到下一个{character}前|
|t{character}|移动到下一个{character}位置|

3. 名词移动
|光标移动|含义| 
|:-:|:-:|
|w|移动到下一个单词|
|W|移动到下一个大单词|
|b|移动到前一个单词|
|W|移动到前一个大单词|
|e|移动到当前单词末尾|
|)|移动到下一条句子|
|}|移动到下一个段落|
4. 屏幕间移动
   
5. 来回跳转
|输入|作用| 
|:-:|:-:|
|Ctrl-i|跳到之前的位置|
|Ctrl-o|跳回实际的位置|
6. 其余动作
|输入|作用| 
|:-:|:-:|
|:$line_num|移动到指定行号|
### 基础修改/插入命令



## Vim 可视化模式

