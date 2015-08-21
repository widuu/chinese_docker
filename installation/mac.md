Mac OS X 安装 Docker
===

> 注意：
> 该 Docker 版本为了支持 Docker 机,于是不再支持 Boot2Docker 命令行。使用 Docker Toolbox 和其它 Docker 工具来安装 Docker 机。

您可以利用 Docker Toolbox 来安装 Docker。Docker Toolbox 提供了以下工具：

* 用于运行 docker-machine 二进制文件的 `Docker Machine`
* 用于运行 docker 二进行文件的 `Docker Engine`
* 用于运行 docker-compose 二进行文件的 `Docker Compose (Mac 特有)`
* Kitematic，Docker 的图形用户界面
* 用于 Docker 命令行环且预先配置好的 shell
* Oracle VM VirtualBox

由于 Docker 的后台程序使用了 Linux 特有的内核特性，所以您不能直接在 OS X 上运行 Docker。相反，您必须使用 `docker-machine` 来创建并附加一台虚拟机（VM）。该虚拟机需要安装 Linux 操作系统以便在您 Mac 机上运行 Docker。

####前提条件

您 Mac 机的 OS X 版本必须大于等于 10.8 "Snow Leopard" 才可以安装 Docker Toolbox。

###在安装之前先来了解一些关键概念

当我们在一台 Linux 主机上安装完 Docker 之后，我们的机器中就包含了本地主机和 Docker 主机。如果从网络层来划分，本地主机就代表你的电脑，而 Docker 主机就是运行 container 的那台机器。

在 Linux 机器上的一种典型安装 Docker 方法：Docker 客户端，Docker 后台程序和 container 会直接运行在您的机器上。这就意味着您可以使用标准的本地主机寻址（例如 `localhost:8000` 或者 `0.0.0.0:8376`）来为 Docker container 分配一个地址。

![linux_docker_host](../images/linux_docker_host.svg)

在 OS X 上安装的 Docker ，其 Docker 后台程序是运行在一个名为 `default` 的 Linux 虚拟机上的。`default` 是 Linux 上的一个轻量级的虚拟机，是专门用于在 Mac OS X 机器上运行 Docker 的。

![mac_docker_host](../images/mac_docker_host.svg)

在 OS X 中，Docker 主机地址就是 Linux 虚拟机的地址。当你使用 `docker-machine` 启动虚拟机的时候，该虚拟机会自动获取到 IP 地址。当您开启一个 container 的时候，container 上的端口会映射到虚拟机的端口上。本页面上的实践操作会一一印证上述内容。

###安装

如果您有一个正在运行着的 VirtualBox，那么请您在运行安装器（Docker Toolbox）之前务必关闭它。

