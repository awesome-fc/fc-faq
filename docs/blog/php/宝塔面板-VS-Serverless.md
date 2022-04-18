---
title: 宝塔面板 VS Serverless.md
description: '宝塔面板-VS-Serverless'
position: 5
category: 'FC_Blog'
---

作者 | 西流

## 前言

最近尝试玩了下[宝塔面板](https://www.bt.cn/new/index.html), 发现这个产品真的是好用，简直就是传统单机开发多 PHP 应用的屠龙术，友好的可视化界面，将 web 应用托管效率大大提升， 难度大大降低，体验确实可以媲美能他们的 slogan: `人人都能上手的服务器运维管理工具`。

![](https://img.alicdn.com/imgextra/i2/O1CN01CDzcB31mCUbKGua41_!!6000000004918-2-tps-1737-998.png)

- 快速创建管理web项目
  > 方便便捷的网站管理功能，例如域名绑定，一键部署SSL证书，更改网站配置等功能

- 熟悉的文件管理系统
  > 方便高效的文件管理器，支持上传、下载、打包、解压等操作，可在线写代码

- 快速预览服务器资源使用情况
  > CPU、内存、磁盘IO、网络IO数据监测，可设置记录保存天数，以及任意查看某天数据。

- 一键安装软件及部署源码
  > 通过web界面，就可以轻松管理安装所用的服务器软件，还有丰富扩展应用。

## 宝塔面板 vs Serverless
为了探寻 Serverless 对于 PHP 开发者体验几何， 笔者专门做了一个表格做了对比：
> FC 是函数计算的简写

| 宝塔面板        | 函数计算  |  备注 |
| --------   | -----  | ----  |
| 一键安装 LNMP      | 内置LNP   |   FC：mysql 直接使用 rds 或者 NAS + sqlite(小应用)     |
| 多网站托管        |   多Service/function 区分   |     |
| FTP       |   部署 [kodbox 辅助函数](https://fcnext.console.aliyun.com/applications/create?template=start-fc-kodbox)   |  FC 通过 kodbox 实现对 NAS 上的 web 工程管理  |
| 数据库管理(内置phpMyAdmin)        |    云厂商 RDS 数据库 web 管理系统    |  每个云厂商都有不弱于 phpMyAdmin 的 web 管理工具 |
| 监控       |   函数实例监控   |  FC：可观测性提供实例监控  |
| 安全       |     |  这块宝塔面板的安全， 主要是做端口防火墙以及屏蔽 IP； FC 本身外界本身就没有能力直接通过 ssh/或者其他端口 触达实例，屏蔽 IP这块 FC 目前没有；其他更高级的限流， 宝塔也需要用户付费， FC 这边暂时可以通过前面挂 API 网关或者  WAF 等其他云产品实现  |
| 文件       |   部署 [kodbox 辅助函数](https://fcnext.console.aliyun.com/applications/create?template=start-fc-kodbox)   |  实现文件在线编辑、保存、预览等功能  |
| 终端       |   FC 控制台和工具已经提供实例登录功能   |    |
| 计划任务 | 定时触发器 | FC： 比如数据库定时备份， 可以在 RDS 控制台设置完成， 其他自定义任务可以通过 FC 设置定时触发器实现自定义逻辑
| 应用商店      |   其他云产品 + FC layer    |   宝塔的商店主要是安全产品及其他版本的 PHP， 对于 FC 来说， 安全产品可以购买其他云产品实现， 其他版本的 PHP 可以通过 layer 来实现（后续会提供那种官方 layer， 用户创建函数直接引用下， 环境就变成了指定版本的 PHP 环境）|

从上面的表格中， 就宝塔面板的几大优势再来聊下：

- 快速创建管理web项目
  > 函数计算最好的体验应该还是通过控制台应用中心一键部署或者命令行工具 S 一键部署

- 熟悉的文件管理系统
  > 函数计算通过部署 KodBox 辅助函数来实现

- 快速预览服务器资源使用情况
  > 函数计算控制台提供实例级别的监控，在 serverless 领域中，这个值可能没有那么重要， 因为会根据请求直接弹性扩容， 很小会出现服务器资源瓶颈问题。

- 一键安装软件及部署源码
  > 函数计算内置的 LNP 环境和充足的扩展， 源码可以借助 s nas 命令行或者 kodbox 辅助函数 UI（浏览器/客户端工具）更新或者上传，具体的视频教程见附录中的视频教程。

![image.png](https://img.alicdn.com/imgextra/i4/O1CN01DqdTpD1TKIeaC1UPa_!!6000000002363-2-tps-2352-1238.png)

总的来说， Serverless 在能力上已经具备了传统 PHP 应用多网站部署、 web 工程在线可视化编辑、上传等能力，虽然在整体一体化体验较宝塔面板有些差距， 但是却具备了 Serverless 的属性加成：

- 流量小的网站， 不宕机， 按量付费， 划算

- 促销类等具有明显波峰波谷的场景， 不宕机， 免去扩容、运维麻烦， 同时具有按量付费加成

- 请求级别的日志以及 tracing 监控

- ...

## 附录

**Kodbox 辅助函数部署**
[![Watch the video](https://img.alicdn.com/imgextra/i2/O1CN010l6Rvp1V5lJRzicKq_!!6000000002602-0-tps-1072-654.jpg)](https://images.devsapp.cn/application/kodbox/kodbox-deploy.mp4)

**Kodbox 辅助函数管理 PHP WEB 工程**
[![Watch the video](https://img.alicdn.com/imgextra/i2/O1CN010l6Rvp1V5lJRzicKq_!!6000000002602-0-tps-1072-654.jpg)](https://images.devsapp.cn/application/kodbox/kodbox_for_php_dev.mp4)

**控制台应用中心一键部署传统 PHP 框架应用**

- [ThinkPHP](https://github.com/devsapp/start-web-framework/tree/master/web-framework/php/thinkphp/src)
- [Laravel](https://github.com/devsapp/start-web-framework/tree/master/web-framework/php/laravel/src)
- [Wordpress](https://github.com/devsapp/start-web-framework/tree/master/web-framework/php/wordpress/src)
- [Z-BlogPHP](https://github.com/devsapp/start-web-framework/tree/master/web-framework/php/zblog/src)
- [Swoole](https://github.com/devsapp/start-fc/tree/master/custom-function/php74)

> 您可以直接根据 readme 中说明， 通过函数计算控制台应用中心一键部署

> [更多](https://github.com/devsapp/start-web-framework/tree/master/web-framework/php)