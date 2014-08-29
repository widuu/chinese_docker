Gentoo
===

在 Gentoo Linux 上安装 Docker 可以通过以下两种方式的任一种实现。如果你正在寻找一种稳定的方案，最好的办法就是直接在 portage tree 上安装官方的 app-emulation/docker 包。

如果你正在寻找二进制的 `-bin` ebuild,  ebuild, or 最新的 ebuild，第二种安装方式是用 [https://github.com/tianon/docker-overlay](https://github.com/tianon/docker-overlay) 提供的 overlay，可以使用 app-portage/layman 添加，最准确、最新的安装和使用的 overlay 的文档可以在 [the overlay README](https://github.com/tianon/docker-overlay/blob/master/README.md#using-this-overlay) 中找到。

请记住有时候官方最新版本和 overlay 是不一致的，并且在 overlay 和 portage tree 中的版本都不是一致的，请耐心等待，最新版本会很快更新的。

##安装

软件包应该正确引入所有必须的依赖关系，并且提示所有必须的内核选项。0.7 以上版本的 ebuilds 包含了 use 标记引入主要存储驱动程序的依赖关系，默认启用了 "device-mapper" use 标记，这是最简单的安装方式。

	$ sudo emerge -av app-emulation/docker 

如果从 ebuild 或者生成的二进制文件出现任何问题，包括特别是缺少内核配置标记/或依赖关系，请 [在 docker-overlay 仓库提交一个 issue](https://github.com/tianon/docker-overlay/issues) 或者直接在 freenode 网络的 #docker IRC 频道上联系 tianon。

更多信息请到这里查看：[tianon's blog](https://tianon.github.io/post/2014/05/17/docker-on-gentoo.html)

##启动 Docker

确保您正在运行的内核，包括所有针对 LXC 的必要模块和/或配置（ device-mapper 和/或 AUFS 则是可选的，取决于你决定要使用的存储驱动程序）。

###OpenRC

启动 docker 守护进程：

	$ sudo /etc/init.d/docker start

开机启动：

	$ sudo rc-update add docker default

###systemd

启动 docker 守护进程：

	$ sudo systemctl start docker.service

开机启动：

	$ sudo systemctl enable docker.service
