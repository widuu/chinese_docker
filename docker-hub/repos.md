Docker Hub上的仓库和镜像
===
![仓库镜像](../images/repos.png)

##搜索仓库和镜像

你可以使用 Docker 来搜索所有公开可用的仓库和镜像。

	$ docker search ubuntu

这将通过 Docker 提供的关键字匹配来显示您可用的仓库列表。

私有仓库将不会显示到仓库搜索结果上。你可以通过 Docker Hub 的简况页面来查看仓库的状态。

##仓库

你的 Docker Hub 仓库有许多特性。

###stars

你的仓库可以用星被标记，你也可以用星标记别的仓库。Stars 也是显示你喜欢这个仓库的一种方法，也是一种简单的方法来标记你喜欢的仓库。

###评论

你可以与其他 Docker 社区的成员和维护者留下评论。如果你发现有不当的评论，你可以标记他们以供审核。

###合作者及其作用

指定的合作者可以通过你提供的权限访问你的私人仓库。一旦指定，他们可以 `push` 和 `pull` 你的仓库。但他们将不会被允许执行任何管理任务，如从删除仓库或者改变其状态。


>注：一个合作者不能添加其他合作者。只有仓库的所有者才有管理权限。

你也可以与在 Docker Hub 上的组织和团队进行协作，更多信息。

##官方仓库

Docker Hub 包含了许多[官方仓库](http://registry.hub.docker.com/official)。这些都是 Docker 供应商和 Docker 贡献者提供的认证库，他们包含了来自供应商，如 Oracle 和 Red Hat的镜像,您可以使用它们来构建应用程序和服务。

如果使用官方库，供应商会对镜像进行持续维护、升级和优化，从而为项目提供强大的驱动力。

>注：如果你的组织、产品或者团队想要给官方资源库做贡献。可以再[这里](https://github.com/docker/stackbrew)查看更多信息。

##私有Docker仓库

私人仓库用来存储你的私有镜像，前提是你需要一个 Docker 账户，或者你已经属于 Docker Hub 上的某个组织或群组。

要使用 Docker Hub 私有仓库，首先在[这里](https://registry.hub.docker.com/account/repositories/add/)进行添加。你的 Docker Hub 账户会免费获得一个私人仓库。如果你需要更多的账户，你需要升级你的 [Docker Hub 计划](https://registry.hub.docker.com/plans/)。

私有仓库建立好后，你可以使用 Docker 来 `push` 和 `pull` 你的镜像。

>注：你需要先登录并获得权限来访问你的私人仓库。

私有仓库和公共仓库基本相同，但是以公共身份是无法浏览或者搜索到私有仓库及其内容的，他们也不会以同样的方式被缓存。

在设置页面你可以指定哪些人有权限访问（如合作者），在这里你可以切换仓库状态（公共到私有，或者反过来）。你需要有一个可用的私有仓库，并开启相关设置才能做这样的转换。如果你无法进行相关操作，请升级你的 [Docker Hub 计划](https://registry.hub.docker.com/plans/)。

##Webhooks

您可以在仓库设置页面来配置你的 webhooks。只有成功 `push` 以后，`webhook` 才会生效。webhooks 会调用 HTTP POST 请求一个json，类似如下所示的例子：

>你可以使用 http 工具进行测试，例如 [requestb.in.](http://requestb.in/)

webhook json例子:


	{
	   "push_data":{
	      "pushed_at":1385141110,
	      "images":[
	         "imagehash1",
	         "imagehash2",
	         "imagehash3"
	      ],
	      "pusher":"username"
	   },
	   "repository":{
	      "status":"Active",
	      "description":"my docker repo that does cool things",
	      "is_automated":false,
	      "full_description":"This is my full description",
	      "repo_url":"https://registry.hub.docker.com/u/username/reponame/",
	      "owner":"username",
	      "is_official":false,
	      "is_private":false,
	      "name":"reponame",
	      "namespace":"username",
	      "star_count":1,
	      "comment_count":1,
	      "date_created":1370174400,
	      "dockerfile":"my full dockerfile is listed here",
	      "repo_name":"username/reponame"
	   }
	}

Webhooks 允许你将你镜像和仓库的更新信息通知指定用户、服务以及其他应用程序。


