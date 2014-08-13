CentOS
===

Docker 已经存在 EPEL 源中。这些指令支持 Redhat 和 CentOS。它可能会与其它的 EL6 二进制发行版本兼容良好，但是他们并没有被测试。

请记住这个包是 Extra Packages for Enterprise Linux (EPEL) 的一个额外包，社区中正在努力创建和维护这个包。

注意由于 Docker 的局限性，Docker 只能运行在64位的系统中。

你需要 CentOS6 或更高的版本，RHEL 的内核版本是 2.6.32-431 或者更高，为了让 Docker 工作需要特定的内核补丁。

###安装

首先，你需要安装 EPEL 镜像源，请查看 EPEL installation instructions.

在 EPEL 中已经提供了 `docker-io` 包

如果你安装了(不相关)的 Docker 包，它将与 `docker-io` 冲突。在安装 `docker-io` 之前，请先卸载 Docker

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

好！

现在你可以去查看[用户指南](../userguide/README.md)。
