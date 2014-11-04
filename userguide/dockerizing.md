在Docker中运行"hello Word"应用
===

Docker在容器内运行应用程序。在一个容器内运行一个应用程序需要一个命令:`docker run`。

###Hello word

让我们现在来试试

	$ sudo docker run ubuntu:14.04 /bin/echo 'Hello world'
	Hello world

刚刚你运行了你的第一个容器！

所以刚才发生了什么?让我们来看看`docker run`运行了哪些步骤。
首先，我们指定了docker二进制中我们想要执行的命令,`run`。`docker run`组合运行容器。

接下来，我们指定一个镜像:ubuntu 14.04。这是我们容器中运行的来源。docker称这个为镜像。在本例中，我们使用一个Ubuntu 14.04操作系统镜像。

当你指定一个镜像，docker会查看这个镜像是否有一次加载到你的docker主机上，如果没有发现，docker就会在镜像仓库[Docker Hub](https://hub.docker.com/)下载公共镜像。

接下来，我们告诉docker在我们的新容器内运行什么命令：

	/bin/echo 'Hello world'

当我们的docker创建一个新的Ubuntu 14.04环境，然后执行`/bin/echo`命令。我们会在命令行看到结果：

	hello world

那么，我们创建容器之后会发生什么呢？这里Docker容器当你输入指令时被激活运行。这里只要"hello word"被输出，容器就会停止。

###一个交互式的容器

让我们尝试再次运行`docker run`，这次我们指定一个新的命令来运行我们的容器。

	$ sudo docker run -t -i ubuntu:14.04 /bin/bash
	root@af8bae53bdd3:/#

在这里我们继续指定`docker run`命令，并运行一个新的ubuntu:14.04的镜像。但是我们也加了两个新的标示：`-t`和`-i`。`-t`表示在新容器内指定一个伪终端或终端，`-i`表示允许我们对容器内的STDIN进行交互。

在我们的容器内还指定了一个新的命令：`/bin/bash`。这将在容器内启动`bash shell`

所以现在当我们的容器启动时我们会看到，我们有一个命令提示符：

	root@af8bae53bdd3:/#

让我们尝试在容器内运行一些命令：

	root@af8bae53bdd3:/# pwd
	/
	root@af8bae53bdd3:/# ls
	bin boot dev etc home lib lib64 media mnt opt proc root run sbin srv sys tmp usr var

你可以看到我们运行`pwd`来显示当前目录，这时候显示的是我们的根目录。我们还列出了跟目录的列表显示了目录列表，通过目录列表我们看出来想一个典型的Linux文件系统。

当你完成你的命令的时候，你退出这个容器，你可以输入`exit`。

	root@af8bae53bdd3:/# exit

与我们之前的容器一样，一旦你的Bash shell退出之后，你的容器就停止了。


###守护进程Hello world

现在一个容器运行一个命令当使用完退出，但是这并不是有帮助的。让我们创建一个容器，让它以守护进程的模式运行，就像docker运行应用程序那样。

我们可以这样运行`docker run`命令:

	$ sudo docker run -d ubuntu:14.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
	1e5535038e285177d5214659a068137486f96ee5c2e85a4ac52dc83f2ebe4147

等待的结果是什么？我们的hello word去哪了。让我们看看它是怎么运行的。它看起来应该很熟悉，我们运行docker但是我们指定了一个`-d`标识。`-d`标识告诉docker运行容器在后台模式运行。

我们也指定了一个相同的镜像:ubuntu:14.04

最终，我们指定命令行运行:

	/bin/sh -c "while true; do echo hello world; sleep 1; done"

这是一个荒谬的hello word进程：一个脚本会一直输出"hello word"

为什么不是我们看到的一大堆的"hello word"?而是docker返回的一个很长的字符串：

	1e5535038e285177d5214659a068137486f96ee5c2e85a4ac52dc83f2ebe4147

这个长的字符串叫做容器ID。它是容器的唯一标识，所以我们可以使用它。

>注意：容器ID是有点长并且非常的笨拙，稍后我们会看到一个断点的ID,某些方面来说它是容器ID的简化版。

我们可以根据容器ID查看"hello word"进程发生了什么、

首先，我们要确保容器正在运行。我们可以使用`docker ps`命令来查看。`docker ps`命令可以查询docker进程的所有容器。

	$ sudo docker ps
	CONTAINER ID  IMAGE         COMMAND               CREATED        STATUS       PORTS NAMES
	1e5535038e28  ubuntu:14.04  /bin/sh -c 'while tr  2 minutes ago  Up 1 minute        insane_babbage

这里我们看到了后台进程容器。`docker ps`命令会返回一些有用的信息，这里包括一个短的容器ID：`1e5535038e28`。

我们还可以看到我们使用的`ubuntu:14.04`来构建它，这个命令已经运行，它被系统自动分配了名称`insane_babbage`。

>注意：docker会在容器启动的时候自动给容器命名，稍后我们可以看到我们如何给容器指定名称。

好了，现在我们知道它运行。但是我们能要求它做什么呢？做到这，我们需要在我们容器内使用`docker logs`命令。让我们给系统自动分配名称的容器使用这个命令。

	$ sudo docker logs insane_babbage
	hello world
	hello world
	hello world
	. . .

`docker logs`命令看起来想运行在容器内并且返回标准输出：这种情况下我们的命令输出`hello word`

太棒了！我们创建的第一个后台应用进程运行了！

现在我们可以创建我们自己的容器了，我们使用后，让我们停止这个后台进程容器。为此让我们使用`docker stop`命令停止。

	$ sudo docker stop insane_babbage
	insane_babbage

`docker stop`命令会通知docker停止正在运行的容器。如果它成功了，它将返回刚刚停止的容器名称。

让我们通过`docker ps`命令来检查它是否还工作。

	$ sudo docker ps
	CONTAINER ID  IMAGE         COMMAND               CREATED        STATUS       PORTS NAMES

太好了，我们的容器停止了。

###下一步

现在我们看到了docker是多么简单，让我们学习如何做一些更高级的任务。

去阅读[使用容器](usingdocker.md)



