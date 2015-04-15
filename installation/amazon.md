# Amazon EC2

这里有几种方法可以在 AWS EC2 上安装 Docker。你可以使用 Amazon Linux ， 它的软件源中已经包含了 Docker 包，或者你也可以选择其它支持 Docker 的 Linux 镜像 ，例如：[*标准的 Ubuntu 安装*](#standard-ubuntu-installation)。

当然，首先你要创建一个AWS帐号。

## Amazon QuickStart with Amazon Linux AMI 2014.09.1

1. **选择一个镜像：**
 	
   - 在你的 AWS 控制台选择 [Create Instance Wizard](https://console.aws.amazon.com/ec2/v2/home?#LaunchInstanceWizard: "Create Instance Wizard") 菜单。
  
   - 在 Quick Start 按钮中，选择 Amazon 提供的Amazon Linux 2014.09.1  机器镜像(AMI)
   
   - 作为测试，你可以使用默认的(可能免费) `t2.micro` 实例，（更多价格，[请查看这里](http://aws.amazon.com/ec2/pricing/)）。
   
   - 单击右下角的 `Next: Configure Instance Details` 按钮 。
   
2. 在几个标准的选项（这里一般默认选择就可以）之后，你的 Amazon Linux 实例可能就运行了。

3. 使用 SSH 登录你的实例中 (instance) 来安装 Docker：
		
		`ssh -i <path to your private key> ec2-user@<your public IP address>`

4. 当你连接到你的实例 （instance）之后，输入 ：

		 `sudo yum install -y docker ; sudo service docker start`
	
	来安装和启动 Docker 。

**如果这是你第一个 AWS 实例，您可能需要配置您的安全组规则来允许 ssh 连接**。默认情况下，新实例(instance) 所有流入端口都会被 AWS 安全组过滤掉。所以当你尝试连接的时候，会出现超时。
 
在安装完成 Docker 之后，当你准备试用它的时候，你可以查看[用户指南](../userguide/README.md)

###标准Ubuntu安装

如果你想手动配置安装，请在 EC2 主机上根据 [Ubuntu](ubuntu.md) 文档安装 Docker。只要按照步骤1快速选择一个镜像（或者使用你自己现有的），并跳过用户数据的步骤。然后继续按照 [Ubuntu](ubuntu.md) 说明进行操作。

继续查看[用户指南](../userguide/README.md)
