---

`提示：CSS不仅可以静态地修饰网页，还可以配合各种脚本语言动态地对网页各元素进行格式化`

@[TOC](文章目录)

---
# CSS

## 01CSS 样式表

cascading stytle sheet 层叠样式表
简单说就是如何修饰网页信息的显示样式

用来展示XHTML或者XML等样式的计算机语言

## 02CSS语法

1. 每个CSS 样式由两部分组成，即选择符和声明符，声明又分为属性和属性值
2. 属性必须放在花括号中，属性与属性值用冒号链接
3. 每条声明用分号结束
4. 当一个属性有多个属性值的时候，属性值与实行值不分先后顺序，用空格隔开
5. 在书写样式过程中，空格、换行等操作

### 01内部样式

```html
 <head>
      <meta charset="UTF-8">
      <title>Lightly-HTML-Project</title>
        <style>
      h1{
      color:red;
    }
    h2{
      color:yellow;
    }
     h3{
       color:blue;
     }
      </style>
  </head>
  <body>
      <h1>11111111</h1>
      <h2>222222222</h2>
      <h3>3333333333333</h3>
  </body>
```

### 02外部样式

#### 2.1 link

> 属于XHTML标签，link引用的ＣＳＳ会同时被加载

```html
<head>
      <meta charset="UTF-8">
      <title>Lightly-HTML-Project</title>
      <link rel="stylesheet" href="../css/02.1.css"
      " type="text/css">   
  </head>
```

#### 2.2 import

> @imoprt 完全是CSS提供的一种方式， @import 引用的CSS会等到页面全部被下载完再被加载

```html

<head>
      <meta charset="UTF-8">
      <title>Lightly-HTML-Project</title>
  
      <style>
        @import url(../css/02.1.css);
      </style>
  </head>
```

### 03行内样式

stytle作为属性直接写在标签后面

```html
<body>
    <h1 style="color: gold;">11111</h1>
     <h2 style="color: blue;">1222222</h2>
      <div style="width:200;height:300px;color:gold;">我是div</div>
  </body>


```

### 04样式表优先级

* !important优先级最高，
* 就近原则
* 优先级针对用一个标签，同一个属性，如果一个标签多了一个属性，那就会保留下来，

```html
  <head>
      <meta charset="UTF-8">
      <title>Lightly-HTML-Project</title>
      <link rel="stylesheet" href="../css/02.1.css">
      <style>
        div{
          color: hotpink;
        }
      </style>
  </head>
  <body>
      <!--!important>行内>内部>外部--->
      <div style="color: green;">人生苦短，我学Python</div>
  </body>
```

**!important放在哪，哪个优先级就最高**

```html
  <head>
      <meta charset="UTF-8">
      <title>Lightly-HTML-Project</title>
      <link rel="stylesheet" href="../css/02.1.css">
      <style>
        div{
          color: hotpink!important;
        }
      </style>
  </head>
  <body>
      <!--!important>行内>内部>外部--->
      <div style="color: green;">人生苦短，我学Python</div>
  </body>
```

## 03CSS选择器

要使用CSS对HTMl页面中的元素实现一对一，一对多或者多对一的控制，就需要用到CSS选择器

### 3.1标签选择器

```html
 <head>
      <meta charset="UTF-8">
      <title>Lightly-HTML-Project</title>
      <style>
        div{
          background-color: aqua;
          color: blue;
        }
      </style>
  </head>
  <body>
      <div>111111111</div>
      <h1>2222222222</h1>
      <p>1222222131321321</p>
      <div>78984464645</div>
  </body>
```

### 3.2类选择器

一个div可以设置多个类 名

看style标签里面的顺序，然后就近原则，显示的颜色是brown

```html
  <head>
      <meta charset="UTF-8">
      <title>Lightly-HTML-Project</title>
      <style>
        .ibm{
          background-color: blueviolet;
        }
        .qianfeng{
          color: brown;
        }
      </style>
  </head>
  <body>
      <div class="ibm qianfeng">zhaosong1111111</div>
      <div>zhaosng222222</div>
      <div>zhaosong 3333333</div>
      <div class ="ibm">zhaosng44444444</div>
      <div class=“"ibm">zhaosng5555555555</div>
      <div>zhaosong5666666</div>
  </body>
```

