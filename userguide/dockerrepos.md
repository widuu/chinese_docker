使用Docker Hub
===

到现在为止，你已经学会了在你的主机上使用Docker命令。你已经学会如何推送或者下载已经存在的镜像，你已经学会了如何创建你自己的镜像。

接下来，你将要学到如何使用`Docker HUB`来简化和提高你的工作流程。

`Docker Hub`是docker公司提供的公共注册中心，它包含了15,000个镜像，你可以下载它来创建容器。它还提供了身份验证、工作组结构、工作流工具像`webhooks`和创建触发器，还有私人仓库来存储你不想分享的镜像。

###Docker命令和Docker Hub

Docker本身就提供Docker Hub服务通过使用`docker search`、`pull`、`login`和`push`命令。这个页面将向您展示如何使用这些命令。

###账号注册和登陆

首先，你需要先在docker中心创建一个账户（如果您尚未有）。你可以直接在Docker Hub创建你的账户，或通过运行：

	$ sudo docker login

这将提示您输入用户名，这将成为你的公共存储库的空间名称。如果你的名字可用，docker会提示您输入一个密码和你的邮箱。然后他会自动记录下。你现在可以提交和推送你的镜像到Docker Hub的你的存储库。

注：你的身份验证凭证将被存储在你本地目录的`.dockercfg`文件中。

###搜索镜像

你可以通过使用搜索接口或者通过使用命令行接口在Docker Hub中搜素。搜素镜像使用镜像名称、用户名或者描述：

	$ sudo docker search centos
	NAME           DESCRIPTION                                     STARS     OFFICIAL   TRUSTED
	centos         Official CentOS 6 Image as of 12 April 2014     88
	tianon/centos  CentOS 5 and 6, created using rinse instea...   21
	...

这里你可以看到两个例子：`centos`和`tianon/centos`。第二个结果是用户仓储库，名称空间是`tianon/`，而第一个结构`centos`没有用户空间这就意味着它是可信的顶级名称空间。`/`字符分割用户镜像和存储库的名称。

当你发现你想要的镜像。你就是用`docker pull <imagename>`来下载它。

	$ sudo docker pull centos
	Pulling repository centos
	0b443ba03958: Download complete
	539c0211cd76: Download complete
	511136ea3c5a: Download complete
	7064731afe90: Download complete

现在你有一个镜像。你可以在容器中运行它。

###像Docker Hub贡献

任何人都可以从`Docker Hub`仓库下载镜像。但是如果你想要分享你的镜像，你就必须先注册，这里你可以看[第一部分的docker用户指南](dockerhub.md)

###推送镜像到Docker Hub





