###docker-compose安装

- 1、采用gitHub地址安装
	
		#用curl下载安装包		（ps :注意好对应的版本）
		curl -L https://github.com/docker/compose/releases/download/1.8.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
		#配置执行权限
		chmod +x /usr/local/bin/docker-compose
		ll /usr/local/bin/docker-compose
		[root@Docker-Elk-01 work]# docker-compose -v
		docker-compose version 1.8.1, build 878cff1