### 3.3id选择器

一个id名称只能对应文档中的一个具体的元素对象 **（唯一性）**

起名字要起英文，不能用关键字，（所有标记，和属性都是关键字）如：head标记

```html
  <head>
      <meta charset="UTF-8">
      <title>Lightly-HTML-Project</title>
      <style>
        #box{
          background-color: burlywood;
          color: blueviolet;
        }
        #box1{
          background-color: chartreuse;
        }
      </style>
  </head>
  <body>
      <div id="box1">人生苦短，我学Python</div>
      <div id="box">三国演义</div>
  </body>
```

### 3.4通配符选择器

`*`：含义就是所有的元素

```html
<head>
      <meta charset="UTF-8">
      <title>Lightly-HTML-Project</title>
      <style>
        *{
          /* 外边距*/
          margin: 0;
          padding: 0;
          /*内边距 */
        }
      </style>
  </head>
  <body>
      <h1>标题</h1>
      <div>晒斑</div>
      <ul>
        <li>11111</li>
        <li>22222</li>
        <li>323333</li>
      </ul>
  </body>
```

### 3.5伪类选择器

主要应用于**a链接**上
**语法：**

```markdown
a:link{属性:属性值;}超链接的起始状态
a:visited{属性:属性值;}超链接被访问后的状态
a:hover{属性:属性值;}鼠标悬停，即鼠标划过超链接时的状态
a:active{属性:属性值;}超链接被激活时的状态，即鼠标按下时超链接的状态
```

**顺序**：Link ---->Visited ---->Hover-----> Active

**顺序错误会使样式失效**

```html
<head>
      <meta charset="UTF-8">
      <title>Lightly-HTML-Project</title>
      <style>
        /*未访问*/
        a:link{
          color: yellow;
        }
        /*访问之后*/
        a:visited{
          color: red;
        }
        /*鼠标移动到上面*/
        a:hover{
          color:orange;
        }
        /*点击，激活*/
        a:active{
          color: orchid;
        }
      </style>
  </head>
  <body>
      <a href="https://www.baidu.com">百度</a>
  </body>
```

### 3.6 群组选择器

```html
<!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Lightly-HTML-Project</title>
      <style>
/*
        div{
          background-color: greenyellow;
        }
        h1{
          background-color: greenyellow;
        }
        p{
          background-color: greenyellow;
        }*/
   

     /*   群组选择器，提出公共代码，节约代码量   */
        div,p,h1,.boxp{
          background-color: greenyellow;
        }
      </style>
  </head>
  <body>
      <div>11111111111</div>
      <p>111111111111111</p>
      <h1>111111111111</h1>
      <div class="boxp">1212313212</div>
  </body>
</html>

```

### 3.7后代选择器

~~从右往左匹配~~

```html
  <head>
      <meta charset="UTF-8">
      <title>Lightly-HTML-Project</title>
     <style>
       /*  后代选择器， 包含选择器  */
       /*从右到左匹配，先匹配p便签，在匹配包含p 的div标签*/
        div p{
          color:blue;
          background-color: greenyellow;
      } 

      p b{
        color: palevioletred;
      }
     </style>
  </head>
  <body>
      <div>1111111</div>
      <p>
        <b>4546545</b>
      </p>
      <div>
        <p>1231314546545</p>
      </div>

  </body>


```

### 3.8选择器的权重

当多个选择器，选中的是同一个元素，且都为他们定义了样式，如果属性发生冲突，会**选择权重高的来执行**

| 选择器                   | 权重，CSS中用四位数表示权重，权重表达方式如：0,0,0,0，       |
| ------------------------ | ------------------------------------------------------------ |
| 类型(元素)选择器         | 0001                                                         |
| Class选择器(类选择器)    | 0010                                                         |
| id选择器                 | 0100                                                         |
| 包含选择符               | 为包含选择符的权重之和      (div p)                          |
| 内联样式                 | 1000                                                         |
| !important               | 10000                                                        |
| **CSS选择器解析规则1：** | **当不同选择符的样式设置有冲突的时候，高权重选择符的样式会覆盖低权重符的样式** |
| **CSS选择器解析规则2：** | **相同权重的选择符，样式遵循就近原则，哪个选择符最后定义，就选择哪个选择符样式。** |

