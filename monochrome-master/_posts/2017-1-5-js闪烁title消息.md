    <!DOCTYPE html>
    <html>
    <head>
    	<meta charset="utf-8">
    	<meta http-equiv="X-UA-Compatible" content="IE=edge">
    	<title></title>
    	<link rel="stylesheet" href="">
    
    </head>
    <body>
    	
    </body>
    	<script type="text/javascript">
    		var tit=document.getElementsByTagName('title')[0];
    		function test(){
    			tit.innerHTML='【new message】hello';
    			setTimeout("tit.innerHTML='【&nbsp;&nbsp;&nbsp;&nbsp;】hello'",500);
    		}
    		setInterval('test ()',1000);
    	</script>
    </html>


这样标签title 就会闪烁显示 【new message】hello,要显示闪烁利用setTimeout 定时改变内容和 setInterval 每多少秒就循环一次 结合起来。