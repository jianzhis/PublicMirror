CentOS Helm Chart
这是一个用于在 Kubernetes 上部署带有 OpenSSH 和常用工具（如 wget、ifconfig 和 curl）的 CentOS 容器的 Helm Chart。

介绍
此 Helm Chart 可用于在 Kubernetes 集群中快速部署一个预装了 OpenSSH 和常用工具的 CentOS 容器。您可以使用该容器运行各种应用程序或服务，并且可以选择不同版本的 CentOS。

前提条件
Kubernetes 1.10+ 版本
Helm 3.0.0+ 版本
安装 Helm Chart
添加 Helm 仓库
首先，将 Helm 仓库添加到您的本地 Helm 配置中：

sh
复制代码
helm repo add myrepo https://public-charts.yunna.net
helm repo update
安装 Chart
使用以下命令安装 CentOS Helm Chart，您可以选择不同版本的 CentOS：

sh
复制代码
# 安装 CentOS 7.6
helm install my-centos-release myrepo/centos --set image.tag="7.6"

# 安装 CentOS 8
helm install my-centos-release myrepo/centos --set image.tag="8"
查看部署状态
您可以使用以下命令查看部署状态：

sh
复制代码
kubectl get pods -l app.kubernetes.io/name=centos
配置
此 Helm Chart 提供了一些可配置的参数，您可以在 values.yaml 文件中设置这些参数。

参数
参数	描述	默认值
replicaCount	副本数量	1
image.repository	镜像仓库	registry.yunna.net/centos
image.tag	镜像标签	8
image.pullPolicy	镜像拉取策略	IfNotPresent
service.type	服务类型	ClusterIP
service.port	服务端口	22
readinessProbe	就绪探针配置	详见 values.yaml
livenessProbe	存活探针配置	详见 values.yaml
resources	资源限制和请求	{}
nodeSelector	节点选择器	{}
tolerations	容忍	[]
affinity	亲和性	{}
自定义配置
您可以通过创建一个自定义的 values.yaml 文件来覆盖默认值。例如：

yaml
复制代码
replicaCount: 2

image:
  repository: registry.yunna.net/centos
  tag: "8"
  pullPolicy: Always

service:
  type: NodePort
  port: 2222

resources:
  limits:
    cpu: 500m
    memory: 512Mi
  requests:
    cpu: 200m
    memory: 256Mi
然后使用以下命令安装 Chart，并应用自定义配置：

sh
复制代码
helm install my-centos-release myrepo/centos -f values.yaml
详细配置说明
replicaCount
副本数量。指定要运行的容器实例的数量。

image.repository
镜像仓库。指定要使用的容器镜像仓库。

image.tag
镜像标签。指定要使用的容器镜像标签。

image.pullPolicy
镜像拉取策略。可选值为 Always、IfNotPresent 和 Never。

service.type
服务类型。可选值为 ClusterIP、NodePort 和 LoadBalancer。

service.port
服务端口。指定容器内服务的暴露端口。

readinessProbe
就绪探针配置。用于检查容器是否已经就绪可以接收流量。

示例：

yaml
复制代码
readinessProbe:
  exec:
    command:
      - cat
      - /etc/service/sshd/down
  initialDelaySeconds: 5
  periodSeconds: 3
  failureThreshold: 6
livenessProbe
存活探针配置。用于检查容器是否处于健康状态，如果探针失败，容器将被重启。

示例：

yaml
复制代码
livenessProbe:
  exec:
    command:
      - cat
      - /etc/service/sshd/down
  initialDelaySeconds: 5
  periodSeconds: 3
resources
资源限制和请求。用于指定容器的资源限制和请求。

示例：

yaml
复制代码
resources:
  limits:
    cpu: 500m
    memory: 512Mi
  requests:
    cpu: 200m
    memory: 256Mi
nodeSelector
节点选择器。用于将 Pod 调度到具有特定标签的节点。

示例：

yaml
复制代码
nodeSelector:
  disktype: ssd
tolerations
容忍。用于允许 Pod 被调度到具有指定容忍度的节点。

示例：

yaml
复制代码
tolerations:
- key: "key1"
  operator: "Equal"
  value: "value1"
  effect: "NoSchedule"
affinity
亲和性。用于将 Pod 调度到符合指定亲和性规则的节点。

示例：

yaml
复制代码
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: kubernetes.io/e2e-az-name
          operator: In
          values:
          - e2e-az1
          - e2e-az2
卸载 Chart
要卸载已安装的 Helm Chart，使用以下命令：

sh
复制代码
helm uninstall my-centos-release
这将删除与该发布相关的所有 Kubernetes 资源。

贡献
欢迎贡献代码和提交问题。请阅读 贡献指南 了解更多详情。

许可证
本项目基于 MIT 许可证 进行分发