网络配置
===

###TL;DR

 `Dockers`启动后，会在宿主机上创建一个虚拟的网卡`docker0`。它会随机选择一个宿主机没有使用，且满足RFC 1918定义的私有网络地址和子网，来分配给 `docker0`。举例来说，在我做这个步骤时，Docker选择了 `172.17.42.1/16`，这个16位的子网掩码会为宿主机和容器提供65534个地址。

>注：本文档讨论的是 `docker` 的网络高级配置选项。在大多数情况下你不会用到这些。如果你仅仅想简单了解`docker`网络或如何进行容器间的连接，请查看[docker用户指南](../userguide/dockerlinks)

不过， `docker0` 不仅是普通接口。它是一个虚拟的网桥，实现其附属的网络接口间的自动数据包转发。这允许容器与主机彼此之间通信。每次 `docker` 创建一个容器，它就创建了一个“对等”的接口，就像一个管道的两段，--一端发送数据包，另一端就会收到。它会给对等一端的容器成为 `eth0`网卡，并且保持另一端的通信，并且在主机的命名空间给唯一的一个名称 `vethAQI2QT`。通过绑定每个 `veth*`接口到 `docker0`桥。 `Docker`每个docker容器和共享主机之间创建一个虚拟子网。

本文的剩余部门来诠释所有的方法，你可以使用 `docker`命令选项，在先前的情况下对linux网络进行调整、补充、或者完全替代docker的默认网络配置。

###快速指南

这是一个 `docker` 网络有关的命令行选项，它会帮助你在下边查找到你想要查看的选项。

一些网络命令行选项只能在 `docker`服务启动时提供，一旦运行，不能改变：

+ -b BRIDGE or --bridge=BRIDGE — 下边查看创建自己的网桥
+ --bip=CIDR — 查看定制docker0
+ -H SOCKET... 或 --host=SOCKET... 这听起来像涉及到容器网络，但是它实际上是另一个方面的作用：它告诉 `Docker` 服务通过什么途径来接收命令像"运行容器"和“通知容器”。
+ --icc=true|false — 查看容器之间的通信
+ --ip=IP_ADDRESS — 查看绑定容器端口
+ --ip-forward=true|false — 查看容器之间的通信
+ --iptables=true|false — 查看容器之间的通信
+ --mtu=BYTES — 查看定制docker0

这里有两个网络选项，可以提供在启动时或者当 `docker run` 运行调用。当提供了启动时，如果没有指定选项的值， `docker run` 会自动设置默认值。

+ --dns=IP_ADDRESS... — 查看设置DNS
+ --dns-search=DOMAIN... --查看设置DNS


最后，几个网络选项只能在`docker run`时调用，因为他们指定给一个容器一些特性：

+ -h HOSTNAME or --hostname=HOSTNAME — 查看配置DNS和如何设置容器网络
+ --link=CONTAINER_NAME:ALIAS — 配置DNS和容器之间的通信
+ --net=bridge|none|container:NAME_or_ID|host — 查看如何设置容器网络
+ -p SPEC or --publish=SPEC — 查看绑定容器端口
+ -P or --publish-all=true|false — 查看绑定容器端口

以下部分解决上述所有的提到的主题，从简单到复杂的。

###配置DNS

docker如何给每一个容器提供一个主机名和DNS配置，无需构建一个定制的镜像和主机写在里面，
