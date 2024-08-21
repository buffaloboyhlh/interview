# kubernetes 基础

Kubernetes（K8s）是一个开源的容器编排平台，广泛应用于管理容器化应用的自动部署、扩展和操作。它能够帮助开发者和运维团队以可扩展和高效的方式管理复杂的容器化应用。下面是一份详解 Kubernetes 的教程，涵盖了从基础概念到高级功能的内容。

## Pod 

Kubernetes 中的 Pod 是集群中最小且最基础的可部署单元，它包含一个或多个容器（通常是 Docker 容器）。Pod 是 Kubernetes 中用于运行应用的基本构件。理解 Pod 的结构和功能对掌握 Kubernetes 至关重要。以下是对 Kubernetes Pod 的详细介绍：

### 1. Pod 的基本概念

- **单容器 Pod**：通常，一个 Pod 包含一个容器，这是最常见的情况。单容器 Pod 是最直接的部署方式。
  
- **多容器 Pod**：在某些情况下，多个容器需要紧密合作，它们共享资源，如网络和存储卷。这时，这些容器可以被放入同一个 Pod 中。Pod 内的多个容器通过共享相同的网络命名空间和存储卷，可以通过 `localhost` 进行通信。多容器 Pod 通常用于需要共享同一网络和存储卷的情况，比如代理和主应用容器的组合。

### 2. Pod 的生命周期

Pod 是短暂的实体，Kubernetes 会根据需求启动和终止 Pod。Pod 的生命周期包括以下几个阶段：

- **Pending**：Pod 已被 Kubernetes API Server 接收，但尚未被调度到节点上。此时可能在等待资源（如 Persistent Volume）绑定或节点资源可用。
  
- **Running**：Pod 已被调度到节点上，所有容器都已启动。Pod 进入 Running 状态意味着容器正在运行或正在启动。

- **Succeeded**：Pod 中的所有容器都已成功终止且不会重启。这个状态通常适用于一次性任务或批处理任务。

- **Failed**：Pod 中的容器之一或多个已经终止，并且至少有一个容器终止时遇到了失败。

- **CrashLoopBackOff**：这是一个特殊状态，表示容器正在崩溃并不断重启。这通常意味着容器内的应用程序有问题。

### 3. Pod 的结构

一个 Pod 的 YAML 配置文件通常包括以下主要部分：

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels:
    app: myapp
spec:
  containers:
  - name: my-container
    image: nginx:latest
    ports:
    - containerPort: 80
  volumes:
  - name: my-volume
    emptyDir: {}
```

#### 3.1 Metadata

- **name**：Pod 的名称，用于唯一标识集群中的 Pod。
- **labels**：Pod 的标签，用于分组和选择 Pod，如服务选择器、Deployment 等会用到标签。

#### 3.2 Spec

- **containers**：这是 Pod 中最重要的一部分，定义了要运行的一个或多个容器。每个容器可以指定镜像、端口、资源需求、环境变量、挂载的卷等。
  
  - **name**：容器的名称。
  - **image**：容器镜像，Kubernetes 会拉取该镜像并运行容器。
  - **ports**：定义容器暴露的端口。

- **volumes**：定义 Pod 中使用的存储卷，这些卷可以在多个容器之间共享。

#### 3.3 Status

Pod 的状态信息，如容器的运行状态、重启次数、IP 地址等，通常由 Kubernetes 动态管理和更新。

### 4. Pod 的网络模型

每个 Pod 都拥有一个唯一的 IP 地址，Pod 内的容器共享这个 IP。Pod 之间通过集群的虚拟网络进行通信。

- **Pod IP**：Pod 的 IP 地址由 Kubernetes 分配，这个 IP 在 Pod 的生命周期内保持不变，但当 Pod 被销毁并重新创建时，IP 可能会发生变化。
- **localhost**：Pod 内的所有容器都可以通过 `localhost` 直接访问彼此，因为它们共享同一个网络命名空间。

### 5. Pod 的存储卷

Pod 可以使用多种类型的存储卷，以实现数据持久化或在容器之间共享数据。常见的存储卷类型包括：

- **emptyDir**：一个空目录卷，Pod 生命周期内的数据会被保留，Pod 删除时数据丢失。
- **hostPath**：将主机文件系统的一个目录挂载到 Pod 内。
- **persistentVolumeClaim（PVC）**：通过 PVC 请求和绑定持久存储卷（PV）。

### 6. Pod 的调度

Pod 是由 Kubernetes 调度器根据集群资源和调度策略分配到具体的节点上的。调度器会考虑节点的资源利用率、Pod 的资源需求、节点的亲和性/反亲和性策略等因素。

- **节点选择器（NodeSelector）**：可以指定 Pod 必须调度到某些节点上。
- **节点亲和性（NodeAffinity）**：更灵活的方式定义 Pod 调度到特定节点的规则。
- **Pod 反亲和性（PodAffinity/PodAntiAffinity）**：控制 Pod 是否应该与其他 Pod 放在同一个节点上或分散到不同节点上。

### 7. Pod 的重启策略

Kubernetes 提供了不同的重启策略，用于控制 Pod 中容器的重启行为：

- **Always**：这是默认策略，表示容器退出后总是重启。
- **OnFailure**：仅在容器以非 0 状态退出时重启。
- **Never**：不重启容器，无论其退出状态如何。

### 8. Pod 控制器

虽然 Pod 是 Kubernetes 中的基础单元，但通常不会直接管理 Pod。相反，使用控制器（如 Deployment、ReplicaSet、StatefulSet 等）来管理 Pod，确保集群中有适当数量的 Pod 正在运行。

### 9. Pod 的健康检查

Kubernetes 支持对 Pod 中的容器进行健康检查，通过以下两种机制来确定容器是否正常：

- **Liveness Probe**：检查容器是否健康，如果检查失败，Kubernetes 会重启该容器。
- **Readiness Probe**：检查容器是否已准备好接收流量，只有准备好的容器才会接收服务流量。

```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 3
  periodSeconds: 3

readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 3
  periodSeconds: 3
```

### 10. Pod 的水平扩展和自动缩放

Kubernetes 支持通过 Horizontal Pod Autoscaler（HPA）根据 CPU 使用率或其他自定义指标自动扩展和缩减 Pod 的数量。

使用 kubectl autoscale 命令或创建 HPA 配置文件来配置自动缩放：
```
kubectl autoscale deployment my-deployment --cpu-percent=50 --min=1 --max=10
```
上面的命令表示，当 CPU 使用率超过 50% 时，Kubernetes 会自动扩展 my-deployment 的副本数，最少 1 个，最多 10 个 Pod。




### 总结

Pod 是 Kubernetes 中用于运行容器化应用的最小单位，了解 Pod 的结构、网络、存储和管理机制，是掌握 Kubernetes 管理和部署的基础。通过合理配置和使用 Pod，你可以在 Kubernetes 中有效地管理和扩展你的应用程序。


## 控制器（Controller）

Kubernetes 控制器（Controller）是 Kubernetes 中的一类资源对象，用于管理集群中不同类型的工作负载和资源。控制器通过监控 Kubernetes 集群的状态，并不断与期望的状态对比，从而自动调整集群中的资源，确保系统的期望状态和实际状态保持一致。

控制器的核心功能是自动化管理任务，例如创建、更新、删除 Pod，确保指定数量的 Pod 副本始终在运行，或者在节点故障时自动迁移工作负载。以下是 Kubernetes 中几种常见的控制器的详细介绍。

### 1. Deployment 控制器

**Deployment** 是 Kubernetes 中最常用的控制器，用于管理无状态应用的 Pod 和 ReplicaSet。它提供了一种声明式的方法来创建和管理 Pod 的副本，并支持滚动更新和回滚。

- **功能**：
  - 管理无状态应用的部署。
  - 支持滚动更新（Rolling Updates）和回滚（Rollback）。
  - 确保始终有指定数量的 Pod 副本在运行。
  
- **用法**：

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

在上面的例子中，Deployment 会创建并维护 3 个运行 `nginx` 容器的 Pod 副本。

### 2. ReplicaSet 控制器

**ReplicaSet** 是一个较低级别的控制器，用于确保指定数量的 Pod 副本在任何时候都在运行。ReplicaSet 是 Deployment 的基础，通常直接使用 Deployment 来管理无状态应用，而不是直接使用 ReplicaSet。

- **功能**：
  - 确保一定数量的 Pod 副本始终在运行。
  - 自动替换失败的 Pod。

- **用法**：

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: my-container
        image: nginx
        ports:
        - containerPort: 80
```

### 3. StatefulSet 控制器

**StatefulSet** 是 Kubernetes 中用于管理有状态应用的控制器。与 Deployment 不同，StatefulSet 确保 Pod 的部署顺序和命名保持一致，适用于数据库、分布式文件系统等有状态服务。

- **功能**：
  - 保持 Pod 的稳定标识符（如 Pod 名称）。
  - 按顺序启动和终止 Pod。
  - 支持持久存储卷的自动管理。

- **用法**：

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: my-statefulset
spec:
  serviceName: "nginx"
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
      volumeMounts:
      - name: www
        mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
```

在这个例子中，每个 Pod 都会有一个稳定的网络标识和存储卷。

### 4. DaemonSet 控制器

**DaemonSet** 是用于在集群中的每个节点上运行一个 Pod 副本的控制器，通常用于部署系统级别的服务，如日志收集、监控代理等。

- **功能**：
  - 在每个节点上部署一个 Pod 副本。
  - 自动在新加入的节点上创建 Pod，并在节点删除时清理 Pod。

- **用法**：

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-daemonset
spec:
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: my-container
        image: my-daemon-image
```

### 5. Job 控制器

**Job** 控制器用于管理一次性任务。它确保任务成功完成，通常用于批处理任务，如数据处理、备份等。

- **功能**：
  - 运行一个或多个 Pod，直到任务成功完成。
  - 支持并行任务处理。

- **用法**：

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: my-job
spec:
  template:
    metadata:
      labels:
        app: myjob
    spec:
      containers:
      - name: my-container
        image: busybox
        command: ["sh", "-c", "echo Hello Kubernetes! && sleep 30"]
      restartPolicy: Never
  backoffLimit: 4
```

在这个例子中，Job 会运行一个容器，执行一个简单的任务，然后退出。

### 6. CronJob 控制器

**CronJob** 是 Kubernetes 中的定时任务控制器，类似于 Linux 的 cron 工具，用于在指定的时间间隔自动运行任务。

- **功能**：
  - 在指定的时间间隔运行 Job。
  - 支持标准 cron 表达式。

- **用法**：

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: my-cronjob
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: my-container
            image: busybox
            command: ["sh", "-c", "date; echo Hello from the Kubernetes cluster"]
          restartPolicy: OnFailure
```

这个 CronJob 每分钟运行一次，执行简单的任务。

### 7. HorizontalPodAutoscaler 控制器

**HorizontalPodAutoscaler (HPA)** 控制器根据 CPU 使用率或其他自定义指标自动调整 Pod 的副本数量，确保应用在负载波动时仍能稳定运行。

- **功能**：
  - 根据指定指标自动调整 Pod 的副本数。
  - 支持 CPU、内存及自定义指标的监控和扩展。

- **用法**：

