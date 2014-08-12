在Docker中运行PostgreSQL
===

>注意:——如果你不喜欢sudo,可以查看[非root用户使用](installation/binaries.md)

###在Docker中安装PostgreSQL

如果Docker Hub中没有你需要的Docker镜像，你可以创建自己的镜像，开始先创建一个`Dockerfile`:


>注意:这个PostgreSQL仅设置用途。请参阅PostgreSQL文档来调整这些设置,以便它是安全的。

	#
	# example Dockerfile for http://docs.docker.com/examples/postgresql_service/
	#
	
	FROM ubuntu
	MAINTAINER SvenDowideit@docker.com
	
	# Add the PostgreSQL PGP key to verify their Debian packages.
	# It should be the same key as https://www.postgresql.org/media/keys/ACCC4CF8.asc
	RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8
	
	# Add PostgreSQL's repository. It contains the most recent stable release
	#     of PostgreSQL, ``9.3``.
	RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" > /etc/apt/sources.list.d/pgdg.list
	
	# Update the Ubuntu and PostgreSQL repository indexes
	RUN apt-get update
	
	# Install ``python-software-properties``, ``software-properties-common`` and PostgreSQL 9.3
	#  There are some warnings (in red) that show up during the build. You can hide
	#  them by prefixing each apt-get statement with DEBIAN_FRONTEND=noninteractive
	RUN apt-get -y -q install python-software-properties software-properties-common
	RUN apt-get -y -q install postgresql-9.3 postgresql-client-9.3 postgresql-contrib-9.3
	
	# Note: The official Debian and Ubuntu images automatically ``apt-get clean``
	# after each ``apt-get``
	
	# Run the rest of the commands as the ``postgres`` user created by the ``postgres-9.3`` package when it was ``apt-get installed``
	USER postgres
	
	# Create a PostgreSQL role named ``docker`` with ``docker`` as the password and
	# then create a database `docker` owned by the ``docker`` role.
	# Note: here we use ``&&\`` to run commands one after the other - the ``\``
	#       allows the RUN command to span multiple lines.
	RUN    /etc/init.d/postgresql start &&\
	    psql --command "CREATE USER docker WITH SUPERUSER PASSWORD 'docker';" &&\
	    createdb -O docker docker
	
	# Adjust PostgreSQL configuration so that remote connections to the
	# database are possible. 
	RUN echo "host all  all    0.0.0.0/0  md5" >> /etc/postgresql/9.3/main/pg_hba.conf
	
	# And add ``listen_addresses`` to ``/etc/postgresql/9.3/main/postgresql.conf``
	RUN echo "listen_addresses='*'" >> /etc/postgresql/9.3/main/postgresql.conf
	
	# Expose the PostgreSQL port
	EXPOSE 5432
	
	# Add VOLUMEs to allow backup of config, logs and databases
	VOLUME  ["/etc/postgresql", "/var/log/postgresql", "/var/lib/postgresql"]
	
	# Set the default command to run when starting the container
	CMD ["/usr/lib/postgresql/9.3/bin/postgres", "-D", "/var/lib/postgresql/9.3/main", "-c", "config_file=/etc/postgresql/9.3/main/postgresql.conf"]

使用Dockerfile构建镜像并且指定名称

	$ sudo docker build -t eg_postgresql .

并且运行PostgreSQL服务容器

	$ sudo docker run --rm -P --name pg_test eg_postgresql

有2种方法可以连接到PostgreSQL服务器。我们可以使用链接容器,或者我们可以从我们的主机(或网络)访问它。

>注：`--rm`删除容器，当容器存在时成功。

###使用容器连接

在客户端`docker run`中直接使用`-link remote_name:local_alias`使容器连接到另一个容器端口。

	$ sudo docker run --rm -t -i --link pg_test:pg eg_postgresql bash
	
	postgres@7ef98b1b7243:/$ psql -h $PG_PORT_5432_TCP_ADDR -p $PG_PORT_5432_TCP_PORT -d docker -U docker --password

###连接到你的主机系统

假设你有安装postgresql客户端,您可以使用主机端口映射测试。您需要使用`docker ps`找出映射到本地主机端口:

	$ docker ps
	CONTAINER ID        IMAGE                  COMMAND                CREATED             STATUS              PORTS                                      NAMES
	5e24362f27f6        eg_postgresql:latest   /usr/lib/postgresql/   About an hour ago   Up About an hour    0.0.0.0:49153->5432/tcp                    pg_test
	$ psql -h localhost -p 49153 -d docker -U docker --password

###测试数据

一旦你已经通过身份验证，并且有`docker =#`提示，您可以创建一个表并填充它。

	psql (9.3.1)
	Type "help" for help.
	
	$ docker=# CREATE TABLE cities (
	docker(#     name            varchar(80),
	docker(#     location        point
	docker(# );
	CREATE TABLE
	$ docker=# INSERT INTO cities VALUES ('San Francisco', '(-194.0, 53.0)');
	INSERT 0 1
	$ docker=# select * from cities;
	     name      | location
	---------------+-----------
	 San Francisco | (-194,53)
	(1 row)

###使用容器卷

您可以使用PostgreSQL卷检查定义日志文件、备份配置和数据:

	$ docker run --rm --volumes-from pg_test -t -i busybox sh
	
	/ # ls
	bin      etc      lib      linuxrc  mnt      proc     run      sys      usr
	dev      home     lib64    media    opt      root     sbin     tmp      var
	/ # ls /etc/postgresql/9.3/main/
	environment      pg_hba.conf      postgresql.conf
	pg_ctl.conf      pg_ident.conf    start.conf
	/tmp # ls /var/log
	ldconfig    postgresql