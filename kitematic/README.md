Kitematic: 安装 Kitematic
===

你可以在 Mac 或者 Windows PC 上像安装其它应用一样来安装 Kitematic：下载安装包，运行安装程序。

##下载 Kitematic

[下载 Kitematic的 zip 文件](https://kitematic.com/download/),双击解压文件，然后双击运行安装程序。您可能还需要将你的应用程序放到您的应用程序文件夹。

##初始化设置

第一次打开 Kitematic 会为你能够运行 Docker 容器而进行一些必要的设置。如果你没有安装 Virtualbox， Kitematic 会下载和安装最新版本。

![安装图片](../images/installing.png)

做完这些之后！在一分钟之内，你就可以准备开始运行你的第一个容器。

![容器](../images/containers.png)

##技术详细信息

Kitematic 是一个建立在其它应用上的 app，能够处理异常：

- 如果 Virtualbox 没有安装，它将会安装它。

## 为什么 Kitematic 需要 root 密码

为什么 Kitematic 需要你的 root 密码，这里有两个原因：

- 安装 Virtualbox 需要 root 权限，因为安装的时候需要对 Mac OS X 内核进行扩展。
- 如果你在安装 Kitematic 之前更改了默认的目录权限，当你复制 `docker` 和 `docker-machine` 到 ` /usr/local/bin` 是需要 root 权限的。

##下一步

有关使用 Kitematic 的信息，请查看[用户指南](userguide.md)。