Gentoo
===

在gentoo linux 上安装docker 可以通过以下两种方式的任一种实现。
第一种也是最好的一种就是如果你正在寻找一种稳定的方式就在portage tree上直接安装官方的app-emulation/docker package

如果你正在寻找a -bin ebuild, a live ebuild, or bleeding edge ebuild changes/fixes, 
第二种安装方式是用https://github.com/tianon/docker-overlay提供的overlay，它可以添加使用app-portage/layman，最准确，最新安装和使用的overlay的文档可以在the overlay README.中找到．

请记住有时候官方最新版本和overlay是不一致的，并且在overlay和portage tree中的版本都不是一致的，请耐心等待，最新版本会很快更新的。

###安装

软件包需要正确引入所有必须的依赖关系，并且提示所有需要的内核选项。0.7+的ebuilds包含use标记会提示依赖关系和主要驱动程序，"device-mapper" use标记是默认启动的，这是最简单的安装路径

	$ sudo emerge -av app-emulation/docker 

如果从ebuild或者生成的二进制文件出现任何问题，包括特别是缺少内核配置标记/或依赖关系，在docker-overlary仓库提交一个问题或者ping tianon 在 #docker IRC 信道上的 freenode 网络。

###Starting Docker

确保您正在运行的内核，包括所有必要的模块和/或配置 LXC （（可选） device-mapper和 AUFS，取决于你决定要使用的存储驱动程序）。

###OpenRC

启动docker进程：

	$ sudo /etc/init.d/docker start

开机启动：

	$ sudo rc-update add docker default

###systemd

启动docker进程：

	$ sudo systemctl start docker.service

开机启动:

	$ sudo systemctl enable docker.service*