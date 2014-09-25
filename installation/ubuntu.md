Ubuntu
===

Docker 支持以下的 Ubuntu 版本

+ Ubuntu Trusty 14.04 (LTS) (64-bit)
+ Ubuntu Precise 12.04 (LTS) (64-bit)
+ Ubuntu Raring 13.04 and Saucy 13.10 (64 bit)

如何你打算使用 [UFW防火墙](https://help.ubuntu.com/community/UFW)，请阅读 [*docker和UFW*](#docker-and-ufw)

##Ubuntu Trusty 14.04 (LTS) (64-bit)

Ubuntu配置了 3.13.0 Linux 内核和将安装 Docker 0.9.1 的 `docker.io` 包，当然这些都取决于你的 Ubuntu 镜像源。

>提示：Ubuntu (和 Debian )包含一个特别陈旧的 KDE3/GNOME2 包叫 `docker` ,所以我们把这个包叫 `docker.io`。

###安装

安装最新版本的 Ubuntu 包（可能不是最新的 Docker 版本包）：

	$ sudo apt-get update
	$ sudo apt-get install docker.io
	$ sudo ln -sf /usr/bin/docker.io /usr/local/bin/docker
	$ sudo sed -i '$acomplete -F _docker docker' /etc/bash_completion.d/docker.io

如果你想安装最新版本的 Docker:

首先，检查你的 APT 系统能够处理 `https` 的 URLs：如果你的主机不存在 `/usr/lib/apt/methods/https` 文件，请先安装 `apt-transport-https` 包。

	[ -e /usr/lib/apt/methods/https ] || {
	  apt-get update
	  apt-get install apt-transport-https
	}

然后，添加 Docker 镜像秘钥到你的本地秘钥库.

	$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9

添加 Docker 镜像到 apt 软件源,更新和安装 `lxc-docker` 包。
你可能会受到一个包不可信的警告信息，输入yes继续安装。

	$ sudo sh -c "echo deb https://get.docker.io/ubuntu docker main\
	> /etc/apt/sources.list.d/docker.list"
	$ sudo apt-get update
	$ sudo apt-get install lxc-docker

>提示：
>这还有一个简单的 `curl` 脚本来协助你完成这一工作。
	
	$ curl -s https://get.docker.io/ubuntu/ | sudo sh

确认它是否正常工作:

	$ sudo docker run -i -t ubuntu /bin/bash

上边的命令会自动下载 Ubuntu 镜像，并且会在容器内执行 bash

##Ubuntu Precise 12.04 (LTS) (64-bit)

###依赖条件

Linux kernel 3.8

由于 LXC 存在的 bug 问题，Docker 最好的工作环境是在 3.8 内核基础上。但是 12.04 使用的是 3.2 内核，所以我们需要升级它。安装附带 AUFS 的内核需要遵循以下步骤。我们需要包含通用的头文件取决于启用的包，像 ZFS 和 VirtualBox guest additions。如果你的 "precise" 内核中没有安装这些头文件，这时候你可以在 "raring" 内核跳过这些头文件。如果你不确定的时候包含这些是比较安全的。

	# install the backported kernel
	$ sudo apt-get update
	$ sudo apt-get install linux-image-generic-lts-raring linux-headers-generic-lts-raring
	
	# reboot
	$ sudo reboot

###安装

>警告：这些指令在 0.6 版本的时候就已经改变了，如果你要从早期版本升级，你需要重新安装他们。

Docker 作为一个 Debain 包，安装起来比较简单，如果你不在美国请手动选择[镜像源](https://docs.docker.com/installation/ubuntulinux/#mirrors)，其它镜像源的 Debain 包安装起来也许会更快（譬如我们使用俄罗斯的镜像）。

首先，检查你的 APT 系统能够处理 `https` 的 URL2：如果你的主机不存在 `/usr/lib/apt/methods/https` 文件，请先安装 `apt-transport-https` 包

	[ -e /usr/lib/apt/methods/https ] || {
	  apt-get update
	  apt-get install apt-transport-https
	}

然后，添加 Docker 镜像秘钥到你的本地秘钥库。

	$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9

添加 Docker 镜像添加你的 apt 软件源,更新和安装 `lxc-docker` 包。
你可能会收到包不可信警告信息，输入yes继续安装。

	$ sudo sh -c "echo deb https://get.docker.io/ubuntu docker main\
	> /etc/apt/sources.list.d/docker.list"
	$ sudo apt-get update
	$ sudo apt-get install lxc-docker

>提示：
>这还有一个简单的 `curl` 脚本来协助你完成这一工作。
	
	$ curl -s https://get.docker.io/ubuntu/ | sudo sh

确认它是否正常工作:

	$ sudo docker run -i -t ubuntu /bin/bash

上边的命令会自动下载 Ubuntu 镜像，并且会在容器内执行 bash,输入 `exit` 来退出。

现在！可以查看[用户指南](../userguide/README.md)。

##Ubuntu Raring 13.04 and Saucy 13.10 (64 bit)

这些指令可以再 Ubuntu 13.04 和 13.10 上使用

###依赖条件

可选 AUFS 文件系统支持

Ubuntu Raring 已经安装好了 3.8 内核，所以我们不需要安装它。然而，不是所有的系统都支持 AUFS 系统。在版本 0.7 上 AUFS 是可选的，它作为一个驱动程序，我们建议使用它。

为了确保安装 AUFS，运行以下命令：

	$ sudo apt-get update
	$ sudo apt-get install linux-image-extra-`uname -r`

###安装

Docker 作为一个 Debain 包，安装起来比较简单

>警告：这些指令在 0.6 版本的时候就已经改变了，如果你要从早期版本升级，你需要重新安装他们。

首先，添加 Docker 镜像秘钥到你的本地秘钥库.

	$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9

添加 Docker 镜像添加到你的 apt 软件源，升级和安装 `lxc-docker` 包。

	$ sudo sh -c "echo deb http://get.docker.io/ubuntu docker main\
	> /etc/apt/sources.list.d/docker.list"
	$ sudo apt-get update
	$ sudo apt-get install lxc-docker

现在下载 `Ubuntu` 镜像运行，确认是否正常工作
	
	$ sudo docker run -i -t ubuntu /bin/bash

输入 `exit` 退出

好！现在你可以去查看[用户指南](../userguide/README.md)了。

###不使用root运行

`docker` 进程一般来说默认用 `root` 用户运行，从 `Docker 0.5.2` 开始，docker 进程绑定 unix socket 来代替 TCP 端口。默认情况下用户 `root` 来管理 unix socket,但是你也可以使用 `sudo` 来使用。

从 0.5.3 版本开始，如果你（你安装的 Docker）创建一个叫 `docker` 的 unix 群组，并且在群组中添加用户。当进程启动的时候，`docker` 群组将有 `docker` 进程 unix socket 的读/写使用权。`docker` 进程必须使用 root 用户运行，但是当使用 `docker` 群组的一个用户来运行 `docker` 客户端的时候，你不需要在命令前添加 `sudo`，从 Docker 0.9.0版本开始你可以使用 `-G` 标记指定用户组。

>警告：`docker` 用户组（或者用 `-G` 指定的用户组）和 root 等效，

举例

	# Add the docker group if it doesn't already exist.
	$ sudo groupadd docker
	
	# Add the connected user "${USER}" to the docker group.
	# Change the user name to match your preferred user.
	# You may have to logout and log back in again for
	# this to take effect.
	$ sudo gpasswd -a ${USER} docker
	
	# Restart the Docker daemon.
	# If you are in Ubuntu 14.04, use docker.io instead of docker
	$ sudo service docker restart


###升级

使用 `apt-get` 指令来安装最新版本的 Docker

	# update your sources list
	$ sudo apt-get update
	
	# install the latest
	$ sudo apt-get install lxc-docker

##内存和交换空间

如果你想使用内存和交换空间，你必须在命令行中添加如下的内核参数:

	cgroup_enable=memory swapaccount=1

系统上使用 GRUB（ Ubuntu 默认使用 GRUB ），你可以编辑 `/etc/default/grub` 这个文件，找到 `GRUB_CMDLINE_LINUX`，像如下

	GRUB_CMDLINE_LINUX=""

并且替换成

	GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"

再运行 `sudo update-grub` 并且重启

这些参数将会使你摆脱如下的警告信息：

	WARNING: Your kernel does not support cgroup swap limit.
	WARNING: Your kernel does not support swap limit capabilities. Limitation discarded.

##故障排除

在 Linux Mint, `cgroup-lite` 默认没有被安装，在 Docker 正常工作之前，你可以通过如下的方式安装：

	$ sudo apt-get update && sudo apt-get install cgroup-lite

##Docker and UFW

Docker 使用桥接的方式来管理网络，默认情况下，UFW 会阻止所有的流量转发。你需要允许 UFW 的转发：

	$ sudo nano /etc/default/ufw

	# Change:
	# DEFAULT_FORWARD_POLICY="DROP"
	# to
	DEFAULT_FORWARD_POLICY="ACCEPT"

重新加载 UFW

	$ sudo ufw reload

UFW 的默认规则是阻止了所有的流入流量。如果你希望能够能从另外一台主机连同你的容器，那么就应该允许 Docker 的默认端口（默认2375）的所有连接。

	$ sudo ufw allow 2375/tcp

##Docker的本地DNS警告

正在运行 Ubuntu 或者 Ubuntu 的衍生系统将使用 127.0.0.1 作为 /etc/resolv.conf 的默认域名服务器。NetworkManager 设置 dnsmasq 使用真实的 dns 服务器连接，并且设置 /etc/resolv.conf 的域名服务为 127.0.0.1。

当你启动容器的时候，用户将会看到如下的警告：

	WARNING: Local (127.0.0.1) DNS resolver found in resolv.conf and containers can't use it. Using default external servers : [8.8.8.8 8.8.4.4]

显示这个的原因是因为容器不能使用本地的 dns 服务器，并且 Docker 使用的是默认的外部域名服务器。

我们可以为 docker 进程指定一个 DNS 服务器来使其正常工作

	$ sudo nano /etc/default/docker
	---
	# Add:
	DOCKER_OPTS="--dns 8.8.8.8"
	# 8.8.8.8 could be replaced with a local DNS server, such as 192.168.1.1
	# multiple DNS servers can be specified: --dns 8.8.8.8 --dns 192.168.1.1

重启 docker 进程

	$ sudo restart docker

>警告：如果你使用你的要连接不同的网络，请使用一个公共的 DNS 服务器。

另外一个解决方案就是禁用 NetworkManager 的 dnsmasq，步骤如下：

	$ sudo nano /etc/NetworkManager/NetworkManager.conf
	----
	# 修改:
	dns=dnsmasq
	# 变成
	#dns=dnsmasq

重启 NetworkManager 和 docker

	$ sudo restart network-manager
	$ sudo restart docker

>警告：这可能使 dns 解析速度慢

##Mirrors

你应该 `ping get.docker.io` 来比较镜像服务的延迟，选择最好的一个。

####Yandex

[Yandex](http://yandex.ru/) 是一个俄罗斯的镜像源，其中的 Docker 包每六个小时更新一次，用 `http://mirror.yandex.ru/mirrors/docker/` 来替代 `http://get.docker.io/ubuntu`，举例如下：

	$ sudo sh -c "echo deb http://mirror.yandex.ru/mirrors/docker/ docker main\
    > /etc/apt/sources.list.d/docker.list"
    $ sudo apt-get update
    $ sudo apt-get install lxc-docker