## 04CSS属性

### 4.0CSS属性和属性值的定义

### 4.1CSS文本属性

|      属性       |     描述     | 说明                                                         |
| :-------------: | :----------: | :----------------------------------------------------------- |
|  **font_size**  |   字体大小   | 单位是px,浏览器默认是16px，设计图常用的是12px                |
|   font_family   |     字体     | 当字体是中文字体、英文字体中有空格时，需要加**双引号**；&#x3c;br />多个字体中间用**逗号**链接，先解析出第一个字体，如果没有解析第二个字体，以此类推 |
|    **color**    |     颜色     | color:red;   color:#ffo;   color:rgb(255,0,0); 0-255         |
|   font-weight   |     加粗     | font-weight:bloder(更粗的)/bold(加粗)/normal(常规)&#x3c;br />font-weight:100-900; 100-500不加粗 600-900加粗 |
|   font-style    |     倾斜     | font-style:ltalic(斜体字)/oblique(倾斜的文字)/normal(正常显示) |
| **text-align**  | 文本水平对齐 | text-align:left;水平靠左&#x3c;br />text-align:right;水平靠右&#x3c;br />text-align:center;水平居中&#x3c;br />text-align:justify;水平两端对齐，但是只对多行起作用 |
|   line-height   |     行高     | line-height的数据=height的数据，可实现单行文本垂直居中       |
|   text-indent   |   首行缩进   | text-indent可以取负值；text-indent属性只对第一行起作用       |
| letter-spacing  |    字间距    | 控制文字和文字之间的间距                                     |
| text-decoration |   文本修饰   | text-decoration:none没有/underline下划线/overline上划线/line-through删除线 |
|      font       |   文件简写   | **font是font-style font-weight font-size /line-height font-family的缩写。&#x3c;br />font:italic 800 30px/80px "宋体";**  **顺序不能变**，必须同时指定font-size和font-family属性时才起作用 |
| text-transform  |  检索大小写  | text-transform: capitalize;首字母大写&#x3c;br />text-transform:lowercase;全部小写&#x3c;br />text-transform:uppercase;全部大写&#x3c;br />text-transform:none; 没效果 |

![image.png](E:\MarkDown\markdown\imgs\1aef4c20c0c67a390046b35ff431ef62.png)

### 4.2CSS列表属性

| 属性                | 描述                     | 说明                                                         |
| ------------------- | ------------------------ | ------------------------------------------------------------ |
| list-style-type     | 定义列表符合样式         | lilst-style-type:disc;(实心圆)/circle(空心圆)/square(实心方块)/none(去掉符号) |
| list-style-image    | 将图片设置为列表符合样式 | list-style-image:url();                                      |
| list-style-position | 设置列表项标记的放置位置 | list-style-position:outside;列表的外面默认值&#x3c;br />list-style-position:inside;列表的里面 |
| **list-style**      | 简写                     | lsit-style:none url() inside ;                               |

### 4.3CSS背景属性

|         属性          | 描述           | 说明                                                         |
| :-------------------: | -------------- | ------------------------------------------------------------ |
|   background-color    | 背景颜色       | background-color:red;&#x3c;br />background-color:rgb(0,0,255);&#x3c;br />background-color:#fff; |
|   background-image    | 背景图片       | background-image:url();                                      |
|   background-repeat   | 背景图片的平铺 | background-repeat:no-repeat（不平铺）/repeat(平铺)/repeat-x(x轴平铺)/repeat-y(y轴平铺); |
|  background-position  | 背景图片的定位 | background-posotion:水平位置，垂直位置；可以给负值&#x3c;br />**水平位置：left center right&#x3c;br />垂直位置：top center bottom** |
| background-attachment | 背景图片的固定 | background-attachment:scroll(滚动)/fixed(固定，固定在浏览器窗口里面，固定之后就相对于浏览器窗口了) |
|    background-size    | 设置图片大小   | background-size：400px 400px&#x3c;br />可以设置跟墙一般大&#x3c;br />background-size：cover;将背景图片扩展至足够大，以使背景图像完全覆盖背景区域，可能有一部分会被裁掉&#x3c;br />（cover 覆盖 contain包含）&#x3c;br />background-size:contain;把图片图像扩展到最大尺寸，以使其宽度和高度完全适应内容区，铺不满盒子，留白 |
|    **background**     | 复合属性       | background:yellow url() center fixed no-repeat&#x3c;br />**顺序随便**&#x3c;br />**background-size只能单独用，不能复合** |

