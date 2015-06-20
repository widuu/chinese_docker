Ubuntu
===

Docker 支持以下的 Ubuntu 版本

+ Ubuntu Trusty 14.04 (LTS) (64-bit)
+ Ubuntu Precise 12.04 (LTS) (64-bit)
+ Ubuntu Raring 13.04 and Saucy 13.10 (64 bit)

这个页面可以指导你安装 Docker 包管理器，并了解其中的安装机制。通过下边的安装方式可以确保你获取的是最新版本的 Docker。如果你想要使用 'Ubuntu包管理器' 安装，你可以查阅你的 Ubuntu 文档。

###前提条件

Docker 需要在64位版本的Ubuntu上安装。此外，你还需要保证你的 Ubuntu 内核的最小版本不低于 3.10，其中3.10 小版本和更新维护版也是可以使用的。

在低于3.10版本的内核上运行 Docker 会丢失一部分功能。在这些旧的版本上运行 Docker 会出现一些BUG，这些BUG在一定的条件里会导致数据的丢失，或者报一些严重的错误。

打开控制台使用 `uname -r`命令来查看你当前的内核版本。

	$ uname -r 
	3.11.0-15-generic
>Docker 要求 Ubuntu 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的Ubuntu版本是否支持 Docker 。

###Trusty 14.04

 这个版本不需要考虑前提条件

###Precise 12.04 (LTS)

对于Ubuntu Precise版本, 安装Docker需要内核在3.13及以上版本。如果你的内核版本低于3.13你需要升级你的内核。 通过下边的表，请查阅下边的表来确认你的环境需要哪些包。

 <table class="tg"> <tr> <td
class="tg-031">linux-image-generic-lts-trusty</td> <td class="tg-031e">Generic
Linux kernel image. This kernel has AUFS built in. This is required to run
Docker.</td> </tr> <tr> <td class="tg-031">linux-headers-generic-lts-trusty</td>
<td class="tg-031e">Allows packages such as ZFS and VirtualBox guest additions
which depend on them. If you didn't install the headers for your existing
kernel, then you can skip these headers for the"trusty" kernel. If you're
unsure, you should include this package for safety.</td> </tr> <tr> <td
class="tg-031">xserver-xorg-lts-trusty</td> <td class="tg-031e"
rowspan="2">Optional in non-graphical environments without Unity/Xorg.
<i>Required</i> when running Docker on machine with a graphical environment.

<p>To learn more about the reasons for these packages, read the installation
instructions for backported kernels, specifically the <a
href="https://wiki.ubuntu.com/Kernel/LTSEnablementStack" target="_blank">LTS
Enablement Stack</a> &mdash; refer to note 5 under each version.</p></td> </tr>
<tr> <td class="tg-031">libgl1-mesa-glx-lts-trusty</td> </tr> </table> &nbsp;

通过下边的操作来升级你的内核和安装额外的包

1. 在Ubuntu系统中打开命令行控制台。

2. 升级你的包管理器

		$ sudo apt-get update

3. 安装所有必须和可选的包

		$ sudo apt-get install linux-image-generic-lts-trusty

	根据个人的系统环境来选择是否安装更多的包（前表列出）。

4. 重启系统

		$ sudo reboot

