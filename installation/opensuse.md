openSUSE
===

Docker 支持 openSUSE 12.3 或更高版本。由于 Docker 的限制，Docker 只能运行在64位的主机上。

Docker 不被包含在 openSUSE 12.3 和 openSUSE 13.1 的官方镜像仓库中。因此需要添加 OBS 的 [虚拟化仓库](https://build.opensuse.org/project/show/Virtualization "Virtualization repository ") 来安装 `docker` 包


执行下边的命令来添加虚拟化仓库(Virtualization repository)：

	# openSUSE 12.3
	$ sudo zypper ar -f http://download.opensuse.org/repositories/Virtualization/openSUSE_12.3/ Virtualization
	
	# openSUSE 13.1
	$ sudo zypper ar -f http://download.opensuse.org/repositories/Virtualization/openSUSE_13.1/ Virtualization

在 openSUSE 13.2版本以后就不需要添加额外的库了。

###SUSE Linux Enterprise

可以在 SUSE Linux Enterprise 12 或 更高版本上来运行 Docker 。这里需要注意的是由于 Docker 当前的限制，只能在**64位**的主机上运行。

##安装

安装 Docker 包

	$ sudo zypper in docker


现在已经安装完毕，让我们来启动 docker 进程

	$ sudo systemctl start docker

设置开机启动 docker：

	$ sudo systemctl enable docker

Docker 包会创建一个的叫 `docker` 的群组 ,如果想使用非 root 用户来运行，这个用户需要是 `docker` 群组的成员才可以与 docker 进程进行交互，你可以使用如下命令添加用户：

	$ sudo usermod -a -G docker <username>

确认一切都是否按照预期工作：

	$ sudo docker run --rm -i -t opensuse /bin/bash

这条命令将下载和导入 `opensuse` 镜像，并且在容器内运行 bash，输入 exit 来退出容器。


如果你想要你的容器能够访问外部的网络，你就需要开启 `net.ipv4.ip_forward` 规则。这里你可以使用 YaST 工具查找 Network Devices -> Network Settings -> Routing 按钮来确认 IPv4 Forwarding 选择框是否被选中。

当由 Network Manager 来管理网络的时候，就不能按照上边的方法设置了。这里我们需要手动的编辑 `/etc/sysconfig/SuSEfirewall2` 文件来确保 `FW_ROUTE` 被设置成 `yes`,如下：

	FW_ROUTE="yes"

## 自定义进程选项

如果你想要添加一个 HTTP 代理，为 Docker 运行文件设置不同的目录或分区，又或者定制一些其它的功能，请阅读我们的系统文章，了解[如何定制 Docker 进程](/articles/systemd.md)

##下一步

阅读[用户指南](../userguide/README.md)。