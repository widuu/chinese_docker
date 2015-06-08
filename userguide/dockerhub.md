开始使用 Docker Hub
===

本节讲述了 Docker Hub 的快速入门,包括如何创建一个账户。

Docker Hub 存放着 Docker 及其组件的所有资源。Docker Hub 可以帮助你与同事之间协作，并获得功能完整的 Docker。为此，它提供的服务有：

- Docker 镜像主机
- 用户认证
- 自动镜像构建和工作流程工具，如构建触发器和 web hooks
- 整合了 GitHub 和 BitBucket 

为了使用 Docker Hub ,首先需要注册创建一个账户。别担心，创建一个账户很简单并且是免费的。

##创建 Docker Hub 账户

这里有两种访问可以创建和注册一个 Docker Hub 账户：

- 1.通过网站，或者
- 2.通过命令行

###通过网站注册

填写[注册表单](https://hub.docker.com/account/signup/)，选择您的用户名和密码并指定您的电子邮箱。你也可以报名参加 Docker 邮件列表，会有很多关于 Docker 的信息。

![](../images/register-web.png)

### 通过命令行

你可以通过使用命令行输入 `docker login` 命令来创建一个 Docker Hub 账号

	$ sudo docker login

### 邮箱确认

一旦你填写完毕表格，请查看你的电子邮件，通过点击欢迎信息中的链接来激活您的账户。

![](../images/register-confirm.png)

### 登陆

在完成确认过程之后，您可以使用web控制台登陆

![](../images/login-web.png)

或者通过在命令行中输入 `docker login` 命令来登录：

	$ sudo docker login

你的 Docker 账户现在已经生效，并随时可以使用。

###下一步

下一步，让我们开始学习如何在 Docker 中运行 "hello word" 的应用。

阅读[Docker中运行应用](dockerizing.md)

	