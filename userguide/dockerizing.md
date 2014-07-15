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

接下来，我们告诉docker在我们的心容器内运行什么命令：

	/bin/echo 'Hello world'

当我们的docker创建一个新的Ubuntu 14.04环境，然后执行`/bin/echo`命令。我们会在命令行看到结果：

	hello word

那么，我们创建容器之后会发生什么呢？这里Docker容器当你输入指令时被激活运行。这里只要"hello word"被输出，容器就会停止。

###一个交互式的容器

让我们尝试再次运行`docker run`，这次我们指定一个新的命令来运行我们的容器。

	$ sudo docker run -t -i ubuntu:14.04 /bin/bash
	root@af8bae53bdd3:/#

在这里我们继续指定`docker run`命令，并运行一个新的ubuntu:14.04的镜像。但是我们也加了两个新的标示：`-t`和`-i`。`-t`标示在心容器内指定一个伪终端或终端，`-i`标示允许我们对容器内的STDIN进行交互。

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

