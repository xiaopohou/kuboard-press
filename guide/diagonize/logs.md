---
description: 使用Kuboard打开并跟踪容器组的最新日志，以便对Kubernetes上容器组的最新运行情况进行了解和诊断问题。
---

# 日志及终端



# 日志

通过 Kuboard 可以实时跟踪容器的日志信息。

假设您已经进入 ***工作负载*** 详情页，如下图所示：

![Kubernetes教程：在Kuboard查看工作负载](./logs.assets/image-20190721104348908.png)

* 点击容器信息中的 ***日志*** 按钮

  可进入日志追踪界面，如下图所示：

![Kubernetes教程：在Kuboard查看日志追踪](./logs.assets/image-20190721104415732.png)



# 终端

* 点击容器信息中的 ***终端*** 按钮

  可进入终端界面，如下图所示：

  > * 在终端中，可以执行的 shell 命令取决于该容器预装的命令。许多容器为了精简自身的大小，只保留了最基本的命令。
  >
  > * 通常会进入终端执行如下诊断操作：
  >   * export 命令查看容器内的环境变量是否被正确设置
  >   * ping, curl 命令检查容器内与集群内其他服务，集群外服务的网络连通性
  >   * vi 命令，临时修改容器内应用程序的配置，并在容器内重启应用程序，以临时性的尝试修复问题，如果有效再将修改更新到应用程序代码或者 Dockerfile

![Kubernetes教程：在Kuboard进入容器的终端界面](./logs.assets/image-20190721104522870.png)
