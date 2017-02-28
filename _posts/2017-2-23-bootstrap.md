#bootstrap #

Bootstrap 是由 Twitter 公司(全球最大的微博)的两名技术工程师研发的一个基于
HTML、CSS、JavaScript 的开源框架。该框架代码简洁、视觉优美，可用于快速、简单
地构建基于 PC 及移动端设备的 Web 页面需求。

Bootstrap 最为重要的部分就是它的响应式布局，通过这种布局可以兼容 PC 端、PAD
以及手机移动端的页面访问。

Bootstrap2中，对某些关键部分增加了对移动设备友好的样式,Bootstrap3是移动设备优
先的;

Bootstrap 下载及演示：

国内文档翻译官网：http://www.bootcss.com

特点

1.跨设备、跨浏览器

2.响应式布局

3.提供的全面的组件（导航栏，标签，按钮等）

4.内置 jQuery 插件

5.支持 HTML5、CSS3

目录结构

解压后，目录呈现这样的结构：

![](http://i.imgur.com/mgP7Yoy.png)

## 栅格系统 ##

移动设备优先

    <meta name="viewport" content="width=device-width, initial-scale=1,maxi
    mum-scale=1, user-scalable=no">
    触屏设备 添加meta user-scalable=no 可以禁用其缩放功能，视情况而定

栅格系统用于通过一系列的行（row）与列（column）的组合来创建页面布局，随着
屏幕尺寸的增加，系统会自动分为最多12列。

行（row）必须包含在 .container中，内容应当放置于列（column）内，并且，只有列
（column）可以作为行（row）的直接子元素。

栅格系统中的列是通过指定1到12的值来表示其跨越的范围。例如，三个等宽的列可以
使用三个 .col-xs-4 来创建。

![](http://i.imgur.com/cCHxxKe.jpg)

    <div class="container-fluid">
     <div class="row">
     <div class="col-md-8">.col-md-8</div>
     <div class="col-md-4">.col-md-4</div>
     </div>
    
     <div class="row">
     <div class="col-sm-4">.col-md-4</div>
     <div class="col-sm-4">.col-md-4</div>
     <div class="col-sm-4">.col-md-4</div>
     </div>
    </div>


## 路径、分页和徽章组件 ##

    路径组件做面包屑导航
	面包屑导航
    <ol class="breadcrumb">
     <li><a href="#">首页</a></li>
     <li><a href="#">产品列表</a></li>
     <li class="active">诺基亚</li>
    </ol>
  	分页组件
    默认分页
    <ul class="pagination">
     <li><a href="#">&laquo;</a></li>
     <li><a href="#">1</a></li>
     <li><a href="#">2</a></li>
     <li><a href="#">3</a></li>
     <li><a href="#">4</a></li>
     <li><a href="#">5</a></li>
     <li><a href="#">&raquo;</a></li>
    </ul>
    首选项和禁用
    <li class="active"><a href="#">1</a></li>
    <li class="disabled"><a href="#">2</a></li>
    翻页效果
    <ul class="pager">
     <li><a href="#">ӤӞᶭ</a></li>
     <li><a href="#">ӥӞᶭ</a></li>
    </ul>
    对其翻页链接
    <ul class="pager">
     <li class="previous"><a href="#">ӤӞᶭ</a></li>
     <li class="next"><a href="#">ӥӞᶭ</a></li>
    </ul>
    //翻页项禁用
    <li class="previous disabled"><a href="#">ӤӞᶭ</a></li>
    徽章
    未读信息数量徽章
    <a href="#">信息<span class="badge">10</span></a>
    按钮中使用徽章
    <button class="btn btn-success">
     提交<span class="badge">3</span>
    </button>
    激活状态自动适配色调
    <ul class="nav nav-pills">
     <li class="active">
     <a href="#">首页<span class="badge">2</span></a>
     </li>
     <li><a href="#">咨询</a></li>
    </ul>


