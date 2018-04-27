### docker 安装 jenkin

 
如果配置完阿里云cdn加速可以直接用官方镜像

	
		docker pull jenkins
		### 启动
		docker run -p 8080:8080 -p 50000:50000 -v /work/jenkins:/var/jenkins_home jenkins
		### 注意 这里会提示没有权限
		### 我们对 volum目录授权  官方文档要求用户是1000
		chmod -R  1000 /work/jenkins

		##再次启动即可