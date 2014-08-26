Binaries
===

本安装说明是提供给那些想在多种环境中安装docker的hacker们的。

在进行安装之前，请检查你的linux发行版本是否有打包好的docker安装包。我们已经发布了许多发行版，这样会节省您很多时间。

###检查运行时的依赖关系

如果想要docker正常运行，需要安装以下软件:

- iptables version 1.4 or later
- Git version 1.7 or later
- procps (or similar provider of a "ps" executable)
- XZ Utils 4.9 or later
- a [properly mounted](https://github.com/tianon/cgroupfs-mount/blob/master/cgroupfs-mount) cgroupfs hierarchy (having a single, all-encompassing "cgroup" mount point [is not sufficien](https://github.com/docker/docker/issues/3485))

###检查内核的依赖关系

docker进程模式需要特定的内核环境支持。详情请检查您的[发行版](../SUMMARY.md)。

一般来说，linux内核3.8（或更高版本）是首选，之前的版本会让docker引发已知的问题。


注意：docker也有客户端模式，它可以运行在任何linux内核上（它甚至可以运行在OS X上）

###获取docker二进制文件

	$ wget https://get.docker.io/builds/Linux/x86_64/docker-latest -O docker
	$ chmod +x docker

>注意：下载二进制文件的时候如果下载慢等，您可以下载最小的压缩版本
>https://get.docker.io/builds/Linux/x86_64/docker-latest.tgz

###运行docker进程

	# start the docker in daemon mode from the directory you unpacked
	$ sudo ./docker -d &

###非root用户运行


`docker`进程一般来说默认用`root`用户运行。从`Docker 0.5.2`开始，docker进程绑定unix socket来代替TCP端口。默认情况下由用户`root`来管理unix socket,但是你也可以使用`sudo`来使用。

从0.5.3版本开始，如果你（你安装的docker）创建一个叫`docker`的unix群组，并且在群组中添加用户，当进程启动的时候，docker群组将有`docker`进程unix socket的读/写使用权。docker进程必须使用root用户运行，但是当使用docker群组的一个用户来运行docker客户端的时候，你不需要在命令前添加`sudo`。从docker 0.9.0版本开始你可以使用`-G`标记指定用户组。

>警告：docker用户组（或者用`-G`指定的用户组）和root等效，

###更新

升级你手动安装的docker,需要先关闭你的docker进程：

	$ killall docker

然后按照常规的步骤安装。

###运行你的第一个docker容器

	# check your docker version
	$ sudo ./docker version
	
	# run a container and open an interactive shell in the container
	$ sudo ./docker run -i -t ubuntu /bin/bash

继续阅读[用户指南](../userguide/README.md)