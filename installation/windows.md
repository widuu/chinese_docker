Microsoft Windows 安装docker
===

###我的感言

>docker越做越好，用户的体验度和操作的便利性也是越来越好，这点可以看我以前写的docker教程,[http://www.widuu.com/docker/](http://www.widuu.com/docker/)，由于正式版出来之后，许多新的特性和安装方式都有所改变，我决定花时间重写docker中文文档。慢慢来，有时间久翻译一点。

#windows

>注意：docker已经在windows7.1和windows 8上通过测试，当然它也可以在低版本的windows上使用。但是你的处理器必须支持硬件虚拟化。

docker 引擎使用的是Linux内核的特性，所以我们需要在 Windows 上使用一个轻量级的虚拟机(vm)来运行 docker。我们使用 Windows的Docker客户端来控制 Docker 虚拟化引擎的构建、运行和管理 Docker 容器。

为了简化这个过程，我们设计了一个叫 Boot2Docker 的应用程序，你可以通过它来安装虚拟机和运行docker 进程。

###安装

1. 下载最新版本的[Docker for Windows Installer](https://github.com/boot2docker/windows-installer/releases/latest)

2. 运行安装文件，它将会安装virtualbox、MSYS-git boot2docker Linux镜像和Boot2Docker的管理工具。

![docker windows软件安装](http://widuu.u.qiniudn.com/windows_docker.png)

3. 从桌面上或者Program Files中找到Boot2Docker for Windows，运行 `Boot2Docker Start` 脚本。这个脚本会要求你输入 ssh 密钥密码 - 可以简单点（但是起码看起来比较安全），然后只需要按[Enter]按钮即可。

![windows docker run](http://widuu.u.qiniudn.com/windows_docker2.png)

`Boot2Docker Start`启动脚本将连接你到虚拟机的shell会话，如果需要的话，它会初始化一个新的VM并且启动它。

>如果提示错误，找不到主机等信息，大家可以在安装目录执行
	
	boot2docker.exe init
	boot2docker.exe start
	boot2docker.exe ssh
###升级

+ 下载最新的 [Docker for Windows Installer](https://github.com/boot2docker/windows-installer/releases/tag/v1.5.0)
+ 运行安装程序，这将升级VirtualBox和Boot2Docker管理工具
+ 打开终端输入如下的命令来升级你现有的容器：

	$ boot2docker stop
	$ boot2docker download
	$ boot2docker start

###运行Docker

> 注意：如果你使用一个远程的 Docker 进程，像 Boot2Docker 。在这时候，当你运行 docker 命令的时候前边不需要像前边的例子那样加入`sudo`。

我们运行一下事例镜像 `hello-world `，运行如下命令：

	$ docker run hello-world

这将下载非常小的 `hello-world` 镜像，并且打印打印 `Hello from Docker.` 信息。

###使用PUTTY登陆来代替CMD命令行

Boot2Docker使用 `%HOMEPATH%\.ssh` 目录来生成你的共有和私有密钥。同样登陆的时候你也需要使用这个目录下的私有密钥。

这个私有密钥需要转换成 PuTTY 所需要的格式。

你可以使用 [puttygen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)来生成，具体操作如下:

1. 打开 `puttygen.exe` 找到（"File"->"Load"）按钮来加载 %HOMEPATH%\.ssh\id_boot2docker 私有密钥文件。

2. 点击` "Save Private Key"`按钮。

3. 在PUTTY中使用刚才保存的文件来登陆 docker@127.0.0.1:2022 

###进一步细节

Boot2Docker 管理工具提供了如下几个命令：

	$ ./boot2docker
	Usage: ./boot2docker [<options>] {help|init|up|ssh|save|down|poweroff|reset|restart|config|status|info|ip|delete|download|version} [<args>]

###容器端口重定向

boot2Docker的默认用户是 `docker` 密码是 `tcuser`。 

最新版本的 boot2docker 可以设置网络适配器来给容器提供端口访问。

如你运行一个暴露内部端口的容器

	docker run --rm -i -t -p 80:80 nginx

当你需要使用一个IP地址来访问 Nginx 服务器，你可以使用如下命令来查看 ip。

	$ boot2docker ip

通常情况下，是192.168.59.103,但是它可以通过 virtualbox 的 dhcp 来改变。

更多细节信息，请查看[Boot2Docker site](http://boot2docker.io/)
