---
title: "Git 基础和常见指令"
date: 2022-11-21T18:14:11+08:00
draft: false
toc: true
tags: ["Git"] 
categories: ["计算机工程技术"] 
slug: ""
---

# Git 介绍
在大型项目的开发中，往往涉及到多人合作，迅速同步；即使在个人的开发过程中，涉及到多台机器开发的场景也并不少见。因此，采用 [Git](https://git-scm.com/) 工具进行代码的分布式管理、云端同步是最佳方案。
本笔记主要是 Git 的基础命令的学习与记录。

## Git 常用命令速查表
![Git 指令](/images/ComputerSkills/gitcommand.jpg)

## Git 本地命令
1. 创建项目目录；
2. 通过`git init`初始化版本库；
3. 提交文件到仓库：
    ~~~ bash
    git add . #将文件提交到仓库，准确来说add提交的是修改
    git commit -m " " # 本次提交的说明
    ~~~
4. 通过`git status`查看仓库状态:
   - unstaged: 未添加到版本库(处于工作区)
   - staged：已添加到版本库但暂未提交(处于暂存区)
   - commited：已提交
5. 通过`git diff`查看文件差异；
6. 通过`git log`查看提交的历史记录：
    ~~~ bash
    git log --oneline  # 按行显示所有的hash值
    git log --pretty=oneline
    git log --graph # 查看分支合并图
    git log --graph --pretty=oneline --abbrev-commit # 查看所有版本的hash值
    ~~~
7. 版本回退：
   - 在git中`HEAD`表示当前版本，上一个版本为`HEAD^`，上两个版本为`HEAD^^`，那么前`n`个版本则为`HEAD~n`。
   - 通过`git reset`进行版本回退，在回退过程中，若想回复到之前的状态，可通过`git reflog`获取之前的版本号进行回复。
    ~~~ bash
    git reset --hard commit_id # 工作区变为对应版本的内容；新增的文件丢失，删除的文件恢复
    git reset --soft commit_id # 移动HEAD指针；工作区和暂存区未发生变化
    git reset --mixed commit_id # 移动HEAD指针；工作区未发生变化，暂存区发生变化
    ~~~
8. 撤销修改：
   ~~~ bash
   git checkout -- file #撤销文件修改，至版本库的内容
   git checkout branch #切换分支
   ~~~
   若已经 commit，则采用上述的`git reset`命令。
9. 删除文件：
    ~~~ bash
    git rm files && git commit # 删除版本库中的文件并提交
    git checkout  -- file # 误删文件后从版本库中恢复
    ~~~

## Git分布式版本控制命令
1. 获取远程仓库：
   ~~~ bash
   git remote add origin git@server-name:path/repo-name.git # 添加远程仓库并设定仓库名为origin
   git clone # 克隆远程仓库
   ~~~
2. 推送本地库至远程库：
   ~~~ bash
   git push -u origin main # 关联本地main分支和远程main分支
   git push origin branch # 本地分支推送到远端
   git push --force origin main # 强制推送本地到远端
   git push origin --delete branch # 删除远端分支
   ~~~
3. 删除本地库与远程库的联系：
   ~~~ bash
   git remote -v # 查看远程库信息
   git remote rm origin # 删除本地的远端库
   ~~~
4. 分支开发：
   - 创建分支并更改当前分支位置：
        ~~~ bash
        git switch -c dev # 创建并切换到新的dev分支
        #---等价于---#
        git checkout -b dev
        #---等价于---#
        git branch dev
        git checkout dev
        ~~~
   - 切回主分支并合并：
        ~~~ bash
        git checkout main
        git merge dev # 将dev分支与main分支合并
        git merge --no-ff -m " " dev # 由于参数 --no--ff，此时禁止 fast forward合并；fast forward可能导致合并的信息丢失
        ~~~

    - 删除分支
        ~~~ bash
        git branch -d dev # 删除分支
        git branch -D dev # 删除一直未合并的分支
        ~~~
5. 临时切换分支时，若存在未提交的文件，需要保存现场：
   ~~~ bash
   git stash # 保存现场至一个栈结构，此时工作区是干净的
   git stash list # 查看所有stash的内容

   git stash pop # 恢复现场并删除栈顶
   #---等价于---#
   git stash apply # 恢复
   git stash drop # 删除

   git stash apply stash@{n} # 恢复指定的stash
   ~~~
6. 通过 `git cherry-pick`复制某个特定的提交。
7. 多人协作工作模式：
  * 通过`git push origin branch`推送修改；
  * 若推送失败，采用`git pull`合并，若拉取失败则关联分支`git branch --set-upstream-to=origin/dev dev`；
  * 若合并存在冲突，则解决冲突并在本地提交；
  * 提交后，进行`git push origin branch`推送。
   