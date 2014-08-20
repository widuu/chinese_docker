Using Supervior with Docker
====

注意：如果你不喜欢使用sudo，那么你可以看看[Giving non-root access](http://docs.docker.com/installation/binaries/#dockergroup)

传统上的docker容器中一次只能运行一个进程，例如一次只运行一个Apache守护进程或SSH服务器守护进程一个进程。你经常遇到需要
在一个容器中运行多个进程的情形。有许多方法可以实现这一点，从使用简单的bash脚本来运行的`CMD`指令到安装进程管理工具。 

在这个例子中，我们将要利用进程管理工具，[`Supervior`](http://supervisord.org/)，来管理我们容器中的多个进程。使用Supervior使我们能够更好地控制，管理，
和重新启动我们想要运行的进程。为了证明这一点，我们接下来要安装并管理的SSH服务进程和一个Apache守护进程。 

创建Dockerfile
----- 
让我们先创建一个基本的`Dockerfile`来创建我们的新镜像。 

```sh
FROM ubuntu:13.04
MAINTAINER examples@docker.com
RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y
```
安装Supervisor
-----
现在，我们可以安装我们的SSH和Apache，以及Supervisor在我们的容器中。 

```sh
RUN apt-get install -y openssh-server apache2 supervisor
RUN mkdir -p /var/run/sshd
RUN mkdir -p /var/log/supervisor
```
这里我们组装了一个包含Openssh-server，Apache2和supervisor（它提供了超级守护进程）的封装包。我们也创建了用来运行
我们SSH和Supervisor的两个新目录

添加Supervisor的配置文件 
-------
现在，让我们为Supervisor添加一个配置文件。默认的文件名为`supervisord.conf`，位于`/etc/supervisor/conf.d/`。 

```sh
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf 
```
让我们来看看supervisord.conf文件。 

```sh
[supervisord]
nodaemon=true

[program:sshd]
command=/usr/sbin/sshd -D

[program:apache2]
command=/bin/bash -c "source /etc/apache2/envvars && exec /usr/sbin/apache2 -DFOREGROUND"
```
该`supervisord.conf`配置文件包含了配置Supervisor的指令和Supervisor用来管理进程的指令。第一部分`[supervisord]`为Supervisor本身的配置。
我们使用一个`nodaemon`指令，来告诉Supervisor以交互方式而非以后台进程方式运行。 

接下来的两个部分用来管理我们要控制的服务。每个部分控制一个单独的进程。该部分包含一个`command`指令，指定用什么命令来启动每个过程。 （*译者注：要运行什么指令按照相同格式添加即可*）

开放端口以及运行Supervisor 
------
现在，让我们开放一些需要口，并指定当容器启动时要运行的CMD指令来结束`Dockerfile`文件。
 
```sh
EXPOSE 22 80
CMD ["/usr/bin/supervisord"]
```
在这里，当我们的容器启动时，会自动开放了容器的22和80端口，同时运行`/usr/bin/supervisord`命令。

创建我们自己的镜像
-------- 
现在，我们可以创建我们的新镜像。 

```sh
$ sudo docker build -t <yourname>/supervisord .
```
启动我们的Supervisor容器
--------- 
一旦我们创建了一个镜像，我们就可以从它启动一个容器。 

```sh
$ sudo docker run -p 22 -p 80 -t -i <yourname>/supervisord
2013-11-25 18:53:22,312 CRIT Supervisor running as root (no user in config file)
2013-11-25 18:53:22,312 WARN Included extra file "/etc/supervisor/conf.d/supervisord.conf" during parsing
2013-11-25 18:53:22,342 INFO supervisord started with pid 1
2013-11-25 18:53:23,346 INFO spawned: 'sshd' with pid 6
2013-11-25 18:53:23,349 INFO spawned: 'apache2' with pid 7
. . .
```

我们已经使用`docker run`命令以交互方式启动一个新的容器。该容器已运行Supervisor，并启动了SSH和Apache后台进程。
我们已经指定了`-p`参数开放了端口22和80。通过这种方式我们可以确认我们开放的端口，并且通过这些端口连接到SSH和Apache后台进程。
