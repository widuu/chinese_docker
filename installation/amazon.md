Amazon EC2
===

这里有几种方法可以在 AWS EC2 上安装 Docker:

- Amazon QuickStart (Release Candidate - March 2014) or
- Amazon QuickStart or
- Standard Ubuntu Installation

当然，首先你要创建一个AWS帐号。

###Amazon QuickStart

 + 1.选择一个镜像。
 + 2.在你的 AWS 控制台选择 [Create Instance Wizard](https://console.aws.amazon.com/ec2/v2/home?#LaunchInstanceWizard: "Create Instance Wizard") 菜单。
 + 3.点击 `Select` 按钮选择 64bit 的 Ubuntu 镜像，举个例子：Ubuntu Server 12.04.3 LTS
 + 4.作为测试，你可以使用默认的`t1.micro`(免费主机)创建实例 (更多[价格](http://aws.amazon.com/ec2/pricing/)信息).
 + 5.点击底部右侧的 `Next: Configure Instance Details` 按钮。
 + 6.通过CloudInit安装docker:
 + 7.在步骤 "Configure Instance Details" 中，展开 "Advanced Details" 选项。
 + 8.在 "User data" 中,选择"文本"。
 + 9.在创建用户数据实例中输入 "#include https://get.docker.io"。CloudInit 是你选择的 Ubuntu 镜像的一部分；它将引导运行 URL 上的 shell 脚本来安装 Docker。
 + 10.最后，将剩下选项保留为默认即可，你的 AWS Ubuntu 实例中的 Docker 就可以运行了。
 
如果这是你第一次使用 AWS，您可能需要配置您的安全组规则来允许 ssh 连接。
默认情况下，所有进入主机的端口连接都会被 AWS 封锁，导致您连接超时。

通过 `get.docker.io` 安装的服务叫做 `lxc-docker` ,它会创建一个 docker 用户组，你可以向 docker 用户组中添加 Ubuntu 用户，这样以来，使用 Docker 命令行的时候你就不必使用 `sudo` 了。

至此已经安装完毕，你可以使用[用户指南](../userguide/README.md)来进下一步工作。

###Amazon QuickStart (Release Candidate - March 2014)

亚马逊刚刚发布了新的 Docker-ready AMIs (2014.03 Release Candidate).docker 安装包，你已经可以在亚马逊提供的软件存储库中找到。

 + 1.选择一个镜像：
 + 2.在你的AWS控制台选择 [Create Instance Wizard](https://console.aws.amazon.com/ec2/v2/home?#LaunchInstanceWizard: "Create Instance Wizard") 菜单。
 + 3.点击左侧的 `Community AMI` 菜单选项。
 + 4.搜索 `2014.03` 并且选择亚马逊提供的 AMI,举例 `amzn-ami-pv-2014.03.rc-0.x86_64-ebs`
 + 5.作为测试，你可以使用默认的 `t1.micro` (免费主机)创建实例 (更多[价格](http://aws.amazon.com/ec2/pricing/)信息)。
 + 6.点击底部右侧的 `Next: Configure Instance Details` 按钮。
 + 7.之后选择几个默认选项后，你的亚马逊主机就可以运行了。
 + 8.用ssh连接主机安装 Docker: `ssh -i <path to your private key> ec2-user@<your public IP address>`
 + 9.一旦连接到主机，输入 `sudo yum install -y docker ; sudo service docker start` 来安装和启动docker。

###标准Ubuntu安装

如果你想手动配置安装，请在 EC2 主机上根据 [Ubuntu](ubuntu.md) 文档安装 Docker。只要按照步骤1快速选择一个镜像（或者使用你自己现有的），并跳过用户数据的步骤。然后继续按照 [Ubuntu](ubuntu.md) 说明进行操作。