### 4.4CSS边框属性

```mark
一 边框属性
1.上边框：

border-top-style:样式
border-top-width:宽度
border-top-color:颜色
border-top:宽度，样式，颜色

2.下边框:

border-bottom-style:样式
border-bottom-width:宽度
border-bottom-color:颜色
border-bottom:宽度，样式，颜色

3.左边框:

border-left-style：样式
border-left-width:宽度
border-left-color:颜色
border-left:宽度，样式，颜色

4.右边框:

border-right-style:样式
border-right-width:宽度
border-right-color:颜色
Border-right：样式，宽度，颜色

二:(1)设置边框样式(border-style)常用属性值如下
·none:没有边框,即忽略所有边框的宽度(默认值)
·solid:边框为单实线
·dashed:边框为虚线
·dotted:边框为点线
·double:边框为双实线
·border-top-style:上边框样式
·border-right-style:右边框样式
·border-bottom-style:下边框样式
·border-left-style:左边框样式
·border-style:上边框样式 右边框样式 下边框样式 左边框
·border-style:上边框样式 左右边样式 下边框样式
·border-style:上下边框样式 左右边框样式
·border-style:上下边框样式

(2)设置边框宽度(border-width)

·border-top-width:上边框宽度
·border-right-width:右边框宽度
·border-bottom-width：下边框宽度
·border-left-width:左边宽宽度
·border-width:上边框宽度[右边框宽度,下边框宽度,左边宽宽度]

(3)设置边框颜色(border-color)

·border-top-color:上边框颜色
·border-right-color:右边框颜色
·border-bottom-color:下边框颜色
·border-left-color:左边宽颜色
·border-color:上边框颜色[右边框颜色 下边框颜色 左边框]

(4)综合设置边框

border:宽度 样式 颜色；
P{border-top:2px solid #ccc;}//定义上边框，各个值顺序任意
例如将二级标题的边框设置为双实线,红色,3像素宽,代码如下： h2{border:3px double red;}
常用的复合属性有font-字体,border-边框,margin-外边距和background-背景等。

```

### 4.5CSS浮动属性

* **浮动**

| 属性  | 描述         | 说明           |
| ----- | ------------ | -------------- |
| float | float:left;  | 元素靠左边浮动 |
| float | float:right; | 元素靠右边浮动 |
| float | float:none;  | 元素不浮动     |

* **浮动的作用**：

1. 定义网页中其他文本如何环绕该元素显示
2. **就是让竖着的东西横起来**

* **清除浮动**：

| 属性  | 描述         | 说明                 |
| ----- | ------------ | -------------------- |
| clear | clear:none;  | 允许有浮动对象       |
| clear | clear:right; | 不允许右边有浮动对象 |
| clear | clear:left;  | 不允许左边有浮动对象 |
| clear | clear:both;  | 不允许有浮动对象     |

**清除浮动**只是改变元素 排序方式，该元素还是漂浮着 的，不占据文档位置。

## 05盒子模型

盒模型是css布局的基石，它规定了网页元素如何显示以及元素之间相关关系，css定义所有的元素都可以拥有像盒子一样的外形和平面空间，即都包含边框、边界、补白、内容区，这就是盒模型

### 5.1内边距padding

```
/*  padding:30px;*/ /*内边距*/
     /* padding:30px 30px;*/
     padding-right: 10px;


      /*
      padding:
      1个值，四个方向都一样
      2个值，上下 ，左右
      3个值，上  左右 下
      4个值，上 右 下 左*/


      /*padding-方向:top bottom left right*/
```

### 5.2边框border

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Lightly-HTML-Project</title>
  <style>
    .box1 {
      width: 100px;
      height: 100px;
      background: yellow;
      border: 10px solid red
    }

    /*border 粗细 样式 颜色*/
    /*样式：solid double dashed dotted*/

    /*  border-top
        border-bottom
        border-left
        border-right
    */
    .box2{
      width: 100px;
      height: 100px;
      background: green;
      border-top: 5px solid blue;
      border-right: 10px dashed yellow;
  
    }
    .box3{
      width: 100px;
      height: 100px;
      background: pink;
      border-width:10px 20px 30px 40px ;
      border-color: yellow tomato black blue;
      border-style: solid dashed double dotted;

        /*
      border-width
      border-style
      border-color
      */
    }
  </style>
