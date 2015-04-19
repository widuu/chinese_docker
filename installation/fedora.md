# Fedora

Docker 已经支持以下版本的 Fedora :

- [*Fedora 20 (64-bit)*](#fedora-20-installation)
- [*Fedora 21 and later (64-bit)*](#fedora-21-and-later-installation)

目前的 Fedora 项目，仅发行版本中的内核支持 Docker。如果你打算在非发行版本的内核上运行 Docker ，内核的改动可能会导致出错。

## Fedora 21 或更高版本安装 Docker 

在你的主机上安装 `docker` 包来安装 Docker 。

    $ sudo yum -y install docker

更新 `docker` :

    $ sudo yum -y update docker

请继续阅读启动 Docker 进程 [Starting the Docker daemon](#starting-the-docker-daemon)。

 
## Fedora 20 安装 Docker

在 `Fedora 20` 中，一个系统自带的可执行的应用程序与 docker 包名字发生冲突，所以我们给 docker 的RPM包重命名为 docker-io 。

`Fedora 20` 中 安装 `docker-io` 之前需要先卸载 `docker` 包。

    $ sudo yum -y remove docker
    $ sudo yum -y install docker-io

更新 `docker`

    $ sudo yum -y update docker-io

请继续阅读启动 Docker 进程 [Starting the Docker daemon](#starting-the-docker-daemon)。

## Starting the Docker daemon

当 Docker 安装完成之后，你需要启动 docker 进程。

    $ sudo systemctl start docker

如果我们希望开机时自动启动 Docker ，如下操作：

    $ sudo systemctl enable docker

现在，我们来验证 Docker 是否正常工作。

    $ sudo docker run -i -t fedora /bin/bash

> 注意 ： 如果你使用的时候提示了 `Cannot start container` 错误，错误中提到了 SELINUX 或者权限不足，你需要更新 SELinux 策略，你可以使用 `sudo yum upgrade selinux-policy` 来改变 SELinux策略并重启。

## 为使用 Docker 用户授权

`docker` 命令行工具通过 socket 文件 `/var/run/docker.sock` 和 `docker` 守护进程进行通信的。而这个 socket 文件的用户权限是 `root:root`。 虽然
[推荐](https://lists.projectatomic.io/projectatomic-archives/atomic-devel/2015-January/msg00034.html)
使用 `sudo` 命令来使用 docker 命令，但是如果你不想使用 `sudo`, 系统管理员可以创建一个 `docker` 用户组，并将 `/var/run/docker.sock` 赋予 docker 用户组权限，然后给 docker 用户组添加用户即可。


    $ sudo groupadd docker
    $ sudo chown root:docker /var/run/docker.sock
    $ sudo usermod -a -G docker $USERNAME

## 自定义进程选项

如果你想要添加一个 HTTP 代理，为 Docker 运行文件设置不同的目录或分区，又或者定制一些其它的功能，请阅读我们的系统文章，了解[如何定制 Docker 进程](/articles/systemd.md)


## 下一步

阅读 [用户指南](../userguide/).
	




	