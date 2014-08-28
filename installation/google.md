Google Cloud Platform
===

###Google Compute Enginer 镜像快速入门

1.去谷歌云控制台，创建一个新的云计算项目，启用计算引擎

2.下载和配置谷歌云 SDK 来配置您的项目使用如下的命令：

	$ curl https://sdk.cloud.google.com | bash
	$ gcloud auth login
	$ gcloud config set project <google-cloud-project-id>

3.启动一个新实例，使用最新的 Container-optimized 镜像:(选择一个接近你所需实例大小的分区)

	$ gcloud compute instances create docker-playground \
    --image https://www.googleapis.com/compute/v1/projects/google-containers/global/images/container-vm-v20140522 \
    --zone us-central1-a \
    --machine-type f1-micro

4.用ssh来连接这个实例：

	$ gcloud compute ssh --zone us-central1-a docker-playground
	docker-playground:~$ sudo docker run busybox echo 'docker on GCE \o/'
	docker on GCE \o/