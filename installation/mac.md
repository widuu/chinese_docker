Mac OS X 安装 Docker
===

 你可以使用 Boot2Docker 来安装 Docker ，然后在命令行运行 `docker`。如果你对命令行比较熟悉或者你打算在 Github 上贡献 Docker 项目，那么你就可以选择此安装方式。
 
 或者，你可以使用 [Kitematic](https://kitematic.com/) , 它是一款图形界面的应用程序（GUI），你可以通过图形界面来轻松的设置 Docker 和运行容器。
 
 <a id="graphic" href="https://kitematic.com/" target="_blank"><img
src="../images/kitematic.png" alt="Download Kitematic"></a>

###Command-line Docker with Boot2Docker

因为 Docker 进程使用的是 Linux 内核特性，所以你不能在原生的 OS X 中安装 Docker，如果你需要安装 Docker ，你必须安装 `Boot2Docker`。 这个程序中包含了 `VirtualBox` 虚拟主机(VM), Docker 和 Boot2Docker 管理工具。

Boot2Docker 是专门为OS X上运行 Docker 而开发的一个轻量级的虚拟主机管理工具。当Virtual Box在内存中启动后，它会下载一个大约 24MB 的 ISO文件（boot2docker.iso），下载完成后，大约5S中就会启动了。

###前提条件

你的 OS X 版本必须大于等于 10.6 "Snow Leopard" 才可以运行 Boot2Docker 。

###在安装之前了解一些概念

当我们在一台 Linux 主机上安装完 Docker 之后，我们的机器中就包含了本地主机和 Docker 主机。如果从网络层来划分，本地主机就代表你的电脑，而 Docker 主机就代表你运行的容器。

在一个典型的 Linux 主机上安装 Docker 客户端，运行 Docker daemon ，并且在本地主机上直接运行一些容器。这就意味着你可以为 Docker 容器指定本地主机端口，例如 `localhost:8000` 或者 `0.0.0.0:8376`。

![linux_docker_host](../images/linux_docker_host.png)

在 OS X 上安装的 Docker ， `docker` 进程是通过 Boot2Docker 在 Linux 虚拟主机上运行的。

![mac_docker_host](../images/mac_docker_host.png)

在 OS X 中，Docker 主机地址就是 Linux 虚拟主机地址。当你启动 `boot2docker` 进程的时候，虚拟主机就是为它指定IP。在 `boot2docker` 下运行的虚拟主机，通过端口映射的方式将端口映射到虚拟主机上。通过你可以通过本页面上的操作实践来体会到这一点。

###安装Docker

1. 点击进入[boot2docker/osx-installer release](https://github.com/boot2docker/osx-installer/releases/latest)页面。

2. 在下载页面中点击 `Boot2Docker-x.x.x.pkg` 来下载 Boot2Docker。

3. 双击安装包来安装 Boot2Docker

	将 Boot2Docker 放到你的 "应用程序（Applications）" 文件夹
	
安装程序会将 `docker` 和 `boot2docker` 二进制包放到 `/usr/local/bin` 文件夹下。

###启动 Boot2Docker 程序


