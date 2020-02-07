### 4.1 exmobi开发

客户端示例代码完整包[下载](img/exmobi_service_client.zip)

#### 4.1.1 全局环境变量定义及微服务鉴权工具类AccessAuthUtil使用示例

对全局使用的环境变量进行统一定义，方便维护和全局调用，同时对微服务鉴权工具类 (AccessAuthUtil)进行封装便于快捷调用。

例如示例tokensdk文件，开发者在实际开发过程中可根据自身需求进行自定义封装；授权码申请参考3.7章节

```javascript
/**
 * 平台接入封装
 * accessCode：访问接入授权码  【注：开发者请替换成自己的访问码，在后台管理页面可以进行正式授权码申请，所填写应用ID需要和本插件应用ID一致】
 * serverUrl：平台接入服务端http协议接入地址【注：开发者请替换成自己搭建的平台http协议访问地址】
 * serverUrl_https：平台接入服务端https协议接入地址【注：开发者请替换成自己搭建的平台https协议访问地址】
 * init：SDK初始化
 * exec：SDK获取token，并执行回调方法同时传递token值
 */
var tokensdk = {
	"accessCode" : "79a1a358-552b-4e70-b875-2e7fc7547cb7",
	"serverUrl" : "http://172.16.2.23:8001",
	init : function() {
		//该方法为SDK初始化操作
		var jsonData = {};
		jsonData.accessCode = this.accessCode;
		AccessAuthUtil.init(jsonData);
	},
	exec : function (func) {
		//调用初始化
		if (!this.isinit) {
			this.init();
			this.isinit = true;
		}
		//获取访问码，调用回调方法
		var jsonData = {};
		jsonData.url = this.serverUrl + "/api/getAccessToken";
		AccessAuthUtil.getToken(jsonData, func);
	}
};
```

#### 4.1.2 登陆并展示给人信息接口调用

```html
<html>
	<head>
		<meta content="charset=utf-8"/>
		<title>Hello World</title>
		<script type="text/javascript" src="res:script/tokensdk.js"></script>
		<script>
		<![CDATA[
			function doLogin(json){
				var username = document.getElementById("username").value;
				var pwd = document.getElementById("pwd").value;
	            var ajaxData = {};
				ajaxData.data = "username="+username+"&pwd="+pwd;
			    ajaxData.isBlock = true;
			    ajaxData.method = "GET";
                //调用SpringMVC服务进行登录获取用户信息
			    ajaxData.url = tokensdk.serviceUrl + "/demo/login?access_token="+json.token;
			    ajaxData.successFunction = "doLoginSuc";
			    ajaxData.failFunction = "failFun";
			    var ajax = new DirectAjax(ajaxData);
	            ajax.send();
			}
			
			function doLoginSuc(data){
				var rsptext = ClientUtil.stringToJson(data.responseText);
				var accountName = rsptext.articles[0].accountName;
				var email = rsptext.articles[0].email;
				var tel = rsptext.articles[0].tel;
				var trueName = rsptext.articles[0].trueName;
				window.open("res:page/user.uixml?accountName="+accountName+"&email="+email+"&tel="+tel+"&trueName="+trueName);
			}
		</script>
	</head>
	<body>
		 <input type="text" prompt="用户名" hideborder="true" id="username" value=""/>
         <input type="password" prompt="密码" hideborder="true" id="pwd" value=""/>
         <input type="button" value="登录" onclick="tokensdk.exec(doLogin)"></input>
	</body>
</html>
```

#### 4.1.3 文件预览

```html
<!-- ExMobi UIXML(XHTML)文件 -->
<html>
	<head>
		<meta content="charset=utf-8"/>
		<title>Hello World</title>
		<script type="text/javascript" src="res:script/tokensdk.js"></script>
		<script>
		<![CDATA[
			
			var totalPage = parseInt(window.getParameter("totalPage"));
			var currentPage = parseInt(window.getParameter("currentPage"));

			function init(){
				var img = window.getStringSession("img");
				document.getElementById("imgId").src = "data:image/jpeg;base64,"+img;
			}
			function previousPage(){
				if(currentPage == 1){
					alert("已经是第一页");
				}else{
					currentPage -= 1;
					tokensdk.exec(changeImg);
				}
			}
			function nextPage(){
				if(currentPage == totalPage){
					alert("已经是最后一页");
				}else{
					currentPage += 1;
					tokensdk.exec(changeImg);
				}
			}
			
			function changeImg(json){
				var pageNum = currentPage;
				var ajaxData = {};      
				ajaxData.data = "pageNum="+pageNum;
			    ajaxData.isBlock = true; 
			    ajaxData.method = "GET";
			    ajaxData.url = tokensdk.serviceUrl + "/demo/doPreview?access_token="+json.token;
			    ajaxData.successFunction = "changeImgSuc";
			    ajaxData.failFunction = "failFun";
			    var ajax = new DirectAjax(ajaxData);
	            ajax.send();
			}
			
			function changeImgSuc(data){
				var rsptext = ClientUtil.stringToJson(data.responseText);
				var img = rsptext.imgBase64Str;
				currentPage = rsptext.currentPageNum;
				document.getElementById("imgId").src = "data:image/jpeg;base64,"+img;
			}
			
			function failFun(){
				alert("fail");
			}
		]]>
		</script>
	</head>
	<body onload="init()">
		<img id="imgId" src="" href=""></img>
		<input type="button" value="上一页" onclick="previousPage()"></input>
		<input type="button" value="下一页" onclick="nextPage()"></input>
	</body>
</html>
```



#### 4.1.4 消息推送并保存消息到数据库接口调用

```html
<html>
	<head>
		<meta content="charset=utf-8"/>
		<title>Hello World</title>
		<script type="text/javascript" src="res:script/tokensdk.js"></script>
		<script>
		<![CDATA[
			function doPush(json){
				var esn = DeviceUtil.getEsn();
				var ajaxData = {};
				ajaxData.data = "esn="+esn;
			    ajaxData.isBlock = true;
			    ajaxData.method = "GET";
                // 调用服务端接口进行消息推送
			    ajaxData.url = tokensdk.serviceUrl + "/demo/sendPush?access_token="+json.token;
			    ajaxData.successFunction = "doPushSuc";
			    ajaxData.failFunction = "failFun";
			    var ajax = new DirectAjax(ajaxData);
	            ajax.send();
			}
			
			function doPushSuc(data){
				var rsptext = ClientUtil.stringToJson(data.responseText);
				alert(rsptext.result);
			}
		</script>
	</head>
	<body>
		 <input type="text" prompt="用户名" hideborder="true" id="username" value=""/>
         <input type="password" prompt="密码" hideborder="true" id="pwd" value=""/>
         <input type="button" value="登录" onclick="tokensdk.exec(doLogin)"></input>
	</body>
</html>
```