```yaml
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: my-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-deployment
  minReplicas: 1
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

### 8. 配置管理控制器

**ConfigMap** 和 **Secret** 是 Kubernetes 中的两种配置管理资源，尽管它们不是传统意义上的控制器，但它们用于存储和管理非敏感与敏感配置信息，并可以注入到 Pod 中使用。

- **ConfigMap**：用于存储非敏感的配置信息，如环境变量、配置文件等。
- **Secret**：用于存储敏感信息，如密码、证书等。

- **用法**：

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  config.json: |
    {
      "key": "value"
    }
```

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  password: cGFzc3dvcmQ=
```

### 总结

Kubernetes 控制器是自动化 Kubernetes 集群管理的关键组成部分。通过使用各种控制器，Kubernetes 能够自动化地处理应用程序的部署、扩展、升级和运维任务，从而确保集群的稳定性和高可用性。掌握这些控制器的概念和使用方法，是深入理解和有效管理 Kubernetes 的基础。


## Service

Kubernetes 的 Service 是一个关键的网络组件，用于将应用（即 Pods）暴露给其他应用或外部用户。Service 提供了一个稳定的 IP 地址和 DNS 名称，用于访问动态变化的 Pod 集合。它还实现了负载均衡，即使 Pod 动态增加或减少，客户端仍能通过 Service 进行访问。以下是对 Kubernetes Service 的详细解析：

### 1. **Service 的基本概念**
- **定义**：Service 是一种 Kubernetes 资源，它为一组逻辑上相同的 Pods 提供了一个单一的访问点。即使 Pods 的 IP 地址不断变化，Service 依然保持稳定的访问方式。
- **作用**：解决 Pod 的动态 IP 问题，通过负载均衡将流量分发到一组 Pods，支持集群内部和外部的访问。

### 2. **Service 类型**
Kubernetes 提供了几种主要的 Service 类型，每种类型适用于不同的使用场景：

#### **ClusterIP**
- **默认类型**：这是 Kubernetes 中的默认 Service 类型。
- **访问方式**：仅在集群内部可访问，通过分配的 ClusterIP 来提供服务。
- **应用场景**：适用于集群内部的服务通信，比如微服务架构中各服务之间的调用。

#### **NodePort**
- **外部访问**：NodePort 类型允许在集群外部通过集群中任意节点的 IP 和指定的端口号访问服务。
- **工作原理**：Kubernetes 在每个节点上开放一个指定范围内的端口（通常在 30000 到 32767 之间），外部用户可以通过 `<NodeIP>:<NodePort>` 访问服务。
- **应用场景**：适用于在没有负载均衡器的环境中，直接从外部访问集群内的服务。

#### **LoadBalancer**
- **云环境支持**：LoadBalancer 类型用于在公有云环境下创建一个外部负载均衡器，并自动分配一个外部 IP 地址。
- **工作原理**：该服务类型会通过云提供商的 API 创建一个负载均衡器，外部用户通过负载均衡器的 IP 地址访问服务，Kubernetes 负责将流量转发到对应的 Pods。
- **应用场景**：适用于需要高可用性、自动扩展的应用场景，通常在云环境中使用。

#### **ExternalName**
- **DNS 名称映射**：ExternalName 类型将 Service 的 DNS 名称映射到外部的 DNS 名称，不使用 ClusterIP。
- **工作原理**：当客户端访问 Service 时，Kubernetes 会将请求重定向到指定的外部 DNS 名称。
- **应用场景**：用于集群内部的应用需要访问集群外部的服务，比如访问外部的数据库或第三方 API。

### 3. **Service 的工作机制**
Service 通过以下几个关键组件来实现其功能：

#### **Label Selector**
- **选择 Pods**：Service 使用标签选择器（Label Selector）来选择一组匹配标签的 Pods，形成服务的后端。
- **动态更新**：当 Pods 动态增加或减少时，Service 会自动更新其后端 Pods 列表。

#### **Endpoints**
- **存储 Pod 信息**：Endpoints 是一个 Kubernetes 资源，存储了属于某个 Service 的 Pod 的 IP 地址和端口信息。随着 Service 后端 Pods 的变化，Endpoints 对象会自动更新。

#### **Kube-proxy**
- **流量转发和负载均衡**：Kube-proxy 运行在每个 Kubernetes 节点上，负责将 Service 的请求流量转发到后端的 Pods。它使用以下几种模式实现：
  - **Userspace 模式**：较旧的方式，通过用户空间处理流量。
  - **IPTables 模式**：通过 iptables 规则在内核层面实现流量转发和负载均衡。
  - **IPVS 模式**：基于 Linux 内核的 IPVS 提供高效的负载均衡，适合大规模集群。

### 4. **服务发现**
Kubernetes 提供了两种主要的服务发现机制，使得其他 Pods 可以方便地访问 Service：

#### **环境变量**
- **自动注入**：在 Pod 启动时，Kubernetes 会为其注入环境变量，包含集群中所有 Service 的 ClusterIP 和端口信息。这些环境变量在 Pod 启动时生成，无法反映后续 Service 的动态变化。

#### **DNS**
- **CoreDNS 插件**：Kubernetes 内建 DNS 服务（通常由 CoreDNS 提供），为每个 Service 创建 DNS 记录。Pods 可以通过 `service-name.namespace.svc.cluster.local` 的格式访问 Service。

### 5. **负载均衡**
- **内建负载均衡**：Service 会自动将请求流量均匀分配到后端 Pods 上，从而实现负载均衡。
- **会话亲和性**：通过配置 `sessionAffinity` 参数，可以将来自同一客户端的请求始终发送到同一个 Pod（常见的实现方式是基于客户端 IP）。

### 6. **高级功能**
- **Headless Service**：一种特殊的 Service，不分配 ClusterIP。通常用于有状态的应用（如 StatefulSets），允许客户端直接访问指定的 Pod。
- **External Traffic Policy**：控制外部流量的路由策略，可以选择将流量路由到本地节点的 Pods（Local 模式）或允许流量路由到所有节点的 Pods（Cluster 模式）。

### 7. **Service 实例**
以下是一个 Service 的示例配置：

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: ClusterIP  # 可以是 ClusterIP, NodePort, LoadBalancer, ExternalName
```

