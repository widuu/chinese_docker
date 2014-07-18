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


