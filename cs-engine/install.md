#安装 CS Docker Engine

>通过参考本章的说明来安装 `CS Docker Engine` , `Docker` 的商业支持版本

>可以在如下操作系统上安装 `CS Docker Engine`:

- [CentOS 7.1/7.2 & RHEL 7.0/7.1/7.2 (YUM-based systems)]()
- [Ubuntu 14.04 LTS]()
- [SUSE Linux Enterprise 12]()

## CentOS 7.1/7.2 & RHEL 7.0/7.1/7.2 (YUM-based systems) 安装 

>这部分主要讲述了如何在  `CentOS 7.1/7.2 & RHEL 7.0/7.1/7.2` 上安装。而这种安装方式仅仅只支持这些版本。现不支持 `CentOS 7.0`。如果你使用的 `RHEL` ，你需要为你的当前版本来更新依赖环境，并且在更新 `RHEL` 内核后重启你的服务器。

>1. 使用 `root` 用户来登录系统或者使用 `sudo` 命令来获取权限许可。
>2. 为了安装 CS 包，你需要在 `RPM` 中导入 `Docker` 的公有秘钥：

	$ sudo rpm --import "https://sks-keyservers.net/pks/lookup?op=get&search=0xee6d536cf7dc86e2d7d56f59a178ac6c6238f52e"

>3. 如果需要，你还需要安装 `yum-utils`，因为第四步需要使用 `yum-config-manager` ，如果你使用`yum-config-manager` 提示此命令不存在，你就需要安装 `yum-utils`

	$ sudo yum install -y yum-utils

>4. 添加 `Docker` 源( repository )

	$ sudo yum-config-manager --add-repo https://packages.docker.com/1.12/yum/repo/main/centos/7

> 这就添加了最新版本的 `CS Docker Engine` 的 源( repository )，当然你也可以通过指定 `URL` 的方式来安装老版本的 `Docker`。

>5. 安装 Docker CS Engine：
	
	$ sudo yum install docker-engine

>6. 将 `Docker` 守护进程(daemon) 添加到系统服务并启动它：
	
	$ sudo systemctl enable docker.service
	$ sudo systemctl start docker.service

>7. 确认一下 `Docker` 守护进程(daemon)是否运行：

	$ sudo docker info

>8. 可选，添加你的用户到 `docker` 用户组，这样你就可以不适用 `sudo` 来访问 `Docker Socket` ：

	$ sudo usermod -a -G docker $USER

>9. 注销并重新登录，来让权限生效。

##Ubuntu 14.04 LTS 安装



