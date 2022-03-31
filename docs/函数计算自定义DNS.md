---
title: 函数计算自定义 DNS
description: '函数计算自定义 DNS'
position: 7
category: '概述'
---

# 函数计算自定义 DNS

函数计算现在支持配置自定义DNS,但是仅支持函数计算官方提供的Runtime和Custom Runtime [查看详情](https://help.aliyun.com/document_detail/359904.html) 

如果是 custom-container runtime， 直接在 Dockerfile 中修改 /etc/hosts

