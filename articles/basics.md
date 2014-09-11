使用docker第一步
===

###检查你的Docker是否安装

本指南假设你已经完成了Docker的安装工作。检查你安装的Docker,运行以下命令：

	# Check that you have a working install
	$ docker info

如果你得到 `docker: command not found`，你可能没有完整的安装上Docker。

如果你得到 `/var/lib/docker/repositories: permission denied` ，那你可能没有权限访问你主机上的Docker。

为获得访问Docker权限可以直接在命令前加`sudo`，或者采取以下步骤授予权限：：

	# 如果还没有docker group就添加一个：
	$ sudo groupadd docker
	# 将用户加入该group内。然后退出并重新登录即可生效。
	$ sudo gpasswd -a ${USER} docker
	# 重启docker
	$ sudo service docker restart

请参考[安装指南](../install/)安装。

###下载预构建镜像

	# Download an ubuntu image
	$ sudo docker pull ubuntu

这将从Docker Hub中查找到 `Ubuntu` 镜像，并且从Docker Hub中下载镜像到本地缓存中。

>提示：当镜像下载成功之后，你将会看到镜像的短id一个12字符的hash, `539c0211cd76: Download complete`，这些短的镜像IDS是完整ID的前12个字符。你可以使用 `docker inspect`和 `docker images --no-trunc=true`来查看完整ID。

如果你使用OS X，你可以使用 `sudo`。

###运行交互shell

	# 在ubuntu镜像中使用运行交互shell,
	# 分配一个控制台,分配输入输出流
	# To detach the tty without exiting the shell,
	# use the escape sequence Ctrl-p + Ctrl-q
	# note: This will continue to exist in a stopped state once exited (see "docker ps -a")
	$ sudo docker run -i -t ubuntu /bin/bash

###绑定Docker到另外的主机/端口或者Unix socket

>提示：修改默认的 docker 进程绑定到一个tcp端口或者 Unix docker群组，这将会增加你的安全风险，允许非 root 用户获得 root 访问的主机权限。请确保你的 `docker` 权限。如果你绑定一个 `TCP` 端口任何人可以通过这个端口来访问你的 `Docker`,所以它不应该放在一个开放的网络中。

使用 `-H` 标记可以使 `Docker` 来监听指定的IP和端口。默认情况下，它将监听 `unix:///var/run/docker.sock`只允许本地root用户连接。你可以将它设置为 `0.0.0.0:2375`或者指定的主机IP来给所有人访问权限。但是这不推荐，因为这样普通用户获得主机上运行进程的root权限。

同样，Docker 客户端可以使用 `-H` 来连接指定的主机端口。

`-H` 使用以下格式来分配主机和端口

	tcp://[host][:port]` or `unix://path

举例：

+ tcp://host:2375 -> TCP connection on host:2375
+ unix://path/to/socket -> Unix socket located at path/to/socket

`-H`，当输入为空的时候，将默认为相同的原始值，即没有`-H`输入。
`-H`也接受短形式的TCP绑定。

	host[:port]` or `:port

进程模式运行Docker

	$ sudo <path to>/docker -H 0.0.0.0:5555 -d &

下载`Ubuntu`镜像：

	sudo docker -H :5555 pull ubuntu

你可以使用多个`-H`标记，例如，你想要同时监听TCP和Unix socket。

	# 进程模式下运行docker
	$ sudo <path to>/docker -H tcp://127.0.0.1:2375 -H unix:///var/run/docker.sock -d &
	# 使用默认的unix socker来下载ubuntu镜像
	$ sudo docker pull ubuntu
	# 或者使用TCP端口
	$ sudo docker -H tcp://127.0.0.1:2375 pull ubuntu

####开始一个长时间运行的工作进程

	# 开始一个非常有用的长时间运行的进程
	$ JOB=$(sudo docker run -d ubuntu /bin/sh -c "while true; do echo Hello world; sleep 1; done")
	
	# 到目前为止收集的输出工作
	$ sudo docker logs $JOB
	
	# 关闭这项进程
	$ sudo docker kill $JOB

####列出容器

	$ sudo docker ps # Lists only running containers
	$ sudo docker ps -a # Lists all containers
	$ sudo docker ps -l # List the last running container

####控制容器

	# 开始一个新的容器
	$ JOB=$(sudo docker run -d ubuntu /bin/sh -c "while true; do echo Hello world; sleep 1; done")
	
	# 停止容器
	$ docker stop $JOB
	
	# 开始容器
	$ docker start $JOB
	
	# 重启容器
	$ docker restart $JOB
	
	# 杀死一个工作
	$ docker kill $JOB
	
	# 删除一个容器
	$ docker stop $JOB # Container must be stopped to remove it
	$ docker rm $JOB


####绑定服务到TCP端口


	#让容器绑定4444端口，并通知netcat监听它。
	$ JOB=$(sudo docker run -d -p 4444 ubuntu:12.10 /bin/nc -l 4444)
	
	# 查看容器转发的公共端口
	$ PORT=$(sudo docker port $JOB 4444 | awk -F: '{ print $2 }')
	
	# 连接这个公共端口
	$ echo hello world | nc 127.0.0.1 $PORT
	
	# 确认网络连接工作
	$ echo "Daemon received: $(sudo docker logs $JOB)"

####提交（存储）一个容器的状态

保存你镜像容器的状态，这也就可以重复利用。

当你提交你的容器，仅仅是镜像和容器之间的不同，从创建到容器的当前状态来存储（作为差异）。你可以使用 `docker images`来查看已有镜像。

	# 使用新的名称来提交你的镜像
	$ sudo docker commit <container_id> <some_name>
	
	# 列出你的容器
	$ sudo docker images

你现在有一个镜像的状态，你可以创建新的实例。

####删除Docker镜像

首先要保证有权限对Docker镜像或者容器进行操作，具体做法参见进入前文安装部分。

	# 停止所有容器
	$ docker stop $(docker ps -a -q)
	
	# 删除指定镜像
	$ docker rmi $image
	
	# 删除无标示镜像，即id为<None>的镜像
	$ docker rmi $(docker images | grep "^<none>" | awk "{print $3}")
	
	# 删除所有镜像
	$ docker rmi $(docker images -q)


