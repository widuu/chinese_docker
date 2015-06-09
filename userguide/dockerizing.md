Docker 应用 "hello World"
===

Docker 允许你在容器内运行应用程序， 使用 `docker run` 命令来在容器内运行一个应用程序。

##Hello world

现在让我们来试试

	$ sudo docker run ubuntu:14.04 /bin/echo 'Hello world'
	Hello world

刚才你启动了你的第一个容器！

那么刚才发生了什么? 我们逐步来分析 `docker run` 命令做了哪些事情。

首先，我们指定了 `docker` 二进制执行文件和我们想要执行的命令 `run`。`docker run` 组合会运行容器。

接下来，我们指定一个镜像: `ubuntu 14.04` 。这是我们运行容器的来源。 Docker 称此为镜像。在本例中，我们使用一个 `Ubuntu 14.04` 操作系统镜像。

当你指定一个镜像，Docker 首先会先从你的 Docker 本地主机上查看镜像是否存在，如果没有，Docker 就会从镜像仓库 [Docker Hub](https://hub.docker.com/) 下载公共镜像。

接下来，我们告诉 Docker 在容器内我们需要运行什么命令：

	/bin/echo 'Hello world'

当我们的容器启动了 Docker 创建的新的 Ubuntu 14.04 环境，并在容器内执行 `/bin/echo` 命令后。我们会在命令行看到如下结果：

	hello world

那么，我们创建容器之后会发生什么呢？ 当命令状态状态处于激活状态的时候 Docker 容器就会一直运行。这里只要 "hello world" 被输出，容器就会停止。

## 一个交互式的容器

让我们尝试再次运行 `docker run`，这次我们指定一个新的命令来运行我们的容器。

	$ sudo docker run -t -i ubuntu:14.04 /bin/bash
	root@af8bae53bdd3:/#

我们继续指定了 `docker run` 命令，并启动了 `ubuntu:14.04` 镜像。但是我们添加了两个新的标识(参数flags)： `-t` 和 `-i` 。`-t` 表示在新容器内指定一个伪终端或终端，`-i`表示允许我们对容器内的 (`STDIN`) 进行交互。

我们在容器内还指定了一个新的命令： `/bin/bash` 。这将在容器内启动 `bash shell`

所以当容器（container）启动之后，我们会获取到一个命令提示符：

	root@af8bae53bdd3:/#

我们尝试在容器内运行一些命令：

	root@af8bae53bdd3:/# pwd
	/
	root@af8bae53bdd3:/# ls
	bin boot dev etc home lib lib64 media mnt opt proc root run sbin srv sys tmp usr var

你可以看到我们运行 `pwd` 来显示当前目录，这时候显示的是我们的根目录。我们还列出了根目录的文件列表，通过目录列表我们看出来这是一个典型的 Linux 文件系统。

你可以在容器内随便的玩耍，你可以使用 `exit` 命令或者使用 CTRL-D 来退出容器。

	root@af8bae53bdd3:/# exit

与我们之前的容器一样，一旦你的 Bash shell 退出之后，你的容器就停止了。


### Hello world 守护进程 

现在当一个容器运行完一个命令后就会退出，但是这样看起来有时候并不好。让我们创建一个容器以进程的方式运行，就像大多数我们运行在 Docker 中的应用程序一样，这里我们可以使用 `docker run` 命令：

	$ sudo docker run -d ubuntu:14.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
	1e5535038e285177d5214659a068137486f96ee5c2e85a4ac52dc83f2ebe4147

等等，怎么回事？我们的 “hello world” 输出呢？让我们看看它是怎么运行的。这个命令看起来应该很熟悉.我们运行`docker run` ，但是我们指定了一个 `-d` 标识。`-d` 标识告诉 docker 在容器内以后台进程模式运行。

我们也指定了一个相同的镜像: `ubuntu:14.04`

最终，我们指定命令行运行:

	/bin/sh -c "while true; do echo hello world; sleep 1; done"

这是一个忠实的 hello world 进程：一个脚本会一直输出 "hello world"

为什么我们看不到的一大堆的 "hello world" ? 而是docker返回的一个很长的字符串：

	1e5535038e285177d5214659a068137486f96ee5c2e85a4ac52dc83f2ebe4147

这个长的字符串叫做容器ID（container ID）。它对于每一个容器来说都是唯一的，所以我们可以使用它。

>注意：容器 ID 是有点长并且非常的笨拙，稍后我们会看到一个短点的 ID,某些方面来说它是容器 ID 的简化版。

我们可以根据容器 ID 查看 "hello world" 进程发生了什么

首先，我们要确保容器正在运行。我们可以使用 `docker ps` 命令来查看。`docker ps` 命令可以查询 docker 进程的所有容器。

	$ sudo docker ps
	CONTAINER ID  IMAGE         COMMAND               CREATED        STATUS       PORTS NAMES
	1e5535038e28  ubuntu:14.04  /bin/sh -c 'while tr  2 minutes ago  Up 1 minute        insane_babbage

这里我们看到了以进程模式运行的容器。`docker ps` 命令会返回一些有用的信息，这里包括一个短的容器 ID：`1e5535038e28`。

我们还可以查看到构建容器时使用的镜像， `ubuntu:14.04`，当命令运行之后，容器的状态随之改变并且被系统自动分配了名称 `insane_babbage`。

>注意：当容器启动的时候 Docker 会自动给这些容器命名，稍后我们可以看到我们如何给容器指定名称。

好了，现在我们知道它正在运行。但是我们能要求它做什么呢？做到这，我们需要在我们容器内使用 `docker logs` 命令。让我们使用容器的名称来填充 `docker logs` 命令。

	$ sudo docker logs insane_babbage
	hello world
	hello world
	hello world
	. . .

`docker logs` 命令会查看容器内的标准输出：这个例子里输出的是我们的命令 `hello world`

太棒了！我们的 docker 进程是工作的，并且我们创建了我们第一个 docker 应用。

现在我们已经可以创建我们自己的容器了，让我们处理正在运行的进程容器并停止它。我们使用 `docker stop` 命令来停止容器 。

	$ sudo docker stop insane_babbage
	insane_babbage

`docker stop` 命令会通知 Docker 停止正在运行的容器。如果它成功了，它将返回刚刚停止的容器名称。

让我们通过 `docker ps` 命令来检查它是否还工作。

	$ sudo docker ps
	CONTAINER ID  IMAGE         COMMAND               CREATED        STATUS       PORTS NAMES

太好了，我们的容器停止了。

###下一步

现在我们看到了Docker 是多么简单，让我们学习如何做一些更高级的任务。

去阅读[使用容器](usingdocker.md)



