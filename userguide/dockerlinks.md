连接容器
===

在使用[ Docker 部分](usingdocker.md), 我们谈到了如何通过网络端口来访问运行在 Docker 容器内的服务。这是与docker容器内运行应用程序交互的一种方法。在本节中，我们打算通过端口连接到一个docker容器，并向您介绍容器连接概念。

###网络端口映射

在[使用docker](usingdocker.md)部分,我们创建了一个python应用的容器。

	$ sudo docker run -d -P training/webapp python app.py

>注：容器有一个内部网络和IP地址（在使用Docker部分我们使用`docker inspect`命令显示容器的IP地址）。Docker可以有各种网络配置方式。你可以再这里学到更多docker网络信息。

我们使用`-P`标记创建一个容器，将容器的内部端口随机映射到主机的高端口49000到49900。这时我们可以使用`docker ps`来看到端口5000绑定主机端口49155。

	$ sudo docker ps nostalgic_morse
	CONTAINER ID  IMAGE                   COMMAND       CREATED        STATUS        PORTS                    NAMES
	bc533791f3f5  training/webapp:latest  python app.py 5 seconds ago  Up 2 seconds  0.0.0.0:49155->5000/tcp  nostalgic_morse

我们也可以使用`-p`标识来指定容器端口绑定到主机端口

	$ sudo docker run -d -p 5000:5000 training/webapp python app.py

我们看这为什么不是一个好的主意呢？因为它限制了我们容器的一个端口。

我们还有很多设置`-p`标识的方法。默认`-p`标识会绑定本地主机上的指定端口。并且我们可以指定绑定的网络地址。举例设置`localhost`

	$ sudo docker run -d -p 127.0.0.1:5001:5002 training/webapp python app.py

这将绑定容器内部5002端口到主机的`localhost`或者`127.0.0.1`的5001端口。

如果要绑定容器端口5002到宿主机动态端口，并且让`localhost`访问，我们可以这样做：

	$ sudo docker run -d -p 127.0.0.1::5002 training/webapp python app.py

我们也可以绑定UDP端口，我们可以在后面添加`/udp`,举例：

	$ sudo docker run -d -p 127.0.0.1:5000:5000/udp training/webapp python app.py

我们可以使用`docker port`快捷方式来绑定我们的端口，这有助于向我们展示特定的端口。例如我们绑定`localhost`,如下是`docker port`输出：

	$ docker port nostalgic_morse 5000
	127.0.0.1:49155

>注：`-p`可以使用多次配置多个端口。

###Docker容器连接

端口映射并不是唯一把docker连接到另一个容器的方法。docker有一个连接系统允许将多个容器连接在一起，共享连接信息。docker连接会创建一个父子关系，其中父容器可以看到子容器的信息。

###容器命名

执行此连接需要依靠你的docker的名字，这里我们可以看到当我们创建每一个容器的时候，它都会自动被命名。事实上我们已经熟悉了老的`nostalgic_morse`指南。你也可以自己命名容器。这种命名提供了两个有用的功能：

- 1.给容器特定的名字使你更容易记住他们，例如：命名web应用程序为web容器。
- 2.它为docker提供一个参考，允许其他容器引用，举例连接web容器到db容器。

你可以使用`--name`标识来命名容器，举例：

	$ sudo docker run -d -P --name web training/webapp python app.py

我们可以看到我们启动了的容器，就是我们使用`--name`标识命名为`web`的容器。我们可以使用`docker ps`命令来查看容器名称。

	$ sudo docker ps -l
	CONTAINER ID  IMAGE                  COMMAND        CREATED       STATUS       PORTS                    NAMES
	aed84ee21bde  training/webapp:latest python app.py  12 hours ago  Up 2 seconds 0.0.0.0:49154->5000/tcp  web

我们也可以使用`docker inspect`来返回容器名字。

	$ sudo docker inspect -f "{{ .Name }}" aed84ee21bde
	/web

>注：容器的名称必须是唯一的。这意味着你只能调用一个web容器。如果你想使用重复的名称来命名容器，你需要使用`docker rm`命令删除以前的容器。在容器停止后删除。

###容器连接

