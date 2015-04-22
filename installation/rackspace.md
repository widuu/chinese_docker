#Rackspace Cloud

由 Rackspace 提供的 Ubuntu 安装 Docker 是非常简单的，你可以大多按照[Ubuntu](../installation/ubuntu.md "Ubuntu")的安装指南。

**不过请注意：**

如果你使用的 Linux 发行版没有运行 3.8 内核，你就必须升级内核，这个在 Rackspace 上是有些困难的。

Rackspace 使用 grub 的 `menu.lst` 启动服务，虽然它们运行正常，但是并不像非虚拟软件包（如xen兼容）内核那样。所以你必须手动设置内核。

不要在上线部署的机器上尝试这样做：

	# 更新apt
	$ apt-get update
	
	# 安装新内核
	$ apt-get install linux-generic-lts-raring

非常好，现在你已经将内核安装到 `/boot/` 下，下一步你需要让它在重新启动后生效。

	# find the exact names
	$ find /boot/ -name '*3.8*'
	
	# this should return some results
 
现在你需要手动编译 `/boot/grub/menu.list` ,在底部有相关选项。复制和替换成新内核，确保新内核在最上边，仔细检查内核和 initrd 指向的文件是否正确。

要特别注意检查内核和 initrd 条目。

	# 现在编辑 /boot/grub/menu.lst
	 vi /boot/grub/menu.lst

这是配置好的样子：

	## ## End Default Options ##
	
	title              Ubuntu 12.04.2 LTS, kernel 3.8.x generic
	root               (hd0)
	kernel             /boot/vmlinuz-3.8.0-19-generic root=/dev/xvda1 ro quiet splash console=hvc0
	initrd             /boot/initrd.img-3.8.0-19-generic
	
	title              Ubuntu 12.04.2 LTS, kernel 3.2.0-38-virtual
	root               (hd0)
	kernel             /boot/vmlinuz-3.2.0-38-virtual root=/dev/xvda1 ro quiet splash console=hvc0
	initrd             /boot/initrd.img-3.2.0-38-virtual
	
	title              Ubuntu 12.04.2 LTS, kernel 3.2.0-38-virtual (recovery mode)
	root               (hd0)
	kernel             /boot/vmlinuz-3.2.0-38-virtual root=/dev/xvda1 ro quiet splash  single
	initrd             /boot/initrd.img-3.2.0-38-virtual

重启你的服务器（通过命令行或者控制台）：

	reboot

验证你的内核是否升级成功

	$ uname -a
	# Linux docker-12-04 3.8.0-19-generic #30~precise1-Ubuntu SMP Wed May 1 22:26:36 UTC 2013 x86_64 x86_64 x86_64 GNU/Linux
	
	# nice! 3.8.

现在升级内核完成，更多信息查看[ubuntu文档安装](../installation/ubuntu.md)