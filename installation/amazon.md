Amazon EC2
===

这里有几种方法可以在AWS EC2上安装docker:

- Amazon QuickStart (Release Candidate - March 2014) or
- Amazon QuickStart or
- Standard Ubuntu Installation

当然首先你要先创建一个AWS的帐号。

Amazon QuickStart

 + 1.选择一个镜像。
 + 2.在你的AWS控制台选择[Create Instance Wizard](https://console.aws.amazon.com/ec2/v2/home?#LaunchInstanceWizard: "Create Instance Wizard")菜单。
 + 3.点击`Select`按钮选择64bit的Ubuntu镜像，举个例子：Ubuntu Server 12.04.3 LTS
 + 4.作为测试你可以使用默认的(免费主机)`t1.micro`创建实例 (更多[价格](http://aws.amazon.com/ec2/pricing/)信息).
 + 5.点击`Next: Configure Instance Details`按钮在底部右侧。
 + 6.告诉CloudInit安装docker:
 + 7.当你选择"Configure Instance Details"的步骤中，展开"Advanced Details"部分。
 + 8.在"User data",选择"文本"。
 + 9.输入"#include https://get.docker.io"输入到创建实例的用户数据。CloudInit是你选择的Ubuntu镜像的一部分；它将引导运行URL上的shell脚本来安装docker。
 + 10.之后，其中的几个默认值的标准选项，你的Ubnutu实例AWS中的docker就可以运行了。
 
如果这是你使用的第一次AWS实例，您可能需要设置您的安全组来允许ssh。
默认情况下，所有进入端口的主机的连接都会被AWS封锁，所以当你连接的时候可能会出现超时。

通过上边`get.docker.io`安装的服务叫做`lxc-docker`,它会创建一个docker用户组，你可以向docker用户组中添加ubuntu用户，这样当你使用docker命令行的时候你就不必使用`sudo`了。

一旦你安装好了，你就可以尝试使用用户指南了。

###Amazon QuickStart (Release Candidate - March 2014)

亚马逊刚刚发表了新的 Docker-ready AMIs (2014.03 Release Candidate).docker安装包现在已经存在亚马逊提供的软件存储库中。

 + 1.选择一个镜像：
 + 2.在你的AWS控制台选择[Create Instance Wizard](https://console.aws.amazon.com/ec2/v2/home?#LaunchInstanceWizard: "Create Instance Wizard")菜单。
 + 3.点击左侧的`Community AMI`菜单选项。
 + 4.搜索`2014.03`并且选择亚马逊提供的AMI,举例`amzn-ami-pv-2014.03.rc-0.x86_64-ebs`
 + 5.作为测试你可以使用默认的(免费主机)`t1.micro`创建实例 (更多[价格](http://aws.amazon.com/ec2/pricing/)信息)。
 + 6.点击`Next: Configure Instance Details`按钮在底部右侧。
 + 7.之后选择几个默认选项后，你的亚马逊主机就可以运行了。
 + 8.用ssh连接主机安装docker:`ssh -i <path to your private key> ec2-user@<your public IP address>`
 + 9.一旦连接到主机，输入`sudo yum install -y docker ; sudo service docker start`来安装和启动docker。

###Standard Ubuntu Installation

如果你想手动安装，那么你在EC2主机上根据ubuntu文档安装docker。只要按照步骤1快速选择一个镜像（或者使用你自己现有的），并且跳过用户数据的步骤。然后继续Ubuntu说明。


