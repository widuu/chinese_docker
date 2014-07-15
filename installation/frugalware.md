FrugalWare
===

在FrugalWare上使用官方包来安装:

- [lxc-docker i686](http://www.frugalware.org/packages/200141)
- [lxc-docker x86_64](http://www.frugalware.org/packages/200130)

`lxc-docker`包将会安装最新版本。

###依赖关系

docker有几个依赖包需要安装，依赖包如下：

- systemd
- lvm2
- sqlite3
- libguestfs
- lxc
- iproute2
- bridge-utils

###安装

简单安装

	pacman -S lxc-docker

将会全部安装。

###开始用docker

这里docker会创建一个系统服务。要启动docker服务：

	$ sudo systemctl start lxc-docker

开机启动：

	$ sudo systemctl enable lxc-docker