这个 Service 通过选择标签为 `app: my-app` 的 Pods，将流量转发到这些 Pods 的 8080 端口，并通过 ClusterIP 在集群内提供 80 端口的服务。

### 8. **总结**
Kubernetes Service 是实现应用网络连接的基础，它提供了稳定的访问方式、负载均衡、服务发现以及跨集群的访问能力。通过不同类型的 Service，可以满足各种场景下的应用需求，从集群内部通信到外部用户访问，Service 都发挥着至关重要的作用。理解并熟练掌握 Service 的使用，是 Kubernetes 运维中的重要技能。


## kubernetes 持久化

在 Kubernetes（K8s）环境中，持久化存储是指在容器重启、迁移或删除后仍然保存的数据。Kubernetes 中的存储持久化主要通过 **PersistentVolume (PV)** 和 **PersistentVolumeClaim (PVC)** 来实现。

### 1. **PersistentVolume (PV)**
PV 是由管理员提供的存储资源，类似于集群中的存储设备。它有自己的生命周期，是集群中的资源。PV 支持多种存储后端，包括 NFS、iSCSI、AWS EBS、GCE PD、Azure Disk 等。

#### PV 的主要属性：
- **容量**：指定存储的大小。
- **访问模式**：定义 PV 如何被挂载。常见的访问模式包括：
  - `ReadWriteOnce (RWO)`：只能被单个节点以读写模式挂载。
  - `ReadOnlyMany (ROX)`：可以被多个节点以只读模式挂载。
  - `ReadWriteMany (RWX)`：可以被多个节点以读写模式挂载。
- **持久化存储类 (StorageClass)**：定义了 PV 的存储类型，如标准存储、快速存储等。
- **回收策略**：指定 PV 被释放后的行为，如保留数据 (`Retain`)、删除数据 (`Delete`) 或重新绑定 (`Recycle`)。

### 2. **PersistentVolumeClaim (PVC)**
PVC 是用户请求存储资源的一种方式。它类似于 Pod 申请 CPU 和内存的方式。用户通过 PVC 请求特定大小和访问模式的存储资源。PVC 绑定到一个符合条件的 PV 上，Kubernetes 会自动处理这个过程。

#### PVC 的主要属性：
- **请求容量**：用户请求的存储大小。
- **访问模式**：与 PV 中的访问模式一致。
- **存储类**：指定要使用的存储类型。

### 3. **StorageClass**
StorageClass 是一种动态提供 PV 的机制。管理员可以定义不同的 StorageClass 来表示不同的存储类型（如 SSD、HDD）或不同的云提供商。每个 StorageClass 包含一个 `provisioner` 字段，决定了如何创建实际的存储资源。

#### StorageClass 的主要属性：
- **provisioner**：指定存储资源的供应商（如 `kubernetes.io/aws-ebs`）。
- **parameters**：定义特定供应商的参数（如磁盘类型、IOPS 等）。
- **reclaimPolicy**：指定 PV 被释放后的回收策略。

### 4. **动态存储**
Kubernetes 支持通过 StorageClass 动态创建 PV。用户创建 PVC 时，如果没有找到匹配的 PV，Kubernetes 会根据 PVC 请求的存储类和大小，动态创建一个 PV。

### 5. **PV、PVC 和 StorageClass 的关系**
- 管理员定义 PV 和 StorageClass。
- 用户创建 PVC 请求存储资源。
- Kubernetes 根据 PVC 的请求，自动匹配或动态创建 PV。
- PVC 和 PV 绑定后，Pod 可以将 PVC 挂载为卷使用，从而实现数据持久化。

### 6. **使用示例**

以下是创建 PV、PVC 和使用它们的一个简单示例：

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: example-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data

---

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: example-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi

---

apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
  - name: example-container
    image: nginx
    volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: example-volume
  volumes:
  - name: example-volume
    persistentVolumeClaim:
      claimName: example-pvc
```

### 7. **持久化存储的最佳实践**
- **选择合适的存储类型**：根据应用程序的需求选择合适的存储类型（如高性能 SSD 或大容量 HDD）。
- **设置正确的访问模式**：确保设置的访问模式与应用程序的需求匹配。
- **使用动态存储**：使用 StorageClass 和动态 PV 供应可以简化存储管理。
- **监控和备份**：定期监控 PV 的使用情况，并做好备份策略，以防数据丢失。

Kubernetes 的持久化存储机制使得容器化应用可以安全可靠地管理数据，即使在容器生命周期结束后，数据依然得以保存。

## kubernetes 配置管理中心

在 Kubernetes 中，配置管理中心是指用于管理应用配置数据的一组资源和工具。这些资源可以帮助应用程序在不重新构建镜像或重新部署的情况下进行配置的修改。Kubernetes 提供了两种主要的配置管理资源：**ConfigMap** 和 **Secret**。

### 1. **ConfigMap**

**ConfigMap** 是用于存储非机密配置信息的对象。它允许你将配置信息以键值对的形式存储，并可以在 Pod 中作为环境变量、命令行参数或配置文件来使用。

#### **ConfigMap 的创建方法**
- **从命令行创建**：你可以通过 kubectl 命令从文件、字典或字面值创建 ConfigMap。
  
  ```bash
  kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2
  ```

- **通过 YAML 文件创建**：
  
  ```yaml
  apiVersion: v1
  kind: ConfigMap
  metadata:
    name: my-config
  data:
    key1: value1
    key2: value2
  ```

#### **使用 ConfigMap 的方法**
1. **作为环境变量**：将 ConfigMap 中的键值对作为 Pod 的环境变量。

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: my-pod
   spec:
     containers:
     - name: my-container
       image: nginx
       envFrom:
       - configMapRef:
           name: my-config
   ```

