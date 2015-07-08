安装 Compose
===

在安装 Compose之前，你需要先安装好 Docker 。然后你需要使用 `curl` 指令来安装 Compose

###安装 Docker


首先，你需要安装大于或者等于1.6版本的 Docker 。

- [MAC OSX 安装指南](../installation/mac.md)
- [Ubuntu  安装指南](../installation/ubuntu.md)
- [其它系统安装指南](../installation/)


###安装 Compose

运行下边的命令来安装 Compose：

	curl -L https://github.com/docker/compose/releases/download/1.3.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
	chmod +x /usr/local/bin/docker-compose

>注意：如果你在安装的时候出现了  “Permission denied” 的错误信息，这说明你的 `/usr/local/bin` 目录是不可写的，你需要使用超级用户来安装。运行 `sudo -i` , 然后运行上边的两个命令，然后 `exit` 退出。

可选，你也可以在 shell 中使用命令行安装。

Compose 适用于 OS X 和 64位的Linux 。 如果你使用其他平台，你可以安装一个 Compose 的 Python 包来完成安装。

	$ sudo pip install -U docker-compose


到这里安装就结束了；Compose已经安装完成。你可以使用 ` docker-compose --version` 来进行测试 。

###升级

如果你使用的是 Compose 1.2或者早期版本，当你升级完成后，你需要删除或者迁移你现有的容器。这是因为，1.3版本， Composer 使用 Docker 标签来对容器进行检测，所以它们需要重新创建索引标记。

如果 Composer 检测到创建的容器没有标签，它将拒绝运行，这样你就不会有两组容器。如果你想要保留已经存在的容器（举例：这里有容器的数据卷上保留这非常重要的数据），你可以使用下边的命令来迁移：

	docker-compose migrate-to-labels

或者，如果这些容器是不必要的，你可以删除它们 - Composer 会重新创建一个新的。

	docker rm -f myapp_web_1 myapp_db_1 ...


###Compose 文档
	
+ [用户指南](../compose/)
+ [使用命令行](../compose/cli.md)
+ [Yaml 文件使用](../compose/yml.md)
+ [Compose 环境变量](../compose/env.md)
+ [命令行安装Compose](../compose/completion.md)