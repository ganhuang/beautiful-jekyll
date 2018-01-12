# OpenShift V2 VS V3

从技术上来看，OpenShift V3 相较于 V2 是一个完全不同的产品。OpenShift V3 是基于 Kubernetes, Docker 等
最新开源技术重新打造的新一代容器平台。但是 OpenShift V3 在重新设计时，继承了 V2 上一些优秀的概念和
思想。

本文主要针对 V2 和 V3 做一个比较，从而帮助用户更好理解 V3 的起源。

## 架构上的对比

### Gears VS Container-engine

Gears 在 V2 中是一个核心组件。其中也是用到了 kernel namespaces, cGroups 和 SELinux 来实现容器技术。

V3 目前支持多种开源容器引擎，从 V3.0 开始使用的 Docker, 到 V3.7 所支持的 [cri-o](https://github.com/kubernetes-incubator/cri-o). 
但其中所使用到的底层技术也是 kernel namespaces, cgroup, systemd, selinux等等。

### Broker VS Master

在 V2 中 Broker 是用来编排，调度应用在 Gears 中运行的组件，同时提供 REST API 供客户端访问，调用。

Master 在 V3 中也是提供调度，编排的功能。

### V2 Node VS V3 Node

Node 的作用和概念在 V2 和 V3 中的概念很类似，和 Gears 或者 Container-Engine 运行在同一操作系统上，用来处理 Broker 或者 Master 的请求，
同时向 Gears 或者 Container-engine 发送请求。

### Cartridge VS Container images

二者都是提供一个容器运行时的环境。不过 Container images 所采用的是基于 Docker 格式的 Image，更易于分发和管理。

### MongoDB VS ETCD

二者都是作为数据库储存集群信息。但是 ETCD 提供了一个简单，安全，快速，可信的分布式 key-value 形式的数据库，更适用于分布式系统，以及
服务发现。

### ActiveMQ VS gRPC

V2 中 Broker 和 Node 通信采用的是ActiveMQ。

V3 中 Master 和 Node 之间采用性能更好，更高效的 gRPC 来进行通信。

## 应用层的对比

V2 中，应用只能是一个单元集，譬如一个前端数据库和一个后段数据库的组合。

在 V3 中，因为有 service 这样的概念，一个应用可以是多个 service 的组合，用户可以构建更复杂的应用。同时也可以更好的进行服务拆分来实现微服务
的架构。

## 总结

总之，相对来说，V3 是一个更先进，更适用现代应用开发的一个企业级容器平台。

拥抱 OpenShift V3!