2. **挂载为卷**：将 ConfigMap 中的数据作为文件挂载到容器中。

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: my-pod
   spec:
     containers:
     - name: my-container
       image: nginx
       volumeMounts:
       - name: config-volume
         mountPath: /etc/config
     volumes:
     - name: config-volume
       configMap:
         name: my-config
   ```

3. **作为命令行参数**：通过环境变量或文件将配置注入到容器的命令行参数中。

### 2. **Secret**

**Secret** 是用于存储敏感数据的对象，如密码、OAuth 令牌和 SSH 密钥。Secret 与 ConfigMap 类似，但它会对数据进行 base64 编码，并且 Kubernetes 在处理 Secret 时会采取一些额外的安全措施，比如避免将 Secret 显示在日志中。

#### **Secret 的创建方法**
- **从命令行创建**：你可以通过 kubectl 命令从字面值或文件创建 Secret。

  ```bash
  kubectl create secret generic my-secret --from-literal=username=admin --from-literal=password='P@ssw0rd'
  ```

- **通过 YAML 文件创建**：
  
  ```yaml
  apiVersion: v1
  kind: Secret
  metadata:
    name: my-secret
  type: Opaque
  data:
    username: YWRtaW4=   # "admin" base64 编码
    password: UEBzc3cwcmQ=   # "P@ssw0rd" base64 编码
  ```

#### **使用 Secret 的方法**
1. **作为环境变量**：将 Secret 中的键值对作为 Pod 的环境变量。

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: my-pod
   spec:
     containers:
     - name: my-container
       image: nginx
       env:
       - name: USERNAME
         valueFrom:
           secretKeyRef:
             name: my-secret
             key: username
       - name: PASSWORD
         valueFrom:
           secretKeyRef:
             name: my-secret
             key: password
   ```

2. **挂载为卷**：将 Secret 中的数据作为文件挂载到容器中。

   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: my-pod
   spec:
     containers:
     - name: my-container
       image: nginx
       volumeMounts:
       - name: secret-volume
         mountPath: /etc/secret
     volumes:
     - name: secret-volume
       secret:
         secretName: my-secret
   ```

### 3. **ConfigMap 和 Secret 的差异**
- **用途**：ConfigMap 主要用于存储非敏感的配置信息，而 Secret 用于存储敏感信息。
- **数据编码**：ConfigMap 中的数据是明文存储的，Secret 中的数据则以 base64 编码形式存储。
- **安全性**：Secret 在 Kubernetes 内部的处理更加谨慎，例如限制默认权限，并且避免在日志中暴露其内容。

### 4. **最佳实践**
- **分离配置和代码**：将应用配置独立管理，使应用程序能够更灵活地适应不同的环境。
- **安全管理 Secret**：尽量减少 Secret 的暴露范围，使用 RBAC 控制访问权限，避免将敏感信息存储在版本控制系统中。
- **版本控制和审计**：确保配置变更有记录，并且能追溯到具体变更内容和责任人。
- **动态更新**：利用 ConfigMap 和 Secret 的自动更新特性，确保应用程序配置能实时更新而无需重启服务。

通过 Kubernetes 的配置管理机制，开发者和运维人员能够更加灵活、高效地管理应用的配置信息，从而实现应用的快速迭代和安全部署。


## Ingress和Ingress Controller

在 Kubernetes 中，**Ingress** 和 **Ingress Controller** 是用于管理和控制集群外部访问集群内部服务的核心组件。它们帮助应用程序实现基于 HTTP 和 HTTPS 的路由，允许通过一个统一的入口点来管理多个服务的访问。

### 1. **Ingress**

**Ingress** 是 Kubernetes 中的一种 API 对象，用于定义如何将集群外部的 HTTP 和 HTTPS 流量路由到集群内部的服务。它是一个七层（应用层）负载均衡器，支持基于域名、路径等的规则进行流量转发。

#### **Ingress 的主要功能**
- **基于域名的路由**：可以根据不同的主机名将流量转发到不同的服务。
- **基于路径的路由**：可以根据 URL 路径将流量转发到不同的服务。
- **SSL/TLS 终止**：支持 HTTPS 协议，通过提供证书来处理 SSL/TLS 终止。
- **反向代理**：提供 HTTP/HTTPS 的反向代理功能，将请求转发到合适的服务。

#### **Ingress 的示例**

下面是一个简单的 Ingress 资源配置示例：

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /service1
        pathType: Prefix
        backend:
          service:
            name: service1
            port:
              number: 80
      - path: /service2
        pathType: Prefix
        backend:
          service:
            name: service2
            port:
              number: 80
  tls:
  - hosts:
    - example.com
    secretName: example-tls
```

这个配置示例说明了：
- `example.com/service1` 的请求将转发到 `service1` 服务。
- `example.com/service2` 的请求将转发到 `service2` 服务。
- 使用了 `example-tls` Secret 来处理 HTTPS 请求。

### 2. **Ingress Controller**

**Ingress Controller** 是实际实现 Ingress 资源中定义的路由规则的组件。Kubernetes 本身并不包含 Ingress Controller，需要通过部署不同的第三方实现来提供这一功能。每个 Ingress Controller 可以有不同的功能和特性，常见的 Ingress Controller 有：

