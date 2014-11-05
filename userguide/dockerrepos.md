使用Docker Hub
===

现在你已经学习了如何利用命令行在本地运行Docker，还学习了如何[拉取镜像](usingdocker.md)用于从现成的镜像中构建容器，并且还学习了如何[创建自己的镜像](dockerimages.md)。

接下来，你将会学到如何利用[Docker Hub](https://hub.docker.com)简化和提高你的Docker工作流程。

[Docker Hub](https://hub.docker.com)是一个由Docker公司负责维护的公共注册中心，它包含了超过15,000个可用来下载和构建容器的镜像，并且还提供认证、工作组结构、工作流工具（比如webhooks）、构建触发器以及私有工具（比如私有仓库可用于存储你并不想公开分享的镜像）。

###Docker命令和Docker Hub

Docker通过`docer search`、`pull`、`login`和`push`等命令提供了连接Docker Hub服务的功能，本页将展示这些命令如何工作的。

###账号注册和登陆

一般，你需要先在docker中心创建一个账户（如果您尚未有）。你可以直接在[Docker Hub](https://hub.docker.com)创建你的账户，或通过运行：

	$ sudo docker login

这将提示您输入用户名，这个用户名将成为你的公共存储库的命名空间名称。如果你的名字可用，docker会提示您输入一个密码和你的邮箱，然后会自动登录到Docker Hub，你现在可以提交和推送镜像到Docker Hub的你的存储库。

>注：你的身份验证凭证将被存储在你本地目录的`.dockercfg`文件中。

###搜索镜像

你可以通过使用搜索接口或者通过使用命令行接口在Docker Hub中搜索，可对镜像名称、用户名或者描述等进行搜索：

	$ sudo docker search centos
	NAME           DESCRIPTION                                     STARS     OFFICIAL   TRUSTED
	centos         Official CentOS 6 Image as of 12 April 2014     88
	tianon/centos  CentOS 5 and 6, created using rinse instea...   21
	...

这里你可以看到两个搜索的示例结果：`centos`和`tianon/centos`。第二个结果是从名为`tianon/`的用户仓储库搜索到的，而第一个结果`centos`没有用户空间这就意味着它是可信的顶级命名空间。`/`字符分割用户镜像和存储库的名称。

当你发现你想要的镜像时，便可以用`docker pull <imagename>`来下载它。

	$ sudo docker pull centos
	Pulling repository centos
	0b443ba03958: Download complete
	539c0211cd76: Download complete
	511136ea3c5a: Download complete
	7064731afe90: Download complete

现在你有一个镜像，基于它你可以运行容器。

###向Docker Hub贡献

任何人都可以从`Docker Hub`仓库下载镜像，但是如果你想要分享你的镜像，你就必须先注册，就像你在[第一部分的docker用户指南](dockerhub.md)看到的一样。

###推送镜像到Docker Hub

为了推送到仓库的公共注册库中，你需要一个命名的镜像或者将你的容器提到为一个命名的镜像，正像[这里](docerimages.md)我们所看到的。

你可以将此仓库推送到公共注册库中，并以镜像名字或者标签来对其进行标记。

	$ sudo docker push yourname/newimage

镜像上传之后你的团队或者社区的人都可以使用它。

###Docker Hub特征

让我们再进一步看看Docker Hub的特色，[这里](http://docs.docker.com/docker-hub/)你可以看到更多的信息。

* 私有仓库
* 组织和团队
* 自动构建
* Webhooks

###私有仓库

有时候你不想公开或者分享你的镜像，所以Docker Hub允许你有私有仓库，你可以在[这里](https://registry.hub.docker.com/plans/)登录设置它。

###组织和机构

私人仓库一个较有用的地方在于你可以将仓库分享给你团队或者你的组织。Docker Hub支持创建组织，这样你可以和你的同事来管理你的私有仓库，在[这里](https://registry.hub.docker.com/account/organizations/)你可以学到如何创建和管理一个组织。

###自动构建

自动构建功能会自动从[Github](https://www.github.com/)和[BitBucket](http://bitbucket.com/)直接将镜像构建或更新至Docker Hub，通过为Github或Bitbucket的仓库添加一个提交的hook来实现，当你推送提交的时候就会触发构建和更新。

设置一个自动化构建你需要：

* 1.创建一个[Docker Hub](https://hub.docker.com/)账户并且登陆
* 2.通过[Link Accounts](https://registry.hub.docker.com/account/accounts/)菜单连接你的GitHub或者BitBucket
* 3.[配置自动化构建](https://registry.hub.docker.com/builds/add/)
* 4.选择一个包含`Dockerfile`的Github或BitBucket项目
* 5.选择你想用于构建的分支（默认是`master`分支）
* 6.给自动构建创建一个名称
* 7.指定一个Docker标签来构建
* 8.指定`Dockerfile`的路径，默认是`/`。

一旦配置好自动构建，在几分钟内就会自动触发构建，你就会在[Docker Hub](https://hub.docker.com/)仓库源看到你新的构建，并且它将会和你的Github或者BitBucket保持同步更新直到你解除自动构建。

如果你想看到你自动化构建的状态，你可以去你的Docker Hub[自动化构建页面](https://registry.hub.docker.com/builds/)，它将会想你展示你构建的状态和构建历史。

一旦你创建了一个自动化构建，你可以禁用或删除它。但是，你不能通过`docker push`推送一个自动化构建，而只能通过在Github或者BitBucket提交你的代码来管理它。

你可以在一个仓库中创建多个自动构建，配置它们只指定的`Dockerfile`或Git 分支。

###构建触发器

自动构建也可以通过Docker Hub的Url来触发，这样你就可以通过命令重构自动构建镜像。

###Webhooks

webhooks属于你的存储库的一部分，当一个镜像更新或者推送到你的存储库时允许你触发一个事件。当你的镜像被推送的时候，webhook可以根据你指定的url和一个有效的Json来递送。

###下一步

去使用Docker!


