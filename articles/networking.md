网络配置
===

###TL;DR(原文的这些个符号，几个意思？)

 `Dockers`启动后，会在宿主机上创建名为`docker0`的虚拟网卡，并给`docker0`随机分配一个宿主机没有使用，且满足RFC 1918定义的私有网络地址和子网段。举例来说，在我做这个步骤时，Docker分配的子网段是`172.17.42.1/16`，（最后那个16是掩码，表示该子网段可以会为宿主机和容器提供65534个地址）。容器的MAC地址根据IP地址生成，可以避免产生ARP冲突，它的范围从 02:42:ac:11:00:00 开始到 02:42:ac:11:ff:ff结束。

>注：本文档讨论的是 `docker` 的网络高级配置选项，初级用户无需阅读。如果想了解`docker`网络或容器间的连接方式，请查看[docker用户指南](../userguide/dockerlinks)

当然， `docker0` 除了是虚拟网卡，还是负责其附属网卡间数据转发的虚拟网桥。`docker`每新建一个容器，就会同时创建了两个相对应的网卡接口，它们就像一条管道的两端：数据从一端进入（发送），从另一端出来（接受）。这两个网卡，一个被分配给新建的容器做`eth0`网卡，另一个被分配给宿主机，并取个类似于`vethAQI2QT`的唯一名称。就这样，通过绑定每个`veth*`网卡到`docker0`网桥，`Docker`在宿主机和各docker容器间搭建起了一个虚拟子网。

接下来，我们详细阐述如何使用`docker`参数或Linux网络命令来进一步修改、完善、或者完全替代`docker`的默认网络配置。

###参数快速指南

首先，来浏览`docker`的网络相关的命令行选项列表，下面的章节对一一进行介绍：

第一部分：只能在`docker`服务启动时使用使用的网络命令行选项，一旦运行，不能改变。

+ `-b BRIDGE` or `--bridge=BRIDGE` — 请查看[搭建私人网桥](#bridge-building)小节
+ `--bip=CIDR` — 请查看[定制 docker0](#docker0)小节
+ `-H SOCKET...` 或 `--host=SOCKET...`  —这个参数听起来像是在说容器网络，但实际上是在干另一件事：用来向`Docker`服务传递类似"运行容器"和“停止容器”的命令。
+ `--icc=true|false` — 请查看[容器间通信](#between-containers)小节小节
+ `--ip=IP_ADDRESS` — 请查看[绑定容器端口](#binding-port)小节
+ `--ipv6=true|false` — 请查看[IPV6](#ipv6)小节
+ `--ip-forward=true|false` — 请查看[容器与外部通讯](#the-world)小节
+ `--iptables=true|false` — 请查看[容器间通信](#between-containers)小节
+ `--mtu=BYTES` — 请查看[定制 docker0](#docker0)小节

第二部分：下面两个参数，可以在启动时或者当 `docker run` 运行调用。如果启动时进行了设置，就会作为`docker run`运行时的初始默认值。

+ `--dns=IP_ADDRESS...`  — 请查看[配置 DNS](#dns)小节
+ `--dns-search=DOMAIN...`  — 请查看[配置 DNS](#dns)小节

第三部分：只能在`docker run`运行时调用的参数，特别用来定制容器的特性：

+ `-h HOSTNAME` or `--hostname=HOSTNAME`  — 请查看[配置 DNS](#dns)小节和[如何设置容器的网络](#containers-networking)小节
+ `--link=CONTAINER_NAME:ALIAS`  — 请查看[配置 DNS](#dns)小节和[容器间通信](#between-containers)小节
+ `--net=bridge|none|container:NAME_or_ID|host`  — 请查看[如何设置容器的网络](#containers-networking)小节
+ `--mac-address=MACADDRESS...`  — 请查看[如何设置容器的网络](#containers-networking)小节
+ `-p SPEC` or `--publish=SPEC`  — 请查看[绑定容器端口](#binding-port)小节
+ `-P` or `--publish-all=true|false`  — 请查看[绑定容器端口](#binding-port)小节

接下来，我们从浅到深的讲解上述题目。

###配置DNS

docker如何给每个容器提供独立的主机名和DNS配置呢？当然不是直接把主机名写到镜像里。docker巧妙的利用可刷新的虚拟文件覆盖了容器`/etc`目录下的三个关键文件。可以在容器内运行`mount`命令进行查看：
