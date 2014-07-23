在Docker中运行Reids服务
===

非常简单，没有任何修饰，redis是使用一个连接附加到一个web应用程序。

##创建一个redis docker容器

首先，我们先为redis创建一个Dockerfile

	FROM        ubuntu:12.10
	RUN         apt-get update
	RUN         apt-get -y install redis-server
	EXPOSE      6379
	ENTRYPOINT  ["/usr/bin/redis-server"]

现在你需要通过Dockerfile创建一个镜像，将<your username>替换成你自己的名字。

	sudo docker build -t <your username>/redis .
	
##运行服务

使用我们刚才创建的redis镜像

使用 -d 运行这个服务分离模式，让容器在后台运行。

重要的是我们没有开放容器端口，相反，我们将使用一个容器来连接redis容器数据库

	sudo docker run -name redis -d <your username>/redis

##创建你的web应用容器

现在我们可以创建我们的应用程序容器，我们使用-link参数来创建一个连接redis容器，我们使用别名db,这将会在redis容器和redis实例容器中创建一个安全的通信隧道

	sudo docker run -link redis:db -i -t ubuntu:12.10 /bin/bash
	
进入我们刚才创建的容器，我们需要安装redis的redis-cli的二进制包来测试连接

	apt-get update
	apt-get -y install redis-server
	service redis-server stop
	
现在我们可以测试连接，首先我么要先查看下web应用程序容器的环境变量，我们可以用我们的ip和端口来连接redis容器

	env
	. . .
	DB_NAME=/violet_wolf/db
	DB_PORT_6379_TCP_PORT=6379
	DB_PORT=tcp://172.17.0.33:6379
	DB_PORT_6379_TCP=tcp://172.17.0.33:6379
	DB_PORT_6379_TCP_ADDR=172.17.0.33
	DB_PORT_6379_TCP_PROTO=tcp
	
我们可以看到我们有一个DB为前缀的环境变量列表，DB来自指定别名连接我们的现在的容器，让我们使用DB_PORT_6379_TCP_ADDR变量连接到Redis容器。

	redis-cli -h $DB_PORT_6379_TCP_ADDR
	redis 172.17.0.33:6379>
	redis 172.17.0.33:6379> set docker awesome
	OK
	redis 172.17.0.33:6379> get docker
	"awesome"
	redis 172.17.0.33:6379> exit
	
我们可以很容易的使用这个或者其他环境变量在我们的web应用程序容器上连接到redis容器