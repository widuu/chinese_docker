使用docker镜像
===

在前边的安装简介中，我们提及到了基础镜像。在[前面的部分](dockerizing.md)我们已经使用了Docker镜像，举例：`ubuntu`镜像和`training/webapp`镜像。

我们还发现Docker主机会从存储中心下载镜像。如果一个镜像不存在，他就会自动从docker镜像仓库去下载：默认的`Docker Hub`公共镜像源。

在这一节中，我们将探讨更多的关于docker镜像的东西：

* 在本地Docker主机上管理和处理镜像
* 创建基本镜像
* 上传docker镜像到docker镜像源

###在主机上列出镜像

让我们来列出本地主机上的镜像。你可以使用`docker images`来完成这项任务：

	$ sudo docker images
	REPOSITORY       TAG      IMAGE ID      CREATED      VIRTUAL SIZE
	training/webapp  latest   fc77f57ad303  3 weeks ago  280.5 MB
	ubuntu           13.10    5e019ab7bf6d  4 weeks ago  180 MB
	ubuntu           saucy    5e019ab7bf6d  4 weeks ago  180 MB
	ubuntu           12.04    74fe38d11401  4 weeks ago  209.6 MB
	ubuntu           precise  74fe38d11401  4 weeks ago  209.6 MB
	ubuntu           12.10    a7cf8ae4e998  4 weeks ago  171.3 MB
	ubuntu           quantal  a7cf8ae4e998  4 weeks ago  171.3 MB
	ubuntu           14.04    99ec81b80c55  4 weeks ago  266 MB
	ubuntu           latest   99ec81b80c55  4 weeks ago  266 MB
	ubuntu           trusty   99ec81b80c55  4 weeks ago  266 MB
	ubuntu           13.04    316b678ddf48  4 weeks ago  169.4 MB
	ubuntu           raring   316b678ddf48  4 weeks ago  169.4 MB
	ubuntu           10.04    3db9c44f4520  4 weeks ago  183 MB
	ubuntu           lucid    3db9c44f4520  4 weeks ago  183 MB

我们可以看到之前使用的镜像。每次从`Docker Hub`下载一个镜像就会在本地创建一个对应的容器。

我们在镜像列表中看到三个至关重要的东西。

* 来自什么镜像源，例如`ubuntu`
* 每个镜像都有标签，例如`14.04`
* 每个镜像都有镜像ID

镜像源中可能有多种不同的镜像。`Ubuntu`中我们会看到多个Ubuntu版本：10.04, 12.04, 12.10, 13.04, 13.10 and 14.04。每个容器有一个唯一的标签，让我们来识别不同的镜像，例如：

	ubuntu:14.04

所以我们可以运行一个带标签镜像的容器：

	$ sudo docker run -t -i ubuntu:14.04 /bin/bash

如果我们想要使用`Ubuntu 12.04`的镜像来构建，我们可以这样做

	$ sudo docker run -t -i ubuntu:12.04 /bin/bash

如果你不指定一个镜像的版本标签，例如你只使用`Ubuntu`，Docker将默认使用`Ubuntu:latest`镜像。

>提示：我们建议使用镜像时指定一个标签，例如`ubuntu:12.04`。这样你知道你使用的是一个什么版本的镜像。

###获取一个新的镜像

现在如何获取一个新的镜像？当我们在本地主机上使用一个不存在的镜像时Docker就会自动下载这个镜像。但是这需要一段时间下载这个镜像。如果我们想预先加载这个镜像，我们可以使用`docker pull`命令来下载它。像我们所说的我们下载`centos`镜像。

	$ sudo docker pull centos
	Pulling repository centos
	b7de3133ff98: Pulling dependent layers
	5cc9e91966f7: Pulling fs layer
	511136ea3c5a: Download complete
	ef52fb1fe610: Download complete
	. . .

我们看到每一层的镜像都被下载下来了，现在我们可以直接使用这个镜像，而不需要在下载这个镜像了。

	$ sudo docker run -t -i centos /bin/bash
	bash-4.1#

###查找镜像

Docker的特点之一是人们创建了各种各样的docker镜像。而且这些镜像已经被上传到了`Docker Hub`。我们可以从`Docker Hub`网站来搜索镜像。

![](../images/search.png)

我们也可以使用`docker search`命令来搜索镜像。譬如说我们的团队需要一个安装了Ruby和Sinatra的镜像来做我们的web应用程序开发。我们可以通过`docker search`命令来搜索所有的`sinatra`镜像来寻找我们合适的镜像

	$ sudo docker search sinatra
	NAME                                   DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
	training/sinatra                       Sinatra training image                          0                    [OK]
	marceldegraaf/sinatra                  Sinatra test app                                0
	mattwarren/docker-sinatra-demo                                                         0                    [OK]
	luisbebop/docker-sinatra-hello-world                                                   0                    [OK]
	bmorearty/handson-sinatra              handson-ruby + Sinatra for Hands on with D...   0
	subwiz/sinatra                                                                         0
	bmorearty/sinatra                                                                      0
	. . . 

