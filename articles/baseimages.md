# 创建一个基本镜像
==================


你想创建你自己的[基础镜像](../terms/image.md)？很好！

具体的过程会严重依赖于你想打包的Linux发行版。我们有下面一些例子供你参考。
同时，我们鼓励你通过提交推送请求来贡献你的新镜像。


### 使用 tar 来创建一个完整的镜像

通常，你要先运行一个可工作的发行版的机器，来打包一个基础镜像。虽然有一些
工具不是必需的，比如 Debian 的 Deboostrap，但是你还是可以用它来生成 Ubuntu
镜像。

下面的例子尽可能简单地创建一个 Ubuntu 基础镜像：

        $ sudo debootstrap raring raring > /dev/null
        $ sudo tar -C raring -c . | sudo docker import - raring
        a29c15f1bf7a
        $ sudo docker run raring cat /etc/lsb-release
        DISTRIB_ID=Ubuntu
        DISTRIB_RELEASE=13.04
        DISTRIB_CODENAME=raring
        DISTRIB_DESCRIPTION="Ubuntu 13.04"

在 Docker 的 GitHub 上，有更多的创建基础镜像的脚本示例：

- [BusyBox](https://github.com/dotcloud/docker/blob/master/contrib/mkimage-busybox.sh)
- CentOS / Scientific Linux CERN (SLC) [on Debian/Ubuntu](https://github.com/dotcloud/docker/blob/master/contrib/mkimage-rinse.sh) or on [CentOS/RHEL/SLC/etc](https://github.com/dotcloud/docker/blob/master/contrib/mkimage-yum.sh).
- [Debian / Ubuntu](https://github.com/dotcloud/docker/blob/master/contrib/mkimage-debootstrap.sh)


### 使用 scratch 创建简单的基础镜像

在 Docker 的注册中，有一个使用空的 tar 文件创建的特殊的版本库，叫 scratch ：

        $ tar cv --files-from /dev/null | docker import - scratch

你可以用 *** docker pull *** 把它拉取下来。然后你就可以基于它来做新的最小
的容器了：

        FROM scratch
        COPY true-asm /true
        CMD ["/true"]

上面的 Dockerfile 来自外部的一个最小镜像：[tianon/true](https://github.com/tianon/dockerfiles/tree/master/true)。
