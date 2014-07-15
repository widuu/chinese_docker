Arch Linux
===

可以使用Arch Linux社区包进行安装：

- [docker](https://www.archlinux.org/packages/community/x86_64/docker/)

或者使用AUR包

- [docker-git](https://aur.archlinux.org/packages/docker-git/)

docker软件包将会安装最新版本的docker。docker-git包是一个构建的分支。

###依赖关系

docker依赖于几个指定的安装包。这些包如下：

- bridge-utils
- device-mapper
- iproute2
- lxc
- sqlite

###安装

一般包的简单安装

	pacman -S docker

对于AUR包的执行：

	yaourt -S docker-git

这里假设你已经安装好了yaourt，如果你之前没有安装和构建这个包，请查看[Arch User Repository](https://wiki.archlinux.org/index.php/Arch_User_Repository#Installing_packages)的信息

###开启Docker

这里docker会创建一个系统服务。要启动docker服务：

	$ sudo systemctl start docker

开机启动：

	$ sudo systemctl enable docker