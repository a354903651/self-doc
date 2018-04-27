### centos7安装docker

- 1、系统版本要求 

	在安装Docker前需要确保操作系统内核版本为 3.10以上，因此需要CentOS7 ，CentOS7内核版本为3.10
- 2、检查是否安装过旧的版本 

	如果系统安装旧版本Docker需要先卸载，命令如下：
		
		yum remove docker \
                  docker-common \
                  docker-selinux \
                  docker-engine

- 3、基于yum方式安装，需要安装以下的依赖包：yum-utils 、device-mapper-persistent-data、lvm2命令如下：
		
			yum install -y yum-utils \
			device-mapper-persistent-data \
			lvm2

- 4、 安装yum源（官方镜像速度比较慢 改为阿里云镜像）
	
		sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
		#  更新并安装 Docker-CE
		sudo yum makecache fast
		sudo yum -y install docker-ce
	
	ps:阿里云的repo文件指向为官网的地址：手动修改阿里云的镜像
	
		 vim /etc/yum.repos.d/docker-ce.repo
		 # 替换下载地址

- 5、 安装成功启动并检测

			# 开启Docker服务
			sudo service docker start
			[root@Docker-Elk-01 work]# docker version
			Client:
			 Version:	18.03.0-ce
			 API version:	1.37
			 Go version:	go1.9.4
			 Git commit:	0520e24
			 Built:	Wed Mar 21 23:09:15 2018
			 OS/Arch:	linux/amd64
			 Experimental:	false
			 Orchestrator:	swarm

- 6、配置阿里云加速镜像（非必须）
		
	登录到阿里云后台- 容器镜像服务--镜像加速器 获取专属的cdn加速url,
	通过修改daemon配置文件/etc/docker/daemon.json来使用加速器：
	
		sudo mkdir -p /etc/docker
		sudo tee /etc/docker/daemon.json <<-'EOF'
		{
			"registry-mirrors": ["https://yourUrl.mirror.aliyuncs.com"]
		}
		EOF
		sudo systemctl daemon-reload
		sudo systemctl restart docker
	
	
