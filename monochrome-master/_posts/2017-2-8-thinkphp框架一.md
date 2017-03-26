<<<<<<< HEAD
**引入thinkphp框架**

1. 解压文件
2. 得到ThinkPHP目录(只要这个就可以)
3. ThinkPHP可以放在任意目录(不一定非要www下)
4. 建立项目, 如shop , cms, blog等
5. 项目目录下,创建index.php

核心版core: 只有框架的MVC核心功能

完全版full: 有框架核心和实用类(上传,分页,验证码等)

在public中index.php内容如下:

define('APP_PATH' , __DIR__.'/'); // 定义当前文件地址
require('../ThinkPHP/ThinkPHP.php'); //修改路径

注意: ThinkPHP中约定,目录必须以'/'结尾

**命名空间的使用**

1.如果没有指定命名空间的使用，就自动使用离调用函数最近的命名空间

如果要调用其他命名空间，那就使用路径

    <?php
    
    namespace kj1;
    function getmsg(){
    echo '123';
    }
    
    namespace kj2;
    function getmsg(){
    echo '456';
    }
    
    \kj1\getmsg(); //123
    
    
    
      ?>


多级命名空间及三种访问方式

多级命名空间

    namespace beijing\haidian; //haidian就是子空间
    class a{
    function getmsg(){
    echo '123';
    }
    }

多级命名空间使用

    namespace shanghai\jiading;//jiading就是子空间
    function getmsg(){
    echo '456';
    }
    
    \shanghai\jiading\getmsg(); //456


三种访问方式

1.非限定名称访问方式

就是不指定空间，就访问当前空间直接调用函数

2.限定名称访问方式

限定就不是从根目录开始，也就是相对路径

    <?php
    
    namespace beijing\haidian; //haidian就是子空间
    function getmsg(){
    echo '123';
    
    }
    
    
    namespace shanghai\jiading;//jiading就是子空间
    function getmsg(){
    echo '456';
    }
    
    beijing\haidian\getmsg();//报错
    
    
    ?>


不是从根目录开始，那么就会从当前命名空间的路径shanghai来找指定的路径

3.完全限定名称访问方式

	namespace shanghai\jiading;//jiading就是子空间
    function getmsg(){
    echo '456';
    }
    
    \shanghai\jiading\getmsg(); //456

命名空间类使用

    namespace beijing\haidian; //haidian就是子空间
    class a{
    function getmsg(){
    echo '123';
    
    }
    }
    
    \beijing\haidian\a::getmsg();//123

或者

    namespace beijing\haidian; //haidian就是子空间
    class a{
    function getmsg(){
    echo '123';
    
    }
    }
    
    $new=new \beijing\haidian\a();
    $new->getmsg();//123


**命名空间的引入机制**

用use+限定名称  这样比完全限定方式简便
    
    namespace shanghai\jiading;//jiading就是子空间
    function getmsg(){
    echo '456';
    }
    
    use shanghai\jiading;
    
    jiading\getmsg();
   


空间类元素的引入：也就是直接引入空间里的元素，上面那种方式是引入整个空间

    namespace beijing\haidian; //haidian就是子空间
    class a{
    static $b='b';
    function getmsg(){
    echo '123';
    
    }
    }
    
    use beijing\haidian\a; //直接在路径后面写上要调用的类名
    echo a::$b;
    

**公共空间**

在1.php中

    namespace beijing;
    class a{
    static $b='b';
    function getmsg(){
    echo '123';
    
    }
    }

在2.php中

    function getmsg(){
    echo '456';
    }

那么2.php没有namespace 就是公共空间

那么在1.php中引入2.php

    <?php
    
    namespace beijing\haidian; //haidian就是子空间
    class a{
    static $b='b';
    function getmsg(){
    echo '123';
    
    }
    }
    
    include './2.php';
    
    getmsg(); //123
    ?>


引入的公共空间对本地空间是没有影响的，优先调用本地命名空间

那要调用公共空间怎么办呢

    <?php
    
    namespace beijing\haidian; //haidian就是子空间
    class a{
    static $b='b';
    function getmsg(){
    echo '123';
    
    }
    }
    
    include './2.php';
    
    \getmsg();//前面加一个 \ 路径就可以
    ?>


