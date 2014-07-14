openSUSE
===

Docker可以运行在openSUSE 12.3或者更高的版本，不过请记住，由于docker的限制，docker只能运行在64位的主机上。

###安装

OBS的[虚拟化项目中](https://build.opensuse.org/project/show/Virtualization "Virtualization project ")来提供 `docker` 包

请添加正确的docker仓库源来安装docker

	# openSUSE 12.3
	$ sudo zypper ar -f http://download.opensuse.org/repositories/Virtualization/openSUSE_12.3/ Virtualization
	$ sudo rpm --import http://download.opensuse.org/repositories/Virtualization/openSUSE_12.3/repodata/repomd.xml.key
	
	# openSUSE 13.1
	$ sudo zypper ar -f http://download.opensuse.org/repositories/Virtualization/openSUSE_13.1/ Virtualization
	$ sudo rpm --import http://download.opensuse.org/repositories/Virtualization/openSUSE_13.1/repodata/repomd.xml.key

安装docker包

	$ sudo zypper in docker

它也可以使用openSUSE`s1-click`来安装，请访问这个[页面](http://software.opensuse.org/package/docker "s1-click"),选择你的openSUSE版本和点击安装链接，这将在你的系统中添加仓库源并且安装docker包。

现在它安装好了，让我们来启动docker进程

	$ sudo systemctl start docker

如果我们想要开机启动docker，你需要这样：

	$ sudo systemctl enable docker

docker包会创建一个新的群组叫做docker,用户、非root用户，需要是这个群组的成员来与docker进程进行交互，你可以使用如下命令添加用户：

	$ sudo usermod -a -G docker <username>

来确认一切都是否按照预期工作：

	$ sudo docker run --rm -i -t ubuntu /bin/bash

这将下载和导入`ubuntu`镜像，并且在容器内运行bash，输入exit来退出容器。

好！

继续阅读用户指南。