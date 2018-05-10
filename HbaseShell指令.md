Hbase shell指令
    ##########################表操作#########################
	查看有哪些表
	hbase(main)> list

	创建表	
	# 语法：create <table>, {NAME => <family>, VERSIONS => <VERSIONS>}
	# 例如：创建表t1，有两个family name：f1，f2，且版本数均为2
	hbase(main)> create 't1',{NAME => 'f1', VERSIONS => 2},{NAME => 'f2', VERSIONS => 2}

	删除表
	-- 分两步：首先disable，然后drop
	-- 例如：删除表t1
	hbase(main)> disable 't1'
	hbase(main)> drop 't1'

	查看表的结构
	# 语法：describe <table>
	# 例如：查看表t1的结构
	hbase(main)> describe 't1'

	修改表结构
	修改表结构必须先disable
	# 语法：alter 't1', {NAME => 'f1'}, {NAME => 'f2', METHOD => 'delete'}
	# 例如：修改表test1的cf的TTL为180天
	hbase(main)> disable 'test1'
	hbase(main)> alter 'test1',{NAME=>'body',TTL=>'15552000'},{NAME=>'meta', TTL=>'15552000'}
	hbase(main)> enable 'test1'

	###########################权限管理#####################
	1)分配权限
	# 语法 : grant <user> <permissions> <table> <column family> <column qualifier> 参数后面用逗号分隔
	# 权限用五个字母表示： "RWXCA".
	# READ('R'), WRITE('W'), EXEC('X'), CREATE('C'), ADMIN('A')
	# 例如，给用户‘test'分配对表t1有读写的权限，
	hbase(main)> grant 'test','RW','t1'

	2）查看权限
	# 语法：user_permission <table>
	# 例如，查看表t1的权限列表
	hbase(main)> user_permission 't1'

	3）收回权限
	# 与分配权限类似，语法：revoke <user> <table> <column family> <column qualifier>
	# 例如，收回test用户在表t1上的权限
	hbase(main)> revoke 'test','t1'

	############################查询功能#########################
	1）添加数据
	# 语法：put <table>,<rowkey>,<family:column>,<value>,<timestamp>
	# 例如：给表t1的添加一行记录：rowkey是rowkey001，family name：f1，column name：col1，value：value01，timestamp：系统默认
	hbase(main)> put 't1','rowkey001','f1:col1','value01'
	用法比较单一。

	2）查询数据
	a）查询某行记录
	# 语法：get <table>,<rowkey>,[<family:column>,....]
	# 例如：查询表t1，rowkey001中的f1下的col1的值
	hbase(main)> get 't1','rowkey001', 'f1:col1'
	# 或者：
	hbase(main)> get 't1','rowkey001', {COLUMN=>'f1:col1'}
	# 查询表t1，rowke002中的f1下的所有列值
	hbase(main)> get 't1','rowkey001'

	b）扫描表
	# 语法：scan <table>, {COLUMNS => [ <family:column>,.... ], LIMIT => num}
	# 另外，还可以添加STARTROW、TIMERANGE和FITLER等高级功能
	# 例如：扫描表t1的前5条数据
	hbase(main)> scan 't1',{LIMIT=>5}

	c）查询表中的数据行数
	# 语法：count <table>, {INTERVAL => intervalNum, CACHE => cacheNum}
	# INTERVAL设置多少行显示一次及对应的rowkey，默认1000；CACHE每次去取的缓存区大小，默认是10，调整该参数可提高查询速度
	# 例如，查询表t1中的行数，每100条显示一次，缓存区为500
	hbase(main)> count 't1', {INTERVAL => 100, CACHE => 500}

	3）删除数据
	a )删除行中的某个列值
	# 语法：d说说elete <table>, <rowkey>,  <family:column> , <timestamp>,必须指定列名
	# 例如：删除表t1，rowkey001中的f1:col1的数据
	hbase(main)> delete 't1','rowkey001','f1:col1'

	注：将删除改行f1:col1列所有版本的数据
	b )删除行
	# 语法：deleteall <table>, <rowkey>,  <family:column> , <timestamp>，可以不指定列名，删除整行数据
	# 例如：删除表t1，rowk001的数据
	hbase(main)> deleteall 't1','rowkey001'

	c）删除表中的所有数据
	# 语法： truncate <table>
	# 其具体过程是：disable table -> drop table -> create table
	# 例如：删除表t1的所有数据
	hbase(main)> truncate 't1'


Thtift 连接Hbase:###############################################

	一两种thrift的java客户端的连接方式 

	如果是使用thrift1 
	hbase-daemon.sh start thrift 

	如果是使用thrift2 
	hbase-daemon.sh start thrift2 

	官方例子在 
	/data/hadoop/hbase/hbase-0.94.17/src/examples/thrift 

	thrift文件在 
	./src/main/resources/org/apache/hadoop/hbase/thrift/Hbase.thrift 

	thrift2 的例子在 
	/data/hadoop/hbase/hbase-0.94.17/src/examples/thrift2 

	thrift2文件在 
	./src/main/resources/org/apache/hadoop/hbase/thrift2/hbase.thrift 
