在 OS X 上安装Docker
===

>注意: Docker 仅支持 OS X 10.6 及更高的版本

Docker 引擎要求 Linux 内核环境，所以要在 OSＸ上运行 Docker 我们就得使用轻量级的虚拟机(VM),并使用 OS X 上的 Docker 客户端来管理 Docker 容器和控制 Docker 引擎的构建、运行、管理。

为了简化这个过程，我们设计了一个应用程序叫 Boot2Docker 安装虚拟机和运行 Docker 进程。

##安装

1.下载最新的版本的 Docker for OS X Installer（因为官方用的亚马逊的云存储，貌似给封了~我下载了一个放到七牛云存储了，大家可以通过下面这个地址下载:[http://qiniu.widuu.com/Boot2Docker-1.1.1.pkg](http://qiniu.widuu.com/Boot2Docker-1.1.1.pkg "Boot2Docker-1.1.1.pkg")<版本：1.1.1>）

2.运行安装文件，这将安装 VirtualBox 和 Boot2Docker 管理工具。

![安装程序](http://widuu.u.qiniudn.com/osx-installer.png)

3.在应用程序文件夹中运行 `Boot2Docker` ：

![安装步骤2](http://widuu.u.qiniudn.com/osx-Boot2Docker-Start-app.png)

或者手动初始化 Boot2Docker ,打开终端并运行：

	$ boot2docker init
	$ boot2docker start
	$ export DOCKER_HOST=tcp://$(boot2docker ip 2>/dev/null):2375

一旦你初始化虚拟机，您可以 `boot2docker stop` 和 `boot2docker start` 来控制。

##升级

+ 下载最新的 Docker for OS X Installer
+ 运行安装程序，这将升级 VirtualBox 和 Boot2Docker 管理工具
+ 升级现有的虚拟机，打开终端并运行

	$ boot2docker stop
	$ boot2docker download
	$ boot2docker start

##运行 Docker

通过终端，你可以运行 Docker 输出 “hello word” 的例子

	$ docker run ubuntu echo hello world

这将下载 Ubuntu 镜像和打印 hello word

##容器端口重定向

最新版本的 boot2docker 可以设置网络适配器提供容器访问的端口

如果你运行容器给定一个指定的端口

	$ docker run --rm -i -t -p 80:80 ngin

然后你应该能够使用IP地址访问 Nginx 服务器：

	$ boot2docker ip

通常，是192.168.59.103,但是可以通过 Virtualbox 的 DHCP 改变。

##更多细节

如果你有求知欲，boot2docker 的默认用户名是 `docker` 密码是 `tcuser`

Boot2Docker 管理工具提供了一些默认命令:

	$ ./boot2docker
	Usage: ./boot2docker [<options>]
	{help|init|up|ssh|save|down|poweroff|reset|restart|config|status|info|ip|delete|download|version} [<args>]
