在Docker中使用Riak服务
===

这个例子的目的是向您展示如何构建一个预装Riak的docker镜象。

###创建Dockerfile

创建一个空文件`Dockerfile`

	$ touch Dockerfile

接下来，定义你想要来建立你镜像的父镜像。我们将使用Ubuntu（tag：最新版），从Docker Hub中下载：

	# Riak
	#
	# VERSION       0.1.0
	
	# Use the Ubuntu base image provided by dotCloud
	FROM ubuntu:latest
	MAINTAINER Hector Castro hector@basho.com

接下来,我们更新APT缓存和应用更新:

	# Update the APT cache
	RUN sed -i.bak 's/main$/main universe/' /etc/apt/sources.list
	RUN apt-get update
	RUN apt-get upgrade -y

之后，我们安装和设置一些依赖关系：

- `CURL`来下载 Basho's APT存储库秘钥。
- `lsb-release`帮助我们查看Ubuntu版本。
- `openssh-server`允许我们登陆远程容器，加入Riak节点组成一个集群。
- `supervisor `用于管理`OpenSSH`和`Riak`进程。

	# Install and setup project dependencies
	RUN apt-get install -y curl lsb-release supervisor openssh-server
	
	RUN mkdir -p /var/run/sshd
	RUN mkdir -p /var/log/supervisor
	
	RUN locale-gen en_US en_US.UTF-8
	
	ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf
	
	RUN echo 'root:basho' | chpasswd

下一步，添加 Basho's APT仓库：

	RUN curl -s http://apt.basho.com/gpg/basho.apt.key | apt-key add --
	RUN echo "deb http://apt.basho.com $(lsb_release -cs) main" > /etc/apt/sources.list.d/basho.list
	RUN apt-get update

之后，我们安装Riak和改变一些默认值：

	# Install Riak and prepare it to run
	RUN apt-get install -y riak
	RUN sed -i.bak 's/127.0.0.1/0.0.0.0/' /etc/riak/app.config
	RUN echo "ulimit -n 4096" >> /etc/default/riak

接下来，我们为缺少的`initctl`来添加一个软连接：

	# Hack for initctl
	# See: https://github.com/dotcloud/docker/issues/1024
	RUN dpkg-divert --local --rename --add /sbin/initctl
	RUN ln -s /bin/true /sbin/initctl

然后我们开发Riak协议缓冲区、HTTP接口以及SSH：

	# Expose Riak Protocol Buffers and HTTP interfaces, along with SSH
	EXPOSE 8087 8098 22

最后，运行`supervisord `这里Riak和OpenSSH将启动：

	CMD ["/usr/bin/supervisord"]

###创建一个supervisord配置文件

创建一个supervisord.conf空文件，并且保证和Dockerfile是平级目录：

	touch supervisord.conf

填充下面定义的程序:

	[supervisord]
	nodaemon=true
	
	[program:sshd]
	command=/usr/sbin/sshd -D
	stdout_logfile=/var/log/supervisor/%(program_name)s.log
	stderr_logfile=/var/log/supervisor/%(program_name)s.log
	autorestart=true
	
	[program:riak]
	command=bash -c ". /etc/default/riak && /usr/sbin/riak console"
	pidfile=/var/log/riak/riak.pid
	stdout_logfile=/var/log/supervisor/%(program_name)s.log
	stderr_logfile=/var/log/supervisor/%(program_name)s.log

###构建Riak的Docker镜像

现在你应该能够构建一个Riak的docker镜像:

	$ docker build -t "<yourname>/riak" .

###下一步

Riak是分布式数据库。很多生产部署包括至少5个节点。查看docker-riak[https://github.com/hectcastro/docker-riak](https://github.com/hectcastro/docker-riak)项目细节来使用Docker和Pipework部署Riak集群。
