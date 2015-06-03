Docker 中运行 MongoDB
===

###描述

在这个例子里，我们会学到如何构建一个预装 `MongoDB` 的镜像。我们还将会看到如何推送镜像到 Docker Hub 并分享给其他人。

使用 Docker 容器来部署 [MongoDB](https://www.mongodb.org/) 实例将会给你带来许多好处，例如：

* 易于维护、高可配置的 MongoDB 实例
* 准备好运行和毫秒级内开始工作
* 基于全球访问的共享镜像

>注意: 如果你不喜欢sudo,可以查看[非 root 用户使用](installation/binaries.md)

###为 MongoDB 创建一个 Dockerfile

让我们创建一个 `Dockerfile` 并构建：

	$ nano Dockerfile
 
虽然这部分是可选的，但是在 `Dockerfile` 开头处添加注释，可以更好的去解释其目的：

	# Dockerizing MongoDB: Dockerfile for building MongoDB images
	# Based on ubuntu:latest, installs MongoDB following the instructions from:
	# http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/

>小贴士：`Dockerfile`文件是灵活的。然而，他们遵循一定的格式。第一项定义的是镜像的名称，这里是 Dockerized MongoDB 应用的父镜像。

我们将使用 Docker Hub 中最新版本的 Ubuntu 镜像来构建。

	# Format: FROM    repository[:version]
	FROM       ubuntu:latest

继续，我们将在 `Dockerfile` 中指定 `MAINTAINER`

	# Format: MAINTAINER Name <email@addr.ess>
	MAINTAINER M.Y. Name <myname@addr.ess>

>注：尽管 Ubuntu 系统已经有 MongoDB 包，但是它们可能过时，因此，在这个例子中，我们将使用 MongoDB 的官方包。

我们将开始导入 MongoDB 公共 GPG 秘钥。我们还将创建一个 MongoDB 库包管理器

	# Installation:
	# Import MongoDB public GPG key AND create a MongoDB list file
	RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
	RUN echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | tee /etc/apt/sources.list.d/10gen.list

这个初步的准备后，我们将更新我们的包并且安装 MongoDB 。

	# Update apt-get sources AND install MongoDB
	RUN apt-get update
	RUN apt-get install -y -q mongodb-org

>小贴士: 您可以通过使用所需的软件包版本列表来指定 MongoDB的特定版本,例如:

	RUN apt-get install -y -q mongodb-org=2.6.1 mongodb-org-server=2.6.1 mongodb-org-shell=2.6.1 mongodb-org-mongos=2.6.1 mongodb-org-tools=2.6.1

MongoDB 需要数据目录，让我们在最后一步中执行

	# Create the MongoDB data directory
	RUN mkdir -p /data/db

最后我们设置 `ENTRYPOINT` 指令来告诉 Docker 如何在我们的 MongoDB 镜像容器内运行 `mongod` 。并且我们将使用 `EXPOSE` 命令来指定端口：

	# Expose port 27017 from the container to the host
	EXPOSE 27017
	
	# Set usr/bin/mongod as the dockerized entry-point application
	ENTRYPOINT usr/bin/mongod

现在保存我们的文件并且构建我们的镜像。

>注：完整版的 `Dockerfile` 可以在这里[下载](Dockerfile)

###构建 MongoDB 的 Docker 镜像

我们可以使用我们的 `Dockerfile` 来构建我们的 MongoD B镜像。除非实验，使用 `docker build` 的 `--tag` 参数来给 docker 镜像指定标签始终是一个很好的做法。

	# Format: sudo docker build --tag/-t <user-name>/<repository> .
	# Example:
	$ sudo docker build --tag my/repo .

当我们发出这个命令时，Docker 将会通过 `Dockerfile` 来构建镜像。最终镜像将被标记成 `my/repo` 。

###推送 MongoDB 镜像到 Docker Hub

`docker push` 命令将会把镜像推送到 Docker Hub 上，你可以在 Docker Hub 上托管和分享推送的镜像。为此，你需要先登录：

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

###使用 MongoDB 的镜像

使用我们创建的MongoDB镜像，我们可以运行一个或多个的 MongoDB 守护进程。

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