5. 等到系统重启成功之后，查看[安装Docker](#Ubuntu安装Docker)

###Saucy 13.10 (64 bit)

Docker 使用 AUFS 作为默认的后端存储方式，如果你之前没有安装 AUFS ，Docker 在安装过程中会自动添加。

##Ubuntu安装Docker

首先要确认你的 Ubuntu 版本是否符合安装 Docker 的前提条件。如果没有问题，你可以通过下边的方式来安装 Docker ：

1. 使用具有`sudo`权限的用户来登录你的Ubuntu。

2. 查看你是否安装了`wget`

		$ which wget

	如果`wget`没有安装，先升级包管理器，然后再安装它。
		
		$ sudo apt-get update $ sudo apt-get install wget

3. 获取最新版本的 Docker 安装包

		$ wget -qO- https://get.docker.com/ | sh

	系统会提示你输入`sudo`密码，输入完成之后，就会下载脚本并且安装Docker及依赖包。

4. 验证 Docker 是否被正确的安装

		$ sudo docker run hello-world
	
 	上边的命令会下载一个测试镜像，并在容器内运行这个镜像。


##Ubuntu Docker可选配置

这部分主要介绍了 Docker 的可选配置项，使用这些配置能够让 Docker 在 Ubuntu 上更好的工作。


- 创建 Docker 用户组
- 调整内存和交换空间(swap accounting) 
- 启用防火墙的端口转发(UFW)
- 为 Docker 配置DNS服务

###创建 Docker 用户组

docker 进程通过监听一个 Unix Socket 来替代 TCP 端口。在默认情况下，docker 的 Unix Socket属于`root`用户，当然其他用户可以使用`sudo`方式来访问。因为这个原因， docker 进程就一直是`root`用户运行的。

为了在使用 `docker` 命令的时候前边不再加`sudo`，我们需要创建一个叫 `docker` 的用户组，并且为用户组添加用户。然后在 `docker` 进程启动的时候，我们的 `docker` 群组有了 Unix Socket 的所有权，可以对 Socket 文件进行读写。

>注意：`docker` 群组就相当于root用户。有关系统安全影响的细节，请查看 [Docker 进程表面攻击细节]()

创建 `docker` 用户组并添加用户

1. 使用具有`sudo`权限的用户来登录你的Ubuntu。

	在这过程中，我们假设你已经登录了Ubuntu。

2. 创建 `docker` 用户组并添加用户。

		$ sudo usermod -aG docker ubuntu

3. 注销登录并重新登录

	这里要确保你运行用户的权限。

4. 验证 `docker` 用户不使用 `sudo` 命令开执行 `Docker`

		$ docker run hello-world

###调整内存和交换空间(swap accounting)

当我们使用 Docker 运行一个镜像的时候，我们可能会看到如下的信息提示：

	WARNING: Your kernel does not support cgroup swap limit. WARNING: Your
	kernel does not support swap limit capabilities. Limitation discarded.、

为了防止以上错误信息提示的出现，我们需要在系统中启用内存和交换空间。我们需要修改系统的 GUN GRUB (GNU GRand Unified Bootloader) 来启用内存和交换空间。开启方法如下：

1. 使用具有`sudo`权限的用户来登录你的Ubuntu。

2. 编辑 `/etc/default/grub` 文件

3. 设置 `GRUB_CMDLINE_LINUX` 的值如下：

		GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"

4. 保存和关闭文件 

5. 更新 GRUB

		$ sudo update-grub

6. 重启你的系统。

### 允许UFW端口转发   

当你在运行 `docker` 的宿主主机上使用UFW（简单的防火墙）。你需要做一些额外的配置。Docker 使用桥接的方式来管理网络。默认情况下，UFW 过滤所有的端口转发策略。因此，当在UFW启用的情况下使用 `docker` ,你必须适当的设置UFW的端口转发策略。

默认情况下UFW是过滤掉所有的入站规则。如果其他的主机能够访问你的容器。你需要允许Docker的默认端口(2375)的所有连接。

设置 UFW 允许Docker 端口的入站规则：

1. 使用具有`sudo`权限的用户来登录你的Ubuntu。
2. 验证UFW的安装和启用状态

		$ sudo ufw status

3. 打开和编辑`/etc/default/ufw`文件 
	
		$ sudo nano /etc/default/ufw

4. 设置 `DEFAULT_FORWARD_POLICY ` 如下：

	DEFAULT_FORWARD_POLICY="ACCEPT"

5. 保存关闭文件。

6. 重新加载UFW来使新规则生效。

		$ sudo ufw reload

7. 允许 Docker 端口的入站规则

		$ sudo ufw allow 2375/tcp


###Docker 配置 DNS 服务

无论是Ubuntu还是Ubuntu 桌面繁衍版在系统运行的时候都是使用`/etc/resolv.conf`配置文件中的127.0.0.1作为域名服务器(nameserver)。NetworkManager设置dnsmasq使用真实的dns服务器连接，并且设置 /etc/resolv.conf的域名服务为127.0.0.1。

在桌面环境下使用这些配置来运行 docker 容器的时候， Docker 用户会看到如下的警告：

	WARNING: Local (127.0.0.1) DNS resolver found in resolv.conf and containers
	can't use it. Using default external servers : [8.8.8.8 8.8.4.4]

该警告是因为 Docker 容器不能使用本地的DNS服务。相反 Docker 使用一个默认的外部域名服务器。

为了避免此警告，你可以给 Docker 容器指定一个DNS服务器。或者你可以禁用 NetworkManager 的 `dnsmasq`。不过当禁止 `dnsmasq` 可能使某些网络的DNS解析速度变慢。

为 Docker 指定一个DNS服务器

1. 使用具有`sudo`权限的用户来登录你的Ubuntu。

2. 打开并编辑 `/etc/default/docker`

		$ sudo nano /etc/default/docker

3. 添加设置 

		DOCKER_OPTS="--dns 8.8.8.8"

	使用8.8.8.8替换如192.168.1.1的本地DNS服务器。你可以指定多个DNS服务器，多个DNS服务器使用空格分割例如

		--dns 8.8.8.8 --dns 192.168.1.1

	>警告:如果你正在使用的电脑需要连接到不同的网络,一定要选择一个公共DNS服务器。

4. 保存关闭文件。 

5. 重启 Docker 进程 

		$ sudo restart docker  

或者，作为替代先前的操作过程，禁止NetworkManager中的`dnsmasq `(这样会使你的网络变慢)

1. 打开和编辑 `/etc/default/docker`

		$ sudo nano /etc/NetworkManager/NetworkManager.conf

2. 注释掉 dns = dsnmasq：

		dns=dnsmasq

3. 保存关闭文件

4. 重启NetworkManager 和 Docker

		$ sudo restart network-manager $ sudo restart docker



###升级Docker

在`wget`的时候使用`-N`参数来安装最新版本的Docker：

	$ wget -N https://get.docker.com/ | sh