</head>

<body>
  <div class="box1"></div>
  <div class="box2"></div>
  <div class="box3"></div>
</body>

</html>
```

### 5.3外间距margin

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Lightly-HTML-Project</title>
  <style>
    div {
      width: 200px;
      height: 200px;
    }

    .box1 {
      background: red;
      border: 1px solid yellow;
      margin: 200px;
      /*margin:50px 40px 20px 40px*/
      /*
          margin-top:10px;
          margin-bottom:20;
          margin-left:30px;
          margin-right:40px;
          */

      float: left;
    }

    .box2 {
      background: green;
      border: 1px solid blue;
      margin-left: 100px;
      margin-bottom: 60px;
    }

    .box3 {
      background-color: black;
      margin: 0 auto;/*居中 上下是零，左右自动*/
    }
  </style>
</head>

<body>
  <div class="box1"></div>
  <div class="box2"></div>
  <div class="box3"></div>
</body>

</html>
```

### 5.3外间距特性问题

```markdown
特性问题：
1. 兄弟关系，两个盒子垂直外边距与水平外边距问题？
 - 垂直方向，外边距取最大值
 - 水平方向，外边距会进行合并处理？
2. 父子关系，给子加外边距，但作用在父身上，怎么解决：

- 从子的margin-top===》父的padding-top，注意高度计算
- 给父盒子设置边框
- 加浮动
- overflow:hidden 

```

* **兄弟关系**

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Lightly-HTML-Project</title>
  <style>
    /*
    特性问题：
    1.兄弟关系，两个盒子垂直外边距与水平外边距问题？
     - 垂直方向，外边距取最大值
     - 水平方向，外边距会进行合并处理？
     2.父子关系，给子加外边距，但作用在父身上，怎么解决：
    */

   
     .box1,.box2,.box3,.box4{
      width: 200px;
      height: 200px;
    }

    .box1 {
      background: red;
      margin-bottom: 100px;
    }
    .box2 {
      background: yellow;  
      margin-top: 50px;
    }
     .box3 {
        float:left;
      background: black;
      margin-right: 50px;
    }
     .box4 {
        float:left;
      background: blue;
      margin-left: 150px;
    }
  
  </style>
</head>

<body>
  <div class="box1"></div>
  <div class="box2"></div>
  <div class="box3"></div>
  <div class="box4"></div>

</body>

</html>
```

* **父子关系**

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Lightly-HTML-Project</title>
  <style>
    /*
    1.从子的margin-top===》父的padding-top，注意高度计算
    2.给父盒子设置边框
    3.加浮动
    4.overflow:hidden 
    */
    .contain {
      width: 500px;
      height: 500px;
      background-color: yellow;
      /*  padding-top:100px;*/
      border: 1px solid transparent;
  
    }

    .box {
      width: 200px;
      height: 200px;
      background-color: tomato;
      margin-top: 100px;
    }
  </style>
</head>

<body>
  <div class="contain">
    <div class="box"></div>
  </div>
</body>

</html>
```

## 06溢出

### 6.1溢出属性

| 属性       | 属性值                                          |
| ---------- | ----------------------------------------------- |
| overflow   | visible:默认值，溢出内容会显示在元素外面        |
| overflow   | hidden:溢出隐藏内容                             |
| overflow   | scroll:滚动，溢出内容以滚动方式显示             |
| overflow   | auto:如果有溢出会添加滚动条，没有溢出则正常显示 |
| overflow   | inherit:规定应该遵从父元素继承overflow属性的值  |
| overflow-x | x轴溢出                                         |
| overflow-y | y轴溢出                                         |

