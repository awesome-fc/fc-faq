---
title: Custom Container 示例
description: 'Custom Container 示例'
position: 3
category: '概述'
---

# Custom Container 示例

```bash
# c++ 例子
s init start-fc-custom-container-http-cpp -d start-cc-http-cpp
s init start-fc-custom-container-event-cpp -d start-cc-event-cpp

# python
s init start-fc-custom-container-event-python3.9 -d start-cc-py39

# nodejs
s init start-fc-custom-container-event-nodejs14 -d start-cc-nodejs14

# springboot
s init start-fc-custom-container-http-springboot -d start-cc-http-springboot

# asp.netcore
s init start-fc-custom-container-http-aspdotnetcore -d start-cc-http-aspdotnetcore

# pytorch, 详情见: [start-pytorch](https://github.com/devsapp/start-pytorch)
s init devsapp/start-pytorch -d start-pytorch

# tensorflow, 详情见: [start-tensorflow](https://github.com/devsapp/start-tensorflow)
s init devsapp/start-tensorflow -d start-tensorflow

# OCR, 详情见: [start-ocr](https://github.com/devsapp/start-ocr)
s init devsapp/start-ocr -d start-ocr

# word 转 pdf, 详情见: [基于 LibreOffice 实现 word 转 pdf](https://github.com/devsapp/start-word2pdf)
s init devsapp/start-word2pdf -d  start-word2pdf

# puppeter 应用示例, 详情见: [puppeter 应用示例](https://github.com/devsapp/start-puppeteer)
# 提供了两种部署方式， nodejs12 runtime 和 custom container， 直接使用 custom container
s init devsapp/start-puppeteer -d start-puppeteer

# 各种 WEB 框架一键迁移，参考 https://github.com/devsapp/Application-Awesome

```

## Custom Runtime 示例

`s init start-fc-custom-samples -d start-fc-custom-samples`

初始化进入对应的目录后， 有各种语言的示例， 然后查看对应的 readme 即可, 支持的语言示例如下:

![](https://img.alicdn.com/imgextra/i3/O1CN016q9iRz26GQGy1dlGP_!!6000000007634-2-tps-1164-1632.png)
