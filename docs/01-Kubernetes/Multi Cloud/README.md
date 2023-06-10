---
title: Multi Cloud
---
>https://github.com/topics/multicloud

随着云计算的发展，为了避免被云服务商绑定（vendor lock-in）从而提高企业的议价能力，也为了提升服务的可用性（更多 Region/Zones），企业对多云的需求越来越强烈。

这个文件夹的内容，主要就是讲多云管理，即在同时使用多家云服务商的情况下，如何管理云环境的各种资源及权限、Kubernetes 集群、运行在 Kubernetes 中的微服务等等。

通常单个 Kubernetes 集群已经足够使用了，但是也要看到它仍然有局限性：

- 控制面可靠性：在版本升级、试用新功能时，始终存在一定的概率，集群控制面出现问题。我就遇到过在试用 `spec.behavior` 时，触发了 HPA 相关代码的空指针异常，导致 controller-manager 炸掉的情况。
- 多地容灾、就近访问：我们会需要在多地创建区域性的 Kubernetes 集群，统一管理、部署，再通过智能 DNS/LB 解析实现跨集群的就近访问、服务迁移能力。
- 集群容量：单个 Kubernetes 集群存在极限，官方宣称能管理 5000 节点，但实际上在 1000-1500 节点时，Etcd/APIServer 等组件就可能存在问题了。

因为上面这些问题，**多集群**仍然是有必要的。

但是多集群仍然存在几个重要的问题：

- 集群间的资源不平衡
- 跨集群访问困难
- 运维与管理成本高
- 多集群如何实现集群间的 fail-over？假设其中一个集群挂了，能否快速恢复或者隔离问题集群？

针对这几个问题，社区也出现了一些解决方案：

- [karmada](https://github.com/karmada-io/karmada): 一款跨集群应用的管理系统，可以将一个应用同时部署在多个集群，并且可以跨集群做实例调度(资源管理)、服务发现 + L4/L7 负载均衡。
  - 通过 ClusterPropagationPolicy 提供跨集群的 L4 负载均衡能力
  - 通过 MultiClusterIngress 提供 L7 负载均衡能力
- [kubevela](https://github.com/kubevela/kubevela): 基于 K8s 的应用平台，支持多集群/多云。
  - 支持基于各种标签、名称的资源跨集群调度，但是不负责解决服务发现、跨集群网络、跨集群负载均衡等问题。

其实很多场景下我们并不需要打通多集群之间的网络，但是基本上一定是需要一个共同的 L4/L7 网关。
如果你确实需要多集群之间的网络连通，除了 karmada 自带的方案外，还可以使用：

- 结合 Istio 可实现跨集群的服务网格能力，而且 Istio 支持对不同的 Region/Zone 设定不同的流量权重，比如强制所有流量都仅转发同一可用区内的其他实例。
- [Cilium Cluster Mesh](https://docs.cilium.io/en/v1.11/gettingstarted/clustermesh/clustermesh/)：支持打通集群间的 Pod-to-Pod 网络


而 K8s 集群自身的创建更新，社区已经提供了比较好的工具：

- [cluster-api](https://github.com/kubernetes-sigs/cluster-api): 以声明的方式在各云平台/裸机上部署或更新 Kubernetes 集群，确保多集群的配置一致性，也降低集群的维护难度。
## 多云管理的挑战

- FinOps: 多云成本分析、管控与优化
- 多云网络与 API 网关搭建
  - 多云网络：使用第三方多云解决方案，最好是开源的
  - 基于 DNS + 开源 API Gateway + Istio 多集群服务网格实现多云流量分发
- 跨云应用的发布更新、实例调度、监控、告警、日志、链路追踪
  - 可能的方案：借助 Kubernetes 与其他云原生解决方案实现，比如多集群管理工具 karmada
- 多云的资源管理：从 SRE 角度看借助 terraform/pulumi 来管理多云资源是个不错的方案
- 多云的权限统一管理：基本都是 RBAC，也可以考虑直接使用 terraform/pulumi 来管理

## 参考

- [Kubernetes 多集群项目介绍](https://xinzhao.me/posts/kubernetes-multi-cluster-projects/)
- [Kubernetes、集群联邦和资源分发](https://draveness.me/kuberentes-federation/)
- [Kubernetes Everywhere: Lessons Learned From Going Multi-Cloud - Niko Smeds, Grafana Labs](https://www.youtube.com/watch?v=ZY5h8Atc14A)



[基于Kubernetes的多云和混合云](https://mp.weixin.qq.com/s/uM4d3_fwLIdQ95fBWcmRjw)