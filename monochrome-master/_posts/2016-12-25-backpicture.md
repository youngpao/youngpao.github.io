#设置背景图片 #

    <!DOCTYPE html>
    <html>
    <head>
    	<meta charset="utf-8">
    	<meta http-equiv="X-UA-Compatible" content="IE=edge">
    	<title>CSS</title>
    	<link rel="stylesheet" href="">
    	<style>
    		  body{
    		  	background-image:url(small.jpg);
    		  	background-repeat:repeat-x;
    		  	background-attachment:fixed;
    		  }
    	</style>
    </head>
    <body>
    	
    </body>
    </html>




background-image:url() 用来引用图片 

background-repeat:repeat-x/repeat-y/no-repeat 用来控制图片平铺方向或者不平铺

background-attachment:fixed 用来表示固定图片位置，设置后图片不随网页移动改变位置

**当背景颜色与图片同时设置时，图片会覆盖颜色，但还要设置颜色的原因是，当网页加载时，图片还没加载出来可以先加载背景颜色，这样有个过渡期，或者当屏幕过大时，超出的那部分图片不能覆盖，就用背景颜色代替，算是一种保险措施。**


#精确控制图片位置 #


当有一张图片时，放在div中，只想显示具体的部位，就要用background-position:x y 来精确控制显示的部位


在计算机中，是以左上角开始为原点，往右为x正轴，往下为y正轴。


    <!DOCTYPE html>
    <html>
    <head>
    	<meta charset="utf-8">
    	<meta http-equiv="X-UA-Compatible" content="IE=edge">
    	<title>CSS</title>
    	<link rel="stylesheet" href="">
    	<style>
    		  div{
    			width: 200px;
    			height: 200px;
    			border: 1px solid black;
    		  	background-image:url(small.jpg);
    		  	background-repeat:repeat-x;
    		  	background-position: -20px -20px;
    		  }
    	</style>
    </head>
    <body>
    	<div></div>
    </body>
    </html>


可以简写为`background: color url() repeat attachment position `按照顺序来写参数，在开发中需要多次使用同一张图片选取不同部位来显示。