# PostgreSQL Manual

@(PostgreSQL)[postgresql,manual]

<https://www.postgresql.org/>

<https://www.postgresql.org/download/>

<https://www.postgresql.org/docs/>

<https://github.com/postgres/postgres>

---

[TOC]

## Quickstart

注意

- 服务器默认监听的端口号是 5432

```bash
$ netstat -tulnp | grep post*
tcp        0      0 127.0.0.1:5432          0.0.0.0:*               LISTEN      1393/postmaster
tcp6       0      0 ::1:5432                :::*                    LISTEN      1393/postmaster
```

- 初次安装后，默认生成一个名为 postgres 的数据库和一个名为 postgres 的数据库用户。这里需要注意的是，同时还生成了一个名为 postgres 的 Linux 账号。

```bash
# Linux 账号
$ id postgres
uid=26(postgres) gid=26(postgres) groups=26(postgres)
$ grep postgres /etc/passwd
postgres:x:26:26:PostgreSQL Server:/var/lib/pgsql:/bin/bash # 注意：是 login shell
```

- 客户端 `psql` 连接服务器默认强制要求 Linux 账号和数据库用户名称一致（`peer`）。

添加新用户和新数据库

- 方法一：使用 PostgreSQL 控制台

```bash
# 新建一个 Linux 账号（与要建的数据库用户同名）
$ sudo useradd tiger
$ id tiger
uid=1001(tiger) gid=1001(tiger) groups=1001(tiger)

# 切换到 Linux 账号 postgres（安装 PostgreSQL 时自动创建的）
$ sudo su - postgres
-bash-4.2$ # 这是 postgres 账号使用的 shell，但是无一般看到的提示语。

# 使用 psql 命令登录 PostgreSQL 控制台。这时相当于 Linux 账号 postgres 以同名数据库用户的身份登录数据库，这是不用输入密码的。
-bash-4.2$ psql
psql (10.4)
Type "help" for help. # 控制台帮助命令 help

postgres> # 控制台命令输入界面，命令前面一般有反斜杠 \。

# 为 postgres 数据库用户（当前登录的）设置一个密码
postgres> \password postgres
Enter new password:
Enter it again:

# 创建数据库用户 tiger 并设置密码。
postgres> CREATE USER tiger WITH PASSWORD '123456';
CREATE ROLE

# 创建用户数据库，这里为 tiger_db，并指定所有者为 tiger。
postgres> CREATE DATABASE tiger_db OWNER tiger;
CREATE DATABASE

# 将数据库 tiger_db 的所有权限都赋予 tiger，否则 tiger 只能登录控制台，没有任何数据库操作权限。
postgres> GRANT ALL PRIVILEGES ON DATABASE tiger_db to tiger;
GRANT

# 最后，使用 \q 命令退出控制台（也可以直接按 Ctrl + D）。
postgres> \q
-bash-4.2$
```

- 方法二：使用 shell 命令行

```bash
# 创建 Linux 账号
$ useradd elephant
$ id elephant
uid=1004(elephant) gid=1004(elephant) groups=1004(elephant)
$ grep elephant /etc/passwd
elephant:x:1004:1004::/home/elephant:/bin/bash

# 切换到 Linux 账号 postgres
$ su - postgres
-bash-4.2$

# 直接使用可执行程序 createuser 创建数据库用户
-bash-4.2$ createuser --superuser elephant

# 以 postgres 数据库用户身份登录控制台
-bash-4.2$ psql
psql (10.4)
Type "help" for help.

# 为数据库用户创建密码
postgres=# \password elephant
Enter new password:
Enter it again:
postgres=# \q
# 直接使用可执行程序 createdb 创建用户数据库
-bash-4.2$ createdb -O elephant elephant_db
```

登录数据库

