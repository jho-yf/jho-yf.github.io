# Kubernetes

## Kubernetes主要功能

* **数据卷**：Pod中容器之间共享数据，可以使用数据卷
* **应用程序健康检查**：容器内服务可能进程堵塞无法处理请求，可以设置监控空检查策略保证应用健壮性
* **复制应用程序实例**：控制器维护着Pod副本数量，保证一个Pod或一组同类的Pod数量始终可用
* **弹性伸缩**：根据设定的指标（CPU利用率）自动缩放Pod副本数
* **服务发现**：使用环境变量或DNS服务插件保证容器中程序发现Pod入口访问地址
* **负载均衡**：一组Pod副本分配一个私有的集群IP地址（ClusterIP），负载均衡转发请求到后端容器。在集群内部，其他Pod可以通过这个ClusterIP访问应用。
* **滚动更新**：更新服务不中断，一次更新一个Pod，而不是同事删除整个服务
* **服务编排**：通过文件描述部署服务，使得应用程序部署变得更高效
* **资源监控**：Node节点组件集成`cAdvisor`资源收集工具，可通过`Heapster`汇总整个集群节点资源数据，然后存储到`InfluxDB`时序数据库，再由`Grafana`展示
* **提供认证和授权**：支持角色访问空空告知（RBAC）认证授权等策略

## 基本对象概念

* **Pod**：Pod是最小部署单元，一个Pod由一个或多个容器组成，Pod中容器共享存储和网络，在同一台Docker主机上运行
* **Service**：
  * Service是一个应用服务抽象，定义了Pod逻辑集合和访问这个Pod集合的策略
  * Service代理Pod集合对外表现为一个访问入口，分配一个集群IP地址，来自这个IP的请求将负载均衡转发到后端Pod中的容器
  * Service通过Lable Selector选择一组Pod提供服务
* **Volume**：数据卷，共享Pod容器使用的数据和持久化节点上的数据
* **Namespace**：
  * 命名空间将对象逻辑上分配到不同Namespace，可以是不同项目、用户等区分管理，并设定控制策略，从而实现多租户
  * 命名空间也称为虚拟集群
* **Lable**：标签用于区分对象（比如Pod、Service），以键/值对存在；每个对象可以有多个标签，通过标签关联对象

## 基于基本对象的更高层次抽象

* **RelicaSet**:
  * 下一代Replication Controller。确保任何给定时间指定的Pod副本数量，并提供声明式更新等功能。
  * RC与RS唯一区别就是Lable Selector支持不同，RS支持新的基于集合的标签，RC仅支持基于等式的标签。
* **Deployment**:
  * Deployment是一个更高层次的API对象，它管理ReplicaSets和Pod，并提供声明式更新等功能
  * 官方建议使用Deployment管理ReplicaSets，而不是直接使用ReplicaSets，这就意味着可能永远不需要直接操作RelicaSet对象
* **StatefulSet**
  * StatefulSet适合持久性的应用程序，有唯一的网络标识符（IP），持久存储，有序的部署、扩展、删除和滚动更新
* **DaemonSet**
  * DeamonSet确保所有（或一些）节点运行同一个Pod
  * 当节点加入Kubernetes集群中，Pod会被调度到该节点上运行
  * 当节点从集群移除时，DaemonSet的Pod会被删除，删除DaemonSet会清理他创建的所有Pod
* **Job**：一次性任务，运行完成后Pod销毁，不再重现启动新容器。还可以任务定时运行
