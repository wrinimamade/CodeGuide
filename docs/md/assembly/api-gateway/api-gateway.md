---
title: API网关架构设计，从单体服务到微服务的架构演进
lock: no
---

# API网关架构设计，从单体服务到微服务的架构演进

作者：小傅哥
<br/>博客：[https://bugstack.cn](https://bugstack.cn)

>沉淀、分享、成长，让自己和他人都能有所收获！😄

<div align="center">
    <img src="https://bugstack.cn/images/article/assembly/api-gateway/api-gateway-0-00.png?raw=true">
    <div style="font-size: 12px;"><a href="https://t.zsxq.com/Ja27ujq">星球介绍：码农会锁 - 实战项目、专属小册、问题解答、简历指导、架构图稿、视频课程</a></div>
</div>

我在`"经海" —— 路，地铁站🚇`这么多年啊，肝了这么多项目，卷了这么多技术，攒了这么多经验。不就是为了给兄弟们👬🏻讲点在真实场景有用的干货吗？！

所以你问我`手撕了Spring源码`、`手撕了MyBatis源码`，接下来是不要`手撕SpringMVC源码`了？其实不会的，因为互联网大厂做项目这么多年就没有几次用SpringMVC的！所以搞它干肾嚒？

## 一、为啥不用SpringMVC？

关于为什么不用 SpringMVC 或者很少使用这样的技术栈，ChatGPT 给出了这样的回答；

<div align="center">
    <img src="https://bugstack.cn/images/article/assembly/api-gateway/api-gateway-0-01.png?raw=true" width="650px">
</div>

ChatGPT 的这个回答，非常正确✅。有了 ChatGPT 可以谨防上当受骗🤨！

---

互联网现代分布式架构应用技术主要以 RPC 通信的微服务模型为主，代表技术如；Dubbo、gRPC、Thrift，它们的通信协议为 RPC 协议（Remote Procedure Call，远程过程调用），这样的调用方式可以提高微服务间的通信性能。

<div align="center">
    <img src="https://bugstack.cn/images/article/assembly/api-gateway/api-gateway-0-02.png?raw=true" width="600px">
</div>

但这样的 RPC 却不是 WEB、H5、小程序 能调用的 HTTP 协议，所以通常需要把 RPC 转换为 HTTP 协议。那么怎么转换，总不能一个个在 SpringMVC 中硬编码吧，**这不是胡来吗**！

所以在互联网大厂应运诞生了 API 网关项目，它可以通过RPC服务的注册和发现机制，把接口注册网关中心，再通过网关算力执行接收 HTTP 请求协议进行解析做 RPC 接口的泛化调用。这中间还包括了；认证、授权、风控和服务治理的相关事项。

## 二、手写一套网关

讲道理，学习源码框架最行之有效的办法就是**上手**，看是看不会的，只有事必亲躬、亲力亲为的体验才能掌握框架的原理和精髓。所以小傅哥在学习这类东西的时候一直也是喜欢上手实践，一次花充足的时间折腾一件事，比反反复复的毛毛躁躁的学习要用的多。

所以从22年的8月开始一直到今天，小傅哥都会在周末的时候研究API网关的设计和代码实现，在这将近**8个月**的时间里终于完成了API网关的框架结构和核心流程，微服务工程数6个，`代码行数总计1万以上`。实现了；`协议转换`、`服务映射`、`调用鉴权`、`注册中心`、`上报接口`、`管理后台`等模块功能，代码使用了众多的分治、抽象和设计模式等思想，保证整体代码的高质量和可维护性。

**工程信息**

<div align="center">
    <img src="https://bugstack.cn/images/article/assembly/api-gateway/api-gateway-0-03.png?raw=true" width="500px">
</div>

**管理后台**

<div align="center">
    <img src="https://bugstack.cn/images/article/assembly/api-gateway/api-gateway-0-08.png?raw=true" width="500px">
</div>

**运行效果**

<div align="center">
    <img src="https://bugstack.cn/images/article/assembly/api-gateway/api-gateway-0-07.png?raw=true" width="500px">
</div>

慢工出细活，只有把每一个知识点都经过深思熟虑的设计，才能做出行之有效的解决方案。也能让参与者在这里学习到有价值的内容。接下来小傅哥在分享下这块的架构模型，看看整体这套网关是如何被运行的。

## 三、网关架构设计

API网关除了基础的功能模块以外，还需要重点考虑负载均衡的设计，只有这样才能被横向扩展支撑高并发的吞吐量。所在负载设计这块，小傅哥也是花了不少的时间来构建，让负载可以被动态的管理。

<div align="center">
    <img src="https://bugstack.cn/images/article/assembly/api-gateway/api-gateway-0-04.png?raw=true" width="950px">
</div>

这是一整套API网关的核心通信模型结构图，以API网关算力的多套服务注册到网关中心开始，拉取RPC应用接口并完成映射HTTP调用操作。最终允许用户通过 Nginx 访问和路径重写的负载均衡管理，调用到具体的网关算力中执行协议解析和RPC接口的泛化调用并最终返回结果数据。—— **就这套架构设计学习完，就够你晋升到P7岗了！**

整个网关模型的复杂度较高，这里的简述还不能概括出全部的核心，**所以小傅哥计划对加入学习的伙伴，后期组织一场网关架构设计的技术直播来讲解里面的具体实现。**

## 四、加入源码学习

`不好好深度学习，再过几年35了，你得遭老罪喽。`

当然小傅哥也不瞎扯，这是一套付费学习项目。在小傅哥花费了大量的周末、假期所沉淀出的技术经验，为有需要技术成长的伙伴开一扇可以进入学习的门。这对新人和有技术提升诉求的伙伴来说非常有必要，因为没人带的时候，自己要浪费很长时间摸索，也找不到正确的路。

### 1. 加入学习

<div align="center">
    <img src="https://bugstack.cn/images/article/assembly/api-gateway/api-gateway-0-05.png?raw=true" width="350px">
</div>

- **加入**：微信扫码加入即可，加入后点击阅读原文直达`API网关项目学习`。或进入链接：[https://t.zsxq.com/0ciYdgP5u](https://t.zsxq.com/0ciYdgP5u)
- **说明**：加入小傅哥的知识星球【`码农会锁`】，得到的不只是一个API网关项目，还有`Lottery - 分布式抽奖系统`、`IM - 仿微信项目`、`手撕源码`、`技术小册`等内容。
- **价值**：星球内的服务和实战项目都是小傅哥本人提供和原创，相信能够给大家带来超过该价格的价值 。举个例子，手把手带大家做进大厂才可能看得见的项目、有笔记有源码、有问题可以提，这比单独买一个课程或一套源码要值得多。其实都不到大城市一节补习班的钱，哪怕把我的课程时长换算成培训机构的课时，也是便宜的超级多。

### 2. 课程目录

整个API网关项目以渐进式的逐步迭代的方式进行设计和开发，包含小册和视频，你会看到从上章节到下一章节新增加了什么设计，添加了什么代码、做了哪些功能。所以即使是小白读者，也能跟着这样的流程做下来。

- 第 1 部分 - 通信组件
  - 第1章：HTTP请求会话协议处理
  - 第2章：代理RPC泛化调用
  - 第3章：分治处理会话流程
  - 第4章：将连接(RPC\HTTP\其他)抽象为数据源
  - 第5章：HTTP请求参数解析
  - 第6章：引入执行器封装服务调用
  - 第7章：权限认证组件(Shiro+Jwt)
  - 第8章：网关会话鉴权处理
  - 第16章：网络通信配置提取
- 第 2 部分 - 注册中心
  - 第9章：网关注册中心服务初始创建
  - 第10章：网关注册中心库表结构设计
  - 第11章：网关注册算力节点领域服务实现
  - 第12章：网关注册服务接口领域服务实现
  - 第14章：网关映射聚合信息查询实现
- 第 3 部分 - 服务发现
  - 第13章：服务发现组件搭建和注册网关连接
  - 第15章：服务配置拉取和组件使用验证
  - 第17章：核心通信组件管理和处理服务映射
  - 第18章：容器关闭监听和异常管理
  - 第22章：订阅服务注册消息驱动网关映射
  - 第25章：网关Nginx负载模型配置
  - 第26章：动态刷新网关Nginx负载均衡配置
  - 第27章：实现网关算力节点动态负载功能
- 第 4 部分 - 镜像文件
  - 第19章：网关引擎打包镜像部署
- 第 5 部分 - 服务注册
  - 第20章：服务注册组件搭建采集接口信息
  - 第21章：应用服务接口注册到注册中心
- 第 6 部分 - 运营后台
  - 第23章：网关运营管理后台框架搭建
  - 第24章：前后端分离应用的跨域接口调用
- 第 7 部分 - 扩展部分
  - 第 28 章：网关组件工程模块合并

### 3. 视频课程

<div align="center">
    <img src="https://bugstack.cn/images/article/assembly/api-gateway/api-gateway-0-06.png?raw=true" width="650px">
</div>

小傅哥为整个项目的每个章节都录制了对应的课程视频，在`小册`、`视频`、`代码`、`作业`，还有`提问回答`的全方位帮助下，你会美滋滋的吸收掉这个项目的技术，并且会让你有非常大的能力提升，也能为你的简历项目增光添彩。—— 所有的学习其实都是为夯实自己的能力，尤其是这样价格实惠，项目扎实的学习内容对自己的提升都是大的。
