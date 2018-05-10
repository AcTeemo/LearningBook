Linux 添加代理的方法

1.首先启用：CentOS Extras repository
	wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
	sudo rpm -Uvh epel-release-latest-7*.rpm

2.安装tinyproxy
	yum -y install tinyproxy

	配置cong
		vim /etc/tinyproxy/tinyproxy.conf
		注释掉allow 127.0.0.1,或者添加指定ip
		更改端口号port 8080
	重启服务
		systemctl restart tinyproxy.service
	自动启动
		systemctl enable tinyproxy.service

3.centos添加代理方法:(无密码则无需添加username:password)
	1.全局的代理设置：

		vi /etc/profile
		http_proxy = http://username:password@yourproxy:8080/
		ftp_proxy = http://username:password@yourproxy:8080/
		export http_proxy
		export ftp_proxy
		最后执行 . /etc/profile  使配置生效

	2.yum的代理设置：

		vi /etc/yum.conf

		proxy = http://username:password@yourproxy:8080/		 

		或者

		proxy=http://yourproxy:808
		proxy=ftp://yourproxy:808
		proxy_username=username
		proxy_password=password

	3.Wget的代理设置：

		vi /etc/wgetrc

		# Proxy
		http_proxy=http://username:password@proxy_ip:port/
		ftp_proxy=http://username:password@proxy_ip:port/

## 添加镜像

### centos

`cd /etc/yum.repos.d`
> 修改 Centos-Base.repo, 添加相关镜像

### 删除缓存
`yum clean all`

### 添加新的缓存
`yum makecache`
