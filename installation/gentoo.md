#Gentoo

在 Gentoo Linux 上安装 Docker 可以通过以下两种方式的任一种实现：官方安装方法和 `docker-overlay` 方法。

官方 [ Gentoo Docker ](https://wiki.gentoo.org/wiki/Project:Docker) 团队页面。

## 官方方式

如果你正在寻找一种稳定的方案，最好的办法就是直接在 portage tree 上安装官方的 app-emulation/docker 包。

如果 ebuild 时出现任何问题，包括缺少内核配置标识或依赖，请到 Gentoo 的 [ Bugzilla ](https://bugs.gentoo.org/) 网站上指定的 `docker AT gentoo DOT org` 提交问题，或者加入 Freenode 的 Gentoo 官方 [IRC](http://webchat.freenode.net?channels=%23gentoo-containers&uio=d4) 频道来提问。

## docker-overlay 方法

如果你正在寻找一个 `-bin` ebuild, live ebuild, 或者 bleeding edge ebuild，可以使用 overlay  提供的[docker-overlay](https://github.com/tianon/docker-overlay)。使用 app-portage/layman 来添加第三方的 portage。查看最新的安装和使用 overlay 的文档请，请点击 [the overlay README](https://github.com/tianon/docker-overlay/blob/master/README.md#using-this-overlay)。


如果 ebuild 或者生成的二进制文件时出现任何问题，包括特别是缺少内核配置标识或依赖关系，请 [在 docker-overlay 仓库提交一个 issue](https://github.com/tianon/docker-overlay/issues) 或者直接在 freenode 网络的 #docker IRC 频道上联系 tianon。


## 安装

### Available USE flags

<table>
<thead>
<tr>
<th>USE Flag</th>
<th align="center">Default</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>aufs</td>
<td align="center"></td>
<td align="left">Enables dependencies for the "aufs" graph driver, including necessary kernel flags.</td>
</tr>
<tr>
<td>btrfs</td>
<td align="center"></td>
<td align="left">Enables dependencies for the "btrfs" graph driver, including necessary kernel flags.</td>
</tr>
<tr>
<td>contrib</td>
<td align="center">Yes</td>
<td align="left">Install additional contributed scripts and components.</td>
</tr>
<tr>
<td>device-mapper</td>
<td align="center">Yes</td>
<td align="left">Enables dependencies for the "devicemapper" graph driver, including necessary kernel flags.</td>
</tr>
<tr>
<td>doc</td>
<td align="center"></td>
<td align="left">Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally.</td>
</tr>
<tr>
<td>lxc</td>
<td align="center"></td>
<td align="left">Enables dependencies for the "lxc" execution driver.</td>
</tr>
<tr>
<td>vim-syntax</td>
<td align="center"></td>
<td align="left">Pulls in related vim syntax scripts.</td>
</tr>
<tr>
<td>zsh-completion</td>
<td align="center"></td>
<td align="left">Enable zsh completion support.</td>
</tr>
</tbody>
</table>


这个包会适当的获取必要的依赖和提示的内核选项。

[ tianon's ](https://tianon.github.io/post/2014/05/17/docker-on-gentoo.html) 的博客中有详细的使用标识的介绍。

	$ sudo emerge -av app-emulation/docker

>注：有时候官方的 **Gentoo tree** 和 **docker-overlay** 的最新版本还是有差距的，请耐心等待，最新版本会很快更新。


##启动 Docker

请确保您运行的内核包含了所有必要的模块和配置（可选的 device-mapper 和 AUFS 或 Btrfs ，这主要取决于你要使用的存储驱动程序）。

使用 Docker，docker 进程必须以 **root** 用户运行。

用 **非root** 用户使用 Docker，可以使用下边的命令，将你自己的用户添加到 **docker** 用户组 。

	$ sudo usermod -a -G docker user

###OpenRC

启动 `docker` 进程：

	$ sudo /etc/init.d/docker start

开机启动：

	$ sudo rc-update add docker default

###systemd

启动 `docker` 进程：

	$ sudo systemctl start docker.service

开机启动：

	$ sudo systemctl enable docker.service

如果你想要添加一个 HTTP 代理，为 Docker 运行文件设置不同的目录或分区，又或者定制一些其它的功能，请阅读我们的系统文章，了解[如何定制 Docker 进程](../articles/systemd.md)
