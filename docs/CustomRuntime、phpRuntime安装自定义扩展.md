# CustomRuntime、phpRuntime安装自定义扩展

### 在使用CustomRuntime、phpRuntime时会需要安装使用一些扩展库，那如何进行安装呢？本文就以安装mongodb扩展为例对此进行介绍。

## CustomRuntime

- Step1：启动并进入 custom runtime 镜像， 并将当前目录挂载到容器的 /code 目录，windows 有问题的话， 可以把 $(pwd) 写成具体的绝对目录
```
docker run -v $(pwd):/code -it --entrypoint=""  registry.cn-beijing.aliyuncs.com/aliyunfc/runtime-custom:latest  bash
```
![](https://img.alicdn.com/imgextra/i2/O1CN01oFvvmk1ZX1J2unv5X_!!6000000003203-2-tps-2226-132.png)

- Step2：在容器内安装 mongodb 扩展, 然后找到mongodb.so并copy到 /code 目录（即拷贝到本地机器的目录了）

    - 在容器内安装 mongodb 扩展:`root@4ddc69d841b4:/code# pecl install mongodb`
    - 找到mongodb扩展：`root@4ddc69d841b4:/code# find /usr -name "mongodb.so"`
    - copy 扩展文件到 /code 目录（即拷贝到本地机器的目录了）：`root@4ddc69d841b4:/code# cp /usr/lib/php/20190902/mongodb.so /code`
    - 退出容器：`root@4ddc69d841b4:/code# exit`

![](https://img.alicdn.com/imgextra/i4/O1CN01GJgv8F1t8YTOfqliF_!!6000000005857-2-tps-1098-334.png)

- Step3：在和bootstrap文件平级的目录创建 extension 目录，将 mongodb.so放到 extension目录， 同时创建一个 myext.ini 文件，文件内容为 “extension=/code/extension/mongodb.so” 如图所示
![](https://img.alicdn.com/imgextra/i1/O1CN01yzgjmD1UacLaa4biK_!!6000000002534-2-tps-1338-596.png)

- Step4：给函数设置这个环境变量，使扩展库能被访问
```
PHP_INI_SCAN_DIR=/code/extension:/etc/php/7.4/cli/conf.d
```
![](https://img.alicdn.com/imgextra/i1/O1CN01oxhv9120RyZlzMRAL_!!6000000006847-2-tps-2122-570.png)

- Step5：使用测试，如下图可以看到，mongodb扩展库已经生效
![](https://img.alicdn.com/imgextra/i4/O1CN01y15CZU1RB1MZT4twt_!!6000000002072-0-tps-1646-1138.jpg)


## PHP Runtime

- 和CustomRuntime安装是类似的步骤，区别是进入php runtime的镜像进行安装，不用配置环境变量

- Step1：启动并进入 custom runtime 镜像， 并将当前目录挂载到容器的 /code 目录，windows 有问题的话， 可以把 $(pwd) 写成具体的绝对目录
```
docker run -v $(pwd):/code -it --entrypoint=""  registry.cn-beijing.aliyuncs.com/aliyunfc/runtime-php7.2:latest  bash
```
![](https://img.alicdn.com/imgextra/i4/O1CN01zMhIFI1bYyE223uJh_!!6000000003478-2-tps-2198-110.png)

- Step2：在容器内安装mongodb扩展, 然后找到mongodb.so并 copy 到 /code 目录（即拷贝到本地机器的目录了）

    - 在容器内安装mongodb扩展:`root@db71692b6afe:/code# pecl install mongodb`
    - 找到mongodb扩展：`root@db71692b6afe:/code# find /usr -name "mongodb.so"`
    - copy 扩展文件到 /code 目录（即拷贝到本地机器的目录了）：`root@db71692b6afe:/code# cp /usr/local/lib/php/extensions/no-debug-non-zts-20170718/mongodb.so /code`
    - 退出容器：`root@2f5b9e70191b:/code# exit`

![](https://img.alicdn.com/imgextra/i3/O1CN012eAGtA1tnm7GzjobD_!!6000000005947-2-tps-1520-302.png)

- Step3：使用扩展，和CustomRuntime是类似的，也是在和bootstrap文件平级的目录创建 extension 目录， 将mongodb.so放到 extension目录， 同时创建一个 mongodb.ini 文件，文件内容为`extension=/code/extension/mongodb.so`
![](https://img.alicdn.com/imgextra/i4/O1CN01T3O3sO29Bn7dpXoSW_!!6000000008030-2-tps-1220-436.png)

- Step4：使用测试，如下图可以看到，mongodb扩展库已经生效
![](https://img.alicdn.com/imgextra/i1/O1CN01kdu7u21CzCW7vfIe2_!!6000000000151-2-tps-1686-886.png)