但是如果调用一个当前命名空间没有而公共空间有的时候，那么就会调用

现在相反情况，在2.php中引入1.php，也就是在公共空间引入有命名空间的情况下

    <?php
    
    function getmsg(){
    echo '456';
    }
    
    include './1.php';
    
    getmsg();//456
	\getmsg();//效果一样，但是这样写比较规范，清楚
      ?>

没有限定命名空间，所以默认为访问当前空间元素，也就是公共空间

那要访问指定空间就要用完全限定

    <?php
    
    function getmsg(){
    echo '456';
    }
    
    include './1.php';
    
    \beijing\a::getmsg();//123
      ?>


基本上运用tp引用的都是空间类元素 \namespace\class

**自动生成**


在thinkphp文件夹中打开\public\index.php和build.php

在index.php中写入自动生成的函数

    <?php
    // +----------------------------------------------------------------------
    // | ThinkPHP [ WE CAN DO IT JUST THINK ]
    // +----------------------------------------------------------------------
    // | Copyright (c) 2006-2016 http://thinkphp.cn All rights reserved.
    // +----------------------------------------------------------------------
    // | Licensed ( http://www.apache.org/licenses/LICENSE-2.0 )
    // +----------------------------------------------------------------------
    // | Author: liu21st <liu21st@gmail.com>
    // +----------------------------------------------------------------------
    
    // [ 应用入口文件 ]
    
    // 定义应用目录
    define('APP_PATH', __DIR__ . '/../application/');
    // 加载框架引导文件
    require __DIR__ . '/../thinkphp/start.php';
    //读取自动生成定义文件
    $build=include APP_PATH.'build.php'; //要修改路径
    //运行自动生成
    \think\Build::run($build);

此时在网址中写入localhost:8081/thinkphp/public 在appliaction 文件夹下就会自动生成一个demo 的文件夹

这是因为build.php 里面写好的参数

    <?php
    // +----------------------------------------------------------------------
    // | ThinkPHP [ WE CAN DO IT JUST THINK ]
    // +----------------------------------------------------------------------
    // | Copyright (c) 2006~2016 http://thinkphp.cn All rights reserved.
    // +----------------------------------------------------------------------
    // | Licensed ( http://www.apache.org/licenses/LICENSE-2.0 )
    // +----------------------------------------------------------------------
    // | Author: liu21st <liu21st@gmail.com>
    // +----------------------------------------------------------------------
    
    return [
    // 生成应用公共文件
    '__file__' => ['common.php', 'config.php', 'database.php'],
    
    // 定义demo模块的自动生成 （按照实际定义的文件名生成）
    'demo' => [
    '__file__'   => ['common.php'],
    '__dir__'=> ['behavior', 'controller', 'model', 'view'],
    'controller' => ['Index', 'Test', 'UserType'],
    'model'  => ['User', 'UserType'],
    'view'   => ['index/index'],
    ],
    // 其他更多的模块定义
    ];
    


还有第二种方法自动生成

不依赖自动生成的模块，直接写

    <?php
    // +----------------------------------------------------------------------
    // | ThinkPHP [ WE CAN DO IT JUST THINK ]
    // +----------------------------------------------------------------------
    // | Copyright (c) 2006-2016 http://thinkphp.cn All rights reserved.
    // +----------------------------------------------------------------------
    // | Licensed ( http://www.apache.org/licenses/LICENSE-2.0 )
    // +----------------------------------------------------------------------
    // | Author: liu21st <liu21st@gmail.com>
    // +----------------------------------------------------------------------
    
    // [ 应用入口文件 ]
    
    // 定义应用目录
    define('APP_PATH', __DIR__ . '/../application/');
    // 加载框架引导文件
    require __DIR__ . '/../thinkphp/start.php';
    //自动生成文件
    \think\Build::module('admin');


\think\Build::module('name')  直接使用此函数,name为自定义名字,就可以生成，但是生成的文件夹内容稍有不同

