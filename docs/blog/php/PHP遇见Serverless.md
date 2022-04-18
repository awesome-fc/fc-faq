---
title: PHP 遇见 Serverless
description: 'PHP 遇见 Serverless，帮你解决这些痛点！'
position: 3
category: 'FC_Blog'
---

作者 | 西流

## 前言

PHP 的应用范围相当广泛，尤其是在网页程序的开发上,  根据最新 [维基百科](https://zh.wikipedia.org/wiki/PHP) 显示，2013年4月的统计资料，PHP已经被安装在超过2亿4400万个网站和210万台服务器上, 而根据 [W3Techs](https://w3techs.com/) 的报告，截至2021年9月, 有78.9%的网站使用PHP。 所以 PHP 是世界第一语言至少在 web 开发领域并不是戏称。

而在技术选型上， PHP 主要采用的是 LAMP(全称是Linux + apache + mysql + php) 或者 LNMP(全称是Linux + nginx + mysql + php)， 这种成熟稳定的技术框架推动 PHP web 开发生态的繁荣和商业上的成功。

![](https://img.alicdn.com/imgextra/i3/O1CN01R4fLOv28c4FJUzQKr_!!6000000007952-2-tps-2036-678.png#crop=0&crop=0&crop=1&crop=1&id=dwyXC&originHeight=678&originWidth=2036&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

在传统的开发模式中， 开发者自己需要安装维护各种软件的安装、维护升级:

- 如果您是一个企业用户， 如果业务体量变大或者为了生产环境的稳定和可用性， 使用负载均衡是一个必然的选项：

     ![](https://img.alicdn.com/imgextra/i1/O1CN012p80GI1yVWQlbayoG_!!6000000006584-2-tps-1030-420.png#crop=0&crop=0&crop=1&crop=1&height=265&id=CjvwI&originHeight=420&originWidth=1030&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=650.9801025390625)
即此时， PHP 开发者或者线上运维的同学关心的事情多了起来：

   -  每个增加的生产机器都需要重新安装一遍相关软件， 做相同的 nginx 配置以及 php-fpm 的配置， 以及维护每个生产机器的安全更新 
   -  假如开发的应用需要一个新的扩展， 可能需要人肉每台机器去增加扩展 
   - 负载均衡器随着业务的变更升配， 后面一台 Worker 机器挂掉了， 如何做运维处理 
   - 业务的波峰波谷怎么应对才能让资源的利用率提高
   - ... 

- 如果您是项目组开发成员比较多的企业用户，能不能不需要给每个开发配置一个安装的 NLP 的 Linux 机器作为开发测试机器（或者多人共享一个机器）？

- 如果您是一个提供网站开发和托管的 ISV 、外包公司或者创业公司， 我的客户都是一些中小企业的门户网站， 我怎么提高我后端机器资源利用率以及更好提供定制化服务？

- 如果您是一个学生或者准备学习 PHP 开发，本地只有 Windows 电脑， 能不能直接近乎免费的方式获取 LNP(Linux+Nginx+PHP)  的环境用来学习呢？

- ...

带着这些问题， 我们去探索一下 Serverless 是如何解决这些痛点的。
## PHP 遇见 Serverless

### 什么是 Serverless

Serverless = Faas (Function as a service) + Baas (Backend as a service)， 我们简单通过两个图快速了解相关概念：

-  传统模式

![](https://img.alicdn.com/imgextra/i4/O1CN01ezJUHP1ytnQHXJwUk_!!6000000006637-2-tps-1648-354.png#crop=0&crop=0&crop=1&crop=1&id=QXGsC&originHeight=354&originWidth=1648&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
 

-  Serverless 模式

![](https://img.alicdn.com/imgextra/i1/O1CN01SmriUV1cisQt6rRVj_!!6000000003635-2-tps-1686-536.png#crop=0&crop=0&crop=1&crop=1&id=B5md6&originHeight=536&originWidth=1686&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
图中的 CDN 和 OSS 就是 BaaS 服务，FC 就是自定义函数逻辑的 FaaS 平台,   通过这个对比， 我们能快速得到 FaaS 的特性和好处：

-  只需要专注业务代码开发， 编写对应的逻辑即可 
-  极致弹性伸缩， 无需管理服务器
-  按量付费，每次调用按毫秒计费
-  ... 

本文后续讨论的 Serverless 主要指的是 FaaS， 如下示意图， 几行代码编写完毕， 保存到云厂商的 FaaS 平台， 就完成了一个弹性高可用的 Web API。

![](https://img.alicdn.com/imgextra/i1/O1CN01HYOzzq1NVOXFPYknV_!!6000000001575-2-tps-1809-919.png#crop=0&crop=0&crop=1&crop=1&height=419&id=njXhW&originHeight=919&originWidth=1809&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=824)

### PHP 遇见 Serverless
PHP 作为一个开发群体的很大的语言， 各大云厂商的 FaaS，比如阿里云的函数计算、AWS 的 Lambda (通过 Custom Runtime 间接支持）、 腾讯的 SCF 等都推出了对 PHP 语言的支持， phper 面对前端领域的 Serverless 技术革新实践(感兴趣的见本文最后的附录)， 应该不遑多让。以阿里云函数计算为例， 有很多 PHP 的开发者有了很多有趣的实践：

- 直接使用 gd 或者 ImageMagick 扩展， 实现弹性高可用的图片、水印等各种 CPU 密集型 API 
- 直接使用 ffmpeg + 性能型实例 + 异步有状态调用完成视频剪辑合成等音视频处理业务
- 使用 HTTP 触发器实现的函数， 埋点到广告平台， 快速实现高可用的买量业务 
- 直接将之前基于框架（如 ThinkPHP）实现的 WEB API 直接迁移到 FaaS 平台，不用再担心宕机和运维问题了 
-  ... 

虽然 FaaS 很好地解决了  phper 如下问题：

-  新业务或者开发新的 web API 
-  存量业务中， 有些 CPU 密集型或者弹性要求很高的 API 单独抽离出来 FaaS 化 

但是传统的开发模式或者存量业务，对开发者有一定的上手和改造成本，比如某 Faas 厂商 PHP Runtime 编程接口示例：
```php
<?php
function handler($event, $context) {
     $eventObj = json_decode($event, $assoc = true);
  	 // do your things
     // ....
     return $eventObj['key'];
}           
```
但是能不能更进一步， 开发者不需要按照 FaaS 厂商的约定的函数入口能实现一个个的 API,  而是能直接将传统运行在 LAMP 或者 LNMP 的项目直接 FaaS 化？

答案是肯定的

阿里云函数计算的 [Custom Runtime](https://help.aliyun.com/document_detail/132044.html) 以及直接基于 HTTP 协议的极简编程模型走在了所有云厂商的前列。

![](https://img.alicdn.com/imgextra/i2/O1CN01ByOg1v1MZEEdByJGL_!!6000000001448-2-tps-1600-362.png#crop=0&crop=0&crop=1&crop=1&id=Unl4r&originHeight=362&originWidth=1600&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

函数计算启动Custom Runtime执行环境时，会默认调用 bootstrap文件(或者您创建函数的时设置的 Args参数)启动您自定义的 HTTP Server， 然后这个HTTP Server接管了函数计算系统的所有请求，即您所有的函数调用请求。
> 函数计算 Custom runtime 执行环境底层系统是 Linux,  并且已经内置的 nginx/1.10.3 和 php-fpm7.4,   对于 PHP 应用，您直接使用即可


以部署一个 [wordpress 项目](https://github.com/devsapp/start-web-framework/tree/master/web-framework/php/wordpress/src) 为例,  只需要将如下目录直接打包成一个 zip 包在函数计算平台创建一个函数即可：

```bash
- start.sh
- nginx.conf
- php-fpm.conf
- php.ini-production
- wordpress
```

其中 wordpress 目录是对应的 web 工程, start.sh 是启动 nginx 和 php-fpm 的脚本即可:

```bash
...
echo "start php-fpm"
php-fpm7.4 -c /code/php.ini-production -y /code/php-fpm.conf

echo "start nginx"
nginx -c /code/nginx.conf
...
```

> start.sh 详情可参考 [WordPress in FC](https://github.com/devsapp/start-web-framework/blob/master/web-framework/php/wordpress/src/code/start.sh)


所以， 使用函数计算这个 Serverless 产品和传统的 PHP 开发相结合后， 您再也不用考虑负载均衡的事情， 不用考虑扩缩容的事情， 不用管理机器、不用担心宕机的事情等等， 只需要安安心心把业务代码开发好即可。![](https://img.alicdn.com/imgextra/i4/O1CN01IsiwmF1rQIcaq4jeS_!!6000000005625-2-tps-1436-1200.png)
从上图可以看出：开发者只需要开发好自己的业务代码即可，唯一需要考虑的事情， 就是函数计算这边扩容不要太多太猛（比如直接在函数计算平台设置下该函数能弹出的最大实例数目即可）， 给下游自己的 Mysql 数据库过大的压力即可。

当然， 从原始的传统的 php web 应用完全迁移到 Serverless 形态的函数计算平台， 某些场景可能需要考虑数据持久化问题， 因为函数计算是无状态的， 数据持久化保存可以借助 NAS、Redis 等服务完成，以 NAS 为例，流程图如下：
![](https://img.alicdn.com/imgextra/i3/O1CN019ciHEo1OkL5oZIUzC_!!6000000001743-2-tps-1550-1246.png)

 以 WordPress 为例， 后台系统上传的图片或者 Session 功能都是需要持久化到磁盘的。

-  设置 web 工程的文件上传目录或者 session 目录为 NAS 盘的某个目录, NAS 盘实现持久化 
-  甚至可以将 web 工程直接放到 NAS 盘上， 此时函数计算纯粹就是 LNP 执行环境 
![](https://img.alicdn.com/imgextra/i1/O1CN01baG0kD1rjXElrjgn1_!!6000000005667-2-tps-588-122.png)   
> 比如将 wordpress 工程不作为函数的代码包的一部分， 而已提前上传到 NAS 盘， 只需要设置好 nginx.conf 中的 root 能知道 web 工程即可， 如上面的 nginx.conf， /mnt/auto 表示挂载的 NAS 目录，mnt/auto/wordpress 则表示在 NAS 上的 web 工程。

> 此时对您来说， 函数再也不用变了， 您可能只是需要开发新的业务代码， 然后上传到 NAS 上即可（或者直接使用 git 直接在 NAS 操作，实现 web 工程的版本和 git 上的 commit 绑定， 使用 git 实现代码的快速升级和混滚）

> 但是从安全生产的角度来说， 还是建议您 web 工程变更最好和函数的变更相关联

## 小结

从上面的讨论和陈述中， 我们不难发现， PHP 遇见 Serverless 是一件令人兴奋的事情， 让 phper 有了更大的想象空间。 Serverless 的理念和 PHP 这个语言出现的理念也是一致的: 即让开发者最大精力集中在自己的业务价值。 PHP 语言一直是 web 领域最好的生产力代表， 而 Serverless 将会让 PHP 如虎添翼。

我们最后来一一解答下前言中提出的问题：
##### 如果您是一个企业用户， 业务体量变大或者为了生产环境的稳定和可用性， 如何做？
如上面陈述， 使用函数计算和传统的 PHP 开发相结合后， 您再也不用考虑负载均衡的事情， 不用考虑扩缩容的事情， 不用管理机器、担心宕机的事情等等， 只需要安安心心把业务代码开发好即可。

##### 如果您是项目组开发成员比较多的企业用户，能不能不需要给每个开发配置一个安装的 NLP 的 Linux 机器作为开发测试机器（或者多人共享一个机器）？
是的， 每个开发者在函数计算上创建一个自己的 Service/函数即可， Service/函数配置开发测试环境的 VPC，实现内网安全访问数据库等其他下游服务。 函数调用的时候， 函数计算会拉一个 NLP 的执行环境来运行您分支上正在开发的 PHP 代码。

- 每个执行环境是相互隔离的
- 按调用次数计费， 不需要预留机器， 免除了机器成本上的浪费
- 也可以很方便进行压测等各种事宜

##### 如果您是一个提供网站开发和托管的 ISV 、外包公司或者创业公司， 我的客户都是一些中小企业的门户网站， 我怎么提高我后端机器资源利用率以及更好提供定制化服务？
通常来说， 很多企业门户网站访问量不大， 但是网站挂掉了会引起客户投诉。每个客户的网站通过service 或者函数区分， 通过函数名或者service去区分您自己的客户： i. 管理方便  ii. 做定制化方便  iii. 做不同vip等级服务方便。 举个例子， 您可以快速通过某个函数的调用指标情况， 可以看出哪个客户的网站访问量大，可以做出客户画像以及制定不同的收费和 vip 服务级别。 

##### 如果您是一个学生或者准备学习 PHP 开发，本地只有 Windows 电脑， 能不能直接近乎免费的方式获取 LNP(Linux+Nginx+PHP)  的环境用来学习呢？
是的， 只要将如下的文件和文件夹打包成 zip 包去函数计算控制台创建函数即可
```
- start.sh
- nginx.conf
- php-fpm.conf
- php.ini-production
- myweb
  | - hello.php
```

> 可以详情可参考 [WordPress in FC](https://github.com/devsapp/start-web-framework/tree/master/web-framework/php/wordpress/src)

这里构建了一个钉钉群： 31897696，  如果您对 PHP 落地 Serverless 感兴趣，您有观点、想法或者想吐槽的， 可以和大家一起交流。

![](https://img.alicdn.com/imgextra/i4/O1CN01FE7yNw22NyMmJQzrP_!!6000000007109-2-tps-278-349.png)

## PHP 框架 Serverless 最佳实践

- [ThinkPHP](https://github.com/devsapp/start-web-framework/tree/master/web-framework/php/thinkphp/src)
- [Laravel](https://github.com/devsapp/start-web-framework/tree/master/web-framework/php/laravel/src)
- [Wordpress](https://github.com/devsapp/start-web-framework/tree/master/web-framework/php/wordpress/src)
- [Z-BlogPHP](https://github.com/devsapp/start-web-framework/tree/master/web-framework/php/zblog/src)
- [Swoole](https://github.com/devsapp/start-fc/tree/master/custom-function/php74)

> 您可以直接根据 readme 中说明， 通过函数计算控制台应用中心一键部署

其他更多: [https://github.com/devsapp/start-web-framework](https://github.com/devsapp/start-web-framework)

## 参考引用

-  [Serverless Architectures](https://martinfowler.com/articles/serverless.html) 
-  [Backend For Frontend（BFF）in Serverless](https://www.infoq.cn/article/0btajez51ysb_qehr526) 
-  [关于Serverless 未来对前端开发影响的具体看法](https://developer.aliyun.com/article/793492) 
-  [当 SSR 遇上 Serverless，轻松实现页面瞬开](https://cnodejs.org/topic/5e394e311225c9423dcd9754) 

## 附录
Serverless 在前端领域如火如荼的发展:

- [Backend For Frontend（BFF）in Serverless](https://www.infoq.cn/article/0btajez51ysb_qehr526) 来提高生产力
   - 前端开发者全栈化 
   - 提高开发效率，减少前端和后端接口同学的沟通联调时间， 后端同学只需要做好原子的接口的稳定性和可靠性即可， 数据的聚合直接由前端同学通过 BFF 实现 

      ![image.png](https://img.alicdn.com/imgextra/i4/O1CN01xqMp6Y1uy8jVjBHyc_!!6000000006105-2-tps-418-388.png)

- [当 SSR 遇上 Serverless，轻松实现页面瞬开](https://cnodejs.org/topic/5e394e311225c9423dcd9754) 
   -  借助于函数即服务（FaaS）的能力，不需要再去搭建传统的 Node 应用，一个函数就可以变成一个服务，开发者可以更纯粹的关注于业务逻辑。 
   - FaaS 以函数为单位的形式以及弹性机制，为 SSR 应用带来了天然的隔离性和动态修复能力，可以更好的避免页面间的交叉污染，或一些边界的异常场景对应用带来致命性的伤害。 
   - 无需运维、按需执行、弹性伸缩这些特性，大大降低了 SSR 应用对开发者的门槛。 
