Compose 相关命令行
=

下面将介绍 `docker-compose` 子命令的使用。也可以通过运行 `docker-compose --help`来查看这些信息。

- [build](#build)
- [help](#help)
- [kill](#kill)
- [ps](#ps)
- [restart](#restart)
- [run](#run)
- [start](#start)
- [up](#up)
- [logs](#logs)
- [port](#port)
- [pull](#pull)
- [rm](#rm)
- [scale](#scale)
- [stop](#stop)


### build
```
用法：build [options] [SERVICE...]
选项：
	--force-rm  总是移除构建过程中产生的中间项容器
	--no-cache  构建镜像过程中不使用Cache
	--pull      总是尝试获取更新版本的镜像
```
构建服务并打上`project_service`风格的标签（如：`composetest_db`）。如果你更改了服务的`Dockerfile`或者构建目录下的内容，需要运行`docker-compose build`重新构建服务。

### help
```
用法：help COMMAND
```
显示命令的帮助信息及用法教程。

### kill
```
用法：kill [options] [SERVICE...]
选项：
	-s SIGNAL         SIGNAL 是发送给容器的信号量，默认是 SIGKILL
```
通过发送`SIGKILL`信号来强制终止运行中的容器，也可以发送指定的信号量，例如：

	$ docker-compose kill -s SIGINT

### ps
```
用法：ps [options] [SERVICE...]
选项：
	-q		仅仅显示容器ID
```
列出容器。

### restart
```
用法：restart [options] [SERVICE...]
选项：
	-t, --timeout TIMEOUT      设置关闭服务的超时时间，单位为秒，默认为10
```
重启服务。

### run
```
用法：run [options] [-e KEY=VAL...] SERVICE [COMMAND] [ARGS...]
选项：
	-d                    分离模式：在后台运行容器，只打印新的容器名称
	--entrypoint CMD      覆盖镜像的入口点（CMD ...）
	-e KEY=VAL            设置环境变量，可以使用多次
	-u, --user=""         通过指定的用户名或用户id来运行
	--no-deps             不启动link连接的服务
	--rm                  运行结束后移除容器，在分离模式下将被忽略
	-p, --publish=[]      将容器暴露端口映射到主机端口
	--service-ports       通过服务映射到主机的端口执行命令
	-T                    禁用pseudo-tty分配，默认会分配一个TTY
```
对服务运行的命令。例如，以下命令启动web服务并运行bash命令

	$ docker-compose run web bash
	
`run`命令，将使用服务中已经定义的配置来创建运行一个新的容器。也就是说，如此创建的容器，将会使用相同的挂载卷、容器连接等相同的配置，但它们依旧可以存在差异。

第一个区别是，可以使用`run`命令覆盖服务中指定的运行命令。例如，`web`服务中的配置指定的运行命令为`bash`，那么`docker-compose run web python app.py`将使用`python app.py`来覆盖它。

第二个区别是，`docker-compose run`命令不会创建任何服务配置中指定的端口映射，这样可以防止多个容器映射同一端口的冲突。如果你需要使得服务的端口创建并映射到主机，需要指定`--service-ports`标记，如下：

	$ docker-compose run --service-ports web python manage.py shell
	
或者可以手动指定端口映射，和使用`docker run`一样，使用`--publish`或`-p`选项：

	$ docker-compose run --publish 8080:80 -p 2022:22 -p 127.0.0.1:2021:21 web python manage.py shell

如果启动一个带有容器连接的服务，`run`命令将首先检查连接到的服务是否已运行，如果是停止状态，将会启动它，直到所有的相关服务都处于正在运行状态，才会执行你创建的命令。例如：

	$ docker-compose run db psql -h db -U docker

这将创建一个与PostgreSQL容器`db`交互服务。

如果你不希望启动相关联容器，可以使用`--no-deps`标记：

	$ docker-compose run --no-deps web python manage.py shell

### start
```
用法：start [SERVICE...]
```
启动服务中已经存在的容器。

### up
```
用法：up [options] [SERVICE...]
选项：
	-d                     分离模式：在后台运行容器，只打印新的容器名称
	--no-color             单色输出
	--no-deps              不启动link连接的服务
	--force-recreate       强制重新创建容器，即使镜像没有任何改变。与--no-recreate会冲突
	--no-recreate          如果对应容器已经存在,不重新创建它。与--force-recreate会冲突
	--no-build             不构建镜像，即使缺失
	-t, --timeout TIMEOUT  为容器设置关闭超时时间，单位：秒 (默认为 10)
```
对服务，构建镜像、(重新)创建容器、启动容器。

该命令还将启动任何相关的且没有被启动的服务。

`docker-compose up`命令将显示所有容器的输出，命令结束时，所有容器都将关闭。运行`docker-compose up -d `将在后台启动运行容器。

如果服务中已经存在运行中的容器了，并且在容器创建后更改服务配置或者镜像，`docker-compose up`命令将会停止当前容器（保存挂载卷）并重新构建启动容器。当然，也可以通过`--no-recreate`选项来避免重新构建。

使用`--force-recreate`标记，可以强制停止并重构所有容器。

### logs
```
用法：logs [options] [SERVICE...]
选项：
	--no-color  单色输出
```
显示服务输出的日志内容。

### port
```
用法：port [options] SERVICE PRIVATE_PORT
选项：
	--protocol=proto  tcp 或 udp [默认为 tcp]
	--index=index     对应实例服务的第几个容器[默认为 1]
```
打印服务中端口绑定对应的主机端口。

### pull
```
用法：pull [options] [SERVICE...]
选项：
	--ignore-pull-failures 尽可能拉取服务，忽略拉取失败
```
拉取服务镜像。

### rm
```
用法：rm [options] [SERVICE...]
选项:
	-f, --force   强制删除，不询问确认信息
	-v            移除容器挂载的卷
```
删除停止的服务容器。

### scale
```
用法：scale [SERVICE=NUM...]
```
设置一个服务需要运行的容器数量。
参数形式为`service=num`。例如：

	$ docker-compose scale web=2 worker=3

### stop
```
用法：stop [options] [SERVICE...]
选项：
	-t, --timeout TIMEOUT      设置关闭容器的超时时间
```
停止容器而不移除，可以通`docker-compose start`重新启动。

