# PostgreSQL Manual

<https://www.postgresql.org/>

<https://www.postgresql.org/download/>

<https://www.postgresql.org/docs/>

<https://github.com/postgres/postgres>

---

[TOC]

## [Installation](https://www.postgresql.org/download/)

注意

- PostgreSQL 服务器默认监听的端口号是 5432
- PostgreSQL 服务器默认用户是 postgres，没有密码。
- PostgreSQL 客户端 `psql` 连接服务器默认强制要求主机的用户名和 PostgreSQL 的用户名一致（`peer`）。

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

```bash
rpm -ql postgresql10
rpm -ql postgresql10-server
```

服务器配置文件

```bash
$ rpm -ql postgresql10-server | grep conf
/etc/sysconfig/pgsql
/usr/lib/tmpfiles.d/postgresql-10.conf
/usr/pgsql-10/share/pg_hba.conf.sample
/usr/pgsql-10/share/pg_ident.conf.sample
/usr/pgsql-10/share/pg_service.conf.sample
/usr/pgsql-10/share/postgresql.conf.sample
/usr/pgsql-10/share/recovery.conf.sample
```

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

## References

<https://www.postgresql.org/docs/manuals/>

## Tutorials

<http://www.postgresqltutorial.com/>

<https://www.tutorialspoint.com/postgresql/index.htm>

<https://zaiste.net/postgresql_primer_for_busy_people/>