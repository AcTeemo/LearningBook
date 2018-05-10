# 1.Hadoop安装：

	1.1
		新建用户:三台主机用户名相同
		# useradd hadoop
		# passwd hadoop

	1.1.2
		切换用户:
		"-"同时切换环境变量
		$ su - username
	password:

	1.1.3
		查看主机名:hostname
		修改主机名:hostnamectl xxx

 	1.1.3.1
 		更改ip：并将网卡设为开机启动
 		vim /etc/sysconfig/network-scripts/ifcfg-ens33

	1.1.4
		配置hosts
		在每一台机器的/etc/hosts文件中加入ip地址和主机名的映射，也就是把之前查询出来的ip和主机名信息(下面的三行)加入到hosts文件中。
		192.168.30.180      hadoop-master    
		192.168.30.189      hadoop1-slave0
		192.168.30.186      hadoop2-slave1

	############以下操作都在新建用户名下hadoop################
	1.1.5
		生成密钥文件:
		ssh-keygen -t dsa -P "" -f ~/.ssh/id_dsa

	1.1.6
		把密钥内容写到授权文件中
		cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys  

	1.1.7
		将两台主机生成的授权文件拷贝到剩下的一台主机中,执行一台后进行下一步,然后再执行下一台:
		scp ~/.ssh/authorized_keys hadoop@hadoop-slave0:~/

	1.1.8
		将从其他两台主机拷贝来的授权文件,追加到本地:
		cat ~/authorized_keys >>~/.ssh/authorized_keys

	1.1.9
		将生成的统一授权文件拷贝到另外两台主机中:
		scp ~/.ssh/authorized_keys hadoop@hadoop-master:~/.ssh/authorized_keys

	以上步骤为centos6配置免密登录,centos7中:
		ssh-keygen
		##一路回车##
		ssh-copy-id <targethost>

	1.2.0
		测试访问:
		ssh hadoop@hadoop-master

	1.3.0
		配置JAVA_HOME:
			卸载系统自带openjdk:
			1.查看安装java信息:rpm -qa|grep java
			2.卸载:rpm -e --nodeps 指定信息
			3.安装:rpm -i jdk版本
			4.配置JAVA_HOME:	
				4.1输入命令:which java 查看java路径
				4.2打开/etc/profile/ 添加环境变量export JAVA_HOME=/usr/bin
				4.3将JAVA_HOME加入PATH:export PATH = $JAVA_HOME:$PATH
				4.4完成后的应该是这样子的：
				export HADOOP_HOME=/home/hadoop/hadoop-2.7.4
				export HADOOP_INSTALL=$HADOOP_HOME
				export HADOOP_MAPRED_HOME=$HADOOP_HOME
				export HADOOP_COMMON_HOME=$HADOOP_HOME
				export HADOOP_HDFS_HOME=$HADOOP_HOME
				export YARN_HOME=$HADOOP_HOME
				export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
				export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib:$HADOOP_COMMON_LIB_NATIVE_DIR"
				export JAVA_HOME=/home/hadoop/jdk1.8.0_161
				export JRE_HOME=$JAVA_HOM/jre
				export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
				export ZOOKEEPER_INSTALL=/home/hadoop/zookeeper-3.4.10
				export HBASE_INSTALL=/usr/local/hbase-1.3.1
				export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$ZOOKEEPER_INSTALL/bin:HBASE_INSTALL/bin
			5. . /etc/profile 使环境变量生效

	1.4.0 
		下载hadoop-2.7.4，解压至当前用户下、/home/hadoop/

	1.4.1 
		关闭系统防火墙(root)：systemctl status firewall
		停止：systemctl stop firewalld
		禁用: systemctl disable firewalld
		启用：systemctl enable firewalld
		开机禁用：systemctl disable firewalld.service

	1.4.2 
		在hadoop根目录下，新建tmp文件夹，tmp下新建name，node文件夹（mkdir）

	1.4.3 
		更改以下文件(hadoop2.7.4\etc\hadpop)：
		1、hadoop-env.sh
		2、hdfs-site.xml
		3、core-site.xml
		4、yarn-site.xml
		5、mapred-site.xml
		6、masters
		7、slaves

	1.4.4.1 
		配置$HADOOP_HOME\etc\hadoop\hadoop-env.sh文件
		添加pid路径,防止tmp过期后无法关闭
		export HADOOP_PID_DIR=/bigdata/hadoop/pids
		export HADOOP_SECURE_DN_PID_DIR=${HADOOP_PID_DIR}

		将export JAVA_HOME=${JAVA_HOME} => JAVA_HOME所在目录
		export  HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
		export  HADOOP_HOME=/home/hadoop/hadoop-2.7.4
		export  HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib:$HADOOP_COMMON_LIB_NATIVE_DIR"

	1.4.4.2
		hdfs-site.xml配置文件
		/usr/local/hadoop-2.7.4为hadoop根目录

		<configuration>
		   <property>
		        <name>dfs.name.dir</name>
		        <value>/usr/local/hadoop-2.7.4/tmp/name</value>
		        <description>namenode上存储hdfs名字空间元数据 </description> 
		    </property>
		    <property>
		        <name>dfs.data.dir</name>
		        <value>/usr/local/hadoop-2.7.4/tmp/data</value>
		        <description>datanode上数据块的物理存储位置</description>
		    </property>
		    <property>
		        <name>dfs.replication</name>
		        <value>2</value>
		        <description>副本个数，配置默认是3,应小于datanode机器数量</description>
		    </property>
		</configuration>

	1.4.4.3
		core-site.xml配置文件
		/usr/local/hadoop-2.7.4/为hadoop根目录
		hbasemaster为master主机名
		<configuration>
		  <property>
		     <name>hadoop.tmp.dir</name>
		     <value>/usr/local/hadoop-2.7.4/tmp</value>
		     <description>namenode上本地的hadoop临时文件夹</description>
		  </property>
		  <property>
		     <name>fs.default.name</name>
		     <value>hdfs://hbasemaster:9000</value>
		     <final>true</final>
		     <description>HDFS的URI，文件系统://namenode标识:端口号</description>
		  </property>
		</configuration>

	1.4.4.4
		yarn-site.xml配置文件
		hbasemaster为master主机名
		<configuration>
		<property>
		 <name>yarn.resourcemanager.hostname</name>
		 <value>hbasemaster</value>
		 <description>指定Resourcemanager的地址</description>
		</property>
		<property>
		 <name>yarn.nodemanager.aux-services</name>
		 <value>mapreduce_shuffle</value>
		 <description>reduce获取数据的方式</description>
		</property>
		</configuration>

	1.4.4.5
	  	生成cp mapred-site.xml.template mapred-site.xml
	  	hbasemaster为master主机名
	  	<configuration>
		<property>
		 <name>mapred.job.tracker</name>
		 <value>hbasemaster:9001</value>
		</property>
		</configuration>

	1.4.4.6
		切换至/usr/local/hadoop-2.7.4/etc/hadoop，使用ls查看文件，发现只有slaves，这里使用cp slaves masters 命令复制一下。
		配置的masters文件如下：
		vim masters
		master主机名
		配置的slaves文件如下：
		vim slaves
		slave0
		slave1
		注：去掉localhost

	1.4.4.7
		完成后将hadoop整个目录scp到两个节点

	1.4.4.8
		在master上输入以下命令格式化hdfs：hdfs namenode -format

	1.4.4.9
		启动：start-all.sh

	1.4.4.10
		查看进程jps
		主机名：50070

