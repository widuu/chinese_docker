Docker Hub上的仓库和镜像
===
![仓库镜像](../images/repos.png)

###搜索存储库和镜像

你可以使用Docker来搜索所有公开可用的存储库和镜像。

	$ docker search ubuntu

这将通过docker提供的关键字匹配来显示您可用的存储库列表。

如果是一个私有仓库将不会显示到仓库搜索结果上。你可以通过Docker Hub的简况页面来查看仓库的状态。

###Repositories

你的Docker Hub 存储库有许多特性

###stars

你的存储库可以用星被标记，你也可以用星标记别的存储库。Stars也是显示你喜欢这个存储库的一种方法。他们也是一种简单方法来标记你喜欢的存储库。

###Comments

你可以与其他docker社区的成员和维护者留下评论。如果你发现评论不合适，你可以标记他们以供审核。

###合作者及其作用

合作者是你想给一个能够访问你的私人存储库。一旦指定，他们可以`push`和`pull`你的仓库。他们将不会被允许执行任何管理任务，如从私人存储库删除或者改变其状态。


>注：一个合作者不能添加其他合作者。只有仓库的所有者才有管理权限。

你可以再Docker Hub的组织和团队协作。你可以再这里越多到更多。

###官方仓库

Docker Hub包含了许多官方存储库。这些都是docker供应商和docker贡献者提供的认证库。docker镜像包含了规范供应商，如Oracle和Red Hat,您可以使用它们来构建应用程序和服务。

如果您使用官方库，会提供支持、优化到最新的镜像来给你的镜像提供动力。

>注：如果你的组织、产品或者团队想要给官方资源库做共享。可以再这里看到更多信息。

###私有Docker仓库

私人存储库允许你存储你想要私有的镜像，前提是你有个账户或者是组织或群组成员。

要使用Docker Hub私有存储库工作，你需要通过私有连接添加。你的Docker Hub账户会免费获得一个私人存储库。如果你需要更多的账户，你需要升级你的Docker Hub计划。

当你私有存储库建立好后，你可以使用docker来`push`和`pull`你的镜像。

>注：你需要获得签名才有权限来访问你的私人存储库。

私有仓库非常想公共仓库。然而，但是不可能浏览或者搜索到它们的存储库注册表。他们不会以同样的方式去缓存到公共存储库中。

在设置页面你可以指定哪些人来访问（合作者），在这里你可以切换存储库状态（公共到私有，或者反过来）你需要有一个可用的私有存储库，插槽属于打开，然后才能做这样的转换。如果你没有可用的。你可以升级你的Docker Hub计划。

###webhooks

您可以配置你的存储库页面来设置配置你的webhooks。成功`push`以后才被称为`webhook`.webhooks会调用HTTP POST请求一个json，类似如下所示的例子。

>作为测试你可以使用http请求工具，例如[requestb.in.](http://requestb.in/)

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

Webhooks允许你通知人，你的镜像或者存储库中服务和其它应用有新的更新。


