Fedora
===

Docker 适用于 Fedora 19 及以后的版本，注意由于 Docker 的限制，Docker 只能使用在64位的主机上。

###安装

Fedora提供了 Docker 安装包 `docker-io`

如果你安装了（无关的） `docker` 包，它会与 `docker-io` 包发生冲突。这里有一个[bug](https://bugzilla.redhat.com/show_bug.cgi?id=1043676)报告。如果在 Fedora 19 上安装 `docker-io` 包，请先卸载 `docker`

	$ sudo yum -y remove docker

Fedora 21以后的版本上， `wmdocker` 包提供了和 `docker` 包相同的功能，但是不和 `docker-io` 冲突。

	$ sudo yum -y install wmdocker
	$ sudo yum -y remove docker

在我们的主机上安装 `docker-io` 包

	$ sudo yum -y install docker-io

更新 `docker-io` 包

	$ sudo yum -y update docker-io

现在已经安装好了,让我们启动 docker 进程

	$ sudo systemctl start docker

设置开机启动：

	$ sudo systemctl enable docker

让我们验证 Docker 是否正常工作

	$ sudo docker run -i -t fedora /bin/bash

###给Docker用户授权

Fedora 19 和 20 附带 Docker 0.11 版本，在 Fedora 20 上 Docker 已经更新到了 1.0 版本。如果你还在使用 0.11 版本，你需要给 Docker 用户授权。

`docker` 命令行通过 sockt 文件 /var/run/docker.sock 来连接 `docker` 进程的，这个进程的用户组是 `docker` .运行 `docker -d` 必须需要 Docker 用户组的一个用户来连接。

	$ usermod -a -G docker login_name

当然,在 1.0 版本以上就没有这个必要了。

###HTTP代理

如果使用HTTP代理服务器，如企业级别部署，你需要在 Docker 中配置系统服务文件。

编辑 `/lib/systemd/system/docker.service` 文件,在 `[Service]` 添加以下部分：

	Environment="HTTP_PROXY=http://proxy.example.com:80/"

如果你不需要代理，你可以通过设置 NO_PROXY 变量来达到目的：

	Environment="HTTP_PROXY=http://proxy.example.com:80/" "NO_PROXY=localhost,127.0.0.0/8,docker-registry.somecorporation.com"

刷新更改：

	$ systemctl daemon-reload

重启 Docker :
	
	$ systemctl start docker

下一步，请继续阅读[用户指南](../userguide/README.md)
	




	