##如果父div中，有两个子div浮动，那父div高度有没有被子div撑起，父div有多高？##


答案是：没有。 因为子div是浮动的，也就是脱离了正常文档流，不受父的控制。所以父div 的高度为零



##一个简易的首页布局 ##

'<!DOCTYPE html>
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
		*{
			border: 1px solid black;
		}
		#header{
			margin: 10px;
			height: 120px;
			background-color: blue;
		}
		#main{
			margin:10px;
			height: 600px;
			background-color: orange;
		}
		#lside{
			margin: 5px 10px 5px 5px;
			float: left;
			width: 700px;
			height:550px;
			background-color: pink;
		}
		.four{
			margin:10px;
			float: left;
			width: 325px;
			height: 250px;
			background-color: yellow;
		}
		#rside{
			margin: 5px 5px 5px 10px;
			float: right;
			width: 260px;
			height:550px;
			background-color: pink;
		}
		#footer{
			margin:10px;
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
				<div class="four"></div>
				<div class="four"></div>
				<div class="four"></div>
				<div class="four"></div>
			</div>
			<div id="rside"></div>
		</div>
		<div id="footer"></div>
	</div>
</body>
</html>'
