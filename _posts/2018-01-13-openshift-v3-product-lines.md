# OpenShift V3 产品线介绍

OpenShift 是面向开发者的一个 PaaS 平台，开发者基于 OpenShift 可以专注应用的代码开发，而不用
考虑应用环境的部署，扩展，监控和运维等各方面。

OpenShift V3 是基于 Kubernetes, Docker 等开源技术重新设计的新一代 PaaS 平台。其开源产品称为 OpenShift Origin,
企业级产品包括 OpenShift Container Platform (OCP), OpenShift Online (OSO), OpenShift Dedicated (OSD)。

本文将简要介绍以上产品线的区别以及应用场景。

## OpenShift Origin

就像 Redhat 其他企业级产品一样，OpenShift Origin 作为其企业级产品的上游社区，持续为创新提供源动力。
其采用 Apache 2.0 的许可证可以让普通用户自由的使用。

开源引领创新，创新基于用户。我们只有参与融入到社区，才能真正享受到开源所给我们带来的益处。

[OpenShift Origin Details](https://github.com/openshift/origin)

## OpenShift Container Platform (OCP)

OCP 作为 OpenShift Origin 的企业级版本，二者使用一套代码，但其能够给用户提供更稳定的服务，更强大的技术支撑。
与 Redhat 其他企业级产品一样，采用订阅模式进行管理，收费。

从企业级用户的角度来看，在保持创新性的同时，稳定性，安全性，可靠性显得更为关键。

在云的时代，OCP 不光提供本地化部署方式，同时支持云上部署模式，包过共有云平台AWS，GCP, Azure，以及私有云平台OpenStack，
Vsphere等。

## OpenShift Online (OSO)

OpenShift Online 是基于 OpenShift Origin 的技术所开发的一套运行在共有云平台上的线上 OpenShift 集群。

Redhat 提供资源和运维，个人用户可以直接使用。

目前给用户提供两种使用方式：

- `Starter`： 提供一定限制性的资源供个人用户免费学习和体验
- `Pro`： 根据资源使用情况按月收费


## OpenShift Dedicated (OSD)

众所周知，对于企业级用户来说，管理和运维一套分布式系统还是比较有挑战的，需要一定的专业性，以及相当的人力。
但是 OSO 又无法满足企业级用户安全性，隔离性的要求，因此推出 OSD 提供专有集群。

Redhat 提供资源和运维，目前提供 AWS 和 GCP 上的部署。按集群规模收费。