```html
<head>
    <style>
        div {
            width: 200px;
            height: 200px;
            background: yellow;
            /* overflow: visible;  显示溢出*/
            /* overflow: hidden; 溢出隐藏，文本裁剪 */
            /* overflow: scroll;不管有屋溢出，都会有滚动条 */

            /* overflow: auto; 文本内容少就正常显示，内容多就出现滚动条 */

            /* overflow: inherit;继承父元素的效果 */
        }
        .box{
            /* overflow: auto; */
            overflow-x:auto ;
            overflow-y: hidden;
        }
    </style>
</head>

<body>
    <div>
        Lorem, ipsum dolor sit amet consectetur adipisicing elit. Quae necessitatibus maxime error, tempore voluptatum
        dignissimos, unde iste nam aliquid alias nostrum sapiente, ipsa dolor exercitationem enim totam! Inventore,
        accusamus optio.
    </div>
    <div class="box">
        <img src="IMG/beautiful.jpeg" alt="">
    </div>
</body>


```

### 6.2空余空间

该属性用来设置如何处理元素内的空白

| 属性        | 属性值                                              |
| ----------- | --------------------------------------------------- |
| white-space | normal：默认值，空白会被浏览器忽略                  |
|             | nowrap:文本会在同一行上继续，知道遇到&#x3c;br/>标签 |

```html
/* nowrap ：不换行
        pre ：显示空格， 回车，tab 不换行
        pre-wrap:显示空格， 回车，tab 换行
        pre-line:显示回车，不显示换行
    */
```

### 6.3省略号显示

```html
text-overflow:clip/ellipsis;
clip:默认值，不显示省略号
ellipsis:显示省略号

```

**当单行文本溢出显示省略号需要同时设置以下声明：**

1. **容器宽度：width:200px;**
2. **强制文本在一行显示：white-space:nowrap;**
3. **溢出内容为隐藏：overflow:hidden;**
4. **溢出文本显示省略号：text-overflow:ellipsis;**

## 07元素显示类型

![image.png](E:\MarkDown\markdown\imgs\2d3c016403b2bff880b643a02d5c539d.png)

**块元素**

```
display:block;
display:list-item;
p标签里放文本可以，不能放块级元素
```

**行内元素**

```
display:inline;
```

**行内块元素**

```
display:inline-block;
```

**元素类型互相转换**

```

```

## 08定位

![image.png](E:\MarkDown\markdown\imgs\90a9f413920b8560b8ddd6af6fe0bcaf.png)

### 8.1静态定位

```html
 <style>
        div{
            width: 200px;
            height: 200px;
        }
        .box1{
            background-color: red;
        }
        .box2{
            background-color: yellow;
            position: relative;
            top: 100px;
            left: 100px;
        }
        .box3{
            background-color: blue;
        }
    </style>
</head>
<body>
    <div class="box1"></div>
    <div class="box2"></div>
    <div class="box3"></div>
</body>

```

### 8.2绝对定位，没有父元素

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .box {
            width: 200px;
            height: 200px;
            background-color: yellow;
            position: absolute;
            top: 100px;
            left: 100px;
        }
    </style>
</head>

<body>
  
    <div class="box"></div>
    <div>Lorem ipsum, dolor sit amet consectetur adipisicing elit. At aperiam repellat dolore totam mollitia, ullam
        exercitationem repudiandae id atque vitae in qui a rLorem ipsum, dolor sit amet consectetur adipisicing elit. At aperiam repellat dolore totam mollitia, ullam
        exercitationem repudiandae id atque vitae in qui a repellendus quLorem ipsum, dolor sit amet consectetur adipisicing elit. At aperiam repellat dolore totam mollitia, ullam
        exercitationem repudiandae id atque vitae in qui a repellendus quLorem ipsum, dolor sit amet consectetur adipisicing elit. At aperiam repellat dolore totam mollitia, ullam
        exercitationem repudiandae id atque vitae in qui a repellendus quepellendus quod veritatis rem, expedita veniam officiis.
    </div>
</body>

</html>
```

### 8.3绝对定位，有父元素

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .box {
            width: 500px;
            height: 500px;
            background-color: yellow;
            margin  :0 auto ;
            position: relative;
        }

        .child{
            width: 200px;
            height: 200px;
            background-color: red;
            left:100px;
            top:100px;
            position: absolute;
        }
    </style>
</head>

<body>
  
    <div class="box">
        <div class="child"></div>
    </div>
  
</body>

</html>
```

