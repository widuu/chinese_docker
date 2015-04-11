欢迎来到docker用户指南
===

通过这个介绍，你可以了解到 Docker 是什么，以及它是如何工作的。在本章节中，我们将 Docker 集成到你的环境中，并且通过使用 Docker 来了解一些基础知识。

我们教你如何使用docker:

- docker中运行你的应用程序。
- 运行你自己的容器。
- 创建docker镜像。
- 分享你的docker镜像。
- 和更多的信息!

我们已经将本指南分为几个主要部分：

###开始使用Docker Hub

如何使用Docker Hub?

Docker Hub是docker的中心仓库。Docker Hub里存储了公共的 Docker 镜像，并且提供服务来帮助你构建和管理你的 Docker 环境。了解解更多。

阅读使用[Docker Hub](dockerhub.md).

###在Docker中运行“hello Word”应用

如何在容器内运行应用程序？

Docker 为你将要运行的应用程序提供了一个基于容器的虚拟化平台。学习如何使用 `Dockerize` 应用程序来运行他们。

阅读[Dockerize应用程序](dockerizing.md)

###使用容器

如何管理我们的容器？

当你在docker容器中运行和管理你的应用程序，我们会展示如何管理这些容器。了解如何检查、监控和管理容器。

阅读[使用容器](usingdocker.md)

###使用docker镜像

我是如何创建、访问和分享我自己的容器呢？

当你学会如何使用docker的时候，是时候进行下一步来学习如何在 Docker 中构建你自己应用程序镜像。

阅读[使用docker镜像](dockerimages.md)

###容器连接

到这里我们学会了如何在 Docker 容器中构建一个单独的应用程序。而现在我们要学习如何将多个容器连接在一起构建一个完整的应用程序。

阅读[容器连接](dockerlinks.md)

###管理容器数据

现在我们知道如何连接 Docker 容器，下一步，我们学习如何管理容器数据，如何将卷挂载到我们的容器中。

阅读[管理容器数据](dockervolumes.md)

###使用Docker Hub

现在我们应该学习更多关于使用 Docker 的知识。例如通过 Docker Hub 提供的服务来构建私有仓库。

阅读[使用Docker Hub](dockerrepos.md)

###Docker Compose

Docker Compose 你只需要一个简单的配置文件就可以自定义你所需要的应用组件，包括容器、配置、网络链接和挂载卷。只需要一个简单的命令就可以启动和运行你的应用程序。

阅读[Docker Compose 用户指南.](../compose/README.md)

###Docker Machine

Docker Machine 可以帮助你快速的启动和运行 Docker 引擎。 Machine 可以帮助你配置本地电脑、云服务商和你的个人数据中心上的 Docker 引擎主机，并且通过配置 Docker 客户端来让它们进行安全的通信。

查阅 [Go to Docker Machine user guide.](../machine/README.md)

###Docker 集群

Docker 集群是将多个 Docker 引擎池连接在一起组合成一个独立的主机来提供给外界。它是以 Docker API 作为服务标准的，所以任何已经在Docker上工作的工具，现在都可以透明地扩展到多个主机上。

阅读 [Go to Docker Swarm user guide.](../swarm/)