Docker中运行Node.js web应用
===

>注意:——如果你不喜欢sudo,可以查看[非root用户使用](installation/binaries.md)

这个例子的目的是向您展示如何通过使用`Dockerfile`来构建自己的docker镜像。我们通过在`Centos`上运行一个简单`node.js`web应用输出'hello word'。您可以在https://github.com/enokd/docker-node-hello/获得完整的源代码。

###创建Node.js应用

首先，先创建一个文件存放目录`src`。然后创建`package.json`来描述你的应用程序和依赖关系：

	{
	  "name": "docker-centos-hello",
	  "private": true,
	  "version": "0.0.1",
	  "description": "Node.js Hello world app on CentOS using docker",
	  "author": "Daniel Gasienica <daniel@gasienica.ch>",
	  "dependencies": {
	    "express": "3.2.4"
	  }
	}

然后，创建一个`index.js`文件使用Express.js框架来做一个web用用程序:

	var express = require('express');
	
	// Constants
	var PORT = 8080;
	
	// App
	var app = express();
	app.get('/', function (req, res) {
	  res.send('Hello world\n');
	});
	
	app.listen(PORT);
	console.log('Running on http://localhost:' + PORT);

在接下来的步骤中，我们将看到如何使用docker的centos容器来运行这个应用程序。首先，你需要为你的应用程序创建一个docker镜像。

###创建Dockerfile

创建一个空文件叫`Dockerfile`:

	touch Dockerfile

使用你喜欢的编辑器打开Dockerfile，并添加如下，定义了docker镜像需要构建的版本（这个示例使用docker 0.3.4）

	# DOCKER-VERSION 0.3.4

接下来，定义构建自己镜像的顶级镜像。在这里我们使用Centos（tag：6.4）：
	
	FROM    centos:6.4

因为我们要构建一个`Node.js`应用，你需要在你的`Centos`镜像中安装Node.js。Node.js运行应用程序需要使用npm安装你package.json中定义的依赖关系。安装Centos包，你可以查看Node.js维基指令：

	# Enable EPEL for Node.js
	RUN     rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
	# Install Node.js and npm
	RUN     yum install -y npm

将你的应用程序源代码添加到你的Docker镜像中，使用`ADD`指令：

	# Bundle app source
	ADD . /src

使用npm安装你的应用程序依赖：

	# Install app dependencies
	RUN cd /src; npm install

应用程序绑定到端口8080，您将使用`EXPOSE`指令做docker进程映射：

	EXPOSE  8080

最后，定义命令，使用`CMD`定义运行时的node服务和应用`src/index.js`的路径（看我们添加源代码到容器的步骤）：

	CMD ["node", "/src/index.js"]

你的`Dockerfile`现在看起来像如下这样：

	# DOCKER-VERSION 0.3.4
	FROM    centos:6.4
	
	# Enable EPEL for Node.js
	RUN     rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
	# Install Node.js and npm
	RUN     yum install -y npm
	
	# Bundle app source
	ADD . /src
	# Install app dependencies
	RUN cd /src; npm install
	
	EXPOSE  8080
	CMD ["node", "/src/index.js"]

###创建你的镜像

去到你的`Dockerfile`目录下，运行命令来构建镜像。`-t`参数给镜像添加标签，为了让我们在`docker images`命令更容易查找到它：

	$ sudo docker build -t <your username>/centos-node-hello .

你的镜像现在将在列表中：

$ sudo docker images

	# Example
	REPOSITORY                            TAG       ID              CREATED
	centos                                6.4       539c0211cd76    8 weeks ago
	<your username>/centos-node-hello     latest    d64d3505b0d2    2 hours ago

###运行镜像

使用`-d`参数来运行你的镜像使容器在后台运行。使用`-p`参数来绑定一个公共端口到私有容器端口上。运行你之前构建的镜像：

	$ sudo docker run -p 49160:8080 -d <your username>/centos-node-hello

打印应用输出：

	# Get container ID
	$ sudo docker ps
	
	# Print app output
	$ sudo docker logs <container id>
	
	# Example
	Running on http://localhost:8080


###测试

要测试应用程序,先得到docker应用程序映射的端口:

	$ sudo docker ps
	
	# Example
	ID            IMAGE                                     COMMAND              ...   PORTS
	ecce33b30ebf  <your username>/centos-node-hello:latest  node /src/index.js         49160->8080

在上面的示例中，docker映射容器的`49160`端口到`8080`端口。
现在你可以使用`curl`来访问你的app（安装curl：sudo apt-get install curl)：

	$ curl -i localhost:49160
	
	HTTP/1.1 200 OK
	X-Powered-By: Express
	Content-Type: text/html; charset=utf-8
	Content-Length: 12
	Date: Sun, 02 Jun 2013 03:53:22 GMT
	Connection: keep-alive
	
	Hello world

我们希望本教程能够帮助您在Docker上的`Centos`镜像运行Node.js。你可以获得全部源代码https://github.com/gasi/docker-node-hello。



