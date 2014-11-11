管理容器数据
===

到目前为止，我们已经介绍了docker的一些基本概念，了解了如何使用docker镜像，以及容器之间如何通过网络连接。本节，我们来讨论如何管理容器和容器间的共享数据。


接下来，我们将主要介绍Docker管理数据的两种主要的方法：

- 数据卷
- 数据卷容器

###数据卷

数据卷是指在存在于一个或多个容器中的特定目录，此目录能够绕过[Union File System](http://docs.docker.com/terms/layer/#ufs-def)提供一些用于持续存储或共享数据的特性。

- 数据卷可在容器之间共享或重用
- 数据卷中的更改可以直接生效
- 数据卷中的更改不会包含在镜像的更新中
- 数据卷的生命周期一直持续到没有容器使用它为止

###添加一个数据卷

你可以在`docker run`命令中使用`-v`标识来给容器内添加一个数据卷，你也可以在一次`docker run`命令中多次使用`-v`标识挂载多个数据卷。现在我们在web容器应用中创建单个数据卷。

	$ sudo docker run -d -P --name web -v /webapp training/webapp python app.py

这会在容器内部创建一个新的卷`/webapp`

>注：类似的，你可以在`Dockerfile`中使用`VOLUME`指令来给创建的镜像添加一个或多个数据卷。

###挂载一个主机目录作为卷

使用`-v`，除了可以创建一个数据卷，还可以挂载本地主机目录到容器中：

	$ sudo docker run -d -P --name web -v /src/webapp:/opt/webapp training/webapp python app.py

这将会把本地目录`/src/webapp`挂载到容器的`/opt/webapp`目录。这在做测试时是非常有用的，例如我们可以挂载宿主机的源代码到容器内部，这样我们就可以看到改变源代码时的应用时如何工作的。宿主机上的目录必须是绝对路径，如果目录不存在docker会自动创建它。

>注：出于可移植和分享的考虑，这种方法不能够直接在`Dockerfile`中实现。作为宿主机目录——其性质——是依赖于特定宿主机的，并不能够保证在所有的宿主机上都存在这样的特定目录。

docker默认情况下是对数据卷有读写权限，但是我们通过这样的方式让数据卷只读：

	$ sudo docker run -d -P --name web -v /src/webapp:/opt/webapp:ro training/webapp python app.py
	
这里我们同样挂载了`/src/webapp`目录，只是添加了`ro`选项来限制它只读。

### 将宿主机上的特定文件挂载为数据卷

除了能挂载目录外，`-v`标识还可以将宿主机的一个特定文件挂载为数据卷：

    $ sudo docker run --rm -it -v ~/.bash_history:/.bash_history ubuntu /bin/bash

上述命令会在容器中运行一个bash shell，当你退出此容器时在主机上也能够看到容器中bash的命令历史。

>注：很多文件编辑工具如`vi`和`sed --in-place`会导致inode change。Docker v1.1.0之后的版本，会产生一个错误：“sed cannot rename ./sedKdJ9Dy: Device or resource busy”。这种情况下如果想要更改挂载的文件，最好是直接挂载它的父目录。

###创建、挂载数据卷容器

如果你想要容器之间数据共享，或者从非持久化容器中使用一些持久化数据，最好创建一个指定名称的数据卷容器，然后用它来挂载数据。

让我们创建一个指定名称的数据卷容器：

	$ sudo docker run -d -v /dbdata --name dbdata training/postgres echo Data-only container for postgres
	
你可以在另外一个容器使用`--volumes-from`标识，通过刚刚创建的数据卷容器来挂载对应的数据卷。

	$ sudo docker run -d --volumes-from dbdata --name db1 training/postgres
	
可以将对应的数据卷挂载到更多的容器中：

	$ sudo docker run -d --volumes-from dbdata --name db2 training/postgres
	
当然，您也可以对一个容器使用多个`--volumes-from`标识，来将多个数据卷桥接到这个容器中。

数据卷容器是可以进行链式扩展的，之前的`dbdata`数据卷依次挂载到了dbdata 、db1和db2容器，我们还可以使用这样的方式来将数据卷挂载到新的容器db3：

	$ sudo docker run -d --name db3 --volumes-from db1 training/postgres
	
即使你删除所有de 挂载了数据卷dbdata的容器（包括最初的`dbdata`容器和后续的`db1`和`db2`），数据卷本身也不会被删除。要删在磁盘上删除这个数据卷，只能针对最后一个挂载了数据卷的容器显式地调用`docker rm -v`命令。这种方式可使你在容器之间方便的更新和迁移数据。

###备份、恢复或者迁移数据卷

数据卷还可以用来备份、恢复或迁移数据。为此我们使用`--volumes-from`参数来创建一个挂载数据卷的容器，像这样：

	$ sudo docker run --volumes-from dbdata -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /dbdata
	
这里我们启动了一个挂载`dbdata`卷的新容器，并且挂载了一个本地目录作为`/backup`卷。最后，我们通过使用tar命令将`dbdata`卷的内容备份到容器中的`/backup`目录下的`backup.tar`文件中。当命令完成或者容器停止，我们会留下我们的`dbdata`卷的备份。

然后，你可以在同一容器或在另外的容器中恢复此数据。创建一个新的容器

	$ sudo docker run -v /dbdata --name dbdata2 ubuntu /bin/bash
	
然后在新的容器中的数据卷里un-tar此备份文件。

	$ sudo docker run --volumes-from dbdata2 -v $(pwd):/backup busybox tar xvf /backup/backup.tar

你可以对熟悉的目录应用此技术，来测试自动备份、迁移和恢复。

###下一步

本小节中，我们学习了关于数据卷和数据卷容器使用方法。接下来，我们要看看如何使用Docker Hub服务，包括自动化构建和私人仓库。

阅读[使用Docker Hub](dockerrepos.md)
