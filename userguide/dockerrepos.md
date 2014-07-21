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

为了推送到仓库的注册表中。你需要给你的镜像命名并且提交你命名的镜像容器，正像这里我们所看到的。

你可以把它命名为你的名字或者标签推送到存储库的注册表中。

	$ sudo docker push yourname/newimage

镜像上传之后你的团队或者社区的人都可以使用它。

###Docker Hub特征

让我们看看一些Docker Hub的特点。这里你可以看到更多的信息。

* 私人仓库
* 组织和团队
* 自动创建
* Webhooks

###私有仓库

有时候你不想你的镜像公开或者分享。所以Docker Hub允许你有私有仓库。你可以登录设置它。

###组织和机构

一个私人仓库有用的地方在于你可以分享给你团队的成员或者你的组织成员。Docker Hub会告诉你如何创建组织，你可以和你的同事来管理你的私有仓库。你可以学到如何创建和管理一个组织。

###自动化基础

Docker　Hub会自动化更新和构建来自`Github`和`BitBucket`的镜像。工作原理是添加一个GitHub或者BitBucket的仓库钩子，当你推送提交的时候就会触发构建和更新。

设置一个自动化构建

* 1.创建一个Docker Hub账户并且登陆
* 2.通过“Link Accounts”按钮连接你的GitHub或者BitBucket
* 3.配置自动化构建
* 4.选择一个`Github`和`BitBucket`项目来构建你想要构建的`Dockerfile`
* 5.选择你想建立的分支（默认是主分支）
* 6.给自动构建创建一个名称
* 7.指定一个Docker标签来构建
* 8.指定Dockerfile的路径，默认是`/`。

一旦你配置好自动构建，他就会自动触发构建，等几分钟，你就会在Docker Hub仓库源看到你新创建的自动构建。它将会和你的Github或者BitBucket保持同步更新直到你解除自动构建。

如果你想看到你自动化构建的状态，你可以去你的Docker Hub[自动化构建页面](https://registry.hub.docker.com/builds/).它将会想你展示你构建的状态和构建历史。

一旦你创建了一个自动化构建，你可以禁用和删除它。然而，你不能通过`docker push`推送一个自动化构建。你只能通过在Github或者BitBucket提交你的代码来管理它。

你可以创建多个自动化的仓库，并且配置它们只想你指定的`Dockerfile`或Git 分支。

建立触发器

你可以通过Docker Hub的Url来实现自动构建。这是满足你重新自动化构建的需求。

###Webhooks

webhooks属于你的存储库的一部分，当一个镜像更新或者推送到你的存储库时允许你触发一个时间。当你的镜像被推送的时候，webhook可以根据你指定的url和一个有效的Json来递送。

###下一步

去使用Docker!






