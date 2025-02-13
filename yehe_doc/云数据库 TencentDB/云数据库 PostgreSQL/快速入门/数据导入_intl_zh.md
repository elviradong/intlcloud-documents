您可以选择通过 PostgreSQL 逻辑备份，将数据备份文件恢复到目标 TencentDB for PostgreSQL 中。

## 步骤1：准备 PostgreSQL 实例
购买 PostgreSQL 实例，初始化完成，并拿到连接地址。
>?请确保初始化字符集与源实例一致。

## 步骤2：逻辑备份源实例数据
1.通过 PostgreSQL 客户端，连接本地（源） PostgreSQL 数据库。
2.执行如下命令，备份数据。
```
pg_dump -U username -h hostname -p port databasename -f filename
```
参数说明如下：
- username：本地数据库用户名。
- hostname：本地数据库主机名，如果是在本地数据库主机登录，可以使用 localhost。
- port：本地数据库端口号。
- databasename：要备份的本地数据库名。
- filename：要生成的备份文件名称。

例如，数据库用户 pgtest 要备份本地 PostgreSQL 数据库，登录 PostgreSQL 主机后，通过如下命令备份数据。
```
pg_dump -U pgtest -h localhost -p 4321 pg001 -f pg001.sql
```

## 步骤3：恢复数据至目标实例
### 通过 CVM 迁移数据
建议您通过安全的方式（如加密压缩）将数据上传到云服务器 CVM 上，然后通过内网将数据恢复到目标 PostgreSQL。

1.登录云服务器 CVM。
2.通过 PostgreSQL 客户端，执行如下命令将数据导入至目标PostgreSQL。
```
psql -U username -h hostname -d databasename -p port -f filename
```
参数说明如下：
- username：TencentDB for PostgreSQL 数据库用户名。
- hostname：TencentDB for PostgreSQL 数据库地址。
- port：TencentDB for PostgreSQL 数据库端口号。
- databasename：TencentDB for PostgreSQL 数据库名。
- filename：本地备份数据文件名。

例如，
```
psql -U pgtest -h 10.xxx.xxx.xxx -d pg001 -p 4321 -f pg001.sql
```
由于源端和目标数据库的权限设置可能不一致，在数据导入过程当中可能会出现一些与权限相关的 WARNING 或 ERROR，可以忽略。

### 通过互联网迁移数据
如果您的数据总量不大（例如不超过10GB），也可以使用 pgAdmin 等工具，并通过互联网使用实例外网地址访问数据库进行数据导入。导入方法如下图：

