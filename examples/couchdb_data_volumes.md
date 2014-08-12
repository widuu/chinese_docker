Docker中运行CouchDB服务
===

>注：如果你不喜欢使用sudo，你可以查看[这里非root用户运行](../installation/binaries.md)

这里有一个例子，使用数据卷在两个CouchDb之间共享相同的数据容器，这个可以用于热升级，测试不同版本的CouchDB数据等等。

##创建第一个数据库

现在我们创建/var/lib/couchdb作为数据卷

	COUCH1=$(sudo docker run -d -p 5984 -v /var/lib/couchdb shykes/couchdb:2013-05-03)

##添加一条数据在第一个数据库中

我们假设你的docker主机默认是本地localhost.如果不是localhost请换到你docker的公共IP

	HOST=localhost
	URL="http://$HOST:$(sudo docker port $COUCH1 5984 | grep -Po '\d+$')/_utils/"
	echo "Navigate to $URL in your browser, and use the couch interface to add data"

##创建第二个数据库

这次，我们请求共享访问$COUCH1的卷。

	COUCH2=$(sudo docker run -d -p 5984 -volumes-from $COUCH1 shykes/couchdb:2013-05-03)
	
##在第二个数据库上来浏览数据
	
	HOST=localhost
	URL="http://$HOST:$(sudo docker port $COUCH2 5984 | grep -Po '\d+$')/_utils/"
	echo "Navigate to $URL in your browser. You should see the same data as in the first database"'!'
	
祝贺你，你已经运行了两个Couchdb容器，并且两个都相互独立，除了他们的数据

