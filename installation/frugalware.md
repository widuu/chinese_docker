FrugalWare
===

在FrugalWare上使用官方包来安装:

- [lxc-docker i686](http://www.frugalware.org/packages/200141)
- [lxc-docker x86_64](http://www.frugalware.org/packages/200130)

`lxc-docker` 包将会安装最新版本的 Docker。

###依赖关系

Docker 有几个依赖包需要安装，核心依赖包如下：

- systemd
- lvm2
- sqlite3
- libguestfs
- lxc
- iproute2
- bridge-utils

###安装

只需一步就可以完整安装：

	pacman -S lxc-docker


###开始用docker

Docker 会创建一个系统服务，使用下面的命令启动该服务：

	$ sudo systemctl start lxc-docker

设置开机启动：

	$ sudo systemctl enable lxc-docker