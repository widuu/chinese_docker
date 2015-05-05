#windows

>提示：Docker 已经在windows7.1和windows 8上通过测试，当然它也可以在低版本的windows上使用。但是你的处理器必须支持硬件虚拟化。

Docker 引擎使用的是Linux内核特性，所以我们需要在 Windows 上使用一个轻量级的虚拟机 (VM) 来运行 Docker。我们使用 Windows的Docker客户端来控制 Docker 虚拟化引擎的构建、运行和管理 。

为了简化这个过程，我们设计了一个叫 [Boot2Docker](https://github.com/boot2docker/boot2docker) 的应用程序，你可以通过它来安装虚拟机和运行 Docker。

虽然你使用的是 Windows 的 Docker 客户端，但是 docker 引擎容器依然是运行在 Linux 宿主主机上（现在是通过Virtual box）。直到我们开发了 windows 版本的 Docker 引擎，你只需要在你的 Windows 主机上启动一个 Linux 容器。

##安装

1. 下载最新版本的[Docker for Windows Installer](https://github.com/boot2docker/windows-installer/releases/latest)

2. 运行安装文件，它将会安装virtualbox、MSYS-git boot2docker Linux镜像和Boot2Docker的管理工具。

	![docker windows软件安装](http://widuu.u.qiniudn.com/windows_docker.png)

3. 从桌面上或者Program Files中找到Boot2Docker for Windows，运行 `Boot2Docker Start` 脚本。这个脚本会要求你输入 ssh 密钥密码 - 可以简单点（但是起码看起来比较安全），然后只需要按[Enter]按钮即可。

4. `Boot2Docker Start` 将启动一个 Unix shell 来配置和管理运行在虚拟主机中的 Docker，运行 `docker version` 来查看它是否正常工作。

![windows docker run](../images/windows-boot2docker-start.png)


###运行 Docker

>注意：如果你使用的是一个远程的 Docker 进程，像 `Boot2docker` ，你就不需要像前边的文档实例中那样在输入 Docker 命令之前输入 `sudo`。


**Boot2docker start** 将会自动启动一个 shell 命令框并配置好环境变量，以便您可以马上使用 Docker ：

让我们尝试运行 `hello-world` 例子。 运行：

	$ docker run hello-world

这将会下载一个非常小的 `hello-world` 镜像，并且打印出 `Hello from Docker.` 信息。


###使用 Windows 的命令行(cmd.exe) 来管理运行 Docker

启动一个 Windows 命令行（cmd.exe）.

运行 Boot2docker 命令，这需要你的 Windows PATH环境变量中包含了 `ssh.exe`。因此我们需要将安装的 Git 的 bin 目录 （其中包含了 ssh.exe） 配置到我们的 `%PATH%` 环境变量中，运行如下命令：

	set PATH=%PATH%;"c:\Program Files (x86)\Git\bin"


现在，我们可以运行 `boot2docker start` 命令来启动 Boot2docker 虚拟机。（如果有虚拟主机不存在的错误提示，你需要运行 `boot2docker init` 命令）。复制上边的指令到 cmd.exe 来设置你的 windows 控制台的环境变量，然后你就可以运行 docker 命令了，譬如 `docker ps` :

![docker cmd.exe](../images/windows-boot2docker-cmd.png)


### PowerShell 中使用 Docker 

启动 PowerShell，你需要将 `ssh.exe` 添加到你的 PATH 中。

	$Env:Path = "${Env:Path};c:\Program Files (x86)\Git\bin"

之后，运行 `boot2docker start` 命令行，它会打印出  PowerShell 命令，这些命令是用来设置环境变量来连接运行在虚拟机中 Docker 的。运行这些命令，然后你就可以运行 docker 命令了，譬如 `docker ps` :

![Powershell Docker](../images/windows-boot2docker-powershell.png)

>提示:你可以使用 `boot2docker shellinit | Invoke-Expression` 来设置你的环境变量来代替复制粘贴 Powershell 命令。

##进一步的细节

Boot2Docker 管理工具提供了如下几个命令：

	$ boot2docker
	Usage: boot2docker.exe [<options>] {help|init|up|ssh|save|down|poweroff|reset|restart|config|status|info|ip|shellinit|delete|download|upgrade|version} [<args>]


###升级

+ 下载最新的 [Docker for Windows Installer](https://github.com/boot2docker/windows-installer/releases/tag/v1.5.0)
+ 运行安装程序，这将升级 Boot2Docker 管理工具
+ 打开终端输入如下的命令来升级你现有的虚拟机：

	$ boot2docker stop
	$ boot2docker download
	$ boot2docker start


###容器端口重定向

boot2Docker的默认用户是 `docker` 密码是 `tcuser`。 

最新版本的 boot2docker 可以设置网络适配器来给容器提供端口访问。

如你运行一个暴露内部端口的容器

	docker run --rm -i -t -p 80:80 nginx

当你需要使用一个IP地址来访问 Nginx 服务器，你可以使用如下命令来查看 ip。

	$ boot2docker ip

通常情况下，是192.168.59.103,但是它可以通过 virtualbox 的 dhcp 来改变。

更多细节信息，请查看[Boot2Docker site](http://boot2docker.io/)


###使用PUTTY登陆来代替CMD命令行

Boot2Docker使用 `%HOMEPATH%\.ssh` 目录来生成你的共有和私有密钥。同样登陆的时候你也需要使用这个目录下的私有密钥。

这个私有密钥需要转换成 PuTTY 所需要的格式。

你可以使用 [puttygen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)来生成，具体操作如下:

1. 打开 `puttygen.exe` 找到（"File"->"Load"）按钮来加载 %HOMEPATH%\.ssh\id_boot2docker 私有密钥文件。

2. 点击` "Save Private Key"`按钮。

3. 在PUTTY中使用刚才保存的文件来登陆 docker@127.0.0.1:2022 


##参考

如果你已经运行 Docker 主机或者你不希望使用 `Boot2docker` 安装，你可以安装 docker.exe 使用非官方的包管理器 Chocolately。了解更多新，请查看 [Docker package on Chocolatey](https://chocolatey.org/packages/docker)。




