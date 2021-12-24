## 常用 Tips

在很多时候， 我们执行一些命令完成一些事情， 不想依赖一个 yml 去完成事情， FC 组件提供了  cli 模式完成一些事宜， 下面列一些很有用的小 TIPS: 

### 1. 查看函数信息
```bash
s cli fc info --region cn-hongkong --service-name s --function-name f --access defalut
```

### 2. 快速克隆函数
[使用s工具进行克隆函数](../%E4%BD%BF%E7%94%A8s%E5%B7%A5%E5%85%B7%E8%BF%9B%E8%A1%8C%E5%85%8B%E9%9A%86%E5%87%BD%E6%95%B0.md)

### 3. 快速删除一个 service
例如我想完整删除一个 service 下面所有的东西, (注：函数关联的自定义域名不会删除)
```bash
s cli fc remove --region cn-hangzhou --service-name s --access default
```

### 4. 版本、别名、预留相关
直接参考  [https://help.aliyun.com/document_detail/295906.html](https://help.aliyun.com/document_detail/295906.html) 使用 cli 模式

### 5. 更多
[常见小贴士](https://gitee.com/devsapp/fc/blob/main/docs/zh/tips.md)