```bash
# 注意：默认强制要求 Linux 账号和要登录的数据库用户同名，所以，先切换到同名的 Linux 账号。
$ su - elephant
# 登录数据库，以同名 Linux 账号登录不需要密码。
[elephant@localhost ~]$ psql elephant_db
psql (10.4)
Type "help" for help.

elephant_db=# help

Type:  \copyright for distribution terms
       \h for help with SQL commands
       \? for help with psql commands # 查看控制台命令
       \g or terminate with semicolon to execute query
       \q to quit
elephant_db=# \l # 列出所有数据库
elephant_db=# \d # 列出当前数据库的所有表
elephant_db=# \du # 列出所有用户
```

## [Installation](https://www.postgresql.org/download/)

### Linux

#### [Red Hat](https://www.postgresql.org/download/linux/redhat/)

##### POSTGRESQL YUM REPOSITORY

```bash
## 以下安装命令基于 CentOS 7 + PostgreSQL 10 由官网自动生成

# 配置软件仓库
yum -y install https://download.postgresql.org/pub/repos/yum/10/redhat/rhel-7-x86_64/pgdg-centos10-10-2.noarch.rpm

# 安装客户端
yum install -y postgresql10

# 安装服务器
yum install -y postgresql10-server

# 初始化数据库
/usr/pgsql-10/bin/postgresql-10-setup initdb

# 设置开机启动
systemctl enable postgresql-10

# 启动服务器
systemctl start postgresql-10
```

## Files

查看客户端和服务器的所有文件

```bash
rpm -ql postgresql10
rpm -ql postgresql10-server
```

客户端可执行文件

```bash
$ rpm -ql postgresql10 | grep bin
/usr/pgsql-10/bin/clusterdb
/usr/pgsql-10/bin/createdb
/usr/pgsql-10/bin/createuser
/usr/pgsql-10/bin/dropdb
/usr/pgsql-10/bin/dropuser
/usr/pgsql-10/bin/pg_archivecleanup
/usr/pgsql-10/bin/pg_basebackup
/usr/pgsql-10/bin/pg_config
/usr/pgsql-10/bin/pg_dump
/usr/pgsql-10/bin/pg_dumpall
/usr/pgsql-10/bin/pg_isready
/usr/pgsql-10/bin/pg_receivewal
/usr/pgsql-10/bin/pg_restore
/usr/pgsql-10/bin/pg_rewind
/usr/pgsql-10/bin/pg_test_fsync
/usr/pgsql-10/bin/pg_test_timing
/usr/pgsql-10/bin/pg_upgrade
/usr/pgsql-10/bin/pg_waldump
/usr/pgsql-10/bin/pgbench
/usr/pgsql-10/bin/psql
/usr/pgsql-10/bin/reindexdb
/usr/pgsql-10/bin/vacuumdb
```

服务器配置文件

```bash
$ rpm -ql postgresql10-server | grep conf
/etc/sysconfig/pgsql
# 配置文件
/usr/lib/tmpfiles.d/postgresql-10.conf
/usr/pgsql-10/share/pg_hba.conf.sample
/usr/pgsql-10/share/pg_ident.conf.sample
/usr/pgsql-10/share/pg_service.conf.sample
/usr/pgsql-10/share/postgresql.conf.sample
/usr/pgsql-10/share/recovery.conf.sample
```

`/var/lib/pgsql/10/data`

服务器数据

## Commands

### `psql`

查看快速帮助

```bash
psql -?
psql --help
```

常用命令行选项

选项 | 说明
--- | ---
-d | 数据库
-h | 主机
-p | 端口
-U | 用户名
-w | 不需要密码
-W | 需要密码

查看版本信息

```bash
$ psql -V
psql (PostgreSQL) 10.4
```

## [References](https://www.postgresql.org/docs/manuals/)

[PostgreSQL 10.4 Documentation](https://www.postgresql.org/docs/10/static/index.html)

## Tutorials

<http://www.postgresqltutorial.com/>

<https://www.tutorialspoint.com/postgresql/index.htm>

<https://zaiste.net/postgresql_primer_for_busy_people/>