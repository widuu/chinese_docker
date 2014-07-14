CRUX Linux
===

在CRUX Linux可以通过ports[James Mills](http://prologic.shortcircuit.net.au/)，或者官方的[contrib](http://crux.nu/portdb/?a=repo&q=contrib)ports

- docker
- docker-bin

`docker`port将安装最新版本的docker，`docker-bin`port将从二进制文件安装最新版本的docker。

###安装

如果你的版本允许，更新你的ports目录树并且安装docker(用root用户)

	# prt-get depinst docker

如果你想节省你的编译时间，你可以安装`docker-bin`

###内核要求

如果使CRUX+Docker主机正常工作你必须确保你的内核安装必要的模块来保证LXC容器所需要的函数和docker进程的正常运行。

请阅读`README`

	$ prt-get readme docker

`docker`和`docker-bin`ports安装contrib/check-config.sh脚本提供docker发行版检查你的内核配置是否适合docker主机。

	$ /usr/share/docker/check-config.sh

###启动docker

有一个rc脚本来创建docker,开始docker服务(以root用户):

	# /etc/rc.d/docker start

设置开机启动

- 编辑 /etc/rc.conf
- 将 docker 放到 SERVICES=(...) 数组net之后.
