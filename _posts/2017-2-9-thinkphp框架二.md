**引入前台首页模板**

存放资源的位置默认在public\ststic中，把前端模板文件放入

在application\index\config.php添加代码

     //输出替换
    'view_replace_str'=>
    '__PUBLIC__'=>'/thinkphp/public/',
    '__ROOT__'=>'/',
    ]

在controller\index.php中

    <?php
    namespace app\index\controller;
    use tnink\Controller;
    class Index extends Controller
    {
    public function index()
    {
    return $this->fetch();
    }
    }


**完成前台模板引入及分离**

