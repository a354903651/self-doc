### code-push 安装教程


- 1、 调整npm  镜像地址

		npm config set registry https://registry.npm.taobao.org --global
		npm config set disturl https://npm.taobao.org/dist --gl

- 2、npm 安装codepush 

		npm install -g code-push-cli


- 3、创建账号

		code-push register

	会打开网站完成注册 推荐用gitHub账号关联

		注册成功后：Authentication succeeded ：c7131cbb19c3a96e87a4089cbe5568f1c7a363db 
		Successfully logged-in. Your session file was written to C:\Users\chenlei1\AppData\Local\.code-push.config. You can run the code-push logout command at any time to delete this file and terminate your session.

- 4、创建一个app项目
 		
		code-push app add soho-demp  windows react-native
		
		会返回两个对应的 Deployment Key     

		┌────────────┬──────────────────────────────────────────────────────────────────┐
		│ Name       │ Deployment Key                                                   │
		├────────────┼──────────────────────────────────────────────────────────────────┤
		│ Production │ FU3dwf94eJWEvOM3oeZoxGL4txTC9980eddc-589e-4920-9c4b-50ac28307f7d │	
		├────────────┼──────────────────────────────────────────────────────────────────┤
		│ Staging    │ Elefym0x5JQeYJcRd9XsC7zTg1vq9980eddc-589e-4920-9c4b-50ac28307f7d │
		└────────────┴──────────────────────────────────────────────────────────────────┘
- 5、npm 安装react-native
		
		npm install -g yarn react-native-cli
		以android 为例子
		react-native 需要android环境和java环境做支撑		
		安装android环境 略
		安装地址 如下：
	[https://reactnative.cn/docs/0.51/getting-started.html#android-studio](https://reactnative.cn/docs/0.51/getting-started.html#android-studio "安装教程地址")
- 6、创建一个react-native 项目

		执行： react-native init sohoDemo

		To run your app on iOS:
		cd C:\workProject\android\sohoDemo
	    react-native run-ios
	    - or -
	    Open ios\sohoDemo.xcodeproj in Xcode
	    Hit the Run button
		To run your app on Android:
	    cd C:\workProject\android\sohoDemo
	    Have an Android emulator running (quickest way to get started), or a device connected
	    react-native run-android
- 6集成 react-native 和code-push
		
		npm install --save react-native-code-push
		npm install rnpm -g  #安装rnpm
		rnpm link react-native-code-push  #rnpm 帮忙加载gradle配置
		
	也可以手动添加配置
		
		-1. 在 android/app/build.gradle文件里面添如下代码： 
		  apply from: “../../node_modules/react-native-code-push/android/codepush.gradle” 
		-2. 在/android/settings.gradle中添加如下代码: 
		  include ‘:react-native-code-push’ 
		  project(‘:react-native-code-push’).projectDir = new File(rootProject.projectDir, ‘../node_modules/react-native-code-push/android/app’) 
		-3. 在 android/app/build.gradle文件里面添如下代码将CodePush添加项目编译: 
		 compile project(‘:react-native-code-push’) 
		-4. 运行 code-push deployment ls RnApp -k 命令获取 部署秘钥
		-5. 在 MainApplication.java 中添加如下代码： 
		  //注意需要将部署键值替换成自己的项目的唯一键值
		  new CodePush(“deployment-key-here”, MainApplication.this, BuildConfig.DEBUG) 
		-6. 在indexjs里加入相应代码块
			componentDidMount{
			    codePushUpdate();
			  }
			  //远程服务检测更新
			  codePushUpdate{
			    codePush.sync({
			      installMode: codePush.InstallMode.IMMEDIATE,
			      updateDialog: true
			     },
			     (status) => {
			      switch (status) {
			        case codePush.SyncStatus.CHECKING_FOR_UPDATE:
			            console.log('codePush.SyncStatus.CHECKING_FOR_UPDATE');
			          break;
			        case codePush.SyncStatus.AWAITING_USER_ACTION:
			          console.log('codePush.SyncStatus.AWAITING_USER_ACTION');
			          break;
			        case codePush.SyncStatus.DOWNLOADING_PACKAGE:
			          console.log('codePush.SyncStatus.DOWNLOADING_PACKAGE');
			          break; 
			        case codePush.SyncStatus.INSTALLING_UPDATE:
			          console.log('codePush.SyncStatus.INSTALLING_UPDATE');
			          break;
			        case codePush.SyncStatus.UP_TO_DATE:
			          console.log('codePush.SyncStatus.UP_TO_DATE');
			          break;
			        case codePush.SyncStatus.UPDATE_IGNORED:
			          console.log('codePush.SyncStatus.UPDATE_IGNORED');
			          break;
			        case codePush.SyncStatus.UPDATE_INSTALLED:
			          console.log('codePush.SyncStatus.UPDATE_INSTALLED');
			          break;
			        case codePush.SyncStatus.SYNC_IN_PROGRESS:
			          console.log('codePush.SyncStatus.SYNC_IN_PROGRESS');
			          break;
			        case codePush.SyncStatus.UNKNOWN_ERROR:
			          console.log('codePush.SyncStatus.UNKNOWN_ERROR');
			          break;
			      }
			     },
			     ({ receivedBytes, totalBytes, }) => {
			      console.log('receivedBytes / totalBytes: ------------    ' + receivedBytes+'/'+totalBytes);
			      }
			    );
			  }
		7. 在RN更目录下新建两个文件夹 
			注意：将项目的VersionName修改为三位数！（例如：1.0.0） 
  - 7 基于codepush 热更新
	  
	第一种方式：通过code-push release-react发布更新

	命令格式：

		code-push release-react <appName> <platform>
		code-push release-react MyApp-iOS ios
		code-push release-react MyApp-Android android  #默认为Staging
		code-push release-react MyApp-iOS ios  --t 1.0.0 --dev false --d Production --des "1.优化操作流程" --m true

	第二中方式：通过code-push release发布更新
	
	需要先生成bundle
	
	具体内容参见[简书地址](https://www.jianshu.com/p/9e3b4a133bcc)  [csdn地址](http://blog.csdn.net/it_luntan/article/details/70243476)
	- 8 其他问题
		
	android 安装相关坑
	
	 - 1、 android studo 下载地址：[https://developer.android.com/studio/index.html](https://developer.android.com/studio/index.html)
	 - 2、android studo 打开 react-native 项目时定位到 projectDir/android 下
	 - 3、添加code-push-react包以后。如果gradle无法编译，需手动从添加 module dependency ,否则相应java代码无法找到对应的jar
	 - 4、需要 AVD本地调试的时候，需通过SDK Manager安装 HAXM（联想电脑可能安装失败需改动注册表的intel Virtualization Technologly为true）
	 - 5、当项目没有错误，通过andorid studo ->run 启动项目时。会指定 AVD设备。当前项目自动将当前项目打包安装到 AVD中，如果有修改建议先卸载重装（应该有更好的方式。不太了解andorid）
	 - 6、打开APP如果红屏 无法连接到服务器。  打开到react-navtive  根目录 执行如下命令
		 
			 react-native start
		ps: 当然如果有其他问题。由于代码的原因。会有具体错误提示。结合具体问题排查
	 - 7 发布命令和查看命令
		 
			code-push release-react soho-demp  android --发布andorid
			code-push deployment history soho-demp Staging -- 查看发不过的staging版本
			