Gentoo
===

在Gentoo Linux安装docker可以使用两种方法其中之一来完成。如果你想找一个稳定的经验，最好的办法就是从Portage树中直接使用官方app-emulation/docker来安装。

如果你再寻找-bin ebuild,

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