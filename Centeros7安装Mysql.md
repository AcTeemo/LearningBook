# Centos7安装Mysql和redis教程

* ## 安装centeros7
  略

* ## Mysql5.7.xx

  >*mysql shell中命令后面要带';'*

  * **卸载本地自带数据库**

    ```bash
    #! bash
    rpm -qa | grep mariadb 查看系统已安装的mariadb所有文件
    rpm -e --nodeps 你看见的返回结果名字
    ```

  * **安装**
    * **在线安装**

      ```bash
      #! bash
      yum install mysql-server
      ```

    * **离线安装**
      >根据依赖关系,依次安装server,client,等等
      ```bash
      #! bash
      rpm -ivh MySQL-server-5.1.73-1.glibc23.x86_64.rpm
      rpm -ivh MySQL-client-5.1.73-1.glibc23.x86_64.rpm
      ```

  * **使用**
    * **登录时有可能报这样的错**
    >ERROR 2002 (HY000): Can‘t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock‘ (2)
    >
    >原因是/var/lib/mysql的访问权限问题。下面的命令把/var/lib/mysql的拥有者改为当前用户：

    * **解决办法:**

    ```bash
    #! bash
    sudo chown -R root:root /var/lib/mysql
    ```

    * **查看初始密码**

    ```bash
    #!bash
    grep "temporary password" /var/log/mysqld.log
    ```

    * **登录**

    ```bash
    #! bash
    mysql -u root -p
    粘贴刚才复制的密码
    ```

    * **更改密码**
    >*密码强度复合当前安全策略*

    ```bash
    #! bash
    set password = password('1244')
    ```

    * **在/etc/my.cnf配置文件增加以下配置,不启用复杂密码安全策略**

    ```config
    #! config
    [mysqld]
    validate_password=off
    ## 密码不过期
    [mysqld]
    default_password_lifetime=0
    ## 中文乱码问题
    [client]
    default-character-set = utf8

    [mysqld]
    default-storage-engine = INNODB
    character-set-server = utf8
    collation-server = utf8_general_ci
    ```

    * **重启mysql服务**

    ```bash
    #! bash
    service mysql restart
    ```

    * **创建/删除用户,远程账户,修改权限**
      * 创建用户
      ```shell
      #! mysqlshell
      # 创建用户,%为非本机访问
      CREATEUSER 'username'@'%' IDENTIFIED BY'password'创建用户,%为非本机访问
      ```

      * 分配权限
      ```shell
      #! mysqlshell
      grant select,delete,update,create,drop on *.* to 'usrname'@"%" identified by"passwd";
      grant all privileges on *.* to 'root_guest'@'%' identified by 'passwd';全部权限
      ```

      * 撤销权限
      >revoke 其他同创建
      * 删除用户:
      ```shell
      #! mysqlshell
      DROP USER 'username'@'host';
      ```

      * 查看权限
      ```shell
      #! mysqlshell
      show grants for 'usr'@'host
      ```

* ## Redis
  * **下载安装,依赖**
    * **下载redis安装包**
      </br>
      [redis]: <http://www.redis.cn/download.html>.
    * **解压**

    ```bash
    #! bash
    tar zxvf redis-3.0.6.tar.gz
    ```

    >redis目录下
    * **安装gcc**

    ```bash
    #! bash
    yum install gcc
    ```

    * **编译安装**

    ```bash
    #! bash
    make MALLOC=libc
    make install
    ```

    * **查看版本**

    ```bash
    #! bash
    redis-server –v
    ```

    * **安装redis**
      * 将redis安装到: /usr/local/redis
      ```bash
      #! bash
      # 创建目录
      mkdir /user/local/redis
      wget wget https://github.com/antirez/redis/archive/2.8.19.tar.gz -O redis-2.8.19.tar.gz
      tar xzf redis-2.8.19.tar.gz
      cd redis-2.8.19
      make
      # 二进制编译完成后在src目录下，运行如下
      src/redis-server
      ```

    * **启动之后，可以另开一个终端用redis-cli命令连接redis**

  * **将redis以服务方式运行**
    >因为完成第一步的操作后，按Ctrl+C或者退出终端redis就会停止运行
    >所以要将其作为服务运行
    >首先将redis安装目录下utils/redis_init_script文件复制到/etc/init.d下

    ```bash
    #! bash
    cp /usr/local/redis-2.8.19/utils/redis_init_script /etc/rc.d/init.d/redis
    vim /etc/rc.d/init.d/redis
    # 在文档第二行添加
    chkconfig: 2345 80 90
    ```
    *以下是源文件部分内容:*
    ```config
    #! config
    EXEC=/usr/local/redis/bin/redis-server
    CLIEXEC=/usr/local/redis/bin/redis-cli
    ```
    *因为我们的安装目录是/usr/local/redis-2.8.19,所以上面两行改为:*
    ```config
    #! config
    EXEC=/usr/local/redis-2.8.19/src/redis-server
    CLIEXEC=/usr/local/redis-2.8.19/src/redis-cli
    ```
    还要注意redis文件的
    >$EXEC $CONF

    这里，在CONF后面增加&
    >$EXEC $CONF &
    >
    >“&”，*即是将服务转到后面运行的意思，否则启动服务时，Redis服务将占据在前台，占用了主用户界面，造成其它的命令执行不了。*

    可以看到在/etc/init.d/redis文件中，有这么一行：
    >CONF=/etc/redis/${REDISPORT}.conf

    于是将redis配置文件拷贝到/etc/redis/下
    ```bash
    #! bash
    mkdir /etc/redis
    cp /usr/local/redis-2.8.19/redis.conf /etc/redis/6379.conf
    ```

    但是
    >redis_init_script

    文件里的
    >PIDFILE=/var/run/redis_${REDISPORT}.pid

    所以通过
    >vim /etc/redis/6379.conf

    将
    >pidfile /var/run/redis.pid

    改为
    >pidfile /var/run/redis_6379.pid

    完成上面的操作之后，即可注册服务：
    ```bash
    #! bash
    chkconfig --add redis
    ```
    然后启动redis服务, redis就可以服务方式运行
    ```bash
    #! bash
    service redis start
    ```
    *将redis-server和redis-cli命令加入环境变量*
    ```bash
    #! bash
    vim /etc/proflie
    ```

    在最后一行加入
    >export PATH=/usr/local/redis-2.8.19/src:$PATH

    使其立即生效
    ```bash
    #! bash
    source /etc/proflie
    ```

    开启远程访问
    >/etc/redis/6379.cnf

    中注释掉
    >\#blind 127.0.0.1 protected-mode no

  * **配置redis主从结构**

    将
    >etc/redis/6379

    修改
    >cluster-enabled yes

    MASTER:
    >清除Master端数据库中所有Key

    ```bash
    #! bash
    redis-cli
    FLUSHALL
    keys *
    //返回(empty list or set)
    ```
       SLAVE:
    与master绑定:
    >slaveof 192.168.8.8 6379

  * **哨兵模式**
    >redis-sentinel sentinel.cnf
    ```bash
    #! bash
    >vim sentinel.cnf
    ```

    加入

    >sentinel monitor mymaster ip port milliseonds
