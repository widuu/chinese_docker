Red Hat Enterprise Linux
===

docker已经存在redhat上的EPEL源中。这些指令支持redhat和centos。它可能会与其它的EL6二进制发行版本兼容良好，但是他们并没有被测试。

请记住这个包是Extra Packages for Enterprise Linux (EPEL)的一个额外包，社区中正在努力创建和维护这个包。

注意由于docker的局限性，docker只能运行在64位的系统中。

你需要RHEL 6.5或更高的版本，RHEL的内核版本是 2.6.32-431或者更高，为了让docker工作需要特定的内核补丁。

###安装

首先，你需要安装EPEL镜像源，请查看 EPEL installation instructions.

在EPEL中已经提供了docker-io包

如果你安装了(不相关)的docker包，它将与docker-io冲突。在安装docker-io之前，请先卸载docker

下一步，我们将要在我们的主机中安装docker,也就是docker-io包

	$ sudo yum -y install docker-io

更新`docker-io`包

	$ sudo yum -y update docker-io

现在docker已经安装好了，我们来启动docker进程

	$ sudo service docker start

如果我们想要开机自启动docker，我们需要这样:

	$ sudo chkconfig docker on

现在，让我们确认docker是否正常工作

	$ sudo docker run -i -t fedora /bin/bash

好！

现在你可以去查看用户指南。