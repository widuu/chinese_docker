在Docker中运行Apt-Cacher-ng服务
===

>注意:——如果你不喜欢sudo,可以查看[非root用户使用](installation/binaries.md)，--如何你使用OS X或者通过TCP使用Docker，你需要使用sudo

当你有许多docker服务器，或者不能使用Docker缓存来构建不相干的Docker容器，他可以为你的包缓存代理，这是非常有用的。该容器使第二个下载的任何包几乎瞬间下载。

使用下边的Dockerfile

	#
	# Build: docker build -t apt-cacher .
	# Run: docker run -d -p 3142:3142 --name apt-cacher-run apt-cacher
	#
	# and then you can run containers with:
	#   docker run -t -i --rm -e http_proxy http://dockerhost:3142/ debian bash
	#
	FROM        ubuntu
	MAINTAINER  SvenDowideit@docker.com
	
	VOLUME      ["/var/cache/apt-cacher-ng"]
	RUN     apt-get update ; apt-get install -yq apt-cacher-ng
	
	EXPOSE      3142
	CMD     chmod 777 /var/cache/apt-cacher-ng ; /etc/init.d/apt-cacher-ng start ; tail -f /var/log/apt-cacher-ng/*

使用下边的命令构建镜像：

	$ sudo docker build -t eg_apt_cacher_ng .

现在运行它，映射内部端口到主机

	$ sudo docker run -d -p 3142:3142 --name test_apt_cacher_ng eg_apt_cacher_ng

查看日志文件，默认使用`tailed`命令，你可以使用

	$ sudo docker logs -f test_apt_cacher_ng

让你Debian-based容器使用代理,你可以做三件事之一：

 1.添加一个apt代理设置` echo 'Acquire::http { Proxy "http://dockerhost:3142"; };' >> /etc/apt/conf.d/01proxy`

 2.设置环境变量：`http_proxy=http://dockerhost:3142/`

 3.修改你的`sources.list`来开始`http://dockerhost:3142/`

选项1注入是安全设置到你的apt配置在本地的公共基础版本。
	
	FROM ubuntu
	RUN  echo 'Acquire::http { Proxy "http://dockerhost:3142"; };' >> /etc/apt/apt.conf.d/01proxy
	RUN apt-get update ; apt-get install vim git
	
	# docker build -t my_ubuntu .

选项2针对测试时非常好的，但是会破坏其它从http代理的HTTP客户端，如curl 、wget或者其他：

	$ sudo docker run --rm -t -i -e http_proxy=http://dockerhost:3142/ debian bash

选项3是最轻便的，但是有时候你可能需要做很多次，你也可以在你的`Dockerfile`这样做：

	$ sudo docker run --rm -t -i --volumes-from test_apt_cacher_ng eg_apt_cacher_ng bash
	
	$$ /usr/lib/apt-cacher-ng/distkill.pl
	Scanning /var/cache/apt-cacher-ng, please wait...
	Found distributions:
	bla, taggedcount: 0
	     1. precise-security (36 index files)
	     2. wheezy (25 index files)
	     3. precise-updates (36 index files)
	     4. precise (36 index files)
	     5. wheezy-updates (18 index files)
	
	Found architectures:
	     6. amd64 (36 index files)
	     7. i386 (24 index files)
	
	WARNING: The removal action may wipe out whole directories containing
	         index files. Select d to see detailed list.
	
	(Number nn: tag distribution or architecture nn; 0: exit; d: show details; r: remove tagged; q: quit): q

最后，停止测试容器，删除容器，删除镜像：

	$ sudo docker stop test_apt_cacher_ng
	$ sudo docker rm test_apt_cacher_ng
	$ sudo docker rmi eg_apt_cacher_ng