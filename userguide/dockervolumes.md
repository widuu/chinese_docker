管理容器数据
===

到目前为止，我们已经介绍了一些基本的docker概念，了解了如何使用docker镜像，以及容器之间通过网络连接。在本节中，我们讨论如果管理容器和容器之间的数据。


我么看一下主要有两种方法来管理docker数据。

- 数据卷
- 容器中的数据卷

###数据卷

数据卷是在一个或多个容器内指定数据卷，并且绕过[Union File System](http://docs.docker.com/terms/layer/#ufs-def)来持续提供一些有用的特性和共享数据。

- 数据卷之间可以共享或重用容器
- 可以直接更改数据卷
- 更改数据卷不会被容器镜像当成更新
- 体积不变一直到容器不使用它们

###添加一个数据卷

你可以在`docker`命令中使用`-v`标识来给容器内添加一个数据卷，你可以使用`-v`标识多次给docker创建多个数据卷。现在让我们创建单个数据卷在我们的web容器应用中。

	$ sudo docker run -d -P --name web -v /webapp training/webapp python app.py

这会在容器内部创建一个新的卷`/webapp`

>注：你可以在`Dockerfile`中使用`VOLUME`指令来给创建的镜像添加一个或多个卷。

###挂载一个主机目录作为卷

除了使用`-v`创建一个数据卷还可以挂载本地主机目录到容器中：

	$ sudo docker run -d -P --name web -v /src/webapp:/opt/webapp training/webapp python app.py

这将会挂载本地目录`/src/webapp`到容器的`/ot/webapp`目录。这在做测试时是非常有用的。例如我们可以挂载我们本地的源代码到容器内部，我们可以看到我们改变源代码时的应用时如何工作的。主机上的目录必须是绝对路径，如果目录不存在docker会自动创建它。

>注：这是不可以在Dockerfile的，因为Dockerfile的便携性和移植性，作为主机目录，其性质，可能使依赖这个目录的所有主机都无法工作。

docker默认情况下是对卷有读写权限，但是我们也可以让卷只读。

	$ sudo docker run -d -P --name web -v /src/webapp:/opt/webapp:ro training/webapp python app.py
	
这里我们挂载相同的`/src/webapp`目录，但是我们添加了`ro`选项来限制它只读。

###创建一个挂在数据卷的容器

如果你想要容器之间数据共享，或者从非持久化容器中使用一些持久化数据，最好创建一个名为数据卷的容器，然后用它挂载数据。

让我们创建一个数据共享命名的容器。

	$ sudo docker run -d -v /dbdata --name dbdata training/postgres
	
你可以使用`--volumes-from`标识来再另外一个容器挂载`/dbdata`卷。

	$ sudo docker run -d --volumes-from dbdata --name db1 training/postgres
	
另外一个

	$ sudo docker run -d --volumes-from dbdata --name db2 training/postgres
	
您也可以使用多个`--volumes-from`参数来将多个数据卷桥接到多个容器中。

你也可以通过挂载卷来扩展链，这里挂载在`dbdata`容器的卷还挂载到了db1和db2容器

	$ sudo docker run -d --name db3 --volumes-from db1 training/postgres
	
当你删除挂载卷的dbdata容器，包括初始化数据化容器，或者随后的db1容器和db2容器，该卷将不会被删除直到没有容器使用该卷。这允许你升级，或者把有效的数据卷在容器之间迁移。

###备份、恢复或者迁移数据卷

另外一个功能我们可以用卷来备份、恢复或迁移数据。为此我们使用`--volumes-from`参数来创建一个容器，并且挂载卷，像这样：

	$ sudo docker run --volumes-from dbdata -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /dbdata
	
这里我们启动了一个新容器并且挂载`dbdata`卷，我们挂载了一个本地目录作为`/backup`卷。最后，我们通过使用tar命令备份`dbdata`卷的内容到我们的`/backup`目录下的`backup.tar`文件中。当命令完成或者容器停止，我们会留下我们的`dbdata`卷的备份。

然后，你可以在同一容器中恢复，或者另一个你所在的容器内进行。创建一个新的容器

	$ sudo docker run -v /dbdata --name dbdata2 ubuntu /bin/bash
	
在新的容器的卷标中解压备份文件。

	$ sudo docker run --volumes-from dbdata2 -v $(pwd):/backup busybox tar xvf /backup/backup.tar

你可以使用此技术进行自动备份、迁移和使用您喜欢的方式进行恢复。

###下一步

现在，我们已经学到了一些关于如何使用docker，我们要看看如何使用Docker和Docker Hub服务，包括自动化构建和私人仓库。

阅读[使用Docker Hub](dockerrepos.md)