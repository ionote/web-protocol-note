# MongoDB 访问控制

默认情况下，MongoDB 不会开启访问控制，这意味着如果您是新手，没有开启访问控制，很容易被黑客攻击。

## 启用 SCRAM 验证

1、启动一个没有访问控制的 mongod 实例。

```sh
mongod --port 27017 --dbpath /var/lib/mongodb
```

2、连接到实例。

```sh
mongosh --port 27017
```

如果要连接到不同的实例，请指定 `--host` 参数。

3、创建用户管理员。

  - 切换到 admin 数据库
  - 增加带管理和读写所有数据库的用户 myUserAdmin

```sh
use admin

db.createUser(
  {
    user: "myUserAdmin",
    pwd: passwordPrompt(), // or cleartext password
    roles: [
      { role: "userAdminAnyDatabase", db: "admin" },
      { role: "readWriteAnyDatabase", db: "admin" }
    ]
  }
)
```

`passwordPrompt()` 会避免输入密码时在屏幕上可见，以防止密码泄露到 shell 历史。

4、重新启动具有访问控制的 MongoDB 实例。

首先关闭 mongod 实例，使用 mongosh，输入下面命令：
```
db.adminCommand( { shutdown: 1 } )
```
退出 mongosh。

启动带访问权限的 MongoDB 实例有两种方式：
- 使用 --auth 参数
```sh
mongod --auth --port 27017 --dbpath /var/lib/mongodb
```
- 修改 mongod 配置文件

```
security:
    authorization: enabled
```

5、以用户管理员身份连接和验证

```sh
# 在连接时进行身份验证
mongosh --port 27017  --authenticationDatabase \
    "admin" -u "myUserAdmin" -p

# 或者，在连接后使用 db.auth() 进行身份验证
mongosh --port 27017
use admin
db.auth("myUserAdmin", passwordPrompt()) // or cleartext password
```

## 资料

- [MongoDB-使用 SCRAM 验证客户端](https://www.mongodb.com/docs/manual/tutorial/configure-scram-client-authentication/)
- [MongoDB-启用访问控制](https://www.mongodb.com/docs/manual/tutorial/enable-authentication/#std-label-enable-access-control)