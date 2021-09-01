## PHP运行时自定义扩展使用及下载

### 自定义扩展使用

您可以在函数入口文件同级目录下创建一个名为extension的目录，并且将扩展对应的.ini和.so文件放在extension目录下来添加PHP自定义扩展。下文演示了一个hello的自定义扩展，假设该扩展里有一个hello_world函数。

```
.
|____extension
| |____hello.ini
| |____hello.so
|____main.php  
```

- hello.ini
```
extension=/code/extension/hello.so            
```
- main.php

```
    <?php
    function handler($event, $context) {
        var_dump(extension_loaded('hello'));
        hello_world(); 
        return "ok";
    }                    
```

###  自定义扩展下载


<a href="https://php-so.oss-cn-hangzhou.aliyuncs.com/mongodb.so" download="mongodb.so">
mongodb.so
</a><br>

<a href="https://php-so.oss-cn-hangzhou.aliyuncs.com/pdo_sqlite%20%281%29.zip" download="pdo_sqlite.so">
pdo_sqlite.so
</a><br>

<a href="https://php-so.oss-cn-hangzhou.aliyuncs.com/pgsql-demo%20%281%29.zip" download="pgsql.so">
pgsql.so
</a><br>

<a href="https://php-so.oss-cn-hangzhou.aliyuncs.com/sqlsrv.zip" download="sqlsrv.so">
sqlsrv.so
</a><br>

<a href="https://php-so.oss-cn-hangzhou.aliyuncs.com/swoole.so" download="swoole.so">
swoole.so
</a>