Red Hat Enterprise Linux 7
===

Red Hat Enterprise Linux 7 [自带Docker](https://access.redhat.com/site/products/red-hat-enterprise-linux/docker-and-containers). 你可以在[发行日志](https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/7.0_Release_Notes/chap-Red_Hat_Enterprise_Linux-7.0_Release_Notes-Linux_Containers_with_Docker_Format.html)中找到概述和指南.


Docker 包含在附加镜像源中，使用下面的方法可以安装 Docker:

开启附加镜像源:

    $ sudo subscription-manager repos --enable=rhel-7-server-extras-rpms
    
安装 Docker:

    $ sudo yum install docker

如果你是RHEL客户，更多的 RHEL-7 安装、配置和[用户指南](https://access.redhat.com/site/articles/881893)可以在[客户中心](https://access.redhat.com/)中找到。

Red Hat Enterprise Linux 6
===

Docker 已经包含在 RHEL 的 EPEL 源中。该源是 [Extra Packages for Enterprise Linux (EPEL)](https://fedoraproject.org/wiki/EPEL) 的一个额外包，社区中正在努力创建和维护相关镜像。

由于 Docker 的局限性，Docker 只能运行在64位的系统中。

你需要 RHEL 6.5 或更高的版本，Docker 工作需要特定的内核补丁, 因此 RHEL 的内核版本应为 2.6.32-431 或者更高。

###安装

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

好！现在你可以去查看[用户指南](../userguide/README.md)。

##问题

遇到问题请到 [Red Hat Bugzilla for docker-io component](https://bugzilla.redhat.com/enter_bug.cgi?product=Fedora%20EPEL&component=docker-io) 进行反馈。