1. 进入 [Docker Toolbox](https://www.docker.com/toolbox) 页面。

2. 点击 Docker Toolbox 下载链接，进行下载。

3. 双击安装包或者通过右击并在快捷菜单中选择“打开”的方式安装 Docker Toolbox。

   此时会弹出“Install Docker Toolbox”（安装 Docker 工具箱）窗口。

   ![mac-welcome-page](../images/mac-welcome-page.png)

4. 点击“继续”（Continue），安装 toolbox。
  
   此时，toolbox 会展现给你一些选项，供您自定义标准安装。
 
   ![mac-page-two](../images/mac-page-two.png)

   默认情况下，标准的 Docker Toolbox 安装是这样的：
   * 向 `/usr/local/bin` 目录中添加 Docker 工具的二进制文件。
   * 修改权限使得这些二进制文件可供任何用户使用。
   * 更新现有的 VirtualBox 安装。
   
   <br/>
   通过点击“自定义”或“更改安装目录”修改这些默认安装值。

5. 点击“安装”，执行标准安装。
 
   系统提示您输入密码。

   ![mac-password-prompt](../images/mac-password-prompt.png)

6. 输入密码，继续安装
   
   安装完成后，Docker Toolbox 会为您提供一些信息，通过这些信息您可以完成一些常见的任务。 

7. 点击“关闭”，离开当前窗口。
  
###运行 Docker Container

想要运行一个 Docker 容器，首先，你需要先启动 `boot2docker` 虚拟机，然后使用 `docker` 命令来加载、运行、管理容器。你可以从你的应用程序文件夹双击启动 `boot2docker`，或者使用命令行来启动。

> 提示： Boot2Docker 是被作为开发工具而设计的，不适用于生产环境中。

###应用程序文件夹

当你从你的“应用程序文件夹(Applications)” 来启动 "Boot2Docker" 程序, 程序会做如下事项：

- 打开一个命令行控制台。
- 创建 $HOME/.boot2docker 目录
- 创建  VirtualBox ISO 虚拟机 和 证书 （ssh key） 
- 启动 VirtualBox 并运行 `docker` 进程

到这里就启动完毕了， 你可以运行 `docker` 命令。你可以运行 `hello-word` 容器来验证你是否安装成功。

		$ docker run hello-world
	    Unable to find image 'hello-world:latest' locally
	    511136ea3c5a: Pull complete
	    31cbccb51277: Pull complete
	    e45a5af57b00: Pull complete
	    hello-world:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
	    Status: Downloaded newer image for hello-world:latest
	    Hello from Docker.
	    This message shows that your installation appears to be working correctly.
	
	    To generate this message, Docker took the following steps:
	     1. The Docker client contacted the Docker daemon.
	     2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
	        (Assuming it was not already locally available.)
	     3. The Docker daemon created a new container from that image which runs the
	        executable that produces the output you are currently reading.
	     4. The Docker daemon streamed that output to the Docker client, which sent it
	        to your terminal.
	
	    To try something more ambitious, you can run an Ubuntu container with:
	     $ docker run -it ubuntu bash
	
	    For more examples and ideas, visit:
	     http://docs.docker.com/userguide/

你可以使用命令行来启动和关闭 `boot2docker` 。

###使用命令行 

使用命令行来初始化和运行 `boot2docker` ，有如下步骤：

1. 创建一个新的 Boot2Docker 虚拟机

		$ boot2docker init

	这会创建一个新的虚拟主机，你只需要运行一次这个命令就可以了，以后就不需要了。

2. 启动 `boot2docker` 虚拟机。

		$ boot2docker start

3. 通过 docker 客户端来查看环境变量

		$ boot2docker shellinit
		Writing /Users/mary/.boot2docker/certs/boot2docker-vm/ca.pem
		Writing /Users/mary/.boot2docker/certs/boot2docker-vm/cert.pem
		Writing /Users/mary/.boot2docker/certs/boot2docker-vm/key.pem
		    export DOCKER_HOST=tcp://192.168.59.103:2376
		    export DOCKER_CERT_PATH=/Users/mary/.boot2docker/certs/boot2docker-vm
		    export DOCKER_TLS_VERIFY=1

	 每台机器的具体路径和地址可能都不相同。

4. 使用 shell 命令来设置环境变量。

		$ eval "$(boot2docker shellinit)"

5.	运行 `hello-word` 容器来验证安装。

		$ docker run hello-world、

###Boot2Docker 基本练习

这一部分，需要你提前运行 `boot2docker` 并初始化 `docker` 客户端环境。你可以运行下边的命令来验证：

	$ boot2docker status
	$ docker version  

本节我们通过使用 `boot2docker` 虚拟机来创建一些容器任务

####容器端口访问

1. 在 Docker 主机上启动一个 Nginx 容器。

		$ docker run -d -P --name web nginx

	一般来说，`docker run` 命令会启动一个容器，运行这个容器，然后退出。`-d` 标识可以让容器在 `docker run ` 命令完成之后继续在后台运行。 `-P` 标识会将容器的端口暴露给主机，这样你就可以从你的 MAC 上访问它。

2. 使用 `docker ps` 命令来查看你运行的容器

		CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                                           NAMES
		5fb65ff765e9        nginx:latest        "nginx -g 'daemon of   3 minutes ago       Up 3 minutes        0.0.0.0:49156->443/tcp, 0.0.0.0:49157->80/tcp   web

	通过这一点我们可以看出 `nginx` 作为一个进程运行。

3. 查看容器端口

		$ docker port web
		443/tcp -> 0.0.0.0:49156
		80/tcp -> 0.0.0.0:49157

	上边的显示告诉我们，`web` 容器将 80 端口映射到 Docker 主机的 49157 端口上。


4. 在浏览器输入地址 `http://localhost:49157` (localhost 是 0.0.0.0):

 	![bad_host.png](../images/bad_host.png)


 	没有正常工作。没有正常工作的原因是 `DOCKER_HOST` 主机的地址不是 localhost (0.0.0.0),但是你可以使用 `boot2docker` 虚拟机的IP地址来访问。


5. 获取 `boot2docker` 主机地址

		$ boot2docker ip
		192.168.59.103

6. 在浏览器中输入 `http://192.168.59.103:49157`

	![good_host.png](../images/good_host.png)

	成功运行！

7. 通过如下方法，停止并删除 `nginx` 容器。

		$ docker stop web
		$ docker rm web

###给容器挂载一个卷

当你启动 `boot2docker` 的时候，它会自动共享 `/Users` 目录给虚拟机。你可以利用这一点，将本地目录挂载到容器中。这个练习中我们将告诉你如何进行操作。

1. 回到你的 $HOME 目录

		$ cd $HOME

2. 创建一个新目录，并命名为 `site`

		$ mkdir site

3. 进入 `site` 目录。

		$ cd site

4. 创建一个 `index.html` 文件。
	
		$ echo "my new site" > index.html

5. 启动一个新的 `nginx` 容器,并将本地的 `site` 目录替换容器中的 `html` 文件夹。

		$ docker run -d -P -v $HOME/site:/usr/share/nginx/html --name mysite nginx

6. 获取 `mysite` 容器端口

		$ docker port mysite
		80/tcp -> 0.0.0.0:49166
		443/tcp -> 0.0.0.0:49165

7. 在浏览器中打开站点。

	![newsite_view.png](../images/newsite_view.png)

8. 现在尝试在  `$HOME/site` 中创建一个页面

		$ echo "This is cool" > cool.html

9.  在浏览器中打开新创建的页面。

	![cool_view.png](../images/cool_view.png)

10. 停止并删除 `mysite` 容器。

		$ docker stop mysite
		$ docker rm mysite  

###升级 Boot2Docker

如果你现在运行的是1.4.1及以上版本 Boot2Docker , 你可以使用命令行来升级 Boot2Docker 。如果你运行的是老版本，你需要使用 `boot2docker` 仓库提供的包来升级。

####命令行操作

你可以参照下边的操作来升级1.4.1以上版本：

1. 在你的机器中打开命令行。

2. 停止 `boot2docker` 应用。

		$ boot2docker stop

3. 执行升级命令。
	
		$ boot2docker upgrade

####安装包方式升级

下边的操作可以升级任何版本的 Boot2Docker:
   
1. 在你的机器中打开命令行。

2. 停止 `boot2docker` 应用。

		$ boot2docker stop

3. 打开 [boot2docker/osx-installer](https://github.com/boot2docker/osx-installer/releases/latest)  发布页。

4. 在下载部分点击 `Boot2Docker-x.x.x.pkg` 来下载 Boot2Docker。

5. 双击安装 Boot2Docker 包。

	将 Boot2Docker 拖放到应用程序文件夹。

###学习更多的知识

使用 `boot2docker help ` 列出完整的命令行参考列表。更多关于使用 SSH 或 SCP 来访问 Boot2Docker 虚拟机的文档，请查看 [Boot2Docker repository](https://github.com/boot2docker/boot2docker) 。

继续阅读[用户指南](../userguide/README.md)
