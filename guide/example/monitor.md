---
vssueId: 77
description: 使用Kuboard在Kubernetes上安装监控套件，并对example微服务实现资源层监控、中间件层监控、链路追踪和APM监控
---

# 监控 example <Badge text="alpha" type="warn"/>

## 前提

必须具备如下条件：

* 已完成 [导入 example 微服务](/guide/example/import.html)

::: warning

监控套件相关的功能目前处于 alpha 状态，虽然在实际投产项目中取得了非常好的效果，但是产品化封装还需要进一步改进。

:::

假设您已进入 example 名称空间，如下图所示：

![Kubernetes教程：在Kuboard中监控-进入名称空间](./import.assets/image-20190723121433809.png)



## 安装监控套件

### 安装全局监控套件

* 在 master 节点执行

```bash
kubectl -n kube-system create secret generic etcd-certs --from-file=/etc/kubernetes/pki/etcd/server.crt --from-file=/etc/kubernetes/pki/etcd/server.key
```

* 将鼠标移到 ***设置*** 菜单

![Kubernetes教程：在Kuboard中监控-进入监控套件安装页](./monitor.assets/image-20190723150525017.png)



* 点击 ***监控套件 - example***

![Kubernetes教程：在Kuboard中监控-进入监控套件安装页](./monitor.assets/image-20190723150616633.png)

* 点击 ***全局监控套件 / 查找并安装***

![Kubernetes教程：在Kuboard中监控-安装全局监控套件-查找](./monitor.assets/image-20190723150853277.png)

* 点击 ***资源层监控套件 / 安装***

![Kubernetes教程：在Kuboard中监控-安装全局监控套件-安装](./monitor.assets/image-20190723151045112.png)

* 点击 ***全局监控套件 / 资源层监控套件 / 安装***

![Kubernetes教程：在Kuboard中监控-安装全局监控套件-安装](./monitor.assets/image-20190723151103353.png)

* 根据向导提示，完成工作负载的导入

![Kubernetes教程：在Kuboard中监控-安装全局监控套件-导入对应的工作负载](./monitor.assets/image-20190723151339137.png)

* 点击 ***资源层监控套件 / 初始化***

  由于下载镜像需要时间，您可能需要等待5-10分钟后，才能成功执行初始化。初始化成功的话，您将看到如下提示信息。

![Kubernetes教程：在Kuboard中监控-安装全局监控套件-初始化](./monitor.assets/image-20190723151357262.png)

### 安装名称空间监控套件

* 点击 ***名称空间监控套件 example / 查找并安装***

![Kubernetes教程：在Kuboard中监控-安装名称空间监控套件-查找](./monitor.assets/image-20190723151422460.png)

* 重复前述过程，安装监控套件

  重复前述过程，完成如下监控套件的安装和初始化：

  * Pinpoint 监控套件
  * Prometheus 监控套件
  * 熔断及限流 Sentinel

  安装完成后，界面如下图所示：

![Kubernetes教程：在Kuboard中监控-安装名称空间监控套件-安装](./monitor.assets/image-20190723151711230.png)

### 查看监控套件的工作负载

* 点击 ***后退***

  回到名称空间后，可查看到刚才安装监控套件时导入的监控层工作负载。

  > 全局监控套件安装在 kube-system 名称空间

![Kubernetes教程：在Kuboard中监控-查看监控套件-全局](./monitor.assets/image-20190723151804727.png)

## 重启 example 容器组

* 点击导航栏中的 ***容器组列表***

  在筛选条件中选择

  * 展现层
  * 网关层
  * 服务层
  * 持久层
  * 中间件

  并点击刷新
  
  > * Kuboard 创建工作负载时，默认将其 imagePullPolicy 设置为 **Always**；
  >
  > * 删除容器组时，Kubernetes 将为该工作负载启动一个新的容器组，以尽可能地使工作负载的容器组数量等于该工作负载期望的副本数（replicas）；
  > * Kubernetes 在启动容器组时，根据 imagePullPolicy 的设置 Always，将会尝试从镜像仓库抓取最新镜像，这一特性，可以用于开发环境和测试环境的版本更新，因为您的 devops 系统肯能不会为 daily build 生成新的镜像标签；
  > * 此处删除容器组的目的是，重新启动容器，以便容器中的监控探针能够检测到其对应监控套件的存在。

![Kubernetes教程：在Kuboard中监控-重启容器组](./monitor.assets/image-20190723151845303.png)

* 在列表中选择

  选中如下容器组：

  * cloud-eureka-0

  * db-example-xxxxxx-xxxx

  * gateway-example-xxxxxx-xxxx

  * svc-example-xxxxxx-xxxx

  * web-example-xxxxxx-xxxx

![Kubernetes教程：在Kuboard中监控-重启容器组-选中容器组](./monitor.assets/image-20190723151902003.png)

* 点击 ***删除*** 按钮

![Kubernetes教程：在Kuboard中监控-重启容器组-删除容器组](./monitor.assets/image-20190723151914994.png)



* 点击 ***确定***

![Kubernetes教程：在Kuboard中监控-重启容器组-确认删除容器组](./monitor.assets/image-20190723151932871.png)



* 点击 ***应用***

![Kubernetes教程：在Kuboard中监控-重启容器组-执行](./monitor.assets/image-20190723151951910.png)

* 等待，直到容器组调整完成

![Kubernetes教程：在Kuboard中监控-重启容器组-等待执行过程](./monitor.assets/image-20190723152020605.png)

* 点击 ***后退***

![Kubernetes教程：在Kuboard中监控-重启容器组-查看结果](./monitor.assets/image-20190723152040219.png)

## 查看监控信息

### 查看资源层监控信息

* 点击 ***展现层 / web-example***

  如果您在下图中没有看到 ***Nginx 监控***， ***容器组监控*** 等，请刷新您的页面，因为监控套件初始化之后需要刷新页面才能加载并显示。

![Kubernetes教程：在Kuboard中监控-查看Nginx监控、容器组监控入口](./monitor.assets/image-20190723152124196.png)

* 点击 ***所在节点监控***

  可查看所在节点的资源使用情况监控信息

![Kubernetes教程：在Kuboard中监控-查看所在节点监控](./monitor.assets/image-20190718104156232.png)





### 查看中间件层监控信息

#### 查看 Nginx 监控

* 点击 Nginx 监控

![Kubernetes教程：在Kuboard中监控-查看Nginx监控](./monitor.assets/image-20190718104729472.png)



#### 查看 MySQL 监控

* 返回 ***example*** 名称空间
* 点击 ***持久层 / example db***
* 点击 ***MySQL 监控***

![Kubernetes教程：在Kuboard中监控-查看MySQL监控](./monitor.assets/image-20190718105420599.png)



#### 查看 JVM 监控

* 返回 ***example*** 名称空间
* 点击 ***微服务层 / example***
* 点击 ***Java 虚拟机监控***

![Kubernetes教程：在Kuboard中监控-查看Java虚拟机监控](./monitor.assets/image-20190718105641651.png)



### 查看链路追踪监控信息

* 在 ***微服务层 / example*** 中点击 ***链路追踪***

![Kubernetes教程：在Kuboard中监控-查看链路追踪信息](./monitor.assets/image-20190718120957255.png)





![Kubernetes教程：在Kuboard中监控-查看链路追踪信息-APM](./monitor.assets/image-20190718121133160.png)
