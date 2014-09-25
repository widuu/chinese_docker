Debian
===

Docker 已经支持以下的 Debian 版本：

- [Debian 8.0 Jessie (64-bit)](https://docs.docker.com/installation/debian/#debian-jessie-8-64-bit)

##Debian Jessie 8.0 (64-bit)Debian

Debian 8 已经使用 3.14.0 的内核版本，可以从 Debian 的仓库源来安装 `docker.io` 包。

>提示：Debian包含一个特别陈旧的KDE3/GNOME2包叫 `docker` ,所以我们把这个包叫`docker.io`。

###安装

安装最新版的 Debian 软件包（可能不是最新版本 Docker 包）

	$ sudo apt-get update
	$ sudo apt-get install docker.io
	$ sudo ln -sf /usr/bin/docker.io /usr/local/bin/docker
	$ sudo sed -i '$acomplete -F _docker docker' /etc/bash_completion.d/docker.io

来确认一下是否正常工作:

	$ sudo docker run -i -t  Ubuntu  /bin/bash

该命令将下载 `Ubuntu` 镜像，并且在容器内运行 `bash`.

###不使用root运行

`docker` 进程一般来说默认用 `root` 用户运行。从 `Docker 0.5.2` 开始，docker 进程绑定 unix socket 来代替 TCP 端口。默认情况下用户 `root` 来管理 unix socket ,但是你也可以使用 `sudo` 来使用。

从 0.5.3 版本开始，如果你（你安装的 Docker）创建一个叫 `docker` 的 unix 群组，并且在群组中添加用户。当进程启动的时候，Docker 群组将有 `docker` 进程 unix socket 的读/写使用权。docker 进程必须使用 root 用户运行，但是当使用 Docker 群组的一个用户来运行 Docker 客户端的时候，你不需要在命令前添加 `sudo` .从 Docker 0.9.0 版本开始你可以使用 `-G` 标记指定用户组。

>警告：Docker 用户组（或者用 `-G` 指定的用户组）和 root 等效，

举例

	# Add the docker group if it doesn't already exist.
	$ sudo groupadd docker
	
	# Add the connected user "${USER}" to the docker group.
	# Change the user name to match your preferred user.
	# You may have to logout and log back in again for
	# this to take effect.
	$ sudo gpasswd -a ${USER} docker
	
	# Restart the Docker daemon.
	# If you are in  Ubuntu  14.04, use docker.io instead of docker
	$ sudo service docker restart

##下一步

继续阅读[用户指南](../userguide/README.md)。
