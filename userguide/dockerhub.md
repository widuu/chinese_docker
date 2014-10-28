开始使用Docker Hub
===

现在你已经学习了如何利用命令行在本地运行Docker，还学习了如何[拉取镜像](usingdocker.md)用于从现成的镜像中构建容器，并且还学习了如何[创建镜像](dockerimages.md)。

接下来，你将会学到如何利用[Docker Hub](https://hub.docker.com)简化和增强你的Docker工作流。

[Docker Hub](https://hub.docker.com)是一个由Docker公司负责维护的公共注册中心，它包含了超过15,000个可用来下载和构建容器的镜像，并且还提供认证、工作组结构、工作流工具（比如webhooks）、构建触发器以及私有工具（比如私有仓库可用于存储你并不想公开分享的镜像）。

### Docker命令和Docker Hub

Docker通过`docer search`、`pull`、`login`和`push`等命令提供了连接Docker Hub服务的功能，本页将展示这些命令如何工作的。

Docker Hub和docker及其组件一起工作。Docker Hub会帮助你和你的同事协作，并获取功能最全的docker。要做到这一点，它提供的服务有：

- Docker镜像主机
- 用户认证
- 自动镜像构建和工作流程工具，如构建触发器和webhook
- 整合了GitHub和BitBucket

为了使用Docker Hub,首先需要注册创建一个账户。别担心，创建一个账户很简单并且是免费的。

###创建Docker Hub账户

这里有两种访问可以创建和注册一个Docker Hub账户：

- 1.通过网站，或者
- 2.通过命令行

####通过网站注册

填写[注册表单](https://hub.docker.com/account/signup/)，选择您的用户名和密码并指定您的电子邮箱。你也可以报名参加docker邮件列表，会有很多关于docker的信息。

![](../images/register-web.png)

####通过命令行

你可以创建一个Docker Hub账号通过命令行`docker login`命令

	$ sudo docker login

####确认你的邮箱

一旦你填写表格，查看你的电子邮件，欢迎信息，确认来激活您的账户。

![](../images/register-confirm.png)

####登陆

在完成确认过程之后，您可以使用web控制台登陆

![](../images/login-web.png)

获取通过命令行`docker login`命令：

	$ sudo docker login

你的Docker账户现在已经激活，你可以使用了。

###下一步

下一步，让我们开始学习如何在docker中运行"hello word"的应用。

阅读[Docker中运行应用](dockerizing.md)

	
