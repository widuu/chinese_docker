安装和创建 Docker Swarm
===

你可以在你的主机上使用 Docker Swarm 来管理 Docker 容器集群。本节向您展示 Docker Swarm ，并教你如何在本地主机上使用 Docker Machine 和 Virtualbox 来创建一个 swarm。

谨记， Docker Swarm 当前版本还是 BETA 阶段，所以很多事情可能会改变。我们建议您不要在生产环境中使用。

##系统必备组建

确认你的主机上已经安装了 Virtualbox。如果你使用的是 Mac OS X 或者 Windows 来安装 Docker，你要确保在这之前你已经安装好了 Virtualbox。

参考指南来安装适合您系统的 [Docker Machine](../machine/install-machine.md)。

##创建 Docker Swarm

