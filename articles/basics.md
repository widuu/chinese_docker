使用docker第一步
===

###检查你的Docker是否安装

本指南假设你已经完成了Docker的安装工作。检查你安装的Docker,运行以下命令：

	# Check that you have a working install
	$ docker info

如果你得到`docker: command not found`或类似`/var/lib/docker/repositories: permission denied`信息，你可能没有完整的安装上Docker或者你没有权限访问你主机上的Docker。

请参考[安装指南](../install/)安装。

###下载预构建镜像

	# Download an ubuntu image
	$ sudo docker pull ubuntu

这将从Docker Hub中查找到`Ubuntu`镜像，并且从Docker Hub中下载镜像到本地缓存中。

>提示：当镜像下载成功之后，你将会看到镜像的短id一个12字符的hash,`539c0211cd76: Download complete`