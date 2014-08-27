openSUSE
===

Docker 支持 openSUSE 12.3 或者更高的版本。由于 Docker 的限制，Docker 只能运行在64位的主机上。

###安装

OBS的 [虚拟化项目中](https://build.opensuse.org/project/show/Virtualization "Virtualization project ") 提供了 `docker` 包

请添加正确的 Docker 仓库源来安装docker

	# openSUSE 12.3
	$ sudo zypper ar -f http://download.opensuse.org/repositories/Virtualization/openSUSE_12.3/ Virtualization
	$ sudo rpm --import http://download.opensuse.org/repositories/Virtualization/openSUSE_12.3/repodata/repomd.xml.key
	
	# openSUSE 13.1
	$ sudo zypper ar -f http://download.opensuse.org/repositories/Virtualization/openSUSE_13.1/ Virtualization
	$ sudo rpm --import http://download.opensuse.org/repositories/Virtualization/openSUSE_13.1/repodata/repomd.xml.key

安装 Docker 包

	$ sudo zypper in docker

它也可以使用openSUSE `s1-click` 来安装，请访问这个[页面](http://software.opensuse.org/package/docker "s1-click")，选择你的 openSUSE 版本和点击安装链接，这将在你的系统中添加仓库源并且安装 Docker 包。

安装完毕，让我们来启动 docker 进程

	$ sudo systemctl start docker

设置开机启动 docker：

	$ sudo systemctl enable docker

Docker 包会创建一个新的群组叫做 `docker` ,用户、非root用户，需要是这个群组的成员来与 docker 进程进行交互，你可以使用如下命令添加用户：

	$ sudo usermod -a -G docker <username>

确认一切都是否按照预期工作：

	$ sudo docker run --rm -i -t ubuntu /bin/bash

这条命令将下载和导入 `Ubuntu` 镜像，并且在容器内运行 bash，输入 exit 来退出容器。

好！更多信息请继续阅读[用户指南](../userguide/README.md)。