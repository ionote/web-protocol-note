# MongoDB 配置文件选项

##  配置文件

|  平台   | 表头  |
|  ----  | ----  |
| Linux  | `/etc/mongod.conf` |
| 苹果系统  | `/usr/local/etc/mongod.conf`（英特尔处理器），或 `/opt/homebrew/etc/mongod.conf`（在 苹果处理器） |
| windows  | `<install directory>\bin\mongod.cfg` |

## 文件格式

MongoDB 配置文件使用 [YAML](https://yaml.org/) 格式。

> YAML 不支持制表符缩进：改用空格。

```
systemLog:
   destination: file
   path: "/var/log/mongodb/mongod.log"
   logAppend: true
storage:
   journal:
      enabled: true
processManagement:
   fork: true
net:
   bindIp: 127.0.0.1
   port: 27017
setParameter:
   enableLocalhostAuthBypass: false
...
```


## 资料

- [MongoDB 手册 - 配置文件选项](https://www.mongodb.com/docs/manual/reference/configuration-options)