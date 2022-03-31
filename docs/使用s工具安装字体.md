---
title: 使用s工具安装字体
description: '使用s工具安装字体'
position: 9
category: '概述'
---

# 使用s工具安装字体
函数计算运行环境中内置一些常用字体，但仍不满足部分用户的需求。如果应用中需要使用其它字体，需要走很多弯路。本文将介绍如何通过 S 工具将自定义字体部署到函数计算，并正确的在应用中被引用。


## 工具安装
- 关于s工具可以点击查看相关介绍 https://help.aliyun.com/document_detail/195473.html
- 安装可以点击参考 https://help.aliyun.com/document_detail/195474.html
- 建议直接使用npm进行安装，执行：
```
npm install @serverless-devs/s -g
```

执行 s --version 检查 s工具 是否安装成功。其中local version代表的是你自己本地的版本，remote version是代表线上最新的版本
```
s --version

💻  local version : 2.0.80
☁️  remote version : 2.0.80
```

## 示例
demo 涉及的代码，可以点击<a href="https://fc-faq.oss-cn-hangzhou.aliyuncs.com/fc-font-demo/fonts.zip?versionId=CAEQHhiBgID98amX3RciIGJmMmQyYTEzMTA5OTQwN2JhNGYyZjExNTM2ZDViZWJl" download="fonts.zip">下载</a>。项目目录结构如下，
![](https://img.alicdn.com/imgextra/i1/O1CN01nfb6n71ia4TvwScNq_!!6000000004428-2-tps-720-414.png)
- index.js 中借助 node 包 font-list 列出系统上可用的字体。
- .fonts.conf文件是访问所需自定义字体文件的配置文件，无需更改。
- 在 s.yml 中添加环境变量，FONTCONFIG_FILE = /code/.fonts.conf，这样在函数运行时就可以正确的读取到自定义字体目录。(已经添加上了，无需在添加)
![](https://img.alicdn.com/imgextra/i2/O1CN01u0nrDN1Sjev92Vnj4_!!6000000002283-2-tps-916-650.png)

<br>

### 1、安装依赖
安装依赖可以通过s build进行安装，FC组件针对不同的语言分别支持一些包管理器manifest文件，执行s build后会通过对应的manifest文件进行安装，具体可以点击查看了解 [s build](https://github.com/devsapp/fc/blob/main/docs/Usage/build/build.md) 
```
s build
```
执行后的结果，如图
![](https://img.alicdn.com/imgextra/i3/O1CN01x0RAB51TRcwUgGBT2_!!6000000002379-2-tps-1020-750.png)

<br>

### 2、部署测试
执行s deploy进行部署到函数计算,通过生成的临时域名进行访问测试
```
s deploy
```
![](https://img.alicdn.com/imgextra/i4/O1CN01Kha50s1NkxyCm418F_!!6000000001609-2-tps-1602-586.png)
执行结果，可以看到自定义字体 Tangerine 已生效!
![](https://img.alicdn.com/imgextra/i2/O1CN01euv1PF1loDZ0cr1Lf_!!6000000004865-2-tps-1080-504.png)
