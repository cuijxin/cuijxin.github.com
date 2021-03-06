---
layout:     post
title:      Kubernetes-整体概述和架构
subtitle:   Kubernetes教程/入门教程
date:       2018-06-01
author:     cjx
header-img: img/home-bg-art.jpg
catalog: true
tags:
    - K8S
---

## Kuberentes-整体概述和架构

### Kuberentes是什么

Kuberentes是一个轻便的和可扩展的开源平台，用于管理容器化应用和服务。通过Kubernetes能够进行应用的自动化部署和扩缩容。在Kuberentes中，会将组成应用的容器组合成一个逻辑单元以更易管理和发现。Kubrenetes积累了作为Google生产环境运行工作负载15年的经验，并吸收了来自社区的最佳想法和实践。Kubernetes经过这几年的快速发展，形成了一个大的生态环境，Google在2014年将Kuberentes作为开源项目。Kubernetes的关键特性包括：

1. 自动化装箱：在不牺牲可用性的条件下，基于容器对资源的要求和约束自动部署容器。同时，为了提高利用率和节省更多资源，将关键和最佳工作量结合在一起。

2. 自愈能力：当容器失败时，会对容器进行重启；当部署的Node节点有问题时，会对容器进行重新部署和重新调度；当容器未通过监控检查时，会关闭此容器；直到容器正常运行时，才会对外提供服务。

3. 水平扩容：通过简单的命令、用户界面或基于CPU的使用情况，能够对应用进行扩容和缩容。

4. 服务发现和负载均衡：开发者不需要使用额外的服务发现机制，就能够基于Kubernetes进行服务发现和负载均衡。

5. 自动发布和回滚：Kubernetes能够程序化的发布应用和相关的配置。如果发布有问题，Kubernetes将能够回归发生的变更。

6. 保密和配置管理：在不需要重新构建镜像的情况下，可以部署和更新保密和应用配置。

7. 存储编排：自动挂接存储系统，这些存储系统可以来自于本地、公共云提供商（例如：GCP和AWS）、网络存储（例如：NFS、iSCSI、Gluster、Ceph、Cinder和Floker等）。

### Kubernetes的整体架构

![](/img/kubernetes架构.png)

Kubernetes属于主从分布式架构，主要由Master Node和Worker Node组成，以及包括客户端命令行工具kubectl和其它附加项。

1. Master Node：作为控制节点，对集群进行调度管理；Master Node由API Server、Scheduler、Cluster State Store和Controller-Manger Server所组成；

2. Worker Node：作为真正的工作节点，运行业务应用的容器；Worker Node包含kubelet、kube proxy和Container Runtime；

3. kubectl：用于通过命令行与API Server进行交互，而对Kubernetes进行操作，实现在集群中进行各种资源的增删改查等操作；

4. Add-on：是对Kubernetes核心功能的扩展，例如增加网络和网络策略等能力。

### Master Node（主节点）

#### API Server（API服务器）

API Server主要用来处理REST的操作，确保它们生效，并执行相关业务逻辑，以及更新etcd（或者其他存储）中的相关对象。API Server是所有REST命令的入口，它的相关结果状态被保存在etcd（或者其他存储）中。API Server的基本功能包括：

1. REST语义，监控，持久化和一致性保证，API版本控制，放弃和生效等；

2. 内置准入控制语义，同步准入控制钩子，以及异步资源初始化；

3. API注册和发现；

另外，API Server也作为集群的网关。默认情况，客户端通过API Server对集群进行访问，客户端需要通过认证，并使用API Server作为访问Node和Pod（以及service）的堡垒和代理/通道。