# ubuntu

这是一个用于在 Kubernetes 集群上部署 Ubuntu 容器的 Helm chart。

## 先决条件

- Kubernetes 1.12+
- Helm 3.0+

## 安装 Chart

使用发布名称 `my-release` 安装此 chart：

helm install my-release ubuntu
该命令会在默认配置下将 ubuntu chart 部署到 Kubernetes 集群中。可以在安装时通过参数配置来覆盖默认值，详细参数请参见 参数 部分。

卸载 Chart
要卸载/删除 my-release 部署：

helm delete my-release
该命令会删除与此 chart 相关的所有 Kubernetes 组件，并删除该发布。

参数
全局参数
参数	描述	默认值
replicaCount	部署副本数量	1
image.repository	Ubuntu 镜像仓库	http://registry.yunna.net/ubuntu
image.tag	Ubuntu 镜像标签	"20.04"
image.pullPolicy	镜像拉取策略	IfNotPresent
service.type	Kubernetes 服务类型	ClusterIP
service.port	Kubernetes 服务端口	80
ingress.enabled	是否启用 Ingress 资源	false
resources	CPU/内存资源请求/限制	{}
nodeSelector	Pod 分配的节点标签	{}
tolerations	Pod 分配的容忍度	[]
affinity	节点/Pod 亲和性设置	{}
镜像标签
可用的 Ubuntu 镜像标签：18.04、20.04、22.04

要覆盖默认的镜像标签，可以使用以下命令：


helm install my-release ubuntu --set image.tag=18.04
贡献
欢迎为改进此 Helm chart 提交贡献。请在 GitHub 仓库 上提交 PR 和问题。

许可证
此 Helm chart 根据 MIT 许可证发布。