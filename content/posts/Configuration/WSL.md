---
title: "WSL 虚拟机安装"
date: 2022-11-21T18:11:36+08:00
draft: false
toc: false
tags: ["Linux"] 
categories: ["配置"] 
slug: ""
---

## WSL
WSL 是 Microsoft 提供的轻量级的 Linux 虚拟化平台，方便于在Windows操作系统下编写Linux相关的代码。
1. 下载
2. 安装
3. 网络配置
   ``` bash
   # Network Proxy
   export windows_host=`cat /etc/resolv.conf|grep nameserver|awk '{print $2}'`
   export ALL_PROXY=socks5://$windows_host:7890
   export HTTP_PROXY=$ALL_PROXY
   export http_proxy=$ALL_PROXY
   export HTTPS_PROXY=$ALL_PROXY
   export https_proxy=$ALL_PROXY

   if [ "`git config --global --get proxy.http`" != "socks5://$windows_host:7890" ]; then
               git config --global proxy.http socks5://$windows_host:7890
   fi
   if [ "`git config --global --get proxy.https`" != "socks5://$windows_host:7890" ]; then
               git config --global proxy.https socks5://$windows_host:7890
   fi
   ```
   此时若pip存在问题，须安装pysocks模块。
   ``` bash
   unset all_proxy && unset ALL_PROXY # 取消所有 socks 代理
   pip install pysocks
   ```