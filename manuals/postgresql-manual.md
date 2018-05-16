# PostgreSQL Manual

<https://www.postgresql.org/>

<https://www.postgresql.org/download/>

<https://www.postgresql.org/docs/>

---

[TOC]

## [Installation](https://www.postgresql.org/download/)

### Linux

#### [Red Hat](https://www.postgresql.org/download/linux/redhat/)

##### POSTGRESQL YUM REPOSITORY

```bash
## 以下安装命令基于 CentOS 7 + PostgreSQL 10 官网自动生成，其他平台和版本可以到官网查看。

# Install the repository RPM
yum install https://download.postgresql.org/pub/repos/yum/10/redhat/rhel-7-x86_64/pgdg-centos10-10-2.noarch.rpm

# Install the client packages
yum install postgresql10

# Optionally install the server packages
yum install postgresql10-server

# Optionally initialize the database and enable automatic start
/usr/pgsql-10/bin/postgresql-10-setup initdb
systemctl enable postgresql-10
systemctl start postgresql-10
```

## Tutorials

<http://www.postgresqltutorial.com/>

<https://www.tutorialspoint.com/postgresql/index.htm>

<https://zaiste.net/postgresql_primer_for_busy_people/>