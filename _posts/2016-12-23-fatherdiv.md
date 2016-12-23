##如果父div中，有两个子div浮动，那父div高度有没有被子div撑起，父div有多高？##


答案是：没有。 因为子div是浮动的，也就是脱离了正常文档流，不受父的控制。所以父div 的高度为零



##一个简易的首页布局 ##

    <!DOCTYPE html>
    <html>
    <head>
    	<meta charset="utf-8">
    	<meta http-equiv="X-UA-Compatible" content="IE=edge">
    	<title>首页布局</title>
    	<link rel="stylesheet" href="">
    	<style>
    		#container{
    			width: 1020px;
    			background-color: gray;
    		}
    		#header{
    			height:120px;
    			background-color: blue;
    		}
    		#main{
    			height: 600px;
    			background-color: orange;
    		}·
    		#lside{
    			float: left;
    			width: 720px;
    			height: 600px;
    			background-color: pink;
    		}
    		#lside1{
    			float: left;
    			width: 300px;
    			height: 300px;
    			background-color: purple;
    		}
    		#rside1{
    			float: right;
    			width: 300px;
    			height: 300px;
    			background-color: purple;
    		}
    		#lside2{
    			float: left;
    			width: 300px;
    			height: 300px;
    			background-color: yellow;
    			clear: left;
    		}
    		#rside2{
    			float: right;
    			width: 300px;
    			height: 300px;
    			background-color: yellow;
    			clear: right;
    		}
    		#rside{
    			float:right;
    			width: 300px;
    			height: 600px;
    			background-color: red;
    		}
    		#footer{
    			height: 120px;
    			background-color: green;
    		}
    	</style>
    </head>
    <body>
    	<div id="container">
    		<div id="header"></div>
    		<div id="main">
    			<div id="lside">
    				<div id="lside1"></div>
    				<div id="rside1"></div>
    				<div id="lside2"></div>
    				<div id="rside2"></div>
    			</div>
    			<div id="rside"></div>
    		</div>
    		<div id="footer"></div>
    	</div>
    </body>
    </html>