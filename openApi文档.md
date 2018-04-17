##sohochina 对接API文档



###1、域名
		
	http://admin.soho3q.com/strategysvc

###2、API
	
 - 获取cid
	- url
			
		  /api/auth/getAid

	- 请求参数
 
		|参数|类型|说明|
		|:-|:-|:-|
		|appId|varchar|唯一标识（由soho提供）|
		|appSecret|varchar|用于加密的秘钥 **只用于加密不传值** （由soho提供)|
		|timestamp|long|成签名的时间戳|
		|nonceStr|varchar|生成签名的随机串|
		|signature|varchar|签名 生成规则见*附录1*|

	- 返回结果 *附录2*
		
			{
			  "status": "Y",
			  "message": "ok",	
			  "code": "0",
			  "result": {
					"aid":"AID"
				},
			  "serverIp": "0.0.0.0"
			}
	
	- 返回参数

		|参数|类型|说明|
		|:-|:-|:-|
		|cid|varchar|接口调用凭证 2小时有效|


- api其他

		
###3、附录

- 附录1 签名算法

参与签名的字段包括noncestr（随机字符串）, appId, **appSecret（用于加密的key）**，timestamp（时间戳）对所有待签名参数按照字段名的ASCII 码从小到大排序（字典序）后，使用URL键值对的格式 （即 key1=value1&key2=value2…）拼接成字符串string1。这里需要注意的是所有参数名均为小写字符。对string1作sha1加密，字段名和字段值都采用原始值，不进行URL 转义。
	
	appId=soho3q
	noncestr=Wm3WZYTPz0wzccnW
	appSecret=6e159ddc3197b7aced821b63f83deaef(请求不要传入)
	timestamp=1523859044520


步骤1. 对所有待签名参数按照字段名的ASCII 码从小到大排序（字典序）后，使用URL键值对的格式（即key1=value1&key2=value2…）拼接成字符串string1：
	
	appId=soho3q&appSecret=6e159ddc3197b7aced821b63f83deaef&nonceStr=eubo8m0b&timestamp=1523859044520

步骤2. 对string1进行sha1签名，得到signature：
		
	0826307930e75695c8a7d814dc65f267

- 附录2 签名
	
	- 响应公共参数的说明
		
		|参数|类型|说明|
		|:-|:-|:-|
		|status|varchar|返回状态 （Y成功）（N失败）|
		|message|varchar|返回的消息提示（请勿直接使用响应消息返回给用户）|
		|code|varchar|返回状态码  （0成功） （其他失败由具体业务来指定）|
		|result|json|返回结果内容|
		|serverIp|varchar|响应的服务器IP|

	- code 码对照表

		|code|对应结果|
		|:-|:-| 
		|0|返回成功|
		|000001|系统错误|
		|008001|缺少必要参数|
		|008002|对称校验失败|