- **NGINX Ingress Controller**：使用 NGINX 作为反向代理和负载均衡器，广泛使用且功能丰富。
- **Traefik**：一种现代化的反向代理和负载均衡器，支持动态配置和多种协议。
- **HAProxy Ingress Controller**：基于 HAProxy 实现的高性能负载均衡器。
- **GCE Ingress Controller**：用于 Google Kubernetes Engine (GKE) 的原生 Ingress Controller，利用 Google Cloud Load Balancer 实现。

#### **部署 Ingress Controller**

以 NGINX Ingress Controller 为例，通常可以通过 Helm 或 YAML 文件直接部署：

```bash
# 使用 Helm 安装 NGINX Ingress Controller
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install my-nginx ingress-nginx/ingress-nginx
```

部署完成后，Ingress Controller 会监听 Kubernetes API 中的 Ingress 资源，并根据定义的规则动态配置路由。

### 3. **Ingress 和 Ingress Controller 的关系**

- **Ingress** 是一种定义流量路由规则的 Kubernetes 资源。
- **Ingress Controller** 是实现这些规则的实际组件。不同的 Ingress Controller 会以不同的方式处理这些规则，并将外部流量路由到正确的服务。

当你在 Kubernetes 集群中创建一个 Ingress 资源时，Ingress Controller 会自动检测到这一变化，并相应地配置底层的负载均衡器或反向代理以处理外部流量。

### 4. **Ingress 的优势**
- **集中管理外部流量**：Ingress 提供了一个统一的入口点，简化了服务的外部访问管理。
- **灵活的路由规则**：支持基于域名、路径的灵活路由配置。
- **负载均衡**：集成了负载均衡功能，可以将流量分发到多个后端服务。
- **SSL/TLS 支持**：支持 HTTPS，通过配置证书轻松实现安全的通信。

### 5. **最佳实践**
- **选择合适的 Ingress Controller**：根据应用需求选择合适的 Ingress Controller，如性能需求、安全性需求等。
- **合理规划路由规则**：设计合理的域名和路径规则，避免冲突，确保流量正确转发。
- **启用 SSL/TLS**：对于生产环境，务必启用 HTTPS 并正确配置证书。
- **监控和日志**：监控 Ingress Controller 的性能和流量，并通过日志分析请求的访问情况和错误。

通过使用 Ingress 和 Ingress Controller，Kubernetes 提供了一种高效且可扩展的方式来管理集群外部流量的入口，使应用程序能够轻松、安全地暴露给外部用户。


## Helm 包管理工具

Helm 是 Kubernetes 的包管理工具，它通过 Chart（即应用程序包）来管理 Kubernetes 中的应用部署、更新和配置。Helm 能够简化应用的部署过程，提供版本控制和回滚功能，从而大大提升了 Kubernetes 应用的可管理性和可维护性。

### 1. **Helm 的基本概念**

在深入了解 Helm 之前，先了解以下几个基本概念：

- **Chart**：Chart 是一个包含了 Kubernetes 应用的配置模板集合，类似于 Linux 系统中的软件包。它描述了应用程序的所有 Kubernetes 资源（如 Deployment、Service、ConfigMap 等）。

- **Repository**：Helm 仓库是存放 Chart 的地方，可以是公共仓库（如 Helm 官方仓库）或者私有仓库。Helm 从这些仓库中拉取 Chart 用于应用安装。

- **Release**：每个通过 Helm 安装的 Chart 实例称为一个 Release，代表了 Chart 在 Kubernetes 集群中的一次部署。每次安装、升级、回滚都生成一个新的 Release 版本。

- **Values**：Values 是 Helm Chart 的配置参数，可以在部署时通过 `values.yaml` 文件或命令行覆盖默认值，从而定制化应用部署。

### 2. **Helm 的安装**

Helm 是一个独立的 CLI 工具，支持在多种操作系统上安装。以下是常见操作系统上的安装方法：

- **macOS**:
  ```bash
  brew install helm
  ```

- **Linux**:
  ```bash
  curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
  ```

