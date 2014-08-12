Docker中运行MongoDB
===

###描述

在这个例子里，我们会学到如何构建一个预装`MongoDB`的Docker镜像。我们还将会看到如何推送镜像到Docker Hub注册表来分享给其他人。

使用Docker容器来部署MongoDB将会给你带来许多好处，例如：

* 易于维护、高可配置的MongoDB实例
* 准备好运行和毫秒级内开始工作
* 基于全球访问的共享镜像

>注意:——如果你不喜欢sudo,可以查看[非root用户使用](installation/binaries.md)

###为MongoDB创建一个Dockerfile

让我们创建一个`Dockerfile`并且来开始构建它：

	$ nano Dockerfile

虽然是可选的，但是在`Dockerfile`开头处的注释说明很方便的说明其目的：

	# Dockerizing MongoDB: Dockerfile for building MongoDB images
	# Based on ubuntu:latest, installs MongoDB following the instructions from:
	# http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/

>提示：Dockerfiles是灵活的。然而，他们遵循一定的格式。第一项定义的是镜像的名称，这里是MongoDB docker应用的父镜像。

我们将使用 Docker Hub中最新版本的Ubuntu镜像来构建镜像。

	# Format: FROM    repository[:version]
	FROM       ubuntu:latest

继续，我们将指定`Dockerfile`中的`MAINTAINER`

	# Format: MAINTAINER Name <email@addr.ess>
	MAINTAINER M.Y. Name <myname@addr.ess>

>注：尽管Ubuntu系统已经有MongoDB包，但是它们可能过时，因此，在这个例子中，我们将使用MongoDB的官方包。

我们将开始导入MongoDB公共GPG秘钥。我们还将创建一个MongoDB库包管理器

	# Installation:
	# Import MongoDB public GPG key AND create a MongoDB list file
	RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
	RUN echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | tee /etc/apt/sources.list.d/10gen.list

这个初步的准备后，我们将更新我们的包并且安装MongoDB。

	# Update apt-get sources AND install MongoDB
	RUN apt-get update
	RUN apt-get install -y -q mongodb-org

>提示:您可以安装MongoDB的特定版本与版本使用所需的软件包列表,例如:
	RUN apt-get install -y -q mongodb-org=2.6.1 mongodb-org-server=2.6.1 mongodb-org-shell=2.6.1 mongodb-org-mongos=2.6.1 mongodb-org-tools=2.6.1

MongoDB需要数据目录，让我们在最后一步中执行

	# Create the MongoDB data directory
	RUN mkdir -p /data/db

最后我们设置`ENTRYPOINT`来告诉Docker如何在我们的MongoDB镜像容器内运行`mongod`。并且我们将使用`EXPOSE`命令来指定端口：

	# Expose port 27017 from the container to the host
	EXPOSE 27017
	
	# Set usr/bin/mongod as the dockerized entry-point application
	ENTRYPOINT usr/bin/mongod

现在保存我们的文件并且构建我们的镜像。

>注：完整版的`Dockerfile`可以在这里[下载](Dockerfile)

###构建MongoDB的Docker镜像

我们可以使用我们的`Dockerfile`来构建我们的MongoDB镜像。除非实验，使用`docker build`的`--tag`参数来标记docker镜像始终是一个很好的做法。

	# Format: sudo docker build --tag/-t <user-name>/<repository> .
	# Example:
	$ sudo docker build --tag my/repo .

当我们发出这个命令时，Docker将会通过`Dockerfile`来构建镜像。最终镜像将被标记成`my/repo`。

###推送MongoDB镜像到Docker Hub

`docker push`命令推送到Docker Hub上的所有镜像，可以再Docker Hub上托管和分享。为此，你需要登录：

	# Log-in
	$ sudo docker login
	Username:
	..
	
	# Push the image
	# Format: sudo docker push <user-name>/<repository>
	$ sudo docker push my/repo
	The push refers to a repository [my/repo] (len: 1)
	Sending image list
	Pushing repository my/repo (1 tags)
	..

###使用MongoDB的镜像

使用我们创建的MongoDB镜像，我们可以运行一个或多个守护进程模式的MongoDB。

	# Basic way
	# Usage: sudo docker run --name <name for container> -d <user-name>/<repository>
	$ sudo docker run --name mongo_instance_001 -d my/repo
	
	# Dockerized MongoDB, lean and mean!
	# Usage: sudo docker run --name <name for container> -d <user-name>/<repository> --noprealloc --smallfiles
	$ sudo docker run --name mongo_instance_001 -d my/repo --noprealloc --smallfiles
	
	# Checking out the logs of a MongoDB container
	# Usage: sudo docker logs <name for container>
	$ sudo docker logs mongo_instance_001
	
	# Playing with MongoDB
	# Usage: mongo --port <port you get from `docker ps`> 
	$ mongo --port 12345