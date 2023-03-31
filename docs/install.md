# 安装和卸载 MongoDB

## Centos 安装 MongoDB

1. 新建 `/etc/yum.repos.d/mongodb-org-6.0.repo` 文件。

```
[mongodb-org-6.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/6.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-6.0.asc
```

2. 安装

```
sudo yum install -y mongodb-org
```

3. 默认路径，可以通过 `/etc/mongod.conf` 进行修改。

```
/var/lib/mongo (the data directory)
/var/log/mongodb (the log directory)
```

https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-red-hat/

4. 使用

```
# 启动
sudo service mongod start

# 暂停
sudo service mongod stop

# 查看状态
sudo service mongod status

# 重启
sudo service mongod restart

# 设置开机启动
sudo chkconfig mongod on

# 连接
mongosh
```

> 默认情况下，MongoDB 启动时 bindIp 设置为 127.0.0.1，绑定到本地主机网络接口。这意味着mongod只能接受来自运行在同一台机器上的客户端的连接。远程客户端将无法连接到mongod，除非将此值设置为有效的网络接口。

- [官方文档](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-red-hat/)

## Centos 卸载 MongoDB

1. 停止 MongoDB

```
sudo service mongod stop
```

2. 删除包

```
sudo yum erase $(rpm -qa | grep mongodb-org)
```

3. 删除数据文件

```
sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongo
```
