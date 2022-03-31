---
title: PHP Runtime 出错信息有 FastCGI
description: 'PHP Runtime 出错信息有 FastCGI'
position: 10
category: '概述'
---

# PHP Runtime 出错信息有 FastCGI

因为历史原因， 一些老的用户使用 php runtime 的时候， 使用了函数计算提供的 $GLOBALS['fcPhpCgiProxy'] 对象用来和 php-fpm 进行交互。

> 强烈建议不要再使用这个接口，函数计算目前已经支持了自定义镜像， 感兴趣的同学直接使用镜像体验更流畅，使用 custom-container, 和传统的 php 使用方法一致， 通过 S 工具可以一键部署
>
> - [start-laravel](https://github.com/devsapp/start-laravel)
> - [start-thinkphp](https://github.com/devsapp/start-thinkphp)
> - [start-zblog](https://github.com/devsapp/start-zblog)
> - [start-wordpress](https://github.com/devsapp/start-wordpress)
> - [start-discuz](https://github.com/devsapp/start-discuz)

主要使用了这个接口：

```php
requestPhpCgi($request, $docRoot, $phpFile = "index.php", $fastCgiParams = [], $options = [])
```

但是会偶尔随机出现如下报错， 比如：

![](https://img.alicdn.com/imgextra/i1/O1CN01zOwtvV1JUF2yx2ov4_!!6000000001031-2-tps-1826-154.png)

这里建议做如下代码重试， 比如您之前的代码:

```php
$proxy    = $GLOBALS['fcPhpCgiProxy'];
...
$resp   = $proxy->requestPhpCgi($request, $root_dir, "index.php",
            ['SERVER_NAME' => $host, 'SERVER_PORT' => '80', 'HTTP_HOST' => $host],
            ['debug_show_cgi_params' => false, 'readWriteTimeout' => 15000]
        );
return $resp;
```

改成:

```php
$proxy    = $GLOBALS['fcPhpCgiProxy'];
...
try {
    $resp   = $proxy->requestPhpCgi($request, $root_dir, "index.php",
            ['SERVER_NAME' => $host, 'SERVER_PORT' => '80', 'HTTP_HOST' => $host],
            ['debug_show_cgi_params' => false, 'readWriteTimeout' => 15000]
        );
    return $resp;
} catch (Exception $e) {
    echo "retry once ...";
    $GLOBALS['fcPhpCgiProxy'] = new \ServerlessFC\PhpCgiProxy();
    $proxy    = $GLOBALS['fcPhpCgiProxy'];
    $resp   = $proxy->requestPhpCgi($request, $root_dir, "index.php",
            ['SERVER_NAME' => $host, 'SERVER_PORT' => '80', 'HTTP_HOST' => $host],
            ['debug_show_cgi_params' => false, 'readWriteTimeout' => 15000]
        );
    return $resp;
}
```
