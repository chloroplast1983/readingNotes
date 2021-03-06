# 13 | 为什么我们需要Pod？

## 笔记

`Pod`是`Kubernetes`项目的原子调度单位. 是`Kubernetes`项目中最小的`API`对象.

`Docker`容器的本质:

* `Namespace`做隔离
* `Cgroups`做限制
* `rootfs`做文件系统

可以理解为: 容器的本质是进程. `Kubernetes`就是操作系统. 在一个真正的操作系统里, 进程不是独自运行的, 而是以**进程组**的方式, "有原则地"组织在一起.

`Kubernetes`其实就是将**进程组**的概念映射到了容器技术中. 在`Google`的`Borg`中, 部署的应用都存在这类似与"进程和进程组"的关系.

**容器的单进程模型**并不是指容器里只能运行"一个"进程, 而是指容器没有管理多个进程的能力. 这是因为容器里`PID=1`的进程就是应用本身, 其他的进程都是这个`PID=1`进程的子进程. 用户编写的应用, 并不能够像正常操作系统里的`init`进程或者`systemd`那样拥有进程管理的功能.

`Pod`是`Kubernetes`里的原子调度单位. 这就意味着, `Kubernetes`项目的调度器, 是统一按照`Pod`而非容器的资源需求进行计算的.

### Pod的实现原理

`Pod`只是一个逻辑概念.

`Kubernetes`真正处理的, 还是宿主机操作系统上`Linux`容器的`Namespace`和 `Cgroups`, 而并不存在一个所谓的`Pod`的边界或者隔离环境. **Pod 是一组共享了某些资源的容器**.

**`Pod`里的所有容器, 共享的是同一个`Network Namespace`, 并且可以声明共享同一个 `Volume`**

在`Kubernetes`项目里, `Pod`的实现需要使用一个中间容器, 这个容器叫作`Infra`容器. 在这个`Pod`中, `Infra`容器永远都是第一个被创建的容器, 而其他用户定义的容器, 则通过 **Join Network Namespace**的方式, 与`Infra`容器关联在一起.

![](./img/13_01.png)

`Infra`容器一定要占用极少的资源, 所以它使用的是一个非常特殊的镜像, 叫作：`k8s.gcr.io/pause`. 这个镜像是一个用汇编语言编写的、永远处于**暂停**状态的容器, 解压后的大小也只有**100~200KB**左右.

**Pod 的生命周期只跟`Infra`容器一致, 而与容器 A 和 B 无关**

网络插件只需要关注`Infra`容器的`Network Namespace`即可.

**init Container**类型的容器, 称为**side car**. 指的就是我们可以在一个`Pod`中, 启动一个辅助容器, 来完成一些独立于主进程(主容器)之外的工作.

## 扩展