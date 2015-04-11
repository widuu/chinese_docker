Debian
===

以下版本的 Debian 支持 Docker：

+ [Debian 8.0 Jessie (64-bit)](#debian-jessie-80-64-bit)
+ [Debian 7.7 Wheezy (64-bit)](#debian-wheezystable-7x-64-bit)


##Debian Jessie 8.0 (64-bit)Debian

Debian 8 使用的是 3.14.0 的内核版本，可以从 Debian 的镜像源来安装 `docker.io` 包。

>提示：Debian 包含一个特别老的KDE3/GNOME2包叫 `docker` ,所以我们把这个包叫`docker.io`。

###安装

安装最新版的 Debian 软件包（可能不是最新版本 Docker ）

	$ sudo apt-get update
	$ sudo apt-get install docker.io

验证 Docker 是否正常工作 ：

	$ sudo docker run -i -t  Ubuntu  /bin/bash

该命令将下载 `Ubuntu` 镜像，并且在容器内运行 `bash`.

> 注意：如果你打算启用内存和交换空间设置，请查看[这里](./ubuntu.md)

##Debian Wheezy/Stable 7.x (64-bit)

安装 Docker 需要内核在3.8版本以上，但是 Wheezy 的内核版本是 3.2（[bug #407](https://github.com/docker/docker/issues/407%20kernel%20versions)对 Docker 需要3.8版本内核进行了讨论。）

幸运的是，官方提供了 `wheezy-backports` ，它内核版本是3.16，可以支持 Docker。

###安装

1. 从 wheezy-backports 镜像源来安装内核

	在 `/etc/apt/sources.list` 文件下添加如下内容：

		deb http://http.debian.net/debian wheezy-backports main

	安装 `linux-image-amd64` 包  (注意使用 `-t wheezy-backports`)

		$ sudo apt-get update
		$ sudo apt-get install -t wheezy-backports linux-image-amd64

2. 从 get.docker.com 获取安装脚本并安装：

		curl -sSL https://get.docker.com/ | sh
	

###使用非root

docker 进程通过监听一个 Unix Socket 来替代 TCP 端口。在默认情况下，docker 的 Unix Socket属于`root`用户，当然其他用户可以使用`sudo`方式来访问。因为这个原因， docker 进程就一直是`root`用户运行的。

如果你（或者说你安装Docker的时候）创建一个叫 `docker` 的用户组，并为用户组添加用户。这时候，当 Docker 进程启动的时候，`docker` 用户组对 Unix Socket 有了读/写权限。 你必须使用root用户来运行 docker 进程，但你可以用 `docker` 群组用户来使用 docker 客户端，你再使用 docker 命令的时候前边就不需要加 `sudo` 了。从Docker 0.9版本开始，你可以使用`-G`来指定用户组。

>警告：Docker 用户组（或者用 `-G` 指定的用户组）有等同于root用户的权限，有关系统安全影响的细节，请查看 [Docker 进程表面攻击细节]()

操作演示：

	# Add the docker group if it doesn't already exist.
	$ sudo groupadd docker
	
	# Add the connected user "${USER}" to the docker group.
	# Change the user name to match your preferred user.
	# You may have to logout and log back in again for
	# this to take effect.
	$ sudo gpasswd -a ${USER} docker
	
	# Restart the Docker daemon.
	$ sudo service docker restart
	##下一步

继续阅读[用户指南](../userguide/README.md)。
