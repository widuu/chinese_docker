安装 Compose
===

在安装 Compose之前，你需要先安装好 Docker 。然后你需要使用 `curl` 指令来安装 Compose

###安装 Docker


首先，你需要安装大于或者等于1.3版本的 Docker 。


如果你使用 OS X 系统，你可以使用 OS X 安装程序 boot2docker 来安装 Docker。 当 boot2docker 运行起来的时候，使用下边的指令来配置 Docker 的环境变量：

	$(boot2docker shellinit)

添加上边输出的信息到 `~/.bashrc` 文件，来保存shell环境变量信息。


需要查看完整的说明文档，或者你使用的是其它平台，你可以查看 [Docker 安装文档](../install/)。

###安装 Compose

运行下边的命令来安装 Compose：

	curl -L https://github.com/docker/compose/releases/download/1.1.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
	chmod +x /usr/local/bin/docker-compose

可选，你也可以在shell中使用命令行安装。

Compose 适用于 OS X 和 64位的Linux 。 如果你使用其他平台，你可以安装一个 Compose 的 Python 包来完成安装。

	$ sudo pip install -U docker-compose


到这里安装就结束了；Compose已经安装完成。你可以使用 ` docker-compose --version` 来进行测试 。

###Compose 文档
	
+ [用户指南](../compose/)
+ [使用命令行](../compose/cli.md)
+ [Yaml 文件使用](../compose/yml.md)
+ [Compose 环境变量](../compose/env.md)
+ [命令行安装Compose](../compose/completion.md)