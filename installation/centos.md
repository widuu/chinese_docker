CentOS
===

在 CentOS-7 中，Docker 是作为自带软件包存在的，而在 CentOS-6 中它由社区维护。请注意，这点上的区别可能会对不同版本间的安装上产生影响。

下面的安装指南仅针对 CentOS6 以及以后的版本，这意味着该指南也适用于 EL6 发行版的 Linux，例如 Scientific Linux，但并未经过测试。

注意由于 Docker 的局限性，Docker 只能运行在64位的系统中。

你需要 [CentOS6](www.centos.org) 或更高的版本，RHEL 的内核版本是 2.6.32-431 或者更高，为了让 Docker 工作需要特定的内核补丁。

###安装 - CentOS-7

Docker 被默认包含在 CentOS-Extras 的镜像中，安装命令如下：

	$ sudo yum install docker

###安装 - CentOS-6

在 CentOS-6 中，Docker 包含在 [EPEL](https://fedoraproject.org/wiki/EPEL) 提供的镜像中，该组织致力于为 RHEL 发行版创建和维护更多可用的软件包。

首先，你需要安装 EPEL 镜像源，请查看 [EPEL installation instructions](https://fedoraproject.org/wiki/EPEL#How_can_I_use_these_extra_packages.3F).

 `docker-io` 在 EPEL 中已经提供了 Docker 包。

如果你安装了(不相关)的 `Docker` 包，它将与 `docker-io` 冲突。我们提供了[反馈提交渠道](https://bugzilla.redhat.com/show_bug.cgi?id=1043676)。在安装 `docker-io` 之前，请先卸载 `Docker`.

下一步，我们将要在我们的主机中安装 Docker,也就是 `docker-io` 包

	$ sudo yum -y install docker-io

现在，它已经安装好了，让我们来启动 Docker 守护进程

	$ sudo service docker start

如果我们需要开机自启动，如下：

	$ sudo chkconfig docker on

现在我们确认 Docker 是否正常工作，首先我们需要获取最新的 CentOS 镜像

	$ sudo docker pull centos:latest

下一步，我们要确保通过如下命令可以看到镜像：

	$ sudo docker images centos

它将会输出如下的信息：

	$ sudo docker images centos
	REPOSITORY      TAG             IMAGE ID          CREATED             VIRTUAL SIZE
	centos          latest          0b443ba03958      2 hours ago         297.6 MB

运行简单的 bash shell 来测试这个镜像

	$ sudo docker run -i -t centos /bin/bash

如果正常，你会获得一个简单的 bash 提示，输入 exit 退出。

###示例镜像

CentOS 项目为开发者提供了大量的的示例镜像，作为开发模板或者学习 Docker 的实例。你可以在这里找到这些示例：

[https://github.com/CentOS/CentOS-Dockerfiles](https://github.com/CentOS/CentOS-Dockerfiles)

好！现在你可以去查看[用户指南](../userguide/README.md)，或者创建你自己的镜像了。

###发现问题？

如果有关于在 CentOS 上的 Docker 问题，请直接在这里提交：[CentOS Bug Tracker](http://bugs.centos.org/).