### 8.4固定定位

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            width: 100%;
            height: 1500px;
            background-color: yellow;
        }
        .ad{
        width: 100px;
        height: 200px;
        background-color: red;
        position: fixed;
        right: 0;
        bottom:200px;
        }
    </style>
</head>
<body>
    <div class="box"></div>
    <div class="ad"></div>
</body>
</html>
```

### 8.5粘性定位

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        *{
            margin:0;
            padding:0;
        }
        .header{
            background-color: yellow;
            height: 100px;
            width: 1000px;
        }
        .nav{
           background-color: red;
           width: 500px;
           height: 50px;
           margin: 0 auto; 
           position: sticky;
           top:0px;
        }
        .body{
            height: 1000px;
            background-color: green;

        }
    </style>
</head>
<body>
    <div class="header"></div>
    <div class="nav"></div>
    <div class="body"></div>
</body>
</html>
```

### 8.6定位的层级

* z-index:值越大，层级就越大，越靠上显示 可以设置负值

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            width: 200px;
            height: 200px;
        }
        .box1{
            background-color: yellow;
            position: relative;
            left: 100px;
            top:100px;
            z-index: 1;/*层级*/
        }
        .box2{
            background-color: red;
            position: relative;
            z-index: 100;
        }
        /* z-index值越大，层级就越大，越靠上显示 可以设置负值 */
    </style>
</head>
<body>
    <div class="box1"></div>
    <div class="box2"></div>
 
</body>
</html>
```

## 09锚点

## 10精灵图

## 11宽高自适应


## CSS3

![image.png](E:\MarkDown\markdown\imgs\350d985b39cc0c834331f0cd687847f4.png)

**层级选择器**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box>li{
            border: 1px solid  red;
        }
        /*当前元素后边的第一个兄弟*/
        /* .child+li{
            background-color: yellow;
        } 
        */
        /*当前元素后面所有的亲兄弟*/
        .child~li{
            background-color: yellow;
        } 
       .containner+p{
        background-color: blue;
       }
  
 
    </style>
</head>
<body>
    <ul class="box">
        <li>111
            <ul>
                <li>1111-111</li>
                <li>22222-222222</li>
                <li>333-3333</li>
            </ul>
        </li>
        <li class="child">222</li>
        <li>333</li>
        <li>444</li>
        <li>555</li>
    </ul>
    <div class="containner">1111</div>
    <p>p-11111111</p>
    <p>p-22222222</p>
   <div> 
    <p>div-33333</p>
</div>
</body>
</html>
```

**属性选择器**

