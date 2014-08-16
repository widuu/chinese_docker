Ubuntu
===

Docker支持以下的ubuntu版本

+ Ubuntu Trusty 14.04 (LTS) (64-bit)
+ Ubuntu Precise 12.04 (LTS) (64-bit)
+ Ubuntu Raring 13.04 and Saucy 13.10 (64 bit)

如何你打算使用UFW防火墙，请阅读docker和UFW

##Ubuntu Trusty 14.04 (LTS) (64-bit)

Ubuntu配置了3.13.0 linux内核和将安装Docker 0.9.1的`docker.io`包，当然这些都取决于你的Ubuntu镜像源。

>提示：Ubuntu(和Debain)包含一个特别陈旧的KED3/GNOME2包叫docker,所以我们把这个包叫`docker.io`。

###安装

安装最新版本的Ubuntu包（可能不是最新的docker版本包）：

	$ sudo apt-get update
	$ sudo apt-get install docker.io
	$ sudo ln -sf /usr/bin/docker.io /usr/local/bin/docker
	$ sudo sed -i '$acomplete -F _docker docker' /etc/bash_completion.d/docker.io

如果你想安装最新版本的docker

首先，检查你的APT系统能够处理https的url：如果你的主机不存在/usr/lib/apt/methods/https文件，请先安装apt-transport-https包

	[ -e /usr/lib/apt/methods/https ] || {
	  apt-get update
	  apt-get install apt-transport-https
	}

然后，添加docker镜像秘钥到你的本地秘钥库.

	$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9

添加Docker镜像添加你的apt软件源,更新和安装`lxc-docker`包。
你可能会受到一个警告信息，这个包不可信，输入yes继续安装。

	$ sudo sh -c "echo deb https://get.docker.io/ubuntu docker main\
	> /etc/apt/sources.list.d/docker.list"
	$ sudo apt-get update
	$ sudo apt-get install lxc-docker

>提示：
>这还有一个简单的`curl`脚本来协助你完成这一工作。
	
	$ curl -s https://get.docker.io/ubuntu/ | sudo sh

来确认它是否正常工作:

	$ sudo docker run -i -t ubuntu /bin/bash

上边的命令会自动下载ubuntu镜像，并且会在容器内执行bash

##Ubuntu Precise 12.04 (LTS) (64-bit)

###依赖条件

Linux kernel 3.8

由于LXC存在的bug问题，docker最好的工作环境是在3.8内核基础上。但是12.04使用的是3.2内核，所以我们需要升级它。安装附带AUFS的内核需要遵循以下步骤。我们需要包含通用的头文件取决于启用的包，像ZFS和VirtualBox guest additions。如果你的"precise"内核中没有安装这些头文件，这时候你可以在"raring"内核跳过这些头文件。如果你不确定的时候包含这些是比较安全的。

	# install the backported kernel
	$ sudo apt-get update
	$ sudo apt-get install linux-image-generic-lts-raring linux-headers-generic-lts-raring
	
	# reboot
	$ sudo reboot

###安装

>警告：这些指令在0.6版本的时候就已经改变了，如果你要从早期版本升级，你需要重新安装他们。

Docker作为一个Debain包，安装起来比较简单，如果你不在美国请选择镜像源，其它镜像源的Debain包安装起来也许会更快（譬如我们使用俄罗斯的镜像）。

首先，检查你的APT系统能够处理https的url：如果你的主机不存在/usr/lib/apt/methods/https文件，请先安装apt-transport-https包

	[ -e /usr/lib/apt/methods/https ] || {
	  apt-get update
	  apt-get install apt-transport-https
	}

然后，添加docker镜像秘钥到你的本地秘钥库.

	$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9

添加Docker镜像添加你的apt软件源,更新和安装`lxc-docker`包。
你可能会受到一个警告信息，这个包不可信，输入yes继续安装。

	$ sudo sh -c "echo deb https://get.docker.io/ubuntu docker main\
	> /etc/apt/sources.list.d/docker.list"
	$ sudo apt-get update
	$ sudo apt-get install lxc-docker

>提示：
>这还有一个简单的`curl`脚本来协助你完成这一工作。
	
	$ curl -s https://get.docker.io/ubuntu/ | sudo sh

来确认它是否正常工作:

	$ sudo docker run -i -t ubuntu /bin/bash

上边的命令会自动下载ubuntu镜像，并且会在容器内执行bash,输入`exit`来退出。

现在！可以查看用户指南。

###Ubuntu Raring 13.04 and Saucy 13.10 (64 bit)

这些指令可以再Ubuntu 13.04和13.10上使用

###依赖条件

可选AUFS文件系统支持

Ubuntu Raring已经安装好了3.8内核，所以我们不需要安装它。然而，不是所有的系统都支持AUFS系统。在版本0.7上AUFS是可选的，它作为一个驱动程序，我们建议使用它。

为了确保安装AUFS，运行以下命令：

	$ sudo apt-get update
	$ sudo apt-get install linux-image-extra-`uname -r`

###安装

Docker作为一个Debain包，安装起来比较简单

>警告：这些指令在0.6版本的时候就已经改变了，如果你要从早期版本升级，你需要重新安装他们。

