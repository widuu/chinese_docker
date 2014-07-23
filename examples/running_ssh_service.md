在Docker中运行SSH进程服务
===

以下是用`Dockerfile`设置sshd服务容器，您可以使用连接并检查其他容器的卷,或者可以快速访问测试容器。

	# sshd
	#
	# VERSION               0.0.1
	
	FROM     ubuntu:12.04
	MAINTAINER Thatcher R. Peskens "thatcher@dotcloud.com"
	
	# make sure the package repository is up to date
	RUN apt-get update
	
	RUN apt-get install -y openssh-server
	RUN mkdir /var/run/sshd
	RUN echo 'root:screencast' |chpasswd
	
	EXPOSE 22
	CMD    ["/usr/sbin/sshd", "-D"]

使用如下命令构建镜像：

	$ sudo docker build --rm -t eg_sshd .

然后运行它，你可以使用`docker port`来找出容器端口22映射到主机的端口

	$ sudo docker run -d -P --name test_sshd eg_sshd
	$ sudo docker port test_sshd 22
	0.0.0.0:49154

现在你可以使用ssh登陆Docker进程的主机IP地址，端口是49154(IP地址可以使用ifconfig获取)：

	$ ssh root@192.168.1.2 -p 49154
	# The password is ``screencast``.
	$$

最后，清理停止的容器，并且删除容器，然后删除镜像。

	$ sudo docker stop test_sshd
	$ sudo docker rm test_sshd
	$ sudo docker rmi eg_sshd