我们看到了返回了大量的`sinatra`镜像。我们看到列表中有镜像名称、描述、Stars(衡量镜像的流行程度-如果用户喜欢这个镜像他就会点击stars)和官方自动构建镜像状态。Stackbrew维护者官方仓库源，镜像源是自动构建的，您可以验证镜像的来源和内容。

我们回顾以前使用的镜像，我们决定使用`sinatra`镜像。到目前为止，我们已经看到了两种类型的镜像，像`ubuntu`镜像，我们称它为基础镜像或者根镜像。这些镜像是由docker公司提供建立、验证和支持。这些镜像都可以通过自己的名字来标示。

我们还可以看到用户的镜像，例如`training/sinatra`,并且我们可以使用`docker pull`命令来下载它。

	$ sudo docker pull training/sinatra

现在我们的团队可以在自己的容器内使用这个镜像了。

	$ sudo docker run -t -i training/sinatra /bin/bash
	root@a8cb6ce02d85:/#

###创建自己的镜像

我们的团队发现`training/sinatra`镜像虽然有用但是不是我们需要的，我们需要针对这个镜像做出更改。现在又两种方法，我们可以更新和创建镜像。

* 1.我们可以从已经创建的容器中更新镜像，并且提交这个镜像。
* 2.我们可以使用`Dockerfile`指令来创建一个镜像。

###更新并且提交镜像

更新一个镜像，首先我们要创建一个我们想更新的容器。

	$ sudo docker run -t -i training/sinatra /bin/bash
	root@0b2616b0e5a8:/#

>注意：创建容器ID`0b2616b0e5a8`,我们在后边还需要使用它。

在我们的容器内添加`json`

	root@0b2616b0e5a8:/# gem install json

当我们完成的时候，输入`exit`命令来退出这个容器。

现在我们有一个根据我们需求做出改变的容器。我们可以使用`docker commit`来提交这个容器。

	$ sudo docker commit -m="Added json gem" -a="Kate Smith" \
	0b2616b0e5a8 ouruser/sinatra:v2
	4f177bd27a9ff0f6dc2a830403925b5360bfe0b93d476f7fc3231110e7f71b1c

这里我们使用了`docker commit`命令。我们可以指定`-m`和`-a`标示。`-m`标示是允许我们指定提交的信息，就像你提交一个版本控制。`-a`标示允许对我们的更新指定一个用户。

我们也指定了我们想要创建的新镜像来自(我们先前记录的ID)`0b2616b0e5a8`和我们指定的目标镜像：

	ouruser/sinatra:v2

让我们分解这个步骤。我们先给这个镜像分配了一个新用户名字`ouruser`；接着，未修改镜像名称，保留了原镜像名称`sinatra`；最后为镜像指定了标签`v2`。

我们可以使用`docker images`命令来查看我们的新镜像`ouruser/sinatra`。

	$ sudo docker images
	REPOSITORY          TAG     IMAGE ID       CREATED       VIRTUAL SIZE
	training/sinatra    latest  5bc342fa0b91   10 hours ago  446.7 MB
	ouruser/sinatra     v2      3c59e02ddd1a   10 hours ago  446.7 MB
	ouruser/sinatra     latest  5db5f8471261   10 hours ago  446.7 MB

使用我们的新镜像来创建一个容器：

	$ sudo docker run -t -i ouruser/sinatra:v2 /bin/bash
	root@78e82f680994:/#

###使用`Dockerfile`创建镜像

使用`docker commit`命令能非常简单的扩展镜像，但是它有点麻烦：在一个团队中不容易共享它的开发过程。为解决这个问题，我们可以使用一个新的命令来创建新的镜像。

为此，我们创建一个`Dockerfile`，其中包含一组指令告诉docker如何创建我们的镜像。

现在让我们创建一个目录，并且创建一个`Dockerfile`

	$ mkdir sinatra
	$ cd sinatra
	$ touch Dockerfile

每一个指令镜像就会创建一个新的层，让我们看一个简单的例子，我们的开发团建创建一个自己的`Sinatra `镜像：

	# This is a comment
	FROM ubuntu:14.04
	MAINTAINER Kate Smith <ksmith@example.com>
	RUN apt-get -qq update
	RUN apt-get -qqy install ruby ruby-dev
	RUN gem install sinatra

【注意】：
	1）、每个指令前缀都必须大写：
		INSTRUCTION statement
	2）、可以使用`#`注释；

让我们看看`Dockerfile`做了什么：
	第一个指令`FROM`，告诉Docker使用哪个镜像源，在这个案例中我们使用了一个`Ubuntu 14.04`基础镜像。
	下一步，我们使用`MAINTAINER `指令指定谁是维护者。
	最后，我们指定三个`RUN`指令，一个`RUN`指令在镜像内执行命令。例如安装包。这里我们在`Sinatra `中更新了APT缓存，安装了`Ruby`和`RubyGems`。

>注意：我们还提供了更多的Dockerfile指令参数。