首先，添加docker镜像秘钥到你的本地秘钥库.

	$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9

添加docker镜像添加到你的apt软件源，升级和安装`lxc-docker`包。

	$ sudo sh -c "echo deb http://get.docker.io/ubuntu docker main\
	> /etc/apt/sources.list.d/docker.list"
	$ sudo apt-get update
	$ sudo apt-get install lxc-docker

现在下载`ubuntu`镜像运行，确认是否正常工作
	
	$ sudo docker run -i -t ubuntu /bin/bash

输入`exit`退出

好！现在你可以去查看用户指南了。

###不使用root运行

`docker`进程一般来说默认用`root`用户运行，从`Docker 0.5.2`开始，docker进程绑定unix socket来代替TCP端口。默认情况下用户`root`来管理unix socket,但是你也可以使用`sudo`来使用。

从0.5.3版本开始，如果你（你安装的docker）创建一个叫`docker`的unix群组，并且在群组中添加用户。当进程启动的时候，docker群组将有`docker`进程unix socket的读/写使用权。docker进程必须使用root用户运行，但是当使用docker群组的一个用户来运行docker客户端的时候，你不需要在命令前添加`sudo`，从docker 0.9.0版本开始你可以使用`-G`标记指定用户组。

>警告：docker用户组（或者用`-G`指定的用户组）和root等效，

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

使用`apt-get`指令来安装最新版本的docker

	# update your sources list
	$ sudo apt-get update
	
	# install the latest
	$ sudo apt-get install lxc-docker

###内存和交换空间

如果你想使用内存和交换空间，你必须在命令行中添加如下的内核参数:

	cgroup_enable=memory swapaccount=1

系统上使用GRUB（ubuntu默认使用GRUB），你可以编辑/etc/default/grub这个文件，找到GRUB_CMDLINE_LINUX，像如下

	GRUB_CMDLINE_LINUX=""

并且替换成

	GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"

再运行`sudo update-grub`并且重启

这些参数将会使你摆脱如下的警告信息：

	WARNING: Your kernel does not support cgroup swap limit.
	WARNING: Your kernel does not support swap limit capabilities. Limitation discarded.

###故障排除

在Linux Mint,`cgroup-lite`默认没有被安装，在docker正常工作之前，你可以通过如下的方式安装：

	$ sudo apt-get update && sudo apt-get install cgroup-lite

###Docker and UFW

docker使用桥接的方式来管理网络，默认情况下，UFW会阻止所有的流量转发。你需要允许UFW的转发：

	$ sudo nano /etc/default/ufw

	# Change:
	# DEFAULT_FORWARD_POLICY="DROP"
	# to
	DEFAULT_FORWARD_POLICY="ACCEPT"

重新加载UFW

	$ sudo ufw reload

UFW的默认规则是阻止了所有的流入流量。如果你希望能够能从另外一台主机连同你的容器，那么就应该允许docker的默认端口（默认2375）的所有连接。

	$ sudo ufw allow 2375/tcp

###docker的本地DNS警告

正在运行Ubuntu或者Ubuntu的衍生系统将使用127.0.0.1作为/etc/resolv.conf的默认域名服务器。NetworkManager设置dnsmasq使用真实的dns服务器连接，并且设置 /etc/resolv.conf的域名服务为127.0.0.1。

当你启动容器的时候，用户将会看到如下的警告：

	WARNING: Local (127.0.0.1) DNS resolver found in resolv.conf and containers can't use it. Using default external servers : [8.8.8.8 8.8.4.4]

显示这个的原因是因为容器不能使用本地的dns服务器，并且docker使用的是默认的外部域名服务器。

我们可以为docker进程指定一个DNS服务器来使其正常工作

	$ sudo nano /etc/default/docker
	---
	# Add:
	DOCKER_OPTS="--dns 8.8.8.8"
	# 8.8.8.8 could be replaced with a local DNS server, such as 192.168.1.1
	# multiple DNS servers can be specified: --dns 8.8.8.8 --dns 192.168.1.1

重启docker进程

	$ sudo restart docker

>警告：如果你使用你的要连接不同的网络，请使用一个公共的DNS服务器。

另外一个解决方案就是禁用NetworkManager的dnsmasq，步骤如下：

	$ sudo nano /etc/NetworkManager/NetworkManager.conf
	----
	# 修改:
	dns=dnsmasq
	# 变成
	#dns=dnsmasq

重启NetworkManager和docker

	$ sudo restart network-manager
	$ sudo restart docker

>警告：这可能使dns解析速度慢

###Mirrors

你应该`ping get.docker.io` 比较镜像的延迟，选择最好的一个。

####Yandex

Yandex是一个俄罗斯的镜像源，其中的docker包每六个小时更新一次，用http://mirror.yandex.ru/mirrors/docker/来替代http://get.docker.io/ubuntu，如下：

	$ sudo sh -c "echo deb http://mirror.yandex.ru/mirrors/docker/ docker main\
    > /etc/apt/sources.list.d/docker.list"
    $ sudo apt-get update
    $ sudo apt-get install lxc-docker
