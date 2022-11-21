---
title: "Python 虚拟环境 Anaconda3"
date: 2022-11-21T17:13:58+08:00
draft: false
toc: false
tags: ["Python"] 
categories: ["配置"] 
slug: ""
---

## anaconda
- Linux环境
   1. [清华源](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive)下载
   ``` bash
   wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2022.10-Linux-x86_64.sh
   ```
   2. 安装
   ``` bash
   sh Anaconda3-2022.10-Linux-x86_64.sh
   ``` 
   3. 环境变量配置
   ``` bash
   ./anaconda3/bin/conda init 
   ```
- 常用的命令
   * 查看环境列表：
   ``` bash
   conda env list
   conda info --env 
   conda info -e
   ```
   * 创建虚拟环境
   ``` bash
   conda create -n env_name python = version
   ```
   * 删除虚拟环境
   ``` bash
   conda remove -n env_name --all
   ```
   * 激活虚拟环境
   ``` bash
   conda activate env_name
   ```
   * 退出虚拟环境
   ``` bash
   conda deactivate
   ```