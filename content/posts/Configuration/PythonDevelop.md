---
title: "Python 开发配置"
date: 2022-11-24T21:21:13+08:00
draft: false
toc: true
tags: ["Python"] 
categories: ["配置"] 
slug: ""
---

## 开发环境配置
由于平时习惯使用`VsCode`开发，因此尝试配置`Python`相关的开发环境，在基本插件下载完成后，
总是无法正确导入模块。因此，提供以下两种方案：

在`VsCode`选中`运行`，点击`添加配置`，于是得到`launch.json`，添加如下选项：
~~~ json
"env": {
    "PYTHONPATH": "${workspaceFolder}"
} 
~~~
或者在`settings.json`中添加配置：
~~~ json
"terminal.integrated.env.osx": {
        "PYTHONPATH":"${workspaceFolder}/"
}

"terminal.integrated.env.linux": {
        "PYTHONPATH":"${workspaceFolder}/"
}

"terminal.integrated.env.windos": {
        "PYTHONPATH":"${workspaceFolder}/"
}
~~~


## 虚拟环境 anaconda 安装与设置
- Linux环境
   1. [清华源](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive)下载
   ``` sh
   wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2022.10-Linux-x86_64.sh
   ```
   2. 安装
   ``` sh
   sh Anaconda3-2022.10-Linux-x86_64.sh
   ``` 
   3. 环境变量配置
   ``` sh
   ./anaconda3/bin/conda init 
   ```
- 常用的命令
   * 查看环境列表：
   ``` sh
   conda env list
   conda info --env 
   conda info -e
   ```
   * 创建虚拟环境
   ``` sh
   conda create -n env_name python = version # 3.8
   ```
   * 删除虚拟环境
   ``` sh
   conda remove -n env_name --all
   ```
   * 激活虚拟环境
   ``` sh
   conda activate env_name
   ```
   * 退出虚拟环境
   ``` sh
   conda deactivate
   ```
- 配置软件源
  * 查看与使用
  ``` sh
  conda install -c channel_name package_name # -c 等价于 -channel 选择下载源+包
  conda config --show channels # 显示所有的包
  ``` 
  * 添加清华源
  ``` sh
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
  ```
