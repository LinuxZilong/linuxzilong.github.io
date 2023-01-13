# 理解 Kubernetes 的基本调度单位-Pod

```
容器本质是被Cgroups/Namespace隔离的linux进程，隔离了网络命名空间 pid空间等，一个进程一个容器不方便调度，比如调度计算问题，应用依赖问题和可观测问题，所以k8s采用pod为基本的调度单位，其进程共享相同的网络命名空间等，通过k8s.gcr.io/pause容器来进行命名空间的初始化。这样使得容器变得更加灵活，衍生出了很多代理模式如sidcare abserm 等
```

我们已经知道，容器是通过 Cgroups/Namespace+rootfs 实现的，它的核心就是 pid=1 的进程，可以说容器的本质就是被隔离的进程。

而 Kubernetes 的 Pod，就是进程组，也是 Kubernetes 的基本调度单位。把多个进程组合成一个进程组 Pod 进行调度，有非常多的好处：

- istio 在 pod 中注入辅助容器（也称 sidecar）实现服务网格
- 在不方便修改应用代码使日志输出到 stdout/stderr 的情况下，可以通过 sidecar 实现容器的日志收集
- argo-workflows 使用 sidecar 来管理 workflow 每个 step 的生命周期
- 可通过 init-containers 来在主容器启动前进行一些必要的环境配置
- ...

Pod 的实现：

- 通过一个 Infra 容器来 hold 容器的名字空间，默认只 hold network namespace
  - Infra 镜像为 `k8s.gcr.io/pause`，它使用汇编语言编写，解压后也只有 100k - 200k，非常轻量，基本不消耗性能
- 其他容器在 Infra 容器之后被创建启动，并加入到 Infra 的 network namespace 中

因此一个 Pod 中的所有容器是共享 network namespace 的，可以通过 `localhost` 互相访问。

那么 Pod 中的容器还可以共享哪些名字空间呢？Pod 有如下可选参数：

```yaml
apiVersion: v1
kind: Pod
...
spec:
  # 共享 pid 名字空间，这样容器就可以看到其他容器的进程
  # 可通过 ps aux 验证，会发现 1 号进程变成了 pause，并且可以看到 Pod 中的所有进程
  # 在某些场景中需要用到它，比如 argo-workflows 就使用它来监测任务容器的执行状态
  shareProcessNamespace: true
  # 直接使用宿主机网络，不新建 network namespace
  hostNetwork: true
  # 直接使用宿主机的 IPC namespace
  hostIPC: true
  # 直接使用宿主机的 pid namespace，这样容器就能看到宿主机的所有进程了
  hostPID: true
```

Pod 的所有参数定义：<https://github.com/kubernetes/api/blob/master/core/v1/types.go#L3059>

Pod 的容器之间，还可以共享数据卷，因为数据卷也是定义的 Pod 级别的。

## 特殊数据卷

- configmap/secret: 将敏感信息或配置投射到容器中
- Downward API: 将 pod 自身的信息投射到容器中
- ServiceAccountToken: 这是一个基于 K8s RBAC 体系的授权 token
  - 可用于授权访问 apiserver，它的内部访问地址为 `kubernetes:8443`
  - 也可用于授权访问其他服务，或者授权访问外部云服务

## 容器的健康检查与恢复

Pod 提供三种探针，分别有不同的作用：

- 启动探针: 在启动探针成功前，阻止其他探针执行
- 存活探针: 如果存活探测失败，重启 Pod
  - 重启 Pod 时，虽然 Pod 还是同一个，但底层的容器实际上已经被重新创建了！
  - 重启 Pod 时，因为 Pod 仍然是同一个，调度信息 `nodeName` 并未改变，因此仍然会运行在同一主机上
- 就绪探针: 如果就绪探针失败，将 Pod 从 Service 的 endpoints 中移除


# Pod 中的容器启动顺序、终止顺序


## PostStart

这实际上是利用了 kubernetes 的 postStart 功能，详见 [Delaying application start until sidecar is ready](https://medium.com/@marko.luksa/delaying-application-start-until-sidecar-is-ready-2ec2d21a7b74)，文中给出的 Pod 示例如下：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: sidecar-starts-first
spec:
  containers:
  - name: sidecar
    image: my-sidecar
    lifecycle:
      postStart:
        exec:
          command:
          - /bin/wait-until-ready.sh
          # Istio 1.7+ 的实现方式：
          # command:
          # - pilot-agent
          # - wait
  - name: application
    image: my-application
```

这个 triky 在官方支持 Sidecar 容器前，是个不错的临时解决方案。其他 Sidecar 软件也可以参考。

注意 postStart 脚本报错也会导致主容器立即启动。

另外还有一点，它只解决了启动顺序的问题，容器的终止顺序它处理不了，这需要借助 `preStop`，在最佳实践中有这个参数。

## Kubernetes 对 Sidecar 的支持

Kubernetes 有一个添加 Sidecar 支持的 KEP(k8s 优化提案): [Sidecar Containers - KEPS](https://github.com/kubernetes/enhancements/tree/master/keps/sig-node/753-sidecar-containers#implementation-history)

年初的时候曾经传出信息说在 1.18 中就会实现这个 KEP，但是实际上是个大乌龙。现在这个 KEP 已经退回到「provisional」状态，不知道什么时候会被拒绝或重新实现。



*jvm预热
优雅下线