# CentOS

以下版本的CentOS 支持 Docker ：

- [*CentOS 7 (64-bit)*](#installing-docker-centos-7)
- [*CentOS 6.5 (64-bit)*](#installing-docker-centos-6.5) or later

该指南可能会适用于其它的  EL6/EL7 的 Linux 发行版，譬如 Scientific Linux 。但是我们没有做过任何测试。

请注意，由于 Docker 的局限性，Docker 只能运行在64位的系统中。


## 内核支持

目前的 CentOS 项目，仅发行版本中的内核支持 Docker。如果你打算在非发行版本的内核上运行 Docker ，内核的改动可能会导致出错。

Docker 运行在 [CentOS-6.5](www.centos.org) 或更高的版本的 CentOS 上，需要内核版本是 2.6.32-431 或者更高版本 ，因为这是允许它运行的指定内核补丁版本。

##安装 - CentOS-7

Docker 软件包已经包含在默认的 CentOS-Extras 软件源里，安装命令如下：

	$ sudo yum install docker

开始运行 [Docker daemon](#starting-the-docker-daemon)。

### FirewallD

CentOS-7 中介绍了 firewalld，firewall的底层是使用iptables进行数据过滤，建立在iptables之上，这可能会与 Docker 产生冲突。

当 `firewalld` 启动或者重启的时候，将会从 iptables 中移除 `DOCKER` 的规则，从而影响了 Docker 的正常工作。

当你使用的是 Systemd 的时候， `firewalld` 会在 Docker 之前启动，但是如果你在 Docker 启动之后再启动 或者重启 `firewalld` ，你就需要重启 Docker 进程了。

## 安装 Docker - CentOS-6.5

在 CentOS-6.5 中，Docker 包含在 [Extra Packages
for Enterprise Linux (EPEL)](https://fedoraproject.org/wiki/EPEL) 提供的镜像源中，该组织致力于为 RHEL 发行版创建和维护更多可用的软件包。

首先，你需要安装 EPEL 镜像源，请查看 [EPEL installation instructions](https://fedoraproject.org/wiki/EPEL#How_can_I_use_these_extra_packages.3F).

在 CentOS-6 中，一个系统自带的可执行的应用程序与 docker 包名字发生冲突，所以我们重新命名 docker 的RPM包名字为 `docker-io` 。

CentOS-6 中 安装 `docker-io` 之前需要先卸载 `docker` 包。

    $ sudo yum -y remove docker

下一步，安装 `docker-io` 包来为我们的主机安装 Docker。

    $ sudo yum install docker-io

开始运行 [Docker daemon](#starting-the-docker-daemon)。

## 手动安装最新版本的 Docker 


当你使用推荐方法来安装 Docker 的时候，上述的 Docker 包可能不是最新发行版本。 如果你想安装最新版本，[你可以直接安装二进制包](./binaries.md)

当你使用二进制安装时，你可能想将 Docker 集成到 Systemd 的系统服务中。为了实现至一点，你需要从github中下载 [service and socket](https://github.com/docker/docker/tree/master/contrib/init/systemd)两个文件，然后安装到 `/etc/systemd/system` 中。

Please continue with the [Starting the Docker daemon](#starting-the-docker-daemon).

## Starting the Docker daemon

当 Docker 安装完成之后，你需要启动 docker 进程。

    $ sudo service docker start

如果我们希望 Docker 默认开机启动，如下操作：

    $ sudo chkconfig docker on

现在，我们来验证 Docker 是否正常工作。第一步，我们需要下载最新的 `centos` 镜像。

    $ sudo docker pull centos

下一步，我们运行下边的命令来查看镜像，确认镜像是否存在：

    $ sudo docker images centos

这将会输出如下的信息：

    $ sudo docker images centos
    REPOSITORY      TAG             IMAGE ID          CREATED             VIRTUAL SIZE
    centos          latest          0b443ba03958      2 hours ago         297.6 MB

运行简单的脚本来测试镜像：

    $ sudo docker run -i -t centos /bin/bash

如果正常运行，你将会获得一个简单的 bash 提示，输入 `exit` 来退出。

## 自定义进程选项

如果你想要添加一个 HTTP 代理，为 Docker 运行文件设置不同的目录或分区，又或者定制一些其它的功能，请阅读我们的系统文章，了解[如何定制 Docker 进程](/articles/systemd.md)

## Dockerfiles

CentOS 项目为开发者提供了大量的的示例镜像，作为开发模板或者学习 Docker 的实例。你可以在这里找到这些示例：

[https://github.com/CentOS/CentOS-Dockerfiles](https://github.com/CentOS/CentOS-Dockerfiles)

好！现在你可以去查看[用户指南](../userguide/README.md)，或者创建你自己的镜像了。


##发现问题？

如果有关于在 CentOS 上的 Docker 问题，请直接在这里提交：[CentOS Bug Tracker](http://bugs.centos.org/).


