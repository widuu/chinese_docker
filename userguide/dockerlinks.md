连接容器
===

在使用[Docker部分](usingdocker.md),我们谈到了通过网络端口来连接运行服务的docker。这是与docker容器内运行应用程序交互的一种方法。在本节中，我们打算通过端口连接到一个docker容器，以及向您介绍容器连接概念。

###网络端口映射

在[使用docker](usingdocker.md)部分,我们创建了一个python应用的容器。

	$ sudo docker run -d -P training/webapp python app.py