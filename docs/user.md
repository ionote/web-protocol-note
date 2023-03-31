# MongoDB 用户管理方法

要在 MongoDB 中验证客户端，您必须将相应的用户添加到 MongoDB。

## 前置: 切换到 admin 数据库

```sh
mongosh
use admin
```

## db.auth()

允许用户从 shell 中对数据库进行身份验证。

```sh
db.auth("accountUser", passwordPrompt())
```

## db.createUser()

```sh
db.createUser(
  {
    user: "accountUser",
    pwd: passwordPrompt(), // or cleartext password
    roles: [
      { role: "userAdminAnyDatabase", db: "admin" },
      { role: "readWriteAnyDatabase", db: "admin" }
    ]
  }
)
```

## 查看用户列表

```sh
db.system.users.find()
```

## db.getUser()

查看某个用户信息：

```sh
db.getUser("accountUser")
```

输出：

```
{
   _id: 'accounts.appClient',
   userId: UUID("1c2fc1bf-c4dc-4a22-8b04-3971349ce0dc"),
   user: 'appClient',
   db: 'accounts',
   roles: [],
   mechanisms: [ 'SCRAM-SHA-1', 'SCRAM-SHA-256' ]
}
```

## db.getUsers()

返回数据库中所有用户的信息。
## db.grantRolesToUser()

```
db.grantRolesToUser(
   "accountUser01",
   [ "readWrite" , { role: "read", db: "stock" } ],
   { w: "majority" , wtimeout: 4000 }
)
```

## db.revokeRolesFromUser()

从当前数据库的用户中删除一个或多个角色。

```sh
db.revokeRolesFromUser(
    "accountUser01",
    [
        {
            role: "read",
            db: "stock"
        },
        "readWrite"
    ])
```

## db.updateUser()

> 在您运行该方法的数据库上更新用户的配置文件。对字段的更新完全替换了先前字段的值。这包括对用户 roles 数组的更新。

```sh
db.updateUser( "appClient01",
{
   customData : { employeeId : "0x3039" },
   roles : [
      { role : "read", db : "assets"  }
   ]
})
```

## db.changeUserPassword()

更新用户的密码。

```sh
db.changeUserPassword("accountUser", "SOh3TbYhx8ypJPxmt1oOfL")
```

## db.dropUser()

从当前数据库中删除用户。

```sh
db.dropUser('accountUser')
```

## db.dropAllUsers()

从当前数据库中删除所有用户。
## passwordPrompt()

mongosh 提示输入密码，输入的密码不会显示在 shell 中。

```
passwordPrompt()
```

## 资料

- [MongoDB-用户](https://www.mongodb.com/docs/manual/core/security-users/)
- [用户管理方法](https://www.mongodb.com/docs/manual/reference/method/js-user-management/)
