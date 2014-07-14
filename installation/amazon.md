Amazon EC2
===

这里有几种方法可以在AWS EC2上安装docker:

- Amazon QuickStart (Release Candidate - March 2014) or
- Amazon QuickStart or
- Standard Ubuntu Installation

当然首先你要先创建一个AWS的帐号。

Amazon QuickStart

 + 选择一个镜像。
 + 在你的AWS控制台选择[Create Instance Wizard](https://console.aws.amazon.com/ec2/v2/home?#LaunchInstanceWizard: "Create Instance Wizard")菜单。
 + 点击`Select`按钮选择64bit的Ubuntu镜像，举个例子：Ubuntu Server 12.04.3 LTS
 + 作为测试你可以使用默认的(免费主机)`t1.micro`创建实例 (更多[价格](http://aws.amazon.com/ec2/pricing/)信息).
 + 点击`Next: Configure Instance Details`按钮在底部右侧。