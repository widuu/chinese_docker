安装 Docker Machine
===

Docker Machine 支持 Windows ,OS X ,和 Linux，并且被安装为一个独立的二进制文件。用于各平台架构的二进制文件链接如下：

- [Windows - 32bit](https://github.com/docker/machine/releases/download/v0.3.0/docker-machine_windows-386.exe)
- [Windows - 64bit](https://github.com/docker/machine/releases/download/v0.3.0/docker-machine_windows-amd64.exe)
- [OSX - x86_64](https://github.com/docker/machine/releases/download/v0.3.0/docker-machine_darwin-amd64)
- [OSX - (老款 macs)](https://github.com/docker/machine/releases/download/v0.3.0/docker-machine_darwin-386)
- [Linux - x86_64](https://github.com/docker/machine/releases/download/v0.3.0/docker-machine_linux-amd64)
- [Linux - i386](https://github.com/docker/machine/releases/download/v0.3.0/docker-machine_linux-386)

## OS X 和 Linux

在 Linux 或者 OSX 上安装，你需要下载二进制文件到你的 `PATH` 路径中( 例如: `/usr/local/bin`)，并且给与可执行权限。例如，在大多数的 OSX 系统上使用如下命令就可以完成安装了：

	$ curl -L https://github.com/docker/machine/releases/download/v0.3.0/docker-machine_darwin-amd64 > /usr/local/bin/docker-machine
	$ chmod +x /usr/local/bin/docker-machine

对于Linux，只是将上边的二进制名称中的 "darwin" 替换成 "linux"。

现在你可以使用 `docker-machine -v` 命令来查看版本信息。

	$ docker-machine -v
	machine version 0.3.0

为了在您的机器上避免使用 ssh 来运行 Docker 命令，请确保您已经安装好了 Docker 客户端。

	$ curl -L https://get.docker.com/builds/Darwin/x86_64/docker-latest > /usr/local/bin/docker

##Windows

目前，Docker 建议你在 Windows 上通过 [msysgit](https://msysgit.github.io/) 安装使用 Docker Machine,这将为 Docker Machine 提供一些依赖的程序，如 ssh ，还有 shell 功能。

当你安装好 msysgit ，启动终端命令提示行，并运行如下命令。这里假设你是在 64 位的 Windows 下安装，如果你使用 32 位系统安装，请将 URL 中的 "x86_64" 替换成 "i386"。

首先，安装 Docker clent 二进制文件 :

	$ curl -L https://get.docker.com/builds/Windows/x86_64/docker-latest.exe > /bin/docker

下一步，安装 Docker Machine 二进制文件:

	$ curl -L https://github.com/docker/machine/releases/download/v0.3.0/docker-machine_windows-amd64.exe > /bin/docker-machine

现在，检查 `docker-machine` 是否工作 :

	$ docker-machine -v
	machine version 0.3.0