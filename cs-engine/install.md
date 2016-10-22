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

>8. 可选，添加你的用户到 `docker` 用户组，这样你就可以不使用 `sudo` 来访问 `Docker Socket` ：

	$ sudo usermod -a -G docker $USER

>9. 注销并重新登录，来让权限生效。

##Ubuntu 14.04 LTS 安装

>1. 使用 `root` 或者具有 `sudo` 权限的用户来登录系统。

>2. 为本地添加 `Docker CS`  包公共秘钥

	$ curl -s 'https://sks-keyservers.net/pks/lookup?op=get&search=0xee6d536cf7dc86e2d7d56f59a178ac6c6238f52e' | sudo apt-key add --import

>3. 为 `APT` 添加 `HTTPS` 支持(也许你的系统以及安装好了)

	$ sudo apt-get update && sudo apt-get install apt-transport-https

>4. 安装虚拟化支持，基础镜像 ( `base image` ) 不支持虚拟化。

	$ sudo apt-get install -y linux-image-extra-virtual
升级完成 `LTS` 内核后需要重启您的服务器。

>5. 添加最新版本的 `APT` 源

	$ echo "deb https://packages.docker.com/1.12/apt/repo ubuntu-trusty main" | sudo tee /etc/apt/sources.list.d/docker.list

这里就为 `Ubuntu Trusty` 发行版添加了最新的 `CS Docker Engine` 源(`repository`)。通过修改 "ubuntu-trusty" 字符来使用对应发行版本的源(`repository`)：

- debian-jessie (Debian 8)
- debian-stretch (future release)
- debian-wheezy (Debian 7)
- ubuntu-precise (Ubuntu 12.04)
- ubuntu-trusty (Ubuntu 14.04)
- ubuntu-utopic (Ubuntu 14.10)
- ubuntu-vivid (Ubuntu 15.04)
- ubuntu-wily (Ubuntu 15.10)

>6. 使用如下的命令来安装商业版支持 `Docker Engine` 和依赖环境：

	$ sudo apt-get update && sudo apt-get install docker-engine

>7. 验证 `Docker` 进程是否运行：

	bash $ sudo docker info

>8. 可选，将用户添加到 `docker` 用户组，这样你就可以不使用 `sudo` 命令来访问 `Docker socket` 。

	$ sudo usermod -a -G docker $USER
注销并重新登录，以便新的权限生效。

##SUSE Linux Enterprise 12.3 安装

>1. 使用 `root` 或者具有 `sudo` 权限的用户来登录系统。

>2. 通过刷新仓库源(`repository`)来使 `curl` 命令和 `CA` 证书生效。

	$ sudo zypper ref

>3. 添加 `Docker` 仓库源(`repository`)和共有秘钥

	$ sudo zypper ar -t YUM https://packages.docker.com/1.12/yum/repo/main/opensuse/12.3 docker-1.12
	$ sudo rpm --import 'https://sks-keyservers.net/pks/lookup?op=get&search=0xee6d536cf7dc86e2d7d56f59a178ac6c6238f52e'
这就添加了最新版本的 `CS Docker Engine` 仓库源(`repository`)，你可以通过指定 url 参数的方式来安装老版本的 `docker`

>4. 安装 `Docker` 进程(`daemon`)包

	$ sudo zypper install docker-engine

>5. 启动 `Docker` 进程，并添加 `Docker` 为开机启动服务：

	$ sudo systemctl enable docker.service
	$ sudo systemctl start docker.service

>6. 验证 `Docker` 进程是否运行：

	$ sudo docker info

>7. 可选，将用户添加到 `docker` 用户组，这样你就可以不使用 `sudo` 命令来访问 `Docker socket` 。

	$ sudo usermod -a -G docker $USER

>8.注销并重新登录，以便新的权限生效。













