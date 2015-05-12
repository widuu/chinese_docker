#Binaries

本安装说明是提供给那些想在多种环境中安装 Docker 的 hacker 们的。

在进行安装之前，请检查你的 Linux 发行版本是否有打包好的 Docker 安装包。我们已经发布了许多发行版包，这样会节省您很多时间。

##检查运行时的依赖关系

如果想要 Docker 正常运行，需要安装以下软件:

- iptables version 1.4 or later
- Git version 1.7 or later
- procps (or similar provider of a "ps" executable)
- XZ Utils 4.9 or later
- a [properly mounted](https://github.com/tianon/cgroupfs-mount/blob/master/cgroupfs-mount) cgroupfs hierarchy (having a single, all-encompassing "cgroup" mount point [is not sufficien](https://github.com/docker/docker/issues/3485))

##检查内核的依赖关系

Docker 进程模式需要特定的内核环境支持。详情请检查您的[发行版](../SUMMARY.md)。

Docker 对 Linux 内核版本的最低要求是3.10，如果内核版本低于 3.10 会缺少一些运行 Docker 容器的功能。这些比较旧的内核，在一定条件下会导致数据丢失和频繁恐慌错误。

推荐使用版本号为（3.x.y）的 3.10 Linux 内核版本（或者新的维护版本），保持跟上内核的次要版本更新来确保内核的BUG已经被修复。

>警告：安装自定义内核时，Linux 版本发行商可能不支持内核软件包。请务必在安装自定义内核之前，先咨询供应商是否支持 Docker。

>警告：一些发行版本上还不能够安装新版本的内核，因为这些发行版本提供的包太老或者与新内核不兼容。

值得注意的是 Docker 可以以客户端模式存在，它几乎可以运行在任何的Linux内核（甚至 OS X 上）。

##Enable AppArmor and SELinux when possible

如果你的 Linux 发行版上支持 AppArmor 或者 Selinux 请启用。这有助于提高安全性并阻止某些漏洞。关与如何启动设置推荐的安全机制，在发行版提供的文档上提供了详细的步骤。

某些 Linux 发行版上默认情况下是启用 AppArmor 或者 Selinux，但是它们的内核不符合安装 Docker 的最低要求（3.10或更高版本）。为了让系统能够启动 Docker 和 运行容器，需要更新内核到3.10或者更高版本。AppArmor/SELinux 用户空间(user space) 工具提供的系统与内核版本的不兼容性可能会阻止 Docker 的运行，容器的启动或者造成容器的意外退出。

>警告：如果机器上启用了安全机制，它就不应该被禁用来使 Docker 和 容器运行。这样会使系统失去发行版供应商的支持，并可能打破严格的监管环境和安全策略。

##获取Docker二进制文件

你可以下载最新版本或者特定版本的二进制版本。下载完二进制文件之后，你必须要给文件可执行权限来运行。

在 Linux 或 OS X 上指定文件的执行权限：

	$ chmod +x docker

从 Github 上获取稳定的发行版本号列表，请查看 `docker/docker` [发布页面](https://github.com/docker/docker/releases)

>注意

> 1) 你可以通过在 URL 中分别附加 MD5 和 SHA256 哈希值来获得二进制包。

> 2) 你可以通过 URL 中附加 .tgz 地址来获得压缩的二进制包。

###获取 Linux 二进制包

通过下边的链接来下载最新版本的 Linux 二进制包：

	https://get.docker.com/builds/Linux/i386/docker-latest

	https://get.docker.com/builds/Linux/x86_64/docker-latest
	
使用下边的链接模式来下载指定版本的 Linux 二进制包：

	https://get.docker.com/builds/Linux/i386/docker-<version>

	https://get.docker.com/builds/Linux/x86_64/docker-<version>
	
举例：

	https://get.docker.com/builds/Linux/i386/docker-1.6.0

	https://get.docker.com/builds/Linux/x86_64/docker-1.6.0

### 获取 Mac OS X 二进制包

Mac OS X 的二进制文件仅仅是一个客户端。你不可以使用它来启动 docker 进程。通过下边的链接来下载最新的 Mac OS X 版本：

	https://get.docker.com/builds/Darwin/i386/docker-latest
	
	https://get.docker.com/builds/Darwin/x86_64/docker-latest
	
通过下边的 URL 模式来下载指定的 Mac OS X 版本：

	https://get.docker.com/builds/Darwin/i386/docker-<version>
	
	https://get.docker.com/builds/Darwin/x86_64/docker-<version>

举例：

	https://get.docker.com/builds/Darwin/i386/docker-1.6.0
	
	https://get.docker.com/builds/Darwin/x86_64/docker-1.6.0
	
### 获取 Windows 的二进制包

从 1.60 版本开始，你可以只下载 Windows 客户端的二进制包。此外，二进制包仅是一个客户端，你不能用它来启动 docker 进程。通过下边的链接来下载最新的 Windows 版本：

	https://get.docker.com/builds/Windows/i386/docker-latest.exe
	
	https://get.docker.com/builds/Windows/x86_64/docker-latest.exe
	
通过下边的 URL 模式来下载指定的 Windows 版本：

	https://get.docker.com/builds/Windows/i386/docker-<version>.exe
	
	https://get.docker.com/builds/Windows/x86_64/docker-<version>.exe

举例：

	https://get.docker.com/builds/Windows/i386/docker-1.6.0.exe

	https://get.docker.com/builds/Windows/x86_64/docker-1.6.0.exe

## 运行Docker进程

	# start the docker in daemon mode from the directory you unpacked
	$ sudo ./docker -d &

##非root用户运行


`docker` 进程一般来说默认用 `root` 用户运行， docker 进程绑定 unix socket 来代替 TCP 端口。默认情况下由用户 `root` 来管理 unix socket ,但是你也可以使用 `sudo` 来使用。

如果你（你安装的 Docker）创建一个叫 `docker` 的 unix 群组，并且在群组中添加用户，当进程启动的时候，Docker 群组将有 `docker` 进程 unix socket 的读/写使用权。docker 进程必须使用root用户运行，但是当使用 Docker 群组的一个用户来运行 Docker 客户端的时候，你不需要在命令前添加 `sudo` 。

>警告：Docker 用户组（或者用`-G`指定的用户组）和 root 等效，

###更新

升级你手动安装的 Docker ,需要先关闭你的 docker 进程：

	$ killall docker

然后按照常规的步骤安装。

###运行你的第一个Docker容器

	# check your docker version
	$ sudo ./docker version
	
	# run a container and open an interactive shell in the container
	$ sudo ./docker run -i -t Ubuntu /bin/bash

继续阅读[用户指南](../userguide/README.md)