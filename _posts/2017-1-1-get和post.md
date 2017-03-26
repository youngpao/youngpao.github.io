#get和post用法 #


$_GET和$_POST都是传递数据，php处理，输出结果，但是$_GET 是通过 URL 参数传递到当前脚本的变量数组。$_POST 是通过 HTTP POST 传递到当前脚本的变量数组。


首先建立一个index.html

    <!DOCTYPE html>
    <html>
    <head>
    	<meta charset="utf-8">
    	<meta http-equiv="X-UA-Compatible" content="IE=edge">
    	<title></title>
    	<link rel="stylesheet" href="">
    </head>
    <body>
    	<form action="http://localhost:8081/test/index.php" method="post" >
    		Name:<input type="text" name="name"><br/><br/>
    		email:<input type="text" name="email"><br/><br/>
    		<input type="submit" value="submit">
    	</form>
    </body>
    </html>



创建一个index.php文件

    <?php 
    
    	echo "welcome:".$_POST["name"]."<br/>";
    	echo "your email adress is:".$_POST["email"]."<br/>";
    
     ?>



然后打开index.html 页面，随意输入名字和邮件，输出

![](http://i.imgur.com/QxnIwaM.jpg)