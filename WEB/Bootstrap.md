[toc]



该框架已经写好了很多样式，有需要只需要下载对应的文件，直接cv即可

在使用bootstrap的时候，所有页面样式都只需要通过class来调节

## 下载

[官网下载](https://v3.bootcss.com/)**推荐使用v3版本**

![image-20221015173050904](E:\MarkDown\markdown\imgs\image-20221015173050904.png)

**下载好的bootstrap-3.4.1-dist只需要保留下图的文件，其余可以删除**

**fonts文件夹里的东西保持原样不动**

![image-20221015173356383](E:\MarkDown\markdown\imgs\image-20221015173356383.png)



**注意:**

==bootstrap的js代码是依赖于jQUery的，意味着在使用Bootstrap的时候要导入jQuery==

## CSS样式

### 布局容器

[Bootstrap全局css样式](https://v3.bootcss.com/css/)

Bootstrap 需要为页面内容和栅格系统包裹一个 `.container` 容器。我们提供了两个作此用处的类。注意，由于 `padding` 等属性的原因，这两种 容器类不能互相嵌套。

`.container` 类用于固定宽度并支持响应式布局的容器。

```html
<div class="container">
  ...
</div>
```

`.container-fluid` 类用于 100% 宽度，占据全部视口（viewport）的容器。

```html
<div class="container-fluid">
  ...
</div>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!---<script src="../jQuery-3.6.0-min.js"></script>--->
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
    <link rel="stylesheet" href="css/bootstrap.min.css">
    <script src="js/bootstrap.min.js" ></script>
    <style>
        .c1{
            background-color: red;
            height: 100px;

        }
    </style>
</head>
<body>
<div class="container c1"></div>
<div class="container-fluid c1"></div>
</body>
</html>

后续在使用Bootstrap做首页的时候，先写一个div class='container'
，然后再里面书写代码
```

### 栅格系统

栅格系统用于通过一系列的行（row）与列（column）的组合来创建页面布局，你的内容就可以放入这些创建好的布局中。下面就介绍一下 Bootstrap 栅格系统的工作原理：

- “行（row）”必须包含在 `.container` （固定宽度）或 `.container-fluid` （100% 宽度）中，以便为其赋予合适的排列（aligment）和内补（padding）。
- 通过“行（row）”在水平方向创建一组“列（column）”。
- 你的内容应当放置于“列（column）”内，并且，只有“列（column）”可以作为行（row）”的直接子元素。
- 类似 `.row` 和 `.col-xs-4` 这种预定义的类，可以用来快速创建栅格布局。Bootstrap 源码中定义的 mixin 也可以用来创建语义化的布局。
- 通过为“列（column）”设置 `padding` 属性，从而创建列与列之间的间隔（gutter）。通过为 `.row` 元素设置负值 `margin` 从而抵消掉为 `.container` 元素设置的 `padding`，也就间接为“行（row）”所包含的“列（column）”抵消掉了`padding`。
- 负值的 margin就是下面的示例为什么是向外突出的原因。在栅格列中的内容排成一行。
- 栅格系统中的列是通过指定1到12的值来表示其跨越的范围。例如，三个等宽的列可以使用三个 `.col-xs-4` 来创建。
- 如果一“行（row）”中包含了的“列（column）”大于 12，多余的“列（column）”所在的元素将被作为一个整体另起一行排列。
- 栅格类适用于与屏幕宽度大于或等于分界点大小的设备 ， 并且针对小屏幕设备覆盖栅格类。 因此，在元素上应用任何 `.col-md-*` 栅格类适用于与屏幕宽度大于或等于分界点大小的设备 ， 并且针对小屏幕设备覆盖栅格类。 因此，在元素上应用任何 `.col-lg-*` 不存在， 也影响大屏幕设备。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="../bootstrap-3.4.1-dist/css/bootstrap.min.css">
    <link rel="stylesheet" href="../bootstrap-3.4.1-dist/js/bootstrap.min.js">
    <script src="../jQuery-3.6.0-min.js"></script>
    <style>
        .c1{
            height: 100px;
            background-color:tomato;
            border:5px solid black;

        }

    </style>
</head>
<body>
    <div class="container ">
        <div class="row"> <!---一行均分成12份--->
            <div class="col-md-6 c1"></div> <!--获取所需要的份数-->
            <div class="col-md-6 c1"></div> 
            
     
            <div class="col-md-1 c1"></div>
            <div class="col-md-1 c1"></div>
            <div class="col-md-1 c1"></div>
            <div class="col-md-1 c1"></div>
            <div class="col-md-1 c1"></div>
            <div class="col-md-1 c1"></div>
            <div class="col-md-1 c1"></div>
            <div class="col-md-1 c1"></div>
            <div class="col-md-1 c1"></div>
            <div class="col-md-1 c1"></div>
            <div class="col-md-1 c1"></div>
            <div class="col-md-1 c1"></div>
            <br>
            <div class="col-md-3 c1"></div>
            <div class="col-md-9 c1"></div>
            <br>
            <div class="col-md-2 c1"></div>
            <div class="col-md-8 c1"></div>
            <div class="col-md-2 c1"></div>
        </div>
    </div>
</body>
</html>
```

### 栅格参数

```python
  手机		 平板		 桌面显示器     大屏幕
.col-xs-	.col-sm-	.col-md-	.col-lg-
#针对不同的显示器，bootstrap会自动选择对应的参数

#如果想兼容所有显示器，就全部加上即可

#在一行移动位置
<div class=col-md-8 col-md-offset-2></div> 从左往右移两位
```

### 排版

bootstrap将所有原生的html标签文本字体统一设置成了肉眼可接受的样式

* 标题
  * ​	`<small>` 标签或赋予 `.small` 类的元素，可以用来标记副标题

```html
<h1>人生苦短，<small>我学Python</small></h1>
```

*  中心内容

  通过添加 `.lead` 类可以让段落突出显示。

```html
<p class="lead">ldfjalk jlkJlkjlkdasjflkjasklfj</p>
```

* 高亮

```html
You can use the mark tag to <mark>highlight</mark> text.
```

* 插入文本
  * 额外插入的文本使用 `<ins>` 标签。

```html
<s>This line of text is meant to be treated as no longer accurate.</s>
```

*  带下划线的文本
  * 为文本添加下划线，使用 `<u>` 标签。

```html
<u>This line of text will render as underlined</u>
```

* 对齐

```html
<p class="text-left">Left aligned text.</p>
<p class="text-center">Center aligned text.</p>
<p class="text-right">Right aligned text.</p>
<p class="text-justify">Justified text.</p>
<p class="text-nowrap">No wrap text.</p>
```

* 改变大小写

```html
<p class="text-lowercase">Lowercased text.</p>
<p class="text-uppercase">Uppercased text.</p>
<p class="text-capitalize">Capitalized text.</p>
```

...........

#### 基本缩略语

当鼠标悬停在缩写和缩写词上时就会显示完整内容

```html
<abbr title="attribute">attr</abbr>
```

#### 首字母缩略语

* 为缩略语添加 `.initialism` 类，可以让 font-size 变得稍微小些。

```html
<abbr title="HyperText Markup Language" class="initialism">HTML</abbr>
```

#### 地址

* 让联系信息以最接近日常使用的格式呈现。在每行结尾添加 `<br>` 可以保留需要的样式。

```html
<address>
  <strong>Twitter, Inc.</strong><br>
  1355 Market Street, Suite 900<br>
  San Francisco, CA 94103<br>
  <abbr title="Phone">P:</abbr> (123) 456-7890
</address>

<address>
  <strong>Full Name</strong><br>
  <a href="mailto:#">first.last@example.com</a>
</address>
```

#### 引用

```html
<blockquote>
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer posuere erat a ante.</p>
</blockquote>
```

#### ........

[查看官网了解更多](https://v3.bootcss.com/css/#type-addresses)

### 表格

```html
<table class="table table-hover table-striped table-bordered">
<tr class="success">
    <td>1</td>
    <td>plane</td>
    <td>123</td>
 </tr> 
    
    
   <!-- On rows -->   可以为行或单元格设置颜色
<tr class="active">...</tr>
<tr class="success">...</tr>
<tr class="warning">...</tr>
<tr class="danger">...</tr>
<tr class="info">...</tr>
```

### 表单

```html
  <div class="container">
    <div class="col0-md-8 col-md-offset-2">
        <h2 class="text-center">登录页面</h2>
        <form action="">
            <p>username:<input type="text" class="form-control"></p>
            <p>password:<input type="password" class="form-control"></p>
            <p>
                <select name="" id="" class="form-control">
                    <option value="">111</option>
                    <option value="">222</option>
                    <option value="">333</option>
                </select>
            </p>
            <textarea name="" id="" cols="30" rows="10" class="form-control"></textarea>
            <input type="submit" >
        </form>
    </div>
   </div>

<!----针对form表单标签，加样式就用form-control
class="form-control"---->

checkbox和radio一般不会加form-control，直接用原生的即可
<input type="checkbox">11
<input type="radio">22


针对报错信息，可以加has-error(给input父标签加)
<p class="has-error">username:<input type="text" class="form-control"></p>
```

### 按钮

```html
<a href="https://www.baidu.com" class="btn-primary">点我</a>
<button class="btn btn-danger ">按钮</button>
<button class="btn btn-default">按钮</button>
<button class="btn btn-success">按钮</button>
<button class="btn btn-info">按钮</button>
<button class="btn btn-warning">按钮</button>
<button class="btn btn-default btn-lg">大按钮</button>
<button class="btn btn-default btn-sm">小按钮</button>
<button class="btn btn-default btn-xs">超小按钮</button>
```

* 通过给按钮添加 .btn-block 类可以将其拉伸至父元素100%的宽度，而且按钮也变为了块级（block）元素。

```html
<input type="submit" class="btn btn-primary btn-block" >

<button type="button" class="btn btn-primary btn-lg btn-block">（块级元素）Block level button</button>
<button type="button" class="btn btn-default btn-lg btn-block">（块级元素）Block level button</button>
```

* 按钮禁用状态

```html
<button type="button" class="btn btn-lg btn-primary" disabled="disabled">Primary button</button>
<button type="button" class="btn btn-default btn-lg" disabled="disabled">Button</button>
```



### 图片形状

```html
<img src="..." alt="..." class="img-rounded">
<img src="..." alt="..." class="img-circle">
<img src="..." alt="..." class="img-thumbnail">
```

### ...........

## 组件

### 图标

[font Awesome中文网](http://www.fontawesome.com.cn/)

![image-20221015191537470](E:\MarkDown\markdown\imgs\image-20221015191537470.png)

![image-20221015191505149](E:\MarkDown\markdown\imgs\image-20221015191505149.png)

![image-20221015191848615](E:\MarkDown\markdown\imgs\image-20221015191848615.png)

​					选择想要的图标，点击去复制即可

![image-20221015192119199](E:\MarkDown\markdown\imgs\image-20221015192119199.png)

​					复制代码

![image-20221015192344122](E:\MarkDown\markdown\imgs\image-20221015192344122.png)

​					下载完后，在Pycharm中导入



![image-20221015192048070](E:\MarkDown\markdown\imgs\image-20221015192048070.png)

​					点击查看，得到更多的图标，直接复制使用

![image-20221015192452215](E:\MarkDown\markdown\imgs\image-20221015192452215.png)

![image-20221015192521689](E:\MarkDown\markdown\imgs\image-20221015192521689.png)

![image-20221015192539903](E:\MarkDown\markdown\imgs\image-20221015192539903.png)

```html
<h2 class="text-center">登录页面
  <span class="glyphicon glyphicon-user"></span>
</h2>

修改图标的颜色直接修改文本颜色就可以
<style>
    span{
        color:greenyellow
    }
</style>
```

### 导航条

```html
<nav class="navbar navbar-default">白色
<nav class="navbar navbar-inverse">   黑色 
 添加 .navbar-fixed-bottom 类可以让导航条固定在底部
<nav class="navbar navbar-default navbar-fixed-bottom">
```

### 分页器

* 接在不同情况下可以定制。你可以给不能点击的链接添加 `.disabled` 类、给当前页添加 `.active` 类。

```html
<nav aria-label="Page navigation">
  <ul class="pagination">
    <li>
      <a href="#" aria-label="Previous">
        <span aria-hidden="true">&laquo;</span>
      </a>
    </li>
    <li class="avtive"><a href="#">1</a></li>
    <li><a href="#">2</a></li>
    <li><a href="#">3</a></li>
    <li class="disabled"><a href="#">4</a></li>
    <li><a href="#">5</a></li>
    <li>
      <a href="#" aria-label="Next">
        <span aria-hidden="true">&raquo;</span>
      </a>
    </li>
  </ul>
</nav>

```

### 弹框

[SweetAlert for Bootstrap](http://lipis.github.io/bootstrap-sweetalert/)

进入GitHub

![image-20221015194055179](E:\MarkDown\markdown\imgs\image-20221015194055179.png)

点击Download ZIP

![image-20221015194150416](E:\MarkDown\markdown\imgs\image-20221015194150416.png)

Pycharm中引入

![image-20221015194422705](E:\MarkDown\markdown\imgs\image-20221015194422705.png)

![image-20221015194513055](E:\MarkDown\markdown\imgs\image-20221015194513055.png)

```js
swal('你还好吗')
undefined
swal('你还好吗','不好，想你了')
undefined
swal('你还好吗','不好，想你了','success')
undefined
swal('你还好吗','不好，想你了','warning')
undefined
swal('你还好吗','不好，想你了','error')
undefined
```

### 进度条



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="../bootstrap-3.4.1-dist/css/bootstrap.min.css">
    <script src="../bootstrap-3.4.1-dist/js/bootstrap.min.js"></script>
    <script src="../jQuery-3.6.0-min.js"></script>
    <link rel="stylesheet" href="../bootstrap-sweetalert-master/dist/sweetalert.css">
    <script src="../bootstrap-sweetalert-master/dist/sweetalert.min.js"></script>
</head>
<body>
    <div class="progress">
        <div id="d2" class="progress-bar progress-bar-striped active" role="progressbar" aria-valuenow="45" aria-valuemin="0" aria-valuemax="100" style="width: 10%">
          <span class="sr-only">50% Complete</span>
        </div>
      </div>
      <button id="d1" class="btn btn-danger">动起来</button>


      <script>
        function func(i){
            let tempWidth = 'width:' + i + '%' //style
            let contextText = i + '%'           //文本
            $('#d2').attr('style',tempWidth).text(contextText)
        }
        $('#d1').click(function(){
            for (let i=0;i<=100;i++){
                setInterval(func(i),5000)
            }

        })
      </script>
</body>
</html>
```



## JS插件

### [模态框](https://v3.bootcss.com/javascript/#modals)

```html
<div class="modal fade" tabindex="-1" role="dialog" aria-labelledby="gridSystemModalLabel">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
        <h4 class="modal-title" id="gridSystemModalLabel">Modal title</h4>
      </div>
      <div class="modal-body">
        <div class="row">
          <div class="col-md-4">.col-md-4</div>
          <div class="col-md-4 col-md-offset-4">.col-md-4 .col-md-offset-4</div>
        </div>
        <div class="row">
          <div class="col-md-3 col-md-offset-3">.col-md-3 .col-md-offset-3</div>
          <div class="col-md-2 col-md-offset-4">.col-md-2 .col-md-offset-4</div>
        </div>
        <div class="row">
          <div class="col-md-6 col-md-offset-3">.col-md-6 .col-md-offset-3</div>
        </div>
        <div class="row">
          <div class="col-sm-9">
            Level 1: .col-sm-9
            <div class="row">
              <div class="col-xs-8 col-sm-6">
                Level 2: .col-xs-8 .col-sm-6
              </div>
              <div class="col-xs-4 col-sm-6">
                Level 2: .col-xs-4 .col-sm-6
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div><!-- /.modal-content -->
  </div><!-- /.modal-dialog -->
</div><!-- /.modal -->
```

### 示例选项卡

```html
<div>

  <!-- Nav tabs -->
  <ul class="nav nav-tabs" role="tablist">
    <li role="presentation" class="active"><a href="#home" aria-controls="home" role="tab" data-toggle="tab">Home</a></li>
    <li role="presentation"><a href="#profile" aria-controls="profile" role="tab" data-toggle="tab">Profile</a></li>
    <li role="presentation"><a href="#messages" aria-controls="messages" role="tab" data-toggle="tab">Messages</a></li>
    <li role="presentation"><a href="#settings" aria-controls="settings" role="tab" data-toggle="tab">Settings</a></li>
  </ul>

  <!-- Tab panes -->
  <div class="tab-content">
    <div role="tabpanel" class="tab-pane active" id="home">...</div>
    <div role="tabpanel" class="tab-pane" id="profile">...</div>
    <div role="tabpanel" class="tab-pane" id="messages">...</div>
    <div role="tabpanel" class="tab-pane" id="settings">...</div>
  </div>

</div>
```

