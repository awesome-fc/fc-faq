---
title: Serverles 网盘
description: '基于Serverles打造如Windows体验的个人专属家庭网盘'
position: 4
category: 'FC_Blog'
---

## 前言
随着全球大数据不断增长，未来数据云存储容量需求也将不断扩大，iiMedia Research(艾媒咨询)数据显示，2020年全球数据中心存储容量将达到272艾字节。不断扩大的个人数据和云储存需求加速了个人云服务市场的发展，2020年中国个人云盘用户规模有超过4亿人。

虽然现在市面上有些网盘产品， 如果免费试用，或多或少都存在一些问题， 可以参考 [2020国内还能用的网盘推荐](https://zhuanlan.zhihu.com/p/107343480)。 本文旨在使用较低成本打造一个 “个人专享的、无任何限速的、如Windows体验的私有云盘”。
## KodBox 遇见 Serverless
### 为什么是 KodBox？

调研了不少开源的 web ui filemanager, [kodbox](https://github.com/kalcaddle/kodbox)  深深打动了我， 功能丰富超出了我的想象，总结下来就是：
简单高效，流畅, 云端存储&协同办公新体验

- 如Windows体验的私有云盘/企业网盘
- 完全支持私有化部署，存储安全可控
- 数百种文件格式在线预览、编辑和播放
- 轻松分享，高效协作，细粒度权限管控
- 全平台客户端覆盖，随时随地访问，轻松同步挂载

更多详情可以参考 [kodbox中文网](https://kodcloud.com/#lang=zh_CN)

### 为什么选择 Serverless 托管 KodBox 应用？
网盘的操作时间就是比较离散的， 尤其是对于个人和家庭的网站， 常备一台机器（数据库也需要安装在本机， 不然还有单独的数据库费用）， 会产生大量的浪费， 比如凌晨大家都睡觉了，机器资源是闲置的。 而对于晚上 8 点， 家庭成员都在娱乐休闲的时候， 可用一台机器的资源又不太够用， 比如大家一起同时在线看不同的 4K 高清电影（当然每个人可以自己先快速下载到自己本地PC 机或者手机）。 而 Serverless 很好的解决了这个需求， 按量付费， 有请求随时扩容。

[阿里云函数计算](https://help.aliyun.com/document_detail/52895.html)是事件驱动的全托管计算服务。使用函数计算:

- 您无需采购与管理服务器等基础设施，只需编写并上传代码。
- 函数计算为您准备好计算资源，弹性地、可靠地运行任务。
- 按量付费、免运维
- 提供日志查询、性能监控和报警等功能。

借助函数计算，您可以快速构建任何类型的应用和服务，并且只需为任务实际消耗的资源付费。
![image.png](https://img.alicdn.com/imgextra/i3/O1CN01mw9qWt1skjudf9ALV_!!6000000005805-2-tps-772-324.png)

将 kodbox 项目部署到函数计算， 数据库持久化使用阿里云[文件存储](https://help.aliyun.com/product/27516.html)，内容存储使用阿里云[对象存储](https://help.aliyun.com/product/31815.html),  我们就得到一个专属的 " 计算+存储都可以 Serverless 无限扩展、不限制网速、支持数百种文件格式在线预览编辑和播放、轻松分享和协作" 的个人&家庭网盘。

### DEMO 体验地址：
[http://kodbox.fc-nas-filemgr.1986114430573743.cn-hangzhou.fc.devsapp.net](http://kodbox.fc-nas-filemgr.1986114430573743.cn-hangzhou.fc.devsapp.net/#)
账号:  test     密码：test@123

登录之后， 您可以得到一个 web 版本的 windows 操作系统的体验， 对您 NAS 盘 或者 OSS 上多媒体文件进行预览、编辑、移动等各种处理。
> 当然：
> 1. 如果您部署成功后， 默认有 admin 账号， 可以实施更高级的用户管理级插件安装等等...
> 1. [https://kodcloud.com/download/](https://kodcloud.com/download/) 可以下载 PC 或者手机客户端实现网盘的自动备份同步功能   ...

### 成本剖析

- 计算费用： 0.000110592元/GB-秒， 每个月有 40万 GB-秒的免费额度，这项基本免费。
- 流量费用：函数请求响应流量：0.50元/GB,  取决于您每个月从您的网盘上下载文件的多少， 上传没有流量费用。上传和下载均没有限速。
- 持久化费用：使用阿里云 NAS,  主要部署 kodbox 应用需要的 sqlite 数据库， 0.35(*结合低频介质，低至0.19) GB/月， 由于 NAS 单价比较贵， 建议 NAS 盘只做 kodbox 的 sqlite 数据库存储， 不会超过1G， 费用即 0.35 元。
- 在单纯存储这块， 可以选择您自己存储类型，以使用 OSS 做文件存储为例， OSS 存储价格如下表， 如果电影收集爱好者， 大部分电影应该是冷归档型，假设有 100GB 的存储资源，那么每个月的存储费用是 1.5 元。
   - ![](https://img.alicdn.com/imgextra/i1/O1CN01GEic5J1LpQdg2ASdO_!!6000000001348-2-tps-2448-166.png#crop=0&crop=0&crop=1&crop=1&id=WaALO&originHeight=166&originWidth=2448&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

   - ![](https://img.alicdn.com/imgextra/i2/O1CN01e6dygX1znDLioRfQe_!!6000000006758-2-tps-1210-756.png#crop=0&crop=0&crop=1&crop=1&id=fGB0B&originHeight=756&originWidth=1210&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

## 快速部署实战操作

- 开通阿里云[函数计算](https://fc.console.aliyun.com/fc/overview)
- 开通阿里云[文件存储](https://nasnext.console.aliyun.com/)
- 在登录阿里云控制台的状态下， 打开这个引导链接 [start-fc-kodbox](https://fcnext.console.aliyun.com/applications/create?template=start-fc-kodbox)， 按照这个指引教程走下去即可， 视频教程如下:

[![Watch the video](https://img.alicdn.com/imgextra/i2/O1CN010l6Rvp1V5lJRzicKq_!!6000000002602-0-tps-1072-654.jpg)](https://images.devsapp.cn/application/kodbox/kodbox-deploy.mp4)

- 使用 admin 账号登录， 进入后台存储管理， 添加适合自己的存储， 比如增加一个 OSS Bucket。

![image.png](https://img.alicdn.com/imgextra/i1/O1CN01PIZPiL1Mmy8ZIoBKl_!!6000000001478-2-tps-1210-756.png)

## 畅想
在文章 [PHP 遇见 Serverless，帮你解决这些痛点！](./PHP%E9%81%87%E8%A7%81Serverless.md)中， 我们十分细致地讨论了 PHP 应用在 Serverless 的最佳实践方式以及带来的巨大价值， 其中最重点的一个点是 FC 弹出的实例演化为存粹的执行环境， PHP web 工程存储到 NAS， 这个时候我们就可以使用 Kodbox + FC 实现 windows 体验般的 WEB UI 对 NAS 上的 PHP 工程就地管理， 包括上传、覆盖、删除、修改等。

![image.png](https://img.alicdn.com/imgextra/i4/O1CN01DqdTpD1TKIeaC1UPa_!!6000000002363-2-tps-2352-1238.png)

[![Watch the video](https://img.alicdn.com/imgextra/i2/O1CN010l6Rvp1V5lJRzicKq_!!6000000002602-0-tps-1072-654.jpg)](https://images.devsapp.cn/application/kodbox/kodbox_for_php_dev.mp4)

## 参考

- [艾媒咨询|2020-2021年中国个人网盘专题调研报告](https://www.iimedia.cn/c400/75531.html)
- [https://github.com/devsapp/start-fc-kodbox](https://github.com/devsapp/start-fc-kodbox)
- [https://github.com/kalcaddle/kodbox](https://github.com/kalcaddle/kodbox)
- [2020国内还能用的网盘推荐](https://zhuanlan.zhihu.com/p/107343480)