- **Windows**:
  使用 [Chocolatey](https://chocolatey.org/) 安装：
  ```bash
  choco install kubernetes-helm
  ```

安装完成后，可以通过以下命令查看 Helm 版本，确认安装成功：
```bash
helm version
```

### 3. **Helm 的核心功能**

#### 3.1 **Chart 的创建和管理**

**创建一个新的 Chart**:
你可以使用 `helm create` 命令来创建一个新的 Helm Chart 模板：
```bash
helm create mychart
```
这个命令会生成一个名为 `mychart` 的目录，包含了 Kubernetes 资源模板和配置文件的基础结构。

**Chart 目录结构**:
- `Chart.yaml`: Chart 的元数据文件，包含名称、版本、描述等信息。
- `values.yaml`: 默认配置值，可以在部署时覆盖。
- `templates/`: 存放 Kubernetes 资源的模板文件。
- `charts/`: 存放依赖的其他 Chart。

**打包和发布 Chart**:
可以使用 `helm package` 命令将 Chart 打包为 `.tgz` 文件，然后上传到 Helm 仓库：
```bash
helm package mychart
helm repo index ./ --url https://myrepo.com/charts
```

#### 3.2 **Chart 的安装与升级**

**安装 Chart**:
你可以使用 `helm install` 命令来安装 Chart，指定 Release 名称和 Chart 路径或名称：
```bash
helm install myrelease ./mychart
```
这将会在集群中部署 `mychart` Chart，并将其实例命名为 `myrelease`。

**升级 Chart**:
当你需要更新部署的应用时，可以使用 `helm upgrade` 命令：
```bash
helm upgrade myrelease ./mychart --set replicaCount=3
```
这个命令会更新 `myrelease` Release，将副本数设置为 3。

**回滚 Chart**:
如果升级出现问题，可以通过 `helm rollback` 回滚到之前的版本：
```bash
helm rollback myrelease 1
```
这里的 `1` 是版本号，表示回滚到上一个版本。

#### 3.3 **管理 Release**

**查看 Release 列表**:
使用 `helm list` 命令可以查看所有已安装的 Release：
```bash
helm list
```

**删除 Release**:
当不再需要某个 Release 时，可以使用 `helm uninstall` 命令将其删除：
```bash
helm uninstall myrelease
```
这个命令会删除 `myrelease` 及其关联的 Kubernetes 资源。

### 4. **Helm 的高级特性**

#### 4.1 **依赖管理**

Helm Chart 可以依赖其他 Chart，通过 `charts/` 目录来管理这些依赖。依赖关系可以在 `Chart.yaml` 文件中声明：
```yaml
dependencies:
  - name: mysql
    version: "1.6.2"
    repository: "https://charts.helm.sh/stable"
```
使用 `helm dependency update` 命令可以自动下载这些依赖的 Chart。

#### 4.2 **使用 `values.yaml` 自定义配置**

`values.yaml` 文件用于定义 Chart 的默认配置。可以在部署时通过命令行或自定义的 `values.yaml` 文件覆盖这些默认配置：
```bash
helm install myrelease ./mychart --values custom-values.yaml
```
或者：
```bash
helm install myrelease ./mychart --set image.tag=v2
```

#### 4.3 **Helm Hooks**

Helm Hooks 是一些特殊的 Kubernetes 资源，它们在特定的时间点被执行，比如在安装前、升级后、删除前等。常见的 Hook 类型有：
- `pre-install`
- `post-install`
- `pre-upgrade`
- `post-upgrade`
- `pre-delete`
- `post-delete`

你可以在 `templates/` 目录中定义这些 Hook，指定执行时机。例如：
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: "{{ .Release.Name }}-pre-install-job"
  annotations:
    "helm.sh/hook": pre-install
spec:
  template:
    spec:
      containers:
      - name: pre-install-job
        image: busybox
        command: ['sh', '-c', 'echo Hello World']
      restartPolicy: Never
```

#### 4.4 **Chart 的复用与共享**

Helm Chart 的强大之处在于它们的复用性和共享性。你可以将 Chart 上传到公共或私有 Helm 仓库中，其他人可以方便地使用和安装这些 Chart。

### 5. **Helm 与 CI/CD 集成**

Helm 非常适合与 CI/CD 流水线集成，帮助自动化应用的部署和升级。

**在 GitLab CI 中使用 Helm**:
你可以在 GitLab CI 中定义一个 CI/CD 流水线，自动化应用的部署和升级。以下是一个示例 `.gitlab-ci.yml` 配置文件：
```yaml
stages:
  - build
  - deploy

build:
  stage: build
  script:
    - docker build -t registry.example.com/myapp:$CI_COMMIT_SHA .
    - docker push registry.example.com/myapp:$CI_COMMIT_SHA

deploy:
  stage: deploy
  script:
    - helm upgrade --install myapp ./mychart --set image.tag=$CI_COMMIT_SHA
```
这个配置会在每次代码提交时自动构建 Docker 镜像，并使用 Helm 部署或升级 Kubernetes 中的应用。

### 6. **Helm 最佳实践**

- **版本控制**：将所有的 Chart 和 `values.yaml` 文件纳入版本控制系统管理，以便于追踪和回滚变更。
- **使用 CI/CD**：通过 CI/CD 管理 Helm 的操作，确保部署过程的自动化和一致性。
- **分环境管理**：通过不同的 `values.yaml` 文件来管理开发、测试和生产环境的配置。
- **定期更新**：定期检查和更新 Chart，以确保它们使用的 Kubernetes API 和依赖项是最新的。

### 7. **总结**

Helm 是 Kubernetes 中不可或缺的工具，它简化了应用的部署和管理流程。通过使用 Helm，你可以更高效地管理 Kubernetes 中的应用生命周期，实现快速的部署、升级和回滚。此外，Helm 的模板化配置和依赖管理使得它非常适合团队协作和 DevOps 流程。


## kubernetes 监控平台

Prometheus 是一个开源的监控系统，主要用于记录实时的时间序列数据。它最初由 SoundCloud 开发，现在已经成为 CNCF（云原生计算基金会）的一部分。Prometheus 在云原生环境下，如 Kubernetes，具有广泛的应用，以下是 Prometheus 的详细教程，涵盖了从安装到实际使用的各个方面。

### 1. **Prometheus 概述**

#### **1.1 什么是 Prometheus？**

Prometheus 是一个强大的监控和告警系统，提供了以下功能：

- **多维度的数据模型**：使用指标名和键值对的方式标识数据。
- **灵活的查询语言（PromQL）**：用于实时查询数据。
- **无需依赖分布式存储**：Prometheus 是一个单体服务器，适合水平扩展。
- **自动化的目标发现**：可通过服务发现机制或静态配置来发现监控目标。
- **推送网关支持**：用于收集短时的批处理任务数据。
- **多种可视化支持**：包括自带的简单 UI 和与 Grafana 集成。

### 2. **Prometheus 安装**

#### **2.1 在本地安装**

Prometheus 可以在本地机器上运行，适合开发和测试环境。

1. **下载 Prometheus**：
   - 访问 [Prometheus 官方下载页面](https://prometheus.io/download/)。
   - 选择适合你操作系统的版本并下载。

2. **解压并运行**：
   - 解压下载的文件：
     ```bash
     tar -xvf prometheus-*.tar.gz
     ```
   - 进入解压后的目录，运行 Prometheus：
     ```bash
     cd prometheus-*
     ./prometheus --config.file=prometheus.yml
     ```
   - Prometheus 默认在端口 `9090` 运行，访问 `http://localhost:9090` 可以查看 Prometheus 的 Web 界面。

#### **2.2 使用 Docker 安装**

通过 Docker 可以快速部署 Prometheus。

1. **运行 Prometheus Docker 容器**：
   ```bash
   docker run -d --name=prometheus -p 9090:9090 prom/prometheus
   ```
   - 容器启动后，Prometheus Web UI 同样在 `http://localhost:9090` 上可用。

#### **2.3 在 Kubernetes 中安装**

Prometheus 在 Kubernetes 集群中的部署通常使用 Helm 进行。

1. **添加 Helm 仓库**：
   ```bash
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   helm repo update
   ```

2. **安装 Prometheus**：
   ```bash
   helm install prometheus prometheus-community/prometheus
   ```
   - 该命令将在 Kubernetes 集群中部署 Prometheus，集成了多种常见的监控配置。

### 3. **Prometheus 配置**

Prometheus 的核心配置文件是 `prometheus.yml`，用来配置监控目标、告警规则和其他全局设置。

#### **3.1 配置示例**

下面是一个简单的 `prometheus.yml` 配置文件示例：

```yaml
global:
  scrape_interval: 15s  # 每 15 秒抓取一次指标数据
  evaluation_interval: 15s  # 每 15 秒评估一次告警规则

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['localhost:9100']
```

- **global**：全局配置项，例如抓取和评估频率。
- **scrape_configs**：定义 Prometheus 的抓取目标。每个目标可以是一个服务、应用程序或其他资源。

#### **3.2 动态目标发现**

Prometheus 支持通过服务发现来动态发现监控目标。以 Kubernetes 为例，可以通过以下配置自动发现 Kubernetes 集群中的服务：

```yaml
scrape_configs:
  - job_name: 'kubernetes-apiservers'
    kubernetes_sd_configs:
      - role: endpoints
```

### 4. **Prometheus 数据查询**

Prometheus 提供了强大的查询语言 PromQL，用于分析收集到的时间序列数据。

#### **4.1 基本查询**

- **获取所有时间序列的最新值**：
  ```promql
  up
  ```
  `up` 是一个内置指标，表示目标的状态（1 为可达，0 为不可达）。

- **计算过去 5 分钟的请求率**：
  ```promql
  rate(http_requests_total[5m])
  ```

- **聚合查询**：
  ```promql
  sum(rate(http_requests_total[5m])) by (method)
  ```

#### **4.2 使用 Web UI 查询**

1. 访问 Prometheus 的 Web UI (`http://localhost:9090`)。
2. 在 “Graph” 选项卡中输入 PromQL 查询，点击 “Execute” 运行查询。
3. 查询结果将显示在图表中，支持多种时间范围查看。

### 5. **Prometheus 告警配置**

Prometheus 具有内置的告警机制，可以在监控数据超过阈值时发出告警。

#### **5.1 配置告警规则**

告警规则通常配置在单独的文件中，然后在 `prometheus.yml` 中引用。例如，在 `alerts.yml` 中定义告警规则：

```yaml
groups:
- name: example
  rules:
  - alert: HighCPUUsage
    expr: 100 - (avg by (instance) (irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 90
    for: 2m
    labels:
      severity: critical
    annotations:
      summary: "High CPU usage detected"
      description: "CPU usage is above 90% for more than 2 minutes."
```

在 `prometheus.yml` 中引用该文件：

```yaml
rule_files:
  - "alerts.yml"
```

#### **5.2 使用 Alertmanager**

Alertmanager 是 Prometheus 的伴生组件，用于管理告警并发送通知。配置 Alertmanager 的步骤如下：

1. **在 `prometheus.yml` 中配置 Alertmanager**：
   ```yaml
   alerting:
     alertmanagers:
     - static_configs:
       - targets: ['localhost:9093']
   ```

2. **配置 Alertmanager 通知**：
   在 Alertmanager 配置文件中，定义通知渠道，如 Slack、电子邮件等。

   ```yaml
   route:
     receiver: 'slack-notifications'
   receivers:
   - name: 'slack-notifications'
     slack_configs:
     - api_url: 'https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK'
       channel: '#alerts'
       send_resolved: true
   ```

### 6. **可视化与 Grafana 集成**

Grafana 是一个流行的开源可视化工具，常与 Prometheus 一起使用。

#### **6.1 配置 Grafana 数据源**

1. **访问 Grafana**，进入 “Configuration” > “Data Sources”。
2. **添加数据源**，选择 “Prometheus”，然后输入 Prometheus 的 URL（例如 `http://localhost:9090`）。
3. 点击 “Save & Test” 以确保配置正确。

#### **6.2 创建 Grafana 仪表板**

1. **创建新仪表板**，点击 “Create” > “Dashboard” > “Add New Panel”。
2. **配置查询**：在查询编辑器中使用 PromQL 输入查询。
3. **选择图表类型**：根据数据特点选择合适的图表类型。
4. **保存仪表板**。

### 7. **总结**

Prometheus 是一个功能强大且灵活的监控系统，适用于各种环境，尤其是云原生应用。通过本教程，你已经掌握了 Prometheus 的基础安装、配置、数据查询、告警设置以及与 Grafana 的集成方法。这些技能将帮助你构建一个高效的监控体系，确保系统的稳定性和性能。

### 8. 详细教程

[prometheus教程](https://yunlzheng.gitbook.io/prometheus-book)
