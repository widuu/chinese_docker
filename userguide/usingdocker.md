使用容器
===

在上一节的用户指南，我们开始了我们的第一个容器。我们使用`docker run`命令启用了两个容器

- 我们在前台进行容器交互
- 在后台进程运行容器

在这个过程中我们了解了几个docker命令：

- `docker ps` 列出容器
- `docker logs`显示容器的标准输出
- `docker stop`停止正在运行的容器

>提示：我们有另外一种方法来学习`docker`命令，与[用户交互](https://www.docker.com/tryit/)

`docker`客户端非常简单。你可以使用docker的每一个标示和参数组合来进行你的操作。

	# Usage:  [sudo] docker [flags] [command] [arguments] ..
	# Example:
	$ docker run -i -t ubuntu /bin/bash

这时候我们使用`docker version`命令来返回安装的docker客户端和进程信息。

这个命令不仅返回了您使用的docker客户端版本信息，还返回了docker的编程语言GO的版本信息。

	Client version: 0.8.0
	Go version (client): go1.2
	
	Git commit (client): cc3a8c8
	Server version: 0.8.0
	
	Git commit (server): cc3a8c8
	Go version (server): go1.2
	
	Last stable version: 0.8.0

###看看docker客户端能做什么

我们可以通过只输入`docker`没有任何其它选项来查看docker客户端所有的命令。

	$ sudo docker

您将看到所有当前可用列表：

	Commands:
     attach    Attach to a running container
     build     Build an image from a Dockerfile
     commit    Create a new image from a container's changes
	 . . .

###docker命令使用

你可以更深入的去了解docker命令的使用。

试着输入Docker [command]，这里会看到docker命令的使用方法：

	$ sudo docker attach
	Help output . . .

或者你可以再docker命令中使用`--help`标示

	$ sudo docker images --help

这将显示所有的描述信息和可用的标示：

	Usage: docker attach [OPTIONS] CONTAINER

	Attach to a running container
	
	  --no-stdin=false: Do not attach stdin
	  --sig-proxy=true: Proxify all received signal to the process (even in non-tty mode)

>注意：你可以看到一个完整的docker命令列表。

###在Docker中运行一个web应用

现在我们已经学习了更多的docker命令，我们需要学习在容器中运行更多重要的事情。到目前为止我们已经运行的容器没有什么特别用处。让我们通过在docker运行一个web应用程序实例来了解。

在我们的web应用中，我们将运行一个python应用。让我们先从`docker run`命令开始。

	$ sudo docker run -d -P training/webapp python app.py

让我们来回顾一下我们的命令。我们指定了`-d`和`-P`两个标示。我们已经知道的是`-d`标示是让docker容器在后台运行。新的`-P`标示通知Docker所需的网络端口映射从主机映射到我们的容器内。现在让我们看看我们的web应用程序。

我们指定了`training/web`镜像。这个预先建立好的镜像被我们创建后就已经包含了简单的python应用程序环境。

最好，我们指定一个容器来运行：`python`

>注意：你可以在[命令参考](http://docs.docker.com/reference/commandline/cli/#run)和[Docker run参考](http://docs.docker.com/reference/run/)看到更多`docker run`命令的细节

###查看web应用容器

现在我们使用`docker ps`查看我们正在运行的容器。

	$ sudo docker ps -l
	CONTAINER ID  IMAGE                   COMMAND       CREATED        STATUS        PORTS                    NAMES
	bc533791f3f5  training/webapp:latest  python app.py 5 seconds ago  Up 2 seconds  0.0.0.0:49155->5000/tcp  nostalgic_morse

你可以看到我们在`docker ps`中已经指定了新的标示`-l`。这通知`docker ps`命令返回最后的容器的状态。

>注意：默认情况下，`docker ps`命令只显示运行中的容器。如果你还想看已经停止的容器，请加上`-a`标示。

我们可以看到一些细节，与我们第一次运行`docker ps`命令的时候相比，这里多了一个重要的列`PORTS`。

	PORTS
	0.0.0.0:49155->5000/tcp

我们通过`docker run`中使用`-P`标示。Docker中开放映射到我们主机的端口。

>注意：当我们学习如果构建镜像的时候，我们将了解更多关于如何使用docker端口镜像的内容。

在这种情况下，docker开放了5000端口（默认Python端口）映射到主机端口49155上。

Docker可以配置绑定网络端口。在最后一个例子中`-P`标示，是`-p 5000`的快捷方式，`-p 5000`可以使端口5000映射到外部的端口(49000到49900端口)。我们也可以指定`-p`标示来指定端口。举例：

	$ sudo docker run -d -p 5000:5000 training/webapp python app.py

他将容器的5000端口映射到我们本地主机5000端口.现在你可能会问：为什么我们只使用1对1端口映射到Docker容器而不是映射到高端口？1:1映射端口只能到你本地主机的端口。假设你想要测试两个Python应用程序，两个容器内绑定到端口5000，没有足够的docker的端口映射你只能访问其中一个。

所以，现在我们打开浏览器访问端口49155。

![应用访问](../images/webapp1.png)

我们的应用程序可以访问了！

>注意：如果你在windows OS X或者Linux上使用boot2docker，您将需要制定虚拟主机IP，而不是使用本地主机，你可以通过运行boot2docker shell。

	$ boot2docker ip
	The VM's Host only interface IP address is: 192.168.59.103

>在这种情况下你可以通过http://192.168.59.103:49155访问上面的例子。

###网络端口快捷方式

使用`docker ps`命令会返回映射端口，就是有点笨手笨脚的。对此，docker提供了一种快捷方式：`docker port`。使用`docker port`可以查看指定（ID或者名字的）容器的某个确定端口映射到宿主机的端口号。

	$ sudo docker port nostalgic_morse 5000
	0.0.0.0:49155

在这种情况下，我们看到容器的5000端口映射到了宿主机的的49155端口。

###查看WEB应用程序日志

让我们看看我们的容器中的应用程序都发生了什么，使用我们学习到的另一个命令`docker logs`来查看。

	$ sudo docker logs -f nostalgic_morse
	* Running on http://0.0.0.0:5000/
	10.0.2.2 - - [23/May/2014 20:16:31] "GET / HTTP/1.1" 200 -
	10.0.2.2 - - [23/May/2014 20:16:31] "GET /favicon.ico HTTP/1.1" 404 -

这次我们添加了一个标示`-f`。这将使`docker log`命令中使用`tail -f`来查看容器标准输出。这里我们从应用程序日志的5000端口的访问日志条目。

###查看我们WEB应用程序容器的过程

我们除了可以查看容器日志，我们还可以使用`docker top`来查看容器进程：

	$ sudo docker top nostalgic_morse
	PID                 USER                COMMAND
	854                 root                python app.py

这里我们可以看到`python app.py`在容器里唯一进程。

###检查我们的WEB应用程序

最后，我们可以使用`docker inspect `来查看Docker的底层信息。它会返回一个JSON文件记录docker容器的配置和状态信息。

	$ sudo docker inspect nostalgic_morse

来让我们看下JSON的输出。

	[{
	    "ID": "bc533791f3f500b280a9626688bc79e342e3ea0d528efe3a86a51ecb28ea20",
	    "Created": "2014-05-26T05:52:40.808952951Z",
	    "Path": "python",
	    "Args": [
	       "app.py"
	    ],
	    "Config": {
	       "Hostname": "bc533791f3f5",
	       "Domainname": "",
	       "User": "",
	. . .

我们也可以针对我们想要的信息进行过滤，例如，返回容器的IP地址，如下：

	$ sudo docker inspect -f '{{ .NetworkSettings.IPAddress }}' nostalgic_morse
	172.17.0.5

###停止我们的容器

好吧，我们看到WEB应用程序工作。现在我们通过使用`docker stop`命令来停止名为 nostalgic_morse 的容器：

	$ sudo docker stop nostalgic_morse
	nostalgic_morse

现在我们使用`docker ps`命令来检查容器是否停止了。

###重启我们的应用程序

哎呀！刚才你停止了另一个开发人员所使用的容器。这里你现在有两个选择：您可以创建一个新的容器或者重新启动旧的。让我们启动我们之前的容器：

	$ sudo docker start nostalgic_morse
	nostalgic_morse

现在运行`docker ps -l`来查看正在运行的容器，或者通过URL访问来查看我们的应用程序是否响应。

>注意：也可以使用`docker restart`命令来停止容器或然后再启动容器。

###移除我们的应用程序

你的同事告诉你他们已经完成了在容器上的工作，不在需要容器了。让我们使用`docker rm`命令来删除它：

	$ sudo docker rm nostalgic_morse
	Error: Impossible to remove a running container, please stop it first or use -f
	2014/05/24 08:12:56 Error: failed to remove one or more containers

发生了什么？实际上，我们不能删除正在运行的容器。这避免你意外删除了可能需要的运行中的容器。让我们先停止容器，再试一次。

	$ sudo docker stop nostalgic_morse
	nostalgic_morse
	$ sudo docker rm nostalgic_morse
	nostalgic_morse

现在我们停止并删除了容器。

>注意：删除容器是最后一步！

###下一步

直到现在，我们使用的镜像都是从Docker Hub下载的。接下来，我们学习创建和分享镜像。

阅读[使用镜像](dockerimages.md)
