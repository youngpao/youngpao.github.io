# ie中存在的双倍margin bug #

    <!DOCTYPE html>
    <html>
    <head>
    	<meta charset="utf-8">
    	<meta http-equiv="X-UA-Compatible" content="IE=edge">
    	<title></title>
    	<link rel="stylesheet" href="">
    	<style type="text/css">
    		div{
    			height: 100px;
    			width: 100px;
    			border: 1px solid black;
    			float: left;
    			margin-left: 50px;
    		}
    	</style>
    </head>
    <body>
    	<div></div>
    </body>
    </html>

在ie 浏览器中， 此时div块会离左边距离有100px。这是ie的特有的bug，只要加入 `_display:inline`  这个代码只有ie能识别，控制ie消除双倍margin bug 。