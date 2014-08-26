Arch Linux
===

你可以使用Arch Linux社区发布的docker软件包进行安装：

- [docker](https://www.archlinux.org/packages/community/x86_64/docker/)

或者使用AUR包

- [docker-git](https://aur.archlinux.org/packages/docker-git/)

docker软件包将会安装最新版本的docker。docker-git则是由当前master分支构建的包。

###依赖关系

docker依赖于几个指定的安装包，核心的几个依赖包为：

- bridge-utils
- device-mapper
- iproute2
- lxc
- sqlite

###安装

一般包的简单安装：

	pacman -S docker

对于AUR包的执行：

	yaourt -S docker-git

这里假设你已经安装好了yaourt。如果你之前没有安装构建过这个包，请参考[Arch User Repository](https://wiki.archlinux.org/index.php/Arch_User_Repository#Installing_packages)。

###开启Docker

docker会创建一个系统服务，用下面命令来启动：

	$ sudo systemctl start docker

设置开机启动：

	$ sudo systemctl enable docker