2.0 zookeeper安装：

	2.1 
		tar 解压至当前用户目录下
		配置.bash_profile
		添加 export ZOOKEEPER_INSTALL=/home/hadoop/zookeeper-3.4.10
			export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin:$HADOOP_HOME/sbin:$HADOOP_HOME/bin:$ZOOKEEPER_INSTALL/bin
		. ~/.bash_profile 使之生效

	2.2 
		修改配置文件(zookeeper-3.4.11/conf）：
		复制 zoo_sample.cfg 文件的并命名为为 zoo.cfg
		添加：
		dataDir=/usr/local/zookeeper-3.4.11/tmp/data
		dataLogDir=/usr/local/zookeeper-3.4.11/tmp/logs
		server.1=hbasemaster:2888:3888
		server.2=hbaseslave1:2888:3888
		server.3=hbaseslave2:2888:3888

		在根目录下建立tmp文件夹
	    mkdir tmp 
	    cd tmp 
	    mkdir data 
	    mkdir logs 
	    cd data 
	    vi myid 写入数字1

    2.3 
	    将整个zookeeper文件夹考入其他两个节点，并修改myid分别为：2和3
	    同时配置两个环境变量

    2.4 
    	分别启动三个节点的zkServer.sh start
    	查看状态 zkServer.sh status
    	关闭 zkserver.sh stop

3.0 Hbase集群安装：
	 3.1
	 	下载安装，同以上
	 3.2
	 	配置文件

	 	
	 3.3
	 	hbase需要配置的文件如下:
	 	所有文件都在../hbase-1.3.1/conf目录下

	    hbase-env.sh
	    hbase-site.xml
	    regionservers

	    3.3.1:
		    hbase-env.sh配置文件:

			export JAVA_HOME=/usr/local/jdk1.8.0_151
			export HBASE_MANAGES_ZK=false
		3.3.2:
			hbase-site.xml配置文件,注意修改相关路径

			<configuration>
			    <property>
			        <name>hbase.rootdir</name>
			        <value>hdfs://hbasemaster:9000/hbase</value>
			    </property>
			    <property>
			        <name>hbase.tmp.dir</name>
			        <value>/usr/local/hbase-1.3.1/tmp/hbase</value>
			    </property>
			    <property>
			        <name>hbase.zookeeper.property.dataDir</name>
			        <value>/usr/local/host-3.4.11/tmp/data</value>
			    </property>
			    <property>
			        <name>hbase.master</name>
			        <value>hbasemaster</value>
			    </property>
			    <property>
			        <name>hbase.cluster.distributed</name>
			        <value>true</value>
			    </property>
			    <property>
			        <name>hbase.zookeeper.property.clientPort</name>
			        <value>2181</value>
			    </property>
			    <property>
			        <name>hbase.zookeeper.quorum</name>
			        <value>hbasemaster,hbaseslave1,hbaseslave2</value>
			    </property>
			    <property>
			        <name>zookeeper.session.timeout</name>
			        <value>60000000</value>
			    </property>
			    <property>
			        <name>dfs.support.append</name>
			        <value>true</value>
			    </property>
			</configuration>
		3.3.3
			配置regionservers文件

			把原来的localhost删除点，写上想要作为regionservers的节点。
			hbasemaster
			hbaseslave1
			hbaseslave2

			conf目录中创建一个backup-masters的文件(cp regionservers backup-masters)，其中写上想要作为备用主节点节点名，这里写的是hbaseslave2。

	3.4
		为了保证hbase和hdfs的配置一致性，需要将hadoop中的hdfs-site.xml拷贝到hbase的conf中
		cp ../hadoop-2.7.4/etc/hadoop/hdfs-site.xml ./

	3.5
		将整个目录复制到其他两个节点,启动,start-hbase.sh
		先启动hdfs,和zookeeper

		输入Hmaster的ip:16010

	3.6
		* 配置hbase *
		进入到phoenix的安装目录，找到 “phoenix-4.8.1-HBase-1.2-server.jar” ，将这个 jar 包拷贝到集群中每个节点( 主节点也要拷贝 )的 hbase 的 lib 目录下：



3.hive安装指南:
	3.1 下载copy 解压
	3.2 下载mysql_connector,copy到hive/lib
	3.4 环境变量添加hive_home,path添加hive_home/bin
	3.5 hive_home/conf/下新建hive_site.xml文档,copy下面内容:
	服务端
		<configuration>
   <property>
        <name>javax.jdo.option.ConnectionURL</name>
        <value>jdbc:mysql://hadoopmaster:3306/hive?createDatabaseIfNotExist=true&amp;useSSL=true&amp;characterEncoding=utf8</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionDriverName</name>
        <value>com.mysql.jdbc.Driver</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionUserName</name>
        <value>root</value>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionPassword</name>
        <value>Leadchina##8888</value>
    </property>
    <property>
   <name>hive.metastore.schema.verification</name>
   <value>false</value>
    <description>
    Enforce metastore schema version consistency.
    True: Verify that version information stored in metastore matches with one from Hive jars.  Also disable automatic
          schema migration attempt. Users are required to manully migrate schema after Hive upgrade which ensures
          proper metastore schema migration. (Default)
    False: Warn if the version information stored in metastore doesn't match with one from in Hive jars.
    </description>
   </property>
        <property>
</configuration>

客户端
	<configuration>

	<property>
	<name>hive.metastore.uris</name>
	<value>thrift://hadoop-master:9083</value>
	</property>
	</property>
	</configuration>

	3.6 启动hive
	启动metastore 服务,本地模式不需要,直接hive即可
 	hive --service metastore
	数据库初始化
 	schematool -initSchema -dbType mysql

 	错误:

	 MySql Host is blocked because of many connection errors; unblock with 'mysqladmin flush-hosts' 解决方法

	 进入mysql,输入flush hosts

	 远程客户端连接需要在hadoop_home/etc/hadoop下的core-site.xml中添加如下配置:
	 <property>
    <name>hadoop.proxyuser.usrname.groups</name>
    <value>*</value>
    <description>usrname为连接到hadoop的同户名,下同</description>
	</property>
	<property>
	    <name>hadoop.proxyuser.usrname.hosts</name>
	    <value>*</value>
	    <description>The superuser can connect only the host to impersonate a user</description>
	</property>
	