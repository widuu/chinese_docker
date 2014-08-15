Debian
===

Docker已经支持以下的debain版本：

- Debian 8.0 Jessie (64-bit)

###Debian Jessie 8.0 (64-bit)

Debian 8 已经使用3.14.0的内核版本，可以从Debian的仓库源来安装docker.io包。

>提示：Debian包含一个特别陈旧的KED3/GNOME2包叫docker,所以我们把这个包叫`docker.io`。

###安装

安装最新版的Debian软件包（可能不是最新版本docker包）

	$ sudo apt-get update
	$ sudo apt-get install docker.io
	$ sudo ln -sf /usr/bin/docker.io /usr/local/bin/docker
	$ sudo sed -i '$acomplete -F _docker docker' /etc/bash_completion.d/docker.io

来确认一下是否正常工作:

	$ sudo docker run -i -t ubuntu /bin/bash

这将下载`ubuntu`镜像，并且在容器内运行`bash`.

###不使用root运行

`docker`进程一般来说默认用`root`用户运行，从`Docker 0.5.2`开始，docker进程绑定unix socket来代替TCP端口。默认情况下用户`root`来管理unix socket,但是你也可以使用`sudo`来使用。

从0.5.3版本开始，如果你（你安装的docker）创建一个叫`docker`的unix群组，并且在群组中添加用户。当进程启动的时候，docker群组将有`docker`进程unix socket的读/写使用权。docker进程必须使用root用户运行，但是当使用docker群组的一个用户来运行docker客户端的时候，你不需要在命令前添加`sudo`，从docker 0.9.0版本开始你可以使用`-G`标记指定用户组。

>警告：docker用户组（或者用`-G`指定的用户组）和root等效，

举例

	# Add the docker group if it doesn't already exist.
	$ sudo groupadd docker
	
	# Add the connected user "${USER}" to the docker group.
	# Change the user name to match your preferred user.
	# You may have to logout and log back in again for
	# this to take effect.
	$ sudo gpasswd -a ${USER} docker
	
	# Restart the Docker daemon.
	# If you are in Ubuntu 14.04, use docker.io instead of docker
	$ sudo service docker restart

###下一步？

继续阅读[用户指南](../userguide/README.md)。
