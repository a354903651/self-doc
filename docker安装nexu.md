### docker 安装 nexu(maven私服)

 
如果配置完阿里云cdn加速可以直接用官方镜像

	
		docker pull sonatype/nexus
		### 启动
		docker run -d -p 8082:8081 --name nexus -v /work/nexus-data:/nexus-data sonatype/nexus
		###如果启动失败涉及到权限问题要对volum目录授权 
		chmod -R  200 /work/nexus-data   ###至于为什么200需要查官方文档？

		##再次启动即可

		附带docker命令
			
		docker logs  ##continerId##

		docker ps -a      
		
		docker stop ##continerId##

		docker rm ##continerId##
		