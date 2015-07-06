使用 Docker 镜像
===

在[了解Docker]()这部分中，我们知道了 Docker 镜像是容器的基础。。在[前面的部分](dockerizing.md)我们使用的是已经构建好的 Docker 镜像，例如： `ubuntu` 镜像和 `training/webapp` 镜像。

我们还了解到 Docker 商店下载镜像到本地的 Docker 主机上。如果一个镜像不存在，他就会自动从 Docker 镜像仓库去下载，默认是从 `Docker Hub` 公共镜像源下载。

在这一节中，我们将探讨更多的关于 Docker 镜像的东西：


* 管理和使用本地 Docker 主机镜像。
* 创建基本镜像
* 上传 Docker 镜像到 [Docker Hub Registry](https://registry.hub.docker.com/)。

### 在主机上列出镜像列表

让我们列出本地主机上的镜像。你可以使用 `docker images` 来完成这项任务：

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

我们可以看到之前使用的镜像。当我们每次要使用镜像启动一个容器的时候都会从 [Docker Hub](https://hub.docker.com/) 下载对应的镜像。

我们在镜像列表中看到三个至关重要的东西。

* 来自什么镜像源，例如 `ubuntu`
* 每个镜像都有标签(tags)，例如 `14.04`
* 每个镜像都有镜像ID

镜像源中可能存储这一个镜像源的多个版本。我们会看到 `Ubuntu` 的多个版本：10.04, 12.04, 12.10, 13.04, 13.10 and 14.04。每个容器有一个唯一的标签，让我们来识别为不同的镜像，例如：

	ubuntu:14.04

所以我们可以运行一个带标签镜像的容器：

	$ sudo docker run -t -i ubuntu:14.04 /bin/bash

如果我们想要使用 `Ubuntu 12.04` 的镜像来构建，我们可以这样做

	$ sudo docker run -t -i ubuntu:12.04 /bin/bash

如果你不指定一个镜像的版本标签，例如你只使用 `Ubuntu`，Docker将默认使用 `Ubuntu:latest` 镜像。

>提示：我们建议使用镜像时指定一个标签，例如 `ubuntu:12.04` 。这样你知道你使用的是一个什么版本的镜像。

### 获取一个新的镜像

现在如何获取一个新的镜像？当我们在本地主机上使用一个不存在的镜像时 Docker 就会自动下载这个镜像。但是这需要一段时间来下载这个镜像。如果我们想预先下载这个镜像，我们可以使用`docker pull`命令来下载它。这里我们下载 `centos` 镜像。

	$ sudo docker pull centos
	Pulling repository centos
	b7de3133ff98: Pulling dependent layers
	5cc9e91966f7: Pulling fs layer
	511136ea3c5a: Download complete
	ef52fb1fe610: Download complete
	. . .

我们看到镜像的每一层都被下载下来了，现在我们可以直接使用这个镜像来运行容器，而不需要在下载这个镜像了。

	$ sudo docker run -t -i centos /bin/bash
	bash-4.1#

### 查找镜像

Docker 的特点之一是人们创建了各种各样的 Docker 镜像。而且这些镜像已经被上传到了 `Docker Hub` 。我们可以从 `Docker Hub` 网站来搜索镜像。

![](../images/search.png)

我们也可以使用 `docker search` 命令来搜索镜像。譬如说我们的团队需要一个安装了 Ruby 和 Sinatra 的镜像来做我们的 web 应用程序开发。我们可以通过 `docker search` 命令来搜索所有的`sinatra` 来寻找适合我们的镜像

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

我们看到了返回了大量的 `sinatra `镜像。我们看到列表中有镜像名称、描述、Stars(衡量镜像的流行程度-如果用户喜欢这个镜像他就会点击 stars )和 是否是正式以及构建状态。[官方镜像仓库](https://docs.docker.com/docker-hub/official_repos/) 是官方精心整理出来服务 Docker 的 Docker 镜像库。自动化构建的镜像仓库是允许你验证镜像的内容和来源。

通过检索镜像，我们决定使用 `training/sinatra` 镜像。到目前为止，我们已经看到了两种类型的镜像，像`ubuntu` 镜像，我们称它为基础镜像或者根镜像。这些镜像是由 Docker 官方提供构建、验证和支持。这些镜像都可以通过用自己的名字来标记。

我们也可以查找其它用户的公开镜像，像我们选择使用的 `training/sinatra` 镜像。个人镜像是由 Docker 社区的成员创建和维护的。你可以用用户名称来识别这些镜像，因为这些镜像的前缀都是以用户名来标记的 ，像 `training` ，就是由 `training` 用户创建的镜像。

###拖取镜像(Pull our image)

我们已经确定了要使用的镜像， `training/sinatra` , 现在我们使用 `docker pull` 命令来下载这个镜像。

	$ sudo docker pull training/sinatra

现在团队成员可以在自己的容器内使用这个镜像了。

	$ sudo docker run -t -i training/sinatra /bin/bash
	root@a8cb6ce02d85:/#

###创建我们自己的镜像

团队成员发现 `training/sinatra` 镜像对于我们来说是非常有用的，但是它不能满足我们的需求，我们需要针对这个镜像做出更改。这里我们有两种方法，更新镜像或者创建一个新的镜像。

* 1.我们可以从已经创建的容器中更新镜像，并且提交这个镜像。
* 2.我们可以使用 `Dockerfile` 指令来创建一个新的镜像。

###更新并且提交镜像

更新镜像之前，我们需要使用镜像来创建一个容器。

	$ sudo docker run -t -i training/sinatra /bin/bash
	root@0b2616b0e5a8:/#

>注意：已创建容器ID `0b2616b0e5a8`,我们在后边还需要使用它。

在运行的容器内使用 gem 来安装 `json`

	root@0b2616b0e5a8:/# gem install json

在完成操作之后，输入 `exit `命令来退出这个容器。

现在我们有一个根据我们需求做出更改的容器。我们可以使用 `docker commit` 来提交容器副本。

	$ sudo docker commit -m="Added json gem" -a="Kate Smith" \
	0b2616b0e5a8 ouruser/sinatra:v2
	4f177bd27a9ff0f6dc2a830403925b5360bfe0b93d476f7fc3231110e7f71b1c

这里我们使用了 `docker commit` 命令。这里我们指定了两个标识(flags) `-m` 和 `-a` 。`-m` 标示我们指定提交的信息，就像你提交一个版本控制。`-a` 标识允许对我们的更新来指定一个作者。

我们也指定了想要创建的新镜像容器来源 (我们先前记录的ID) `0b2616b0e5a8` 和 我们指定要创建的目标镜像：

	ouruser/sinatra:v2

我们分解一下前边的步骤。我们先给这个镜像分配了一个新用户名字 `ouruser`，接着，未修改镜像名称，保留了原有镜像名称` sinatra`；最后为镜像指定了标签(tag) `v2`。

我们可以使用 `docker images` 命令来查看我们的新镜像 `ouruser/sinatra`。

	$ sudo docker images
	REPOSITORY          TAG     IMAGE ID       CREATED       VIRTUAL SIZE
	training/sinatra    latest  5bc342fa0b91   10 hours ago  446.7 MB
	ouruser/sinatra     v2      3c59e02ddd1a   10 hours ago  446.7 MB
	ouruser/sinatra     latest  5db5f8471261   10 hours ago  446.7 MB

使用我们的新镜像来创建一个容器：

	$ sudo docker run -t -i ouruser/sinatra:v2 /bin/bash
	root@78e82f680994:/#

### 使用 `Dockerfile` 构建镜像

使用 `docker commit` 命令能够非常简单的扩展镜像。但是它有点麻烦，并且在一个团队中也不能够轻易的共享它的开发过程。为解决这个问题，我们使用一个新的命令 `docker build` ， 从零开始来创建一个新的镜像。

为此，我们需要创建一个 `Dockerfile` 文件，其中包含一组指令来告诉 Docker 如何构建我们的镜像。

现在创建一个目录，并且创建一个 `Dockerfile`

	$ mkdir sinatra
	$ cd sinatra
	$ touch Dockerfile

如果你是在 Windows 上使用的 Boot2Docker，你可以通过使用 `cd` 命令来访问你的本地主机目录 `/c/Users/your_user_name`

每一个指令都会在镜像上创建一个新的层，来看一个简单的例子，我们的开发团建来构建一个自己的 `Sinatra `镜像：

	# This is a comment
	FROM ubuntu:14.04
	MAINTAINER Kate Smith <ksmith@example.com>
	RUN apt-get update && apt-get install -y ruby ruby-dev
	RUN gem install sinatra

让我们看一下我们的 `Dockerfile` 做了什么。每一个指令的前缀都必须是大写的。

	INSTRUCTION statement

>提示：我们可以使用 **#** 来注释

第一个指令 `FROM` 是告诉 Docker 使用的哪个镜像源，在这个案例中，我们使用的是 Ubuntu 14.04 基础镜像。

下一步，我们使用 `MAINTAINER ` 指令来指定谁在维护这个新镜像。

最后，我们指定了两个 `RUN` 指令。 `RUN` 指令在镜像内执行一条命令，例如：安装一个包。这里我们更新了 APT 的缓存，并且安装 Ruby 和 RubyGems ，然后使用 gem 安装 Sinatra 。


>注意：我们还提供了更多的 Dockerfile 指令参数。

现在，我们使用 `Dockerfile` 文件通过 `docker build` 命令来构建一个镜像。

	$ docker build -t ouruser/sinatra:v2 .
	Sending build context to Docker daemon 2.048 kB
	Sending build context to Docker daemon 
	Step 0 : FROM ubuntu:14.04
	 ---> e54ca5efa2e9
	Step 1 : MAINTAINER Kate Smith <ksmith@example.com>
	 ---> Using cache
	 ---> 851baf55332b
	Step 2 : RUN apt-get update && apt-get install -y ruby ruby-dev
	 ---> Running in 3a2558904e9b
	Selecting previously unselected package libasan0:amd64.
	(Reading database ... 11518 files and directories currently installed.)
	Preparing to unpack .../libasan0_4.8.2-19ubuntu1_amd64.deb ...
	Unpacking libasan0:amd64 (4.8.2-19ubuntu1) ...
	Selecting previously unselected package libatomic1:amd64.
	Preparing to unpack .../libatomic1_4.8.2-19ubuntu1_amd64.deb ...
	Unpacking libatomic1:amd64 (4.8.2-19ubuntu1) ...
	Selecting previously unselected package libgmp10:amd64.
	Preparing to unpack .../libgmp10_2%3a5.1.3+dfsg-1ubuntu1_amd64.deb ...
	Unpacking libgmp10:amd64 (2:5.1.3+dfsg-1ubuntu1) ...
	Selecting previously unselected package libisl10:amd64.
	Preparing to unpack .../libisl10_0.12.2-1_amd64.deb ...
	Unpacking libisl10:amd64 (0.12.2-1) ...
	Selecting previously unselected package libcloog-isl4:amd64.
	Preparing to unpack .../libcloog-isl4_0.18.2-1_amd64.deb ...
	Unpacking libcloog-isl4:amd64 (0.18.2-1) ...
	Selecting previously unselected package libgomp1:amd64.
	Preparing to unpack .../libgomp1_4.8.2-19ubuntu1_amd64.deb ...
	Unpacking libgomp1:amd64 (4.8.2-19ubuntu1) ...
	Selecting previously unselected package libitm1:amd64.
	Preparing to unpack .../libitm1_4.8.2-19ubuntu1_amd64.deb ...
	Unpacking libitm1:amd64 (4.8.2-19ubuntu1) ...
	Selecting previously unselected package libmpfr4:amd64.
	Preparing to unpack .../libmpfr4_3.1.2-1_amd64.deb ...
	Unpacking libmpfr4:amd64 (3.1.2-1) ...
	Selecting previously unselected package libquadmath0:amd64.
	Preparing to unpack .../libquadmath0_4.8.2-19ubuntu1_amd64.deb ...
	Unpacking libquadmath0:amd64 (4.8.2-19ubuntu1) ...
	Selecting previously unselected package libtsan0:amd64.
	Preparing to unpack .../libtsan0_4.8.2-19ubuntu1_amd64.deb ...
	Unpacking libtsan0:amd64 (4.8.2-19ubuntu1) ...
	Selecting previously unselected package libyaml-0-2:amd64.
	Preparing to unpack .../libyaml-0-2_0.1.4-3ubuntu3_amd64.deb ...
	Unpacking libyaml-0-2:amd64 (0.1.4-3ubuntu3) ...
	Selecting previously unselected package libmpc3:amd64.
	Preparing to unpack .../libmpc3_1.0.1-1ubuntu1_amd64.deb ...
	Unpacking libmpc3:amd64 (1.0.1-1ubuntu1) ...
	Selecting previously unselected package openssl.
	Preparing to unpack .../openssl_1.0.1f-1ubuntu2.4_amd64.deb ...
	Unpacking openssl (1.0.1f-1ubuntu2.4) ...
	Selecting previously unselected package ca-certificates.
	Preparing to unpack .../ca-certificates_20130906ubuntu2_all.deb ...
	Unpacking ca-certificates (20130906ubuntu2) ...
	Selecting previously unselected package manpages.
	Preparing to unpack .../manpages_3.54-1ubuntu1_all.deb ...
	Unpacking manpages (3.54-1ubuntu1) ...
	Selecting previously unselected package binutils.
	Preparing to unpack .../binutils_2.24-5ubuntu3_amd64.deb ...
	Unpacking binutils (2.24-5ubuntu3) ...
	Selecting previously unselected package cpp-4.8.
	Preparing to unpack .../cpp-4.8_4.8.2-19ubuntu1_amd64.deb ...
	Unpacking cpp-4.8 (4.8.2-19ubuntu1) ...
	Selecting previously unselected package cpp.
	Preparing to unpack .../cpp_4%3a4.8.2-1ubuntu6_amd64.deb ...
	Unpacking cpp (4:4.8.2-1ubuntu6) ...
	Selecting previously unselected package libgcc-4.8-dev:amd64.
	Preparing to unpack .../libgcc-4.8-dev_4.8.2-19ubuntu1_amd64.deb ...
	Unpacking libgcc-4.8-dev:amd64 (4.8.2-19ubuntu1) ...
	Selecting previously unselected package gcc-4.8.
	Preparing to unpack .../gcc-4.8_4.8.2-19ubuntu1_amd64.deb ...
	Unpacking gcc-4.8 (4.8.2-19ubuntu1) ...
	Selecting previously unselected package gcc.
	Preparing to unpack .../gcc_4%3a4.8.2-1ubuntu6_amd64.deb ...
	Unpacking gcc (4:4.8.2-1ubuntu6) ...
	Selecting previously unselected package libc-dev-bin.
	Preparing to unpack .../libc-dev-bin_2.19-0ubuntu6_amd64.deb ...
	Unpacking libc-dev-bin (2.19-0ubuntu6) ...
	Selecting previously unselected package linux-libc-dev:amd64.
	Preparing to unpack .../linux-libc-dev_3.13.0-30.55_amd64.deb ...
	Unpacking linux-libc-dev:amd64 (3.13.0-30.55) ...
	Selecting previously unselected package libc6-dev:amd64.
	Preparing to unpack .../libc6-dev_2.19-0ubuntu6_amd64.deb ...
	Unpacking libc6-dev:amd64 (2.19-0ubuntu6) ...
	Selecting previously unselected package ruby.
	Preparing to unpack .../ruby_1%3a1.9.3.4_all.deb ...
	Unpacking ruby (1:1.9.3.4) ...
	Selecting previously unselected package ruby1.9.1.
	Preparing to unpack .../ruby1.9.1_1.9.3.484-2ubuntu1_amd64.deb ...
	Unpacking ruby1.9.1 (1.9.3.484-2ubuntu1) ...
	Selecting previously unselected package libruby1.9.1.
	Preparing to unpack .../libruby1.9.1_1.9.3.484-2ubuntu1_amd64.deb ...
	Unpacking libruby1.9.1 (1.9.3.484-2ubuntu1) ...
	Selecting previously unselected package manpages-dev.
	Preparing to unpack .../manpages-dev_3.54-1ubuntu1_all.deb ...
	Unpacking manpages-dev (3.54-1ubuntu1) ...
	Selecting previously unselected package ruby1.9.1-dev.
	Preparing to unpack .../ruby1.9.1-dev_1.9.3.484-2ubuntu1_amd64.deb ...
	Unpacking ruby1.9.1-dev (1.9.3.484-2ubuntu1) ...
	Selecting previously unselected package ruby-dev.
	Preparing to unpack .../ruby-dev_1%3a1.9.3.4_all.deb ...
	Unpacking ruby-dev (1:1.9.3.4) ...
	Setting up libasan0:amd64 (4.8.2-19ubuntu1) ...
	Setting up libatomic1:amd64 (4.8.2-19ubuntu1) ...
	Setting up libgmp10:amd64 (2:5.1.3+dfsg-1ubuntu1) ...
	Setting up libisl10:amd64 (0.12.2-1) ...
	Setting up libcloog-isl4:amd64 (0.18.2-1) ...
	Setting up libgomp1:amd64 (4.8.2-19ubuntu1) ...
	Setting up libitm1:amd64 (4.8.2-19ubuntu1) ...
	Setting up libmpfr4:amd64 (3.1.2-1) ...
	Setting up libquadmath0:amd64 (4.8.2-19ubuntu1) ...
	Setting up libtsan0:amd64 (4.8.2-19ubuntu1) ...
	Setting up libyaml-0-2:amd64 (0.1.4-3ubuntu3) ...
	Setting up libmpc3:amd64 (1.0.1-1ubuntu1) ...
	Setting up openssl (1.0.1f-1ubuntu2.4) ...
	Setting up ca-certificates (20130906ubuntu2) ...
	debconf: unable to initialize frontend: Dialog
	debconf: (TERM is not set, so the dialog frontend is not usable.)
	debconf: falling back to frontend: Readline
	debconf: unable to initialize frontend: Readline
	debconf: (This frontend requires a controlling tty.)
	debconf: falling back to frontend: Teletype
	Setting up manpages (3.54-1ubuntu1) ...
	Setting up binutils (2.24-5ubuntu3) ...
	Setting up cpp-4.8 (4.8.2-19ubuntu1) ...
	Setting up cpp (4:4.8.2-1ubuntu6) ...
	Setting up libgcc-4.8-dev:amd64 (4.8.2-19ubuntu1) ...
	Setting up gcc-4.8 (4.8.2-19ubuntu1) ...
	Setting up gcc (4:4.8.2-1ubuntu6) ...
	Setting up libc-dev-bin (2.19-0ubuntu6) ...
	Setting up linux-libc-dev:amd64 (3.13.0-30.55) ...
	Setting up libc6-dev:amd64 (2.19-0ubuntu6) ...
	Setting up manpages-dev (3.54-1ubuntu1) ...
	Setting up libruby1.9.1 (1.9.3.484-2ubuntu1) ...
	Setting up ruby1.9.1-dev (1.9.3.484-2ubuntu1) ...
	Setting up ruby-dev (1:1.9.3.4) ...
	Setting up ruby (1:1.9.3.4) ...
	Setting up ruby1.9.1 (1.9.3.484-2ubuntu1) ...
	Processing triggers for libc-bin (2.19-0ubuntu6) ...
	Processing triggers for ca-certificates (20130906ubuntu2) ...
	Updating certificates in /etc/ssl/certs... 164 added, 0 removed; done.
	Running hooks in /etc/ca-certificates/update.d....done.
	 ---> c55c31703134
	Removing intermediate container 3a2558904e9b
	Step 3 : RUN gem install sinatra
	 ---> Running in 6b81cb6313e5
	unable to convert "\xC3" to UTF-8 in conversion from ASCII-8BIT to UTF-8 to US-ASCII for README.rdoc, skipping
	unable to convert "\xC3" to UTF-8 in conversion from ASCII-8BIT to UTF-8 to US-ASCII for README.rdoc, skipping
	Successfully installed rack-1.5.2
	Successfully installed tilt-1.4.1
	Successfully installed rack-protection-1.5.3
	Successfully installed sinatra-1.4.5
	4 gems installed
	Installing ri documentation for rack-1.5.2...
	Installing ri documentation for tilt-1.4.1...
	Installing ri documentation for rack-protection-1.5.3...
	Installing ri documentation for sinatra-1.4.5...
	Installing RDoc documentation for rack-1.5.2...
	Installing RDoc documentation for tilt-1.4.1...
	Installing RDoc documentation for rack-protection-1.5.3...
	Installing RDoc documentation for sinatra-1.4.5...
	 ---> 97feabe5d2ed
	Removing intermediate container 6b81cb6313e5
	Successfully built 97feabe5d2ed


我们使用 `docker build` 命令并指定 `-t` 标识(flag)来标示属于 `ouruser` ，镜像名称为 `sinatra `,标签是 `v2`。

如果 `Dockerfile` 在我们当前目录下，我们可以使用 `.` 来指定 `Dockerfile`

>提示：你也可以指定 `Dockerfile` 路径

===
翻译到这里了Now we can see the build process at work
===

现在我们可以看到构建过程。docker做的第一件事是通过你的上下文构建。基本上是目录的内容构建。docker会根据本地的内容来在docker进程中去构建。

下一步，我们`Dockerfile`一步一步执行命令。我们可以看到，每个步骤可以创建一个新的容器，在容器内运行指令并且提交改变，就像我们早期看到的`docker commit`流程、当所有的指令执行完成之后，我们就会得到`324104cde6ad `镜像（有助于标记ouruser/sinatra:v2）,然后所有中间容器会被删除干净。

我们可以从我们的新镜像中创建一个容器：

	$ sudo docker run -t -i ouruser/sinatra:v2 /bin/bash
	root@8196968dac35:/#

>注意：这是比较简单的创建镜像方法。我们跳过了你可以使用的一大堆指令。在后面的部门我们将会看到更多的指令指南，或者你可以参考`Dockerfile`参考的例子和详细描述每一个指令。

###设置镜像标签

你可以给现有的镜像添加标记，然后提交和构建。我们可以使用`docker tag`命令。让我们给`ouruser/sinatra`镜像添加一个新的标签。

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
