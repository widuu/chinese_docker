Microsoft Windows 安装docker
===

###我的感言

>docker越做越好，用户的体验度和操作的便利性也是越来越好，这点可以看我以前写的docker教程,[http://www.widuu.com/docker/](http://www.widuu.com/docker/)，由于正式版出来之后，许多新的特性和安装方式都有所改变，我决定花时间重写docker中文文档。慢慢来，有时间久翻译一点。

#windows

>注意：docker已经在windows7.1和windows 8上通过测试，也支持运行老的版本，你的处理器必须支持硬件虚拟化。

docker引擎使用的是Linux内核的特性，所以在OSＸ上运行docker我们就待使用轻量级的虚拟机(VM),你使用windows上的Docker客户端来管理Docker容器和控制docker引擎的构建、运行、管理。

为了简化这个过程，我们设计了一个应用程序叫Boot2Docker安装虚拟机和运行docker进程。

###安装

1.下载最新版本的Docker for Windows Installer(由于S3可能被墙了，所以国内有时候下载不了，但是我把它放到国内七牛上了，大家可以下载试试[http://qiniu.widuu.com/docker-install.exe"](http://qiniu.widuu.com/docker-install.exe)<版本1.11>)

2.运行安装文件，它将会安装virtualbox、MSYS-git boot2docker Linux镜像和Boot2Docker的管理工具。

![docker windows软件安装](http://widuu.u.qiniudn.com/windows_docker.png)

3.从你的桌面上或者从你的应用程序文件夹中找到应用的快捷方式，运行`Boot2Docker Start`，启动脚本会让你输入一个ssh秘钥密码（简单点但最起码安全）

![windows docker run](http://widuu.u.qiniudn.com/windows_docker2.png)

`Boot2Docker Start`启动脚本将连接你到虚拟机的shell会话，如果需要的话，它会初始化一个新的VM并且启动它。

>如果提示错误，找不到主机等信息，大家可以在安装目录执行
	
	boot2docker.exe init
	boot2docker.exe start
	boot2docker.exe ssh
###升级

+ 下载最新的 Docker for OS X Installer
+ 运行安装程序，这将升级VirtualBox和Boot2Docker管理工具
+ 升级现有的虚拟机，打开终端并运行

	$ boot2docker stop
	$ boot2docker download
	$ boot2docker start

###运行Docker

通过终端，你可以运行docker输出“hello word”的例子

	$ docker run ubuntu echo hello world

这将下载Ubuntu镜像和打印hello word

###Docker测试

>这个不是官方的，因为我们大家知道下载docker从国外是灰常慢的，而且不一定什么时候就被墙了，所以在这里我就提供几个测试的，让大家下载来使用。

	wget qiniu.widuu.com/busybox.tar
	docker load -i busybox.tar
	//如果没有tag或者name
	docker tag id name:tag
	
	//或者大家使用docker中文社区提供的,这里非常感谢docker中文社区
	
	docker pull index.dockboard.org/widuucom/busybox
	
	docker run index.dockboard.org/widuucom/busybox echo hello word

###容器端口重定向

最新版本的boot2docker可以设置网络适配器提供容器访问的端口

如果你运行容器给定一个指定的端口

	$ docker run --rm -i -t -p 80:80 ngin

然后你应该能够使用IP地址访问Nginx服务器：

	$ boot2docker ip

通常，是192.168.59.103,但是可以通过virtualbox的dhcp改变。

###进一步的细节

如果你有求知欲，boot2docker的默认用户名是`docker`密码是`tcuser`

Boot2Docker管理工具提供了一些默认命令:

	$ ./boot2docker
	Usage: ./boot2docker [<options>]
	{help|init|up|ssh|save|down|poweroff|reset|restart|config|status|info|ip|delete|download|version} [<args>]
