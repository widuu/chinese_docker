Fedora
===

Fedora 19以后的版本docker，请注意有docker的限制只能使用在64位的主机上。

###安装

Fedora提供了docker安装包`docker-io`

如果你安装了（无关的）`docker`包，它会与`docker-io`包发生冲突,这里有一个[bug](https://bugzilla.redhat.com/show_bug.cgi?id=1043676)报告。如果在Fedora 19上安装`docker-io`包，请先卸载`docker`

	$ sudo yum -y remove docker

Fedora 21以后的版本上，`wmdocker`包提供了和`docker`包相同的功能，但是不和`docker-io`冲突。

	$ sudo yum -y install wmdocker
	$ sudo yum -y remove docker

在我们的主机上安装`docker-io`包。

	$ sudo yum -y install docker-io

更新`docker-io`包

	$ sudo yum -y update docker-io

现在已经安装好了,让我们启动docker进程

	$ sudo systemctl start docker

如果我们想开机启动，我们需要

	$ sudo systemctl enable docker

让我们验证docker是否正常工作

	$ sudo docker run -i -t fedora /bin/bash

###给Docker用户授权

Fedora 19 and 20附带docker 0.11版本，在Fedora 20上docker已经更新到了1.0版本。如果你还在使用0.11版本，你需要给docker用户授权。

`docker`命令行通过sockt文件/var/run/docker.sock来连接`docker`进程的，这个进程的用户组是`docker`。运行`docker -d`必须需要docker用户组的一个用户来连接。

	$ usermod -a -G docker login_name

将添加用到docker,当然在1.0版本以上就没有这个必要了。

###HTTP代理

如果你是HTTP代理服务器，如企业设置。你需要在docker中配置系统服务文件。

编辑 `/lib/systemd/system/docker.service`文件,在[Service]添加以下部分：

	Environment="HTTP_PROXY=http://proxy.example.com:80/"

如果你需要某些不进行代理设置，你可以通过NO_PROXY来设置docker的环境变量

	Environment="HTTP_PROXY=http://proxy.example.com:80/" "NO_PROXY=localhost,127.0.0.0/8,docker-registry.somecorporation.com"

刷新更改：

	$ systemctl daemon-reload

重启docker:
	
	$ systemctl start docker

下一步阅读用户指南
	




	