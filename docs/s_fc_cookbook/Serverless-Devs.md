Serverless Devs 是一个开源开放的 Serverless 开发者平台，致力于为开发者提供强大的工具链体系。通过该平台，开发者可以一键体验多云 Serverless 产品，极速部署 Serverless 项目。
​

![image.png](https://img.alicdn.com/imgextra/i1/O1CN01i7NQQV1ORYpr9PIbH_!!6000000001702-2-tps-2218-1236.png)


结合上面这幅图， 我们可以列下 S 工具的设计哲学：

- S 可以通过不同的组件，完成各种功能， 比如有 FC 组件， SCF 组件， lambda 组件， 从而完成对多云的支持
- 组件可以一层一层往高抽象， 比如 FC 组件， 是 fc-deploy,  fc-nas ... 子组件搭建而成， 实现了各子组件自己的独自开发升级管理，实现解耦
- 组件是 lazy load 模式， 只有第一次使用的时候， 才会下载使用
- s 可以 init 出来很多示例应用， 直接供用户使用或者二次开发使用

更多详情可参考: [Serverless Devs](https://github.com/Serverless-Devs/Serverless-Devs/tree/master/docs/zh)