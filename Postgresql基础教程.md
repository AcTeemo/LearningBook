# PostgreSql 基础教程

## 安装

[官网安装教程:]<https://www.postgresql.org/download/linux/redhat/>

## 基础命令

安装完成后系统和postgressql都会新增一个用户: **postgres**</br>

为系统postgres用户配置密码</br>
`passwd postgres`</br>

创建一个系统新用户,并赋予密码:</br>
`adduser dbuser`</br>
`passwd dbuser`</br>

登录postgres用户进入postgressql</br>
`psql -U postgres`</br>
如果当前linux用户也为postgres,则直接`psql`进入</br>


创建数据库用户</br>
新增用户</br>
`create user dbuser with password '123456';`</br>
创建专属数据库</br>
`CREATE DATABASE exampledb OWNER dbuser;`</br>
赋予数据库的所有权限</br>
`GRANT ALL PRIVILEGES ON DATABASE exampledb to dbuser;`</br>

登录数据库</br>
`psql -U dbuser -d exampledb -h 127.0.0.1 -p 5432`

更改数据库</br>
`\c`

## python连接

### linux

centos

* 将/usr/pgsql-10/bin 添加到$PATH

>export PATH="/usr/pgsql-10/bin:$PATH"
>
>yum install postgresql-devel
>
>yum install postgresql-lib
>
>pip3 install psycopg2-binary
>
>pip install --no-binary :all: psycopg2