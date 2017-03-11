websocket只是一个网络通信协议

就像http/ftp 等都是网络通信协议

相对于HTTP 这种非持久的协议，webscocket 是一个持久化网络通信协议

websocket 和http 是有交集的，但不是全部

websocket 只是借用了http 的一部分协议来完成一次握手。（http的三次握手，此处只完成一次）

HTTP:

原来的时候，客户端通过http带着信请求服务器，服务器处理请求，再次通过http返回，链接断开

WEBSOCKET：

客户端通过http带着信请求服务器，但同时，携带了upgrade:websocket和connection:upgrade （相当于两根管道），服务器如果支持websocket协议，使用websocket协议返回可用信息，伺候信息的传递均使用这两根管道，除非有一方人为的将管子切断；弱服务器不支持，客户端请求链接失败，返回错误信息。

webhsocket 和ajax轮询的区别

首先是ajax轮询，原理是让浏览器隔几秒就发送一次请求，询问服务器是否有新信息

轮询就是在不断地建立http连接，然后等待服务端处理，可以体现http协议的另一外一个特点，就是被动性。同事，hettp的每一次请求与相应结束后，服务器将客户端信息全部丢弃，下次请求，必须携带身份信息，**无状态性**。而且很容易造成 信息冗余。

websocket就解决了这些问题

服务端有新信息的时候会自动推送给客户端

下面写段客户端请求服务端websocket 的代码：

    <!DOCTYPE html>
    <html>
    <head>
    	<title>模拟聊天室</title>
    </head>
    <body>
    <div id="msg">
    	
    </div>
    <input type="text" id="text">
    <input type="submit" value="update" onclick="send()">
    </body>
    <script type="text/javascript">
    	var msg=document.getElementById('msg');
    	var text=document.getElementById('text');
    
    	//先打开websocket链接
    	var server='ws://127.0.0.1:8081';//服务器地址和端口
    	var ws=new WebSocket(server);
    	ws.onopen=function(evt){
    		msg.innerHTML=ws.readyState;
    		/*
    		websocket.readyState属性
    		CONNECTIING 0 The connection is not yet open
    		open 1 the connectuion is open and ready to communicate
    		closing 2 the connection is in the process of closing
    		closed 3 the connection is closed por couldn't be opened
    		 */
    	};
    
    
    	//一点击就获取值
    	function send(){
    		var t=text.value;
    		t.value ='';
    		ws.send(t);
    	}
    
    	//监听服务器数据的推送
    	ws.onmessage = function(evt){
    		msg.innerHTML += evt.data + '<br/>';
    	};
    
    	//监听链接关闭
    	ws.onclose = function(evt){
    		console.log('disconnected');
    	};
    
    	//监听链接错误信息
    	ws.onerroe=function(evt,e){
    		console.log('error occured:' + evt.data );
    	};
    </script>
    </html>