![image.png](E:\MarkDown\markdown\imgs\b6c041ebcaf2bc16ad4b4ad74f8d6a00.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        /* [class]{
            background-color: yellow;
        } */
        /* div下所有的class */
        div[class]{
            background-color: blue;
        }
        p[class]{
            background-color: rebeccapurple;
        }
        /*完全匹配*/
        /* div[class=box1]{
            border: 1px solid pink;
        } */
        /*包含就匹配*/
        div[class~=box1]{
            border: 1px solid pink;
        }
        input[name]{
            background-color: yellow;
        }
        input[type=email]{
            background-color: aqua;
        }
        /* 模糊匹配 */
        /* class^=b 以b开头
        class$=b 以b结尾的
        class*=b 包含某个字符 */
        div[class^='b'],p[class*='1']{
            color:white
        }
    </style>
</head>
<body>
    <div class="box1">div11111</div>
    <div class="box2">div2222222222222</div>
    <div >333333</div>
    <div class="box1 box3"> div555555  </div>
    <p class="p1">p-1111</p>
    <p class="p2">p-222222</p>
    <p>p-33363333</p>
    <div>
        用户名：<input type="text" name="username"><br>
        密码:<input type="password" name="password"><br>
        年龄：<input type="number" name="age"><br>
        邮箱：<input type="email">
    </div>
</body>
</html>
```

**结构伪类选择器**

![image.png](E:\MarkDown\markdown\imgs\2d9d5244ac09268aed973a4aff2cc488.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box{
            width: 940px;
            height: 100px;
            margin:0 auto;
            background: yellow;
        }
        .box div{
            float: left;
            width: 300px;
            height: 100px;
            background-color: red;
            margin-right: 20px;
        }

        .box div:last-child{
            margin-right: 0;
        }
        /* li:first-child{
            background-color: yellow;
        }
        li:last-child{
            background-color: brown;
        }
        /*第几个*/
        /* li:nth-child(2){
            background-color: blueviolet;
        } */ 
        /*偶数 nth-child(2n),nth-child(even)*/
        li:nth-child(2n){
            background-color: red;
        }
        /*奇数 nth-child(2n-1),nth-child(odd)*/
        li:nth-child(odd){
            background-color: burlywood;
        }
        .icon1{
            border: 1px solid black;
        }
        .icon1 p{
            background-color: antiquewhite;
        }
        /*独生子女*/
        .icon1 p:only-child{
            color: blueviolet;
        }
        /*没有孩子的，*/
        .empty1:empty{
            width: 200px;
            height: 200px;
            background-color: black;
        }
        /*根*/
        :root,body{
            background-color: black;
        }
    </style>
</head>
<body>
    <div class="box">
        <div></div>
        <div></div>
        <div class="last"></div>
    </div>

     <ul>
        <li>1111</li>
        <li>2222</li>
        <li>3333</li>
        <li>4444</li>
        <li>5555</li>
     </ul>

     <div class="icon1">
        <p>1111</p>
        <p>2222</p>
     </div>
     <div class="icon1">
        <p>1111</p>   
     </div>
     <div class="empty1"></div>
     <div class="root1"></div>
</body>
</html>

```

**目标伪类选择器**

![image.png](E:\MarkDown\markdown\imgs\8deb22f500881b10a62b255d94a47785.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        *{
            margin:0;
            padding: 0;
        }
        ul{
            list-style: none;
            position: fixed;
            right: 0px;
            top:100px
        }
        li{
            width: 100px;
            height: 50px;
            line-height: 50px;
            border: 1px solid black;
        }
        div{
            height: 600px;
            border: 1px solid #ccc;
        }
        div:target{
            background-color: yellow;
        }
    </style>
</head>
<body>
     <ul>
        <li>
            <a href="#a">京东秒杀</a>
        </li>
        <li>
            <a href="#b">双11</a>
        </li>
        <li>
            <a href="#c">频道优选</a>
        </li>
        <li>
            <a href="#d">特色广场</a>
        </li>
     </ul>

     <div  id="a">京东秒杀</div>
     <div  id="b">双11</div>
     <div  id="c">频道优选</div>
     <div  id="d">特色广场</div>
</body>
</html>
```

**UI元素状态伪类选择器**

![image.png](E:\MarkDown\markdown\imgs\72ac0c09cd0b48969b84249688f662dd.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        input:enabled{
            /* background-color: rebeccapurple; */
        }
        input:disabled{
            background-color: yellow;
        }
        /*焦点会匹配此选择器*/
        input:focus{
            background-color: blue;
        }
        input[type=checkbox]{
            /*去掉默认样式*/
            appearance:none;
            width: 20px;
            height: 20px;
            border: 1px solid black;
        }
        input:checked{
            background-color: red;
        }
        div::selection{
            background-color: yellow;
            color: red;
        }
    </style>
</head>
<body>
    <form action="">
        用户名<input type="text" name="username" ><br>
        密码<input type="password" name="password" ><br>
        记住用户名<input type="checkbox"><br>
        <input type="submit" disabled>

    </form>
    <div>Lorem ipsum dolor, sit amet consectetur adipisicing elit. Deserunt dolorem reprehenderit beatae magni, quas numquam accusantium optio explicabo corrupti aspernatur, fugiat, id repudiandae. Corrupti maiores recusandae eos ex placeat molestiae.</div>
</body>
</html>
```

**否定伪类选择器**

![image.png](E:\MarkDown\markdown\imgs\e86018c2db37b20026e8f43608e64915.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        /* li:not(:first-child){
            background-color: yellow;
        } */
        li:not(:nth-child(2n+1)){
            background-color: blue;
        }
        div:not(:empty){
            width: 200px;
            height: 200px;
            background-color: greenyellow;
        }
    </style>
</head>
<body>
    <ul>
        <li>1111</li>
        <li>2222</li>
        <li>3333</li>
    </ul>
    <div>
        <ul><li>121</li></ul>
    </div>
    <div></div>
</body>
</html>
```

# WebApp布局

# 渐变动画变形

# Grid网格布局