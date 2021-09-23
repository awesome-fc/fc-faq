# CustomRuntime、phpRuntime安装自定义扩展

### 在使用CustomRuntime、phpRuntime时会需要安装使用一些扩展库，那如何进行安装呢？本文就以安装zip扩展为例进行介绍。

## CustomRuntime

- Step1：启动并进入 custom runtime 镜像， 并将当前目录挂载到容器的 /code 目录，windows 有问题的话， 可以把 $(pwd) 写成具体的绝对目录
```
docker run -v $(pwd):/code -it --entrypoint=""  registry.cn-beijing.aliyuncs.com/aliyunfc/runtime-custom:latest  bash
```
![](https://img.alicdn.com/imgextra/i2/O1CN01oFvvmk1ZX1J2unv5X_!!6000000003203-2-tps-2226-132.png)

- Step2：在容器内安装 zip 扩展, 然后找到 zip.so 并 copy 到 /code 目录（即拷贝到本地机器的目录了）

    - 在容器内安装 zip 扩展:`root@2f5b9e70191b:/code# pecl install zip`
    - 找到zip扩展：`root@2f5b9e70191b:/code# find /usr -name "zip.so"`
    - copy 扩展文件到 /code 目录（即拷贝到本地机器的目录了）：`root@2f5b9e70191b:/code# cp /usr/lib/php/20190902/zip.so /code`
    - 退出容器：`root@2f5b9e70191b:/code# exit`

![](https://img.alicdn.com/imgextra/i2/O1CN013jUsaM1yb19sp2cMK_!!6000000006596-2-tps-1314-308.png)

- Step3：在和bootstrap文件平级的目录创建 extension 目录， 将 zip.so 放到 extension目录， 同时创建一个 myext.ini 文件，文件内容为 “/code/extension/zip.so” 如图所示
![](https://img.alicdn.com/imgextra/i1/O1CN012Si4BV1z3PiGGw4gG_!!6000000006658-2-tps-1392-794.png)

- Step4：给函数设置这个环境变量，使扩展库能被访问
```
PHP_INI_SCAN_DIR=/code/extension:/etc/php/7.4/cli/conf.d
```
![](https://img.alicdn.com/imgextra/i3/O1CN01uv3KGK1YEPb8X13fs_!!6000000003027-2-tps-2258-1296.png)

- Step5：使用测试，如下图可以看到，zip扩展库已经生效
![](https://img.alicdn.com/imgextra/i4/O1CN018YfNbc23V893yjSqO_!!6000000007260-0-tps-1579-1510.jpg)


## PHP Runtime

- 和CustomRuntime安装是类似的步骤，区别是进入php runtime的镜像进行安装，不用配置环境变量

- Step1：启动并进入 custom runtime 镜像， 并将当前目录挂载到容器的 /code 目录，windows 有问题的话， 可以把 $(pwd) 写成具体的绝对目录
```
docker run -v $(pwd):/code -it --entrypoint=""  registry.cn-beijing.aliyuncs.com/aliyunfc/runtime-php7.2:latest  bash
```
![](https://img.alicdn.com/imgextra/i4/O1CN01zMhIFI1bYyE223uJh_!!6000000003478-2-tps-2198-110.png)

- Step2：在容器内安装 zip 扩展, 然后找到 zip.so 并 copy 到 /code 目录（即拷贝到本地机器的目录了）

    - 在容器内安装 zip 扩展:`root@2f5b9e70191b:/code# pecl install zip`
    - 找到zip扩展：`root@2f5b9e70191b:/code# find /usr -name "zip.so"`
    - copy 扩展文件到 /code 目录（即拷贝到本地机器的目录了）：`root@2f5b9e70191b:/code# cp /usr/lib/php/20190902/zip.so /code`
    - 退出容器：`root@2f5b9e70191b:/code# exit`

![](https://img.alicdn.com/imgextra/i2/O1CN013jUsaM1yb19sp2cMK_!!6000000006596-2-tps-1314-308.png)

- Step3：使用扩展，具体可以查看 [php 运行时自定义扩展使用](/docs/php运行时自定义扩展使用及下载.md)