![](http://i.imgur.com/6ZPf68D.jpg)

这就是默认和定义文件夹的区别

=======
**引入thinkphp框架**

1. 解压文件
2. 得到ThinkPHP目录(只要这个就可以)
3. ThinkPHP可以放在任意目录(不一定非要www下)
4. 建立项目, 如shop , cms, blog等
5. 项目目录下,创建index.php

核心版core: 只有框架的MVC核心功能

完全版full: 有框架核心和实用类(上传,分页,验证码等)

在public中index.php内容如下:

define('APP_PATH' , __DIR__.'/'); // 定义当前文件地址
require('../ThinkPHP/ThinkPHP.php'); //修改路径

注意: ThinkPHP中约定,目录必须以'/'结尾

**命名空间的使用**

1.如果没有指定命名空间的使用，就自动使用离调用函数最近的命名空间

如果要调用其他命名空间，那就使用路径

    <?php
    
    namespace kj1;
    function getmsg(){
    echo '123';
    }
    
    namespace kj2;
    function getmsg(){
    echo '456';
    }
    
    \kj1\getmsg(); //123
    
    
    
      ?>


多级命名空间及三种访问方式

多级命名空间

    namespace beijing\haidian; //haidian就是子空间
    class a{
    function getmsg(){
    echo '123';
    }
    }

多级命名空间使用

    namespace shanghai\jiading;//jiading就是子空间
    function getmsg(){
    echo '456';
    }
    
    \shanghai\jiading\getmsg(); //456


三种访问方式

1.非限定名称访问方式

就是不指定空间，就访问当前空间直接调用函数

2.限定名称访问方式

限定就不是从根目录开始，也就是相对路径

    <?php
    
    namespace beijing\haidian; //haidian就是子空间
    function getmsg(){
    echo '123';
    
    }
    
    
    namespace shanghai\jiading;//jiading就是子空间
    function getmsg(){
    echo '456';
    }
    
    beijing\haidian\getmsg();//报错
    
    
    ?>


不是从根目录开始，那么就会从当前命名空间的路径shanghai来找指定的路径

3.完全限定名称访问方式

	namespace shanghai\jiading;//jiading就是子空间
    function getmsg(){
    echo '456';
    }
    
    \shanghai\jiading\getmsg(); //456

命名空间类使用

    namespace beijing\haidian; //haidian就是子空间
    class a{
    function getmsg(){
    echo '123';
    
    }
    }
    
    \beijing\haidian\a::getmsg();//123

或者

    namespace beijing\haidian; //haidian就是子空间
    class a{
    function getmsg(){
    echo '123';
    
    }
    }
    
    $new=new \beijing\haidian\a();
    $new->getmsg();//123


**命名空间的引入机制**

用use+限定名称  这样比完全限定方式简便
    
    namespace shanghai\jiading;//jiading就是子空间
    function getmsg(){
    echo '456';
    }
    
    use shanghai\jiading;
    
    jiading\getmsg();
   


空间类元素的引入：也就是直接引入空间里的元素，上面那种方式是引入整个空间

    namespace beijing\haidian; //haidian就是子空间
    class a{
    static $b='b';
    function getmsg(){
    echo '123';
    
    }
    }
    
    use beijing\haidian\a; //直接在路径后面写上要调用的类名
    echo a::$b;
    

**公共空间**

在1.php中

    namespace beijing;
    class a{
    static $b='b';
    function getmsg(){
    echo '123';
    
    }
    }

在2.php中

    function getmsg(){
    echo '456';
    }

那么2.php没有namespace 就是公共空间

那么在1.php中引入2.php

    <?php
    
    namespace beijing\haidian; //haidian就是子空间
    class a{
    static $b='b';
    function getmsg(){
    echo '123';
    
    }
    }
    
    include './2.php';
    
    getmsg(); //123
    ?>


引入的公共空间对本地空间是没有影响的，优先调用本地命名空间

那要调用公共空间怎么办呢

    <?php
    
    namespace beijing\haidian; //haidian就是子空间
    class a{
    static $b='b';
    function getmsg(){
    echo '123';
    
    }
    }
    
    include './2.php';
    
    \getmsg();//前面加一个 \ 路径就可以
    ?>


但是如果调用一个当前命名空间没有而公共空间有的时候，那么就会调用

现在相反情况，在2.php中引入1.php，也就是在公共空间引入有命名空间的情况下

    <?php
    
    function getmsg(){
    echo '456';
    }
    
    include './1.php';
    
    getmsg();//456
	\getmsg();//效果一样，但是这样写比较规范，清楚
      ?>

没有限定命名空间，所以默认为访问当前空间元素，也就是公共空间

那要访问指定空间就要用完全限定

    <?php
    
    function getmsg(){
    echo '456';
    }
    
    include './1.php';
    
    \beijing\a::getmsg();//123
      ?>


基本上运用tp引用的都是空间类元素 \namespace\class

**自动生成**


在thinkphp文件夹中打开\public\index.php和build.php

在index.php中写入自动生成的函数

    <?php
    // +----------------------------------------------------------------------
    // | ThinkPHP [ WE CAN DO IT JUST THINK ]
    // +----------------------------------------------------------------------
    // | Copyright (c) 2006-2016 http://thinkphp.cn All rights reserved.
    // +----------------------------------------------------------------------
    // | Licensed ( http://www.apache.org/licenses/LICENSE-2.0 )
    // +----------------------------------------------------------------------
    // | Author: liu21st <liu21st@gmail.com>
    // +----------------------------------------------------------------------
    
    // [ 应用入口文件 ]
    
    // 定义应用目录
    define('APP_PATH', __DIR__ . '/../application/');
    // 加载框架引导文件
    require __DIR__ . '/../thinkphp/start.php';
    //读取自动生成定义文件
    $build=include APP_PATH.'build.php'; //要修改路径
    //运行自动生成
    \think\Build::run($build);

此时在网址中写入localhost:8081/thinkphp/public 在appliaction 文件夹下就会自动生成一个demo 的文件夹

这是因为build.php 里面写好的参数

    <?php
    // +----------------------------------------------------------------------
    // | ThinkPHP [ WE CAN DO IT JUST THINK ]
    // +----------------------------------------------------------------------
    // | Copyright (c) 2006~2016 http://thinkphp.cn All rights reserved.
    // +----------------------------------------------------------------------
    // | Licensed ( http://www.apache.org/licenses/LICENSE-2.0 )
    // +----------------------------------------------------------------------
    // | Author: liu21st <liu21st@gmail.com>
    // +----------------------------------------------------------------------
    
    return [
    // 生成应用公共文件
    '__file__' => ['common.php', 'config.php', 'database.php'],
    
    // 定义demo模块的自动生成 （按照实际定义的文件名生成）
    'demo' => [
    '__file__'   => ['common.php'],
    '__dir__'=> ['behavior', 'controller', 'model', 'view'],
    'controller' => ['Index', 'Test', 'UserType'],
    'model'  => ['User', 'UserType'],
    'view'   => ['index/index'],
    ],
    // 其他更多的模块定义
    ];
    


还有第二种方法自动生成

不依赖自动生成的模块，直接写

    <?php
    // +----------------------------------------------------------------------
    // | ThinkPHP [ WE CAN DO IT JUST THINK ]
    // +----------------------------------------------------------------------
    // | Copyright (c) 2006-2016 http://thinkphp.cn All rights reserved.
    // +----------------------------------------------------------------------
    // | Licensed ( http://www.apache.org/licenses/LICENSE-2.0 )
    // +----------------------------------------------------------------------
    // | Author: liu21st <liu21st@gmail.com>
    // +----------------------------------------------------------------------
    
    // [ 应用入口文件 ]
    
    // 定义应用目录
    define('APP_PATH', __DIR__ . '/../application/');
    // 加载框架引导文件
    require __DIR__ . '/../thinkphp/start.php';
    //自动生成文件
    \think\Build::module('admin');


\think\Build::module('name')  直接使用此函数,name为自定义名字,就可以生成，但是生成的文件夹内容稍有不同

![](http://i.imgur.com/6ZPf68D.jpg)

这就是默认和定义文件夹的区别

>>>>>>> origin/master
