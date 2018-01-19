# OpenShift 中的容器引擎

OpenShift V3 自第一个版本发布至今，其默认支持的容器引擎都是 Docker, 
这也是绝大多数以 k8s 为基础的容器平台的支持方式。

但是 Docker 公司作为一个商业公司，本身已经拥有自己的容器编排引擎 Docker Compose，
不可避免的与 k8s 形成了竞争关系。所以 Docker 从设计之初到后续发展并不是为 k8s
这样的平台服务。从而导致 k8s 平台在使用 Docker 时遇到各种各样兼容性问题。当然，
最近 Docker 开始拥抱 k8s，在最新 Docker 中也开始支持管理 k8s 集群，从侧面也体现
k8s 已经成为事实上容器编排系统的标准。

从技术角度上来看，在 k8s 1.5 之前，容器运行式是通过 Docker Shim 集成在 k8s 中进行
管理的。但是随着容器运行时的快速发展，渐渐开始出现了多种容器运行时，包括 Hyper, Rkt,
cri-containerd, cri-o。为了减轻 k8s 核心代码的维护量，社区引进容器运行时接口的概念-CRI。
在 k8s 中只提供运行时接口，具体实现只要容器运行时兼容这个接口即可。

包括网络，存储都是延用这种设计思路，来减轻 k8s 核心代码负担，同时提供更灵活的插件机制来
满足更多场景下的需求。

以下主要介绍在 OpenShift 中容器运行时的主要实现形式。

## Docker

截至 OpenShift v3.7, Docker 还是 OpenShift 中的默认容器引擎。包括 RHEL 及 Atomic Host 上，均是 
以 RPM 形式部署 Docker。

Docker 的存储驱动的选择和 Kernel 版本紧密联系。在 RHEL-7.4/Atomic Host 7.4 之前，推荐 Devicemapper,
之后推荐使用 Overlay2。

### Issues of Docker

如以上所述，由于 Docker 的兼容性问题, 不同版本的 OpenShift 需要匹配指定版本的 Docker,
在安装或者升级时会引入比较多依赖性和版本锁定的问题。为此 OpenShift 中引入 atomic-openshift-docker-excluder,
以及 Docker System Container 来解决这样的问题。

对于 Docker C/S 这种架构以及本身的特性，对于 Systemd 的支持不太友好。而容器中所运行的服务，希望
能被 Systemd 所接管。如果用 Systemd 通过 Docker 去管理服务，也就是把 `docker run` 封装在 service unit file
之中，实际上服务的运行时是在 Docker daemon 上，但是 Systemd 所管理的是 Docker Client。Systemd 的一些属性
无法在 Docker Container 所运行的服务中很好的体现。

为此 Redhat 开发了一套专门针对于 k8s 的容器运行时，兼容 CRI，CNI 和 OCI 标准。

## Docker System Container

System container 是基于 Atomic，runc 的命令行工具。服务的运行时是通过 runc 跑在 container 中。服务的管理通过 Systemd。

将 Docker 运行在以 systemd 管理，runc 运行的容器中，用户可以自由选择 Docker 的版本，而不必考虑兼容性，升级
带来的困扰。

## CRI-O

在 OpenShift 部署中，CRI-O 也是通过 system container 这种方式运行。

container 的启动，停止是通过 runc。
