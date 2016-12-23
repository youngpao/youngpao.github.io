#div田字格布局 #

##清除浮动 ##

	<meta charset="utf-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<title></title>
	<style>
		#header{
			float: left;
			background-color: blue;
			width: 200px;
			height:200px;
		}
		#footer{
			float: left;
			background-color: gray;
			width: 200px;
			height:200px;
		}
		#down{
			float: left;
			background-color: red;
			width:200px;
			height: 200px;
			clear: left;
		}
		#down1{
			float: left;
			background-color: orange;
			width: 200px;
			height: 200px;
		}
	</style>
</head>

<body>
	<div id="header"></div>
	<div id="footer"></div>
	<div id="down"></div>
	<div id="down1"></div>
</body>
</html>


在红块的代码中，加入 `clear:left` ，就会形成四色田子方块，clear是声明不允许有浮动元素