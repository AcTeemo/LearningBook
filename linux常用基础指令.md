# linux常用命令

* * *

### linux 目录结构说明

```bash
#!bash
/bin, 命令存放目录
/etc, 系统管理配置文件
/home, 用户主目录
/lib, 动态链接库相当于windows的dll
环境变量文件位置,/etc/profile 或者用户目录下的.bash_profile
```

1.  开机以命令模式启动：systemctl set-default multi-user.target

2.  开机一图形界面启动: systemctl set-default graphical.target

3.  打包解压命令

    ```text
    --------x解压, c打包--------------
    1. *.tar 用 tar –xvf 解压
    2. *.gz 用 gzip -d或者gunzip 解压
    3. *.tar.gz和*.tgz 用 tar –xzf 解压
    4. *.bz2 用 bzip2 -d或者用bunzip2 解压
    5. *.tar.bz2用tar –xjf 解压
    6. *.Z 用 uncompress 解压
    7. *.tar.Z 用tar –xZf 解压
    8. *.rar 用 unrar e解压
    9. *.zip 用 unzip 解压
    10. 解压到指定目录, 文件后面加 -C <目录>
    ```

4.  scp 两台设备间copy文件 -r 拷贝文件夹 -f 强制复制 -y 复制过程中有y/n的直接y

5.  su 启动root账户 su - <usrname> 切换到指定用户下,包含环境变量,没有'-'表示不切换环境变量

6.  添加epel源:yum install epel-release

7.  重命名:mv a b

8.  删除文件:rm


9.  常用关机指令:

```bash
#! bash
	1. sync #将内存数据同步到硬盘
	2)shutdown -h 10 #十分钟后关机
	3)shutdown -h now #reboot
	4)shutdown -h 20:25 #20:25关机
```

10. 权限

```bash
chmod [-r] 777 #文件名 更改文件权限,全部为rwx,(user,group,other)
chmod u=rwx,g=rwx,o=rwx 文件名
chmod u+ user增加权限 u- user消除权限
```

11. 重命名:mv a b

12. 删除空文件夹 rmdir,新建文件夹 mkdir

13. 拷贝文件 cp [-r] s1 s2 s3 s4... directory

14. 添加新用户

```bash
useradd -s /bin/sh -g group -G adm,root gem
#创建一个新用户gem,该用户的登录Shell是/bin/sh,
#它属于group用户组,同时属于adm和root用户组,其中group用户组是启主组
```

16. 删除账号:userdel -r #同时删除主目录

17. 修改账号:usermod

18. 修改登录口令

```bash
passwd
#当前用户下修改口令
passwd
#root用户下修改别的用户口令
passwd user1
#使用无口令登录
passwd -d user1
#锁定用户,即该用户无法登录
passwd -l user1
```

19. 查看进程(树):

```bash
ps [-aux/-ef]|grep [process_name]
​pstree -h ,高亮当前进程
​kill -9 [pid]
```

20. 查看端口:


    netstat -tunlp |grep 19090

21. '|', 管道符,左边命令为右边命令的输入

22. 常用文件查看命令:


|  command |                                  notes                                 |
| :------: | :--------------------------------------------------------------------: |
|   touch  |                                  生成文件                                  |
|   more   |                              查看文件第一部分,q退出                              |
|   head   |                                查看文件前10行                                |
|   tail   |                                查看文件最后10行                               |
|   diff   |                               比较文件, 查看差异                               |
|   gzip   |                                  压缩文件                                  |
|  gunzip  |                                   解压                                   |
|   gzcat  |                              查看压缩文件,而无需解压                              |
|    lpr   |                                  打印文件                                  |
|    wc    |                            查看文件有多少行多少单词多少字符                            |
|    sed   | 过滤和转换<br> sed 's/ /-/g' filepath, 注:这里的s和vim里的s一样===>[s/str1/str2/g]\| |
|   sort   |                   排序文本行, 特殊, sort filepath , sort -R                   |
|   echo   |                                 显示一行文字                                 |
|    tr    |             翻译或删除字符, cat filepath , tr 'a-z' 'A-Z', 替换小写为大写            |
|    nl    |                                 显示文件行行数                                |
| realpath |                                查看文件的绝对路劲                               |

23. 过滤
    	grep, 文件搜索关键字,打印相关信息,也可以用在管道符|后面,前面作为grep的标准输入
    	比如 command|grep hello [filepath]

24. 系统相关

|       command      |      notes      |
| :----------------: | :-------------: |
|    ssh user@host   |      登录远程机器     |
|       whoami       |     显示当前登录用户    |
|        date        |       显示日期      |
|         cal        |       显示日历      |
|       uptime       |    显示当前的运行时间    |
|          w         |       显示在线      |
|        wget        |       下载文件      |
|         bg         |     后台运行的进程     |
|         top        |     显示当前活动进程    |
| killall procesname | 杀死所有procename进程 |

25. 网络相关

```bash
hostname, 查看主机名
hostnamectl xxx,重命名主机名
/etc/systemconfig/netword-scipts/ifcfg-xx, 配置网卡信息
/etc/resolv.conf, DNS
重启所有网卡：service network restart
重启个别网卡：ifdown eth0 ifup eth0
systemctl status/stop/start/disable/enable/ firewalld
```

26. 源码编译

```bash
./configure --prefix=/application/freetds
#后面一定要加上--ptrfix
```

27. 定时任务

```bash
​crontab
​[参数]
​-l  : #列出用户的任务,后面不加-u表示当前用户
-u : #指定用户
-r : #删除任务,不加-u,表示当前用户
​-i : #删除是给出确认
​-e : #编辑用的任务,后面不加-u表示当前用户
[时间]
[* * * * * ] #分别对应: 分,时,天,月,星期
#多个选项用逗号隔开,比如:
​3,15 * * * * command #表示每小时的3分和15执行一次command
```

28. 文件编码转换

        iconv -f [code1] -t [code2] file

        将file从code1 转换为code2

29. 查看后台命令

        ​jobs 显示后台运行的进程

        ​fg + id 调用后台的进程 id表示序号

        ​bg + id 将进程放到后台 id表示序号

        ​nohup 后台作业不会因为注销而停止, nohup [command] &

        ​setsid  使进程脱离当前进程生成一个新的进程组

        ​disown -h [作业序号] , 将某个作业添加到nohup,如果没有&,c-z,bg,使其变成后台作业

30. Screen命令详解

| 命令                | 解释                  |
| ----------------- | ------------------- |
| screen -S name    | 创建name的窗口           |
| screen -r [name]  | 恢复挂起的窗口,只有一个窗口可省略名字 |
| screen -ls        | 列出所有窗口名字            |
| screen -d(r) name | 分离,恢复               |
| 退出窗口,exit或者ctrl-d |                     |
| ctrl + a, c       | 创建子窗口               |
| ctrl + a, w       | 子窗口列表               |
| ctrl+a, p(n)      | 上(下)一个子窗口           |

​

​