现在我们使用`docker build`命令和`Dockerfile`命令来创建一个镜像。

	$ sudo docker build -t="ouruser/sinatra:v2" .
	Uploading context  2.56 kB
	Uploading context
	Step 0 : FROM ubuntu:14.04
	 ---> 99ec81b80c55
	Step 1 : MAINTAINER Kate Smith <ksmith@example.com>
	 ---> Running in 7c5664a8a0c1
	 ---> 2fa8ca4e2a13
	Removing intermediate container 7c5664a8a0c1
	Step 2 : RUN apt-get -qq update
	 ---> Running in b07cc3fb4256
	 ---> 50d21070ec0c
	Removing intermediate container b07cc3fb4256
	Step 3 : RUN apt-get -qqy install ruby ruby-dev
	 ---> Running in a5b038dd127e
	Selecting previously unselected package libasan0:amd64.
	(Reading database ... 11518 files and directories currently installed.)
	Preparing to unpack .../libasan0_4.8.2-19ubuntu1_amd64.deb ...
	. . .
	Setting up ruby (1:1.9.3.4) ...
	Setting up ruby1.9.1 (1.9.3.484-2ubuntu1) ...
	Processing triggers for libc-bin (2.19-0ubuntu6) ...
	 ---> 2acb20f17878
	Removing intermediate container a5b038dd127e
	Step 4 : RUN gem install sinatra
	 ---> Running in 5e9d0065c1f7
	. . .
	Successfully installed rack-protection-1.5.3
	Successfully installed sinatra-1.4.5
	4 gems installed
	 ---> 324104cde6ad
	Removing intermediate container 5e9d0065c1f7
	Successfully built 324104cde6ad

我们使用`docker build`命令和`-t`来创建我们的新镜像，用户是`ouruser`、仓库源名称`sinatra`、标签是`v2`。

如果`Dockerfile`在我们当前目录下，我们可以使用`.`来指定`Dockerfile`

>提示：你也可以指定`Dockerfile`路径

现在我们可以看到构建过程。docker做的第一件事是通过你的上下文构建。基本上是目录的内容构建。docker会根据本地的内容来在docker进程中去构建。

下一步，我们`Dockerfile`一步一步执行命令。我们可以看到，每个步骤可以创建一个新的容器，在容器内运行指令并且提交改变，就像我们早期看到的`docker commit`流程、当所有的指令执行完成之后，我们就会得到`324104cde6ad `镜像（有助于标记ouruser/sinatra:v2）,然后所有中间容器会被删除干净。

我们可以从我们的新镜像中创建一个容器：

	$ sudo docker run -t -i ouruser/sinatra:v2 /bin/bash
	root@8196968dac35:/#

>注意：这是比较简单的创建镜像方法。我们跳过了你可以使用的一大堆指令。在后面的部门我们将会看到更多的指令指南，或者你可以参考`Dockerfile`参考的例子和详细描述每一个指令。

###设置镜像标签

你可以给现有的镜像添加标记，然后提交和构建。我们可以使用`docke tag`命令。让我们给`ouruser/sinatra`镜像添加一个新的标签。

	$ sudo docker tag 5db5f8471261 ouruser/sinatra:devel

`docker tag`指令标记镜像ID，这里是`5db5f8471261`,设定我们的用户名称、镜像源名称和新的标签。

让我们使用`docker images`命令查看新的标签。

	$ sudo docker images ouruser/sinatra
	REPOSITORY          TAG     IMAGE ID      CREATED        VIRTUAL SIZE
	ouruser/sinatra     latest  5db5f8471261  11 hours ago   446.7 MB
	ouruser/sinatra     devel   5db5f8471261  11 hours ago   446.7 MB
	ouruser/sinatra     v2      5db5f8471261  11 hours ago   446.7 MB

###向Docker Hub推送镜像

一旦你构建或创造一个新的镜像，你可以使用`docker push`命令推送到Docker Hub。可以对其他人公开进行分享，或把它添加到你的私人仓库中。

	$ sudo docker push ouruser/sinatra
	The push refers to a repository [ouruser/sinatra] (len: 1)
	Sending image list
	Pushing repository ouruser/sinatra (3 tags)
	. . .

###主机中移除镜像

你也可以删除你主机上的镜像，某种程度上我们可以使用`docker rmi`命令。

让我们删除已经不需要的容器：`training/sinatra`。

	$ sudo docker rmi training/sinatra
	Untagged: training/sinatra:latest
	Deleted: 5bc342fa0b91cabf65246837015197eecfa24b2213ed6a51a8974ae250fedd8d
	Deleted: ed0fffdcdae5eb2c3a55549857a8be7fc8bc4241fb19ad714364cbfd7a56b22f
	Deleted: 5c58979d73ae448df5af1d8142436d81116187a7633082650549c52c3a2418f0

>提示：在容器从主机中移除前，请确定容器没有被使用。

###下一步

现在，我们已经看到如何在容器中构建单独的应用程序。接下来，我们要学习如何把多个docker容器连接在一起构建一个完整的应用程序。

请阅读[连接容器](dockerlinks.md)
