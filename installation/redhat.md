# Red Hat Enterprise Linux

以下是支持 Docker 的 RHEL 版本：

- [*Red Hat Enterprise Linux 7 (64-bit)*](#red-hat-enterprise-linux-7-installation)
- [*Red Hat Enterprise Linux 6.5 (64-bit)*](#red-hat-enterprise-linux-6.5-installation) 或更高版本

## 内核支持

如果你的 RHEL 运行的是发行版内核。那就仅支持通过 *extras* 渠道或者 EPEL 包来安装 Docker。如果你打算在非发行版本的内核上运行 Docker ，内核的改动可能会导致出错

## Red Hat Enterprise Linux 7 installation

**Red Hat Enterprise Linux 7 （64位）** [自带Docker](https://access.redhat.com/site/products/red-hat-enterprise-linux/docker-and-containers). 你可以在[发行日志](https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/7.0_Release_Notes/chap-Red_Hat_Enterprise_Linux-7.0_Release_Notes-Linux_Containers_with_Docker_Format.html)中找到概述和指南。

Docker 包含在 **extras** 镜像源中，使用下面的方法可以安装 Docker:

1. 启用 **extras** 镜像源:

        $ sudo subscription-manager repos --enable=rhel-7-server-extras-rpms

2. 安装 Docker :

        $ sudo yum install docker 

如果你是RHEL客户，更多的 RHEL-7 安装、配置和[用户指南](https://access.redhat.com/site/articles/881893)可以在[客户中心](https://access.redhat.com/)中找到。

请继续阅读 [ 启动 Docker 进程 ](#starting-the-docker-daemon).

## Red Hat Enterprise Linux 6.5 installation

你需要在 **64位** 的 [RHEL 6.5](https://access.redhat.com/site/articles/3078#RHEL6) 或更高的版本上来安装 Docker，Docker 工作需要特定的内核补丁, 因此 RHEL 的内核版本应为 2.6.32-431 或者更高。

Docker 已经包含在 RHEL 的 EPEL 源中。该源是 Extra Packages for Enterprise Linux (EPEL) 的一个额外包，社区中正在努力创建和维护相关镜像。

## 内核支持

如果你的 RHEL 运行的是发行版内核。那就仅支持通过 *extras* 渠道或者 EPEL 包来安装 Docker。如果你打算在非发行版本的内核上运行 Docker ，内核的改动可能会导致出错

> **Warning**:
> Please keep your system up to date using `yum update` and rebooting
> your system. Keeping your system updated ensures critical security
>  vulnerabilities and severe bugs (such as those found in kernel 2.6.32)
> are fixed.

首先，你需要安装EPEL镜像源，请查看 [EPEL installation instructions](https://fedoraproject.org/wiki/EPEL#How_can_I_use_these_extra_packages.3F).

在EPEL中已经提供了 `docker-io` 包

如果你安装了(不相关)的 Docker 包，它将与 `docker-io` 冲突。在安装 `docker-io` 之前，请先卸载 Docker

下一步，我们将要在我们的主机中安装 Docker,也就是 `docker-io` 包:

	$ sudo yum -y install docker-io

更新`docker-io`包:

	$ sudo yum -y update docker-io

现在 Docker 已经安装好了，我们来启动 docker 进程:

	$ sudo service docker start

设置开机启动:

	$ sudo chkconfig docker on

现在，让我们确认 Docker 是否正常工作：

	$ sudo docker run -i -t fedora /bin/bash


继续 [ 启动 Docker 进程 ]( #启动Docker进程 )

## 启动 Docker 进程

现在 Docker 已经安装好了，让我们来启动 Docker 进程

    $ sudo service docker start

如果我们想要开机启动 Docker ，我们需要执行如下的命令：

    $ sudo chkconfig docker on

现在测试一下是否正常工作.

    $ sudo docker run -i -t fedora /bin/bash

> 注意: 如果你运行的时候提示一个  `Cannot start container` 的错误，错误中提到了 SELINUX 或者 权限不足。你需要更新 SELINUX 规则。你可以使用 `sudo yum upgrade selinux-policy` 然后重启。


**下一步**

好！现在你可以去查看[用户指南](../userguide/README.md)。

## 自定义进程选项

如果你想要添加一个 HTTP 代理，为 Docker 运行文件设置不同的目录或分区，又或者定制一些其它的功能，请阅读我们的系统文章，了解[如何定制 Docker 进程](/articles/systemd.md)


##问题

遇到问题请到 [Red Hat Bugzilla for docker-io component](https://bugzilla.redhat.com/enter_bug.cgi?product=Fedora%20EPEL&component=docker-io) 进行反馈。
