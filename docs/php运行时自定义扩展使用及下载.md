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


<a href="https://fc-faq.oss-cn-hangzhou.aliyuncs.com/php-so/mongodb.so?versionId=CAEQHhiBgMDmxrGX3RciIDg3OGM2MTYxNGU3YzQ2Y2Y5MzY3N2U3MmE3ZDA3NjQx" download="mongodb.so">
mongodb.so
</a><br>

<a href="https://fc-faq.oss-cn-hangzhou.aliyuncs.com/php-so/pdo_sqlite.zip?versionId=CAEQHhiBgICs7rqX3RciIDRiZmMzMDFlNTA1YjRlM2U5MGY0YTUxNDg0MDIyYmM4" download="pdo_sqlite.so">
pdo_sqlite.so
</a><br>

<a href="https://fc-faq.oss-cn-hangzhou.aliyuncs.com/php-so/pgsql-demo.zip?versionId=CAEQHhiBgICo7rqX3RciIDU2NmUwNzRhMDkyOTQzNWE5MDQ5ZWUwNjFhYjYzYTcw" download="pgsql.so">
pgsql.so
</a><br>

<a href="https://fc-faq.oss-cn-hangzhou.aliyuncs.com/php-so/sqlsrv.zip?versionId=CAEQHhiBgMCBx7GX3RciIGY4MDhlNDU5OTVjNjQwNzJhOTJhM2Y2NThjMjBhZTQ1" download="sqlsrv.so">
sqlsrv.so
</a><br>

<a href="https://fc-faq.oss-cn-hangzhou.aliyuncs.com/php-so/swoole.so?versionId=CAEQHhiBgID5xrGX3RciIGE3NDBkOGQyY2EyZDQ3MjJiNTcyNGI4Y2I3YjdhOWY2" download="swoole.so">
swoole.so
</a>