连接允许容器之间可见并且安全地进行通信。使用`--link`创建连接。我们创建一个新容器，这个容器是数据库。

	$ sudo docker run -d --name db training/postgres

这里我们使用`training/postgres`容器创建一个新的容器。容器是PostgreSQL数据库。

现在我们创建一个`web`容器来连接`db`容器。

	$ sudo docker run -d -P --name web --link db:db training/webapp python app.py

这将使我们的web容器和db容器连接起来。`--link`的形式

	--link name:alias

`name`是我们连接容器的名字，`alias`是link的别名。让我们看如何使用alias。

让我们使用`docker ps`来查看容器连接.

	$ docker ps
	CONTAINER ID  IMAGE                     COMMAND               CREATED             STATUS             PORTS                    NAMES
	349169744e49  training/postgres:latest  su postgres -c '/usr  About a minute ago  Up About a minute  5432/tcp                 db
	aed84ee21bde  training/webapp:latest    python app.py         16 hours ago        Up 2 minutes       0.0.0.0:49154->5000/tcp  db/web,web

我们可以看到我们命名的容器，`db`和`web`，我们还在名字列中可以看到web容器也显示`db/web`。这告诉我们web容器和db容器是父/子关系。

我们连接容器做什么？我们发现连接的两个容器是父子关系。这里的父容器是`db`可以访问子容器`web`。为此docker在容器之间打开一个安全连接隧道不需要暴露任何端口在容器外部。你会注意到当你启动db容器的时候我们没有使用`-P`或者`-p`标识。我们连接容器的时候我们不需要通过网络给PostgreSQL数据库开放端口。

Docker在父容器中开放子容器连接信息有两种方法：

- 环境变量
- 更新`/etc/hosts`文件。

让我们先看看docker的环境变量。我们运行`env`命令来查看列表容器的环境变量。

 	$ sudo docker run --rm --name web2 --link db:db training/webapp env
   	 	. . .
    	DB_NAME=/web2/db
    	DB_PORT=tcp://172.17.0.5:5432
    	DB_PORT_5000_TCP=tcp://172.17.0.5:5432
    	DB_PORT_5000_TCP_PROTO=tcp
    	DB_PORT_5000_TCP_PORT=5432
    	DB_PORT_5000_TCP_ADDR=172.17.0.5
   		. . .

>注：这些环境变量只设置顶一个进程的容器。同样，一些守护进程(例如sshd)进行shell连接时就会去除。

我们可以看到docker为我们的数据库容器创建了一系列环境变量。每个前缀变量是`DB_`填充我们指定的别名。如果我们的别名是`db1`，前缀别名就是`DB1_`。您可以使用这些环境变量来配置您的应用程序连接到你的数据库db容器。该连接时安全、私有的，只能在web容器和db容器之间通信。

docker除了环境变量，可以添加信息到父主机的`/etc/hosts`

	root@aed84ee21bde:/opt/webapp# cat /etc/hosts
	172.17.0.7  aed84ee21bde
	. . .
	172.17.0.5  db

这里我们可以看到两个主机项。第一项是web容器用容器ID作为主机名字。第二项是使用别名引用IP地址连接数据库容器。现在我们试试ping这个主机：

	root@aed84ee21bde:/opt/webapp# apt-get install -yqq inetutils-ping
	root@aed84ee21bde:/opt/webapp# ping db
	PING db (172.17.0.5): 48 data bytes
	56 bytes from 172.17.0.5: icmp_seq=0 ttl=64 time=0.267 ms
	56 bytes from 172.17.0.5: icmp_seq=1 ttl=64 time=0.250 ms
	56 bytes from 172.17.0.5: icmp_seq=2 ttl=64 time=0.256 ms

>注：我们不得不安装`ping`，因为容器内没有它。

我们使用`ping`命令来ping`db`容器，使用它解析到`127.17.0.5`主机。我们可以利用这个主机项配置应用程序来使用我们的`db`容器。

>注：你可以使用一个父容器连接多个子容器。例如，我们可以有多个web容器连接到我们的db数据库容器。

###下一步

现在我们已经学会如何连接容器了，下一步我们学习融合管理数据、卷标和如果在容器内挂载。

阅读[管理容器数据](dockervolumes.md)
