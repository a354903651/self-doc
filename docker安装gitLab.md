###Docker 安装 gitlab

- 1、基于阿里云镜像安装
	
	- 镜像站中搜索 “gitlab-ce-zh”
	
		
		镜像名称：gitlab-ce-zh
		镜像来源：阿里云镜像
		创建日期：2016-06-01 更新日期：2017-03-14
		镜像性质：公开
		镜像地域：华东 1
		公网地址：docker pull registry.cn-hangzhou.aliyuncs.com/lab99/gitlab-ce-zh
		经典内网：docker pull registry-internal.cn-hangzhou.aliyuncs.com/lab99/gitlab-ce-zh
		VPC网络：docker pull registry-vpc.cn-hangzhou.aliyuncs.com/lab99/gitlab-ce-zh

    - 基于docker-compose安装

   	创建docker-compose.yml 文件

		version: '2'
		services:
		    web:
		      image: 'twang2218/gitlab-ce-zh:latest'
		      restart: always
		      hostname: 'gitlab.example.com'
		      environment:
		        GITLAB_OMNIBUS_CONFIG: |
		          external_url 'http://gitlab.example.com'
		          # Add any other gitlab.rb configuration here, each on its own line
		      ports:
		        - '80:80'
		        - '443:443'
		        - '22:22'
		      volumes:
		        - config:/etc/gitlab
		        - data:/var/opt/gitlab
		        - logs:/var/log/gitlab
		volumes:
		    config: {}
		    data: {}
		    logs: {}


	打开到docker-compose 的文件目录执行
	
		docker-compose up -d

   - 基于docker run 启动

		
		docker run --detach \
	    --hostname gitlab.example.com \
	    --publish 443:443 --publish 80:80 --publish 22:22 \
	    --name gitlab \
	    --restart always \
	    --volume /srv/gitlab/config:/etc/gitlab \
	    --volume /srv/gitlab/logs:/var/log/gitlab \
	    --volume /srv/gitlab/data:/var/opt/gitlab \
	    twang2218/gitlab-ce-zh:latest

		docker run --detach \
	    --hostname gitlab.soho3q.com \
	    --publish 8080:80 --publish 8022:22 \
	    --name gitlab \
	    --restart always \
	    --volume /srv/gitlab/config:/etc/gitlab \
	    --volume /srv/gitlab/logs:/var/log/gitlab \
	    --volume /srv/gitlab/data:/var/opt/gitlab \
	    twang2218/gitlab-ce-zh:latest


第一次启动 GitLab 后，使用下列默认用户和密码登录，并修改密码：
	
		用户名: `root`
		密码: `5iveL!fe`