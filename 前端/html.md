[TOC]



---
# HTML

## 01.网页

**1.1什么是网页**

**网站**是指在因特网中根据一定的规则，使用HTML等制作的用于展示特定的内容相关的网页的**集合**

**网页**是网站中的一页，通常是**HTML格式的文件**，他要通过浏览器来阅读

**网页是构成网站的基本元素**，它通常由图片，链接，文字，声音，视频等**元素**组成，通常我们看到的网页都是以 `.html`的后缀结尾的文件，因此将其称为**HTML文件**

网页生成制作：由前端人员写HTML文件，然后浏览器打开，就能看到网页

**1.2什么是HTML**

HTML是超文本标记语言（Hyper Text Markup Language），一种用来描述网页的语言

HTML不是编程语言，而是一种标记语言（markup language）

标记语言是一套标记标签（markup tag）

**所谓超文本指的是**：

1. 可以加入图片，声音，动画，多媒体等内容（**超越了文本的限制）**
2. 可以从一个文件跳转到另一个文件，与世界各地主机的文件连接（**超链接文本）**

## 02.WEB标准

Web是由W3C组织和其他标准化组织指定的一系列标准集合，

W3C（万维网联盟）是国际最著名的标准化组织

**2.1为什么使用web标准**

浏览器不同，显示的页面或者排版就有差异

**2.2web标准构成**、

主要由结构（Structure），表现（Presentation） 行为（Behavior）三个方面构成

| 标准 | 说明                                                 |
| ---- | ---------------------------------------------------- |
| 结构 | 结构用于对**网页元素**进行整理和分类                 |
| 表现 | 表现用于设置网页元素的版式，颜色，大小等**外观样式** |
| 行为 | 行为是指网页模型的定义以及交互的编写                 |

Web标准提出最佳体验方案：**结构，样式，行为相分离**

简单理解：**结构写到HTML文件，表现写道CSS文件，行为写道JavaScript文件中**


## 03.快捷键

> ctrl +shift +上下键   快速复制
> **ctrl + / 注释**
>
> 输入感叹号! 回车自动生成 或者 html5:也可生成

## 04.vscode插件：

> htmltagwrap 标签包裹
>
> view-in-browser 预览页面
>
> live server
>
> Auto Rename Tag

## 05.基本语法

```html
1. <常规标记>双标记
<标记></标记>
<标记 属性="属性值" 属性="属性值"></标记>

标记也叫标签或者元素
例如：<head></head>


2.空标记也叫单标记
<标记/>
<标记 属性="属性值" />

例如： <br />
```

## 06.常用标签

#### 1.文本标签

```html
<h1>一级标题</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<h4>四级标题</h4>
<h5>五级标题</h5>
<h6>六级标题</h6>
注：文本标题标签自带加粗，有自己的文本大小，并且独占一行，有默认间距
```

#### 2.段落文本(P)

```html
<p>段落文本内容</p>
标识一个段落（段落与段落之间有段间距）
```

#### 3.换行(br)

```html
<br/>
换行是一个空标记（强制换行）  
```

#### 4.水平线

```html
<hr/>空标记
```

#### 5.加粗有两个标记（推荐srtong）

```html
<b>加粗内容</b>
<strong>强调的内容</strong>突出的文本
```

#### 6.倾斜有两个标记（推荐em ）

```html
<em>强调文本</em>
<i>倾斜文本</i>
```

#### 7.删除线有两个标记（推荐del）

```html
<s>文本</s>   删除线
<del>文本</del>  删除线
```

#### 9.下划线有两个标记（推荐ins）

```
<ins></ins>
<u></u>
```

#### 8.扩展

```html

<sub></sub>  下标
<sup></sup>  上标
```

```

```

## 07.不一般的hr

```html
<hr  color ="red" width="300" align="left">
color  颜色
width   宽度
align   对齐方式  （left right）
noshade 取消阴影

```

---

## 08.特殊符号

| 特殊符号 |                             解释                             |
| :------: | :----------------------------------------------------------: |
|  尖角号  |                &lt ; 左尖角号   &gt ;右尖角号                |
|   空格   | &nbsp ; 该空格占据宽度受【字体】影响，明显而强烈     &emsp ;:占据的宽度正好是一个中文宽度，基本不受字体影响 |
|   版权   |                         &copy ; 圈c                          |
|   商标   |                    &trade ;  TM    ® 圈R                     |
|    😃     | &#128515,表情 最后一位的数字5，可以是任意的数字，表情也就不同 |

| **特殊字符** | **描述**           | **字符代码** |
| ------------ | ------------------ | ------------ |
|              | **空格符**         | `&nbsp;`     |
| **&#x3c;**   | **小于号**         | `&lt;`       |
| **>**        | **大于号**         | `&gt;`       |
| **&**        | **和号**           | `&amp;`      |
| **￥**       | **人民币**         | `&yen;`      |
| **©**        | **版权**           | `&copy;`     |
| **®**        | **注册商标**       | `&reg;`      |
| **°**        | **摄氏度**         | `&deg;`      |
| **×**        | **乘号**           | `&times;`    |
| **÷**        | **除号**           | `&divide;`   |
| **²**        | **平方2（上标2）** | `&sup2;`     |
| **³**        | **立方3（上标3）** | `&sup3;`     |
| **±**        | **正负号**         | `&plusmn;`   |

---

## 09.div 和span标签

div和span是没有语义的，它们只是一个盒子，用来装内容的

div 是division的缩写，表示分割，分区， span意为跨度，跨距

#### 1. div标签

> div*3可以快速生成3个div标签
> div{123131} 按回车，
> 没有具体意义，用来划分页面的区域，独占一行

特点：

* 用来布局，一行只能有一个div，单独一行。**大盒子**
* span用来布局，一行可以放多个span **小盒子**

#### 2.span标签

没有实际意义，主要用于文本独立修饰，内容有多宽就占用多宽的空间距离

## 10.列表

#### 无序列表

```html
<ul>
    <!--1.ul里面只能放li 
        2.默认是黑色实心圆
        3.type: disc sircle square none (none用的多)  
        -->
    <li>无须列表</li>
    <li>无序列表</li>
</ul>
```

#### 有序列表

```html
 <!-----ol 里边只能嵌套li标签 
             li里面可以随意嵌套标签
             type="a/1/i/A/I"  start="3" start取值只能是数字-->
<ol>
    <li>有须列表</li>
    <li>有序列表</li>
</ol>
```

#### 自定义列表

```html
<dl>
    <dd>相关文字</dd>
    <dt>可以使文字也可以是图</dt>
</dl>
```

## 11.图片标签

#### 1.路径

绝对路径：指文件在硬盘上真正存在的路径

> d:/catalog/Pytohn/crawl

相对路径：相对于自己的目标文件位置

> &#x3c;img src='1.gif'>

```html
  <!-- 同级目录
      1.code.gif
      2. ./code.gif
    不同级目录
    1. ../  上一级
    2.../../  上一级的上一级
      --->
      <img src="./IMG/3.jpeg" alt="">
```

#### 2.属性

```html
<img src="" title="鼠标悬停上去之后的提升信息" alt="图片不显示之后（加载失败）的提示信息" width="200px" height="200px" border=/>

```

| 属性   | 属性值   | 说明                                 |
| ------ | -------- | ------------------------------------ |
| src    | 图片路径 | 必须属性                             |
| alt    | 文本     | 替换文本，图像不能显示的文字         |
| title  | 文本     | 提示文本，鼠标放到图像上，显示的文字 |
| width  | 像素     | 设置图像的宽度                       |
| height | 像素     | 设置图像的高度                       |
| border | 像素     | 设置图像的边框粗细                   |

#### 3.注意

图像标签属性注意点：

1. 图像标签可以拥有多个属性，必须写在标签名字的后面
2. 属性之间不分先后顺序，标签名与属性，属性与属性之间均以空格分开
3. 属性采取键值对的格式，即key ="value"的格式，属性="属性值"

## 12.超链接标签

> 实现不同页面的跳转

| 属性   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| href   | 用于指定链接的目标地址（必须属性）                           |
| target | 用于指定链接页面的打开方式，其中_self为默认值，_blank为在新窗口打开方式 |

```html
<!----
target属性 规定在何处打开文档
target="_self"默认值，当前页面打开
target=“_blank"新窗口打开

a 标签可以嵌套img标签，达到点击图片就能进行页面跳转
------->
<a href="路径" title="鼠标悬停上去之后的提升信息" target="规定在何处打开文档">内容</a>

 <a href="https://www.baidu.com/kerwin" title="百度这一块">百度</a>
 <a href="https://www.csdn.net/" target="_blank" title="专业开发者社区">CSDN</a>

<a href="06-图片标签.html">图片</a>
   
<a href="07-费曼学习法案例.html" title="费德曼学习法">
         <img src="./IMG/费德曼.jpg" alt="">
</a>
```

#### 1.链接分类

* 外部链接：&#x3c;a href="www.baidu.com">百度&#x3c;/a>
* 内部链接：打开自己电脑上自己写的html文件
* 空连接：如果没有确定链接目标时：&#x3c;a href="#">首页&#x3c;/a>
* 下载链接:如果href里面是一个文件或者是压缩包，会下载这个文件
* 网页元素链接：在网页中的各种网页元素，如文本、图像、表格、音频、视频等都可以添加超链接
* 锚点链接：点我们点击的链接，可以快速定位到页面中的某个位置
  * 在链接文本的Href属性中，设置属性值为#名字的形式：如&#x3c;a href="#live">个人生活&#x3c;/a>
  * 找到目标位置标签，里面添加一个id属性，属性值为刚才定义的名字：如：&#x3c;h3 id="live">&#x3c;/h3>

```html
<!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>周杰伦</title>
  </head>
  <body>
    <div id="fanhui"></div>
      <h2>目录</h2><br>
      <hr>
    1.早年经历<br>
    2.<a href="#zhuanji">JayChou</a>专辑发布<br>     <!--锚点链接--->
    3.个人生活<br>
<hr>
    <h4>早年经历</h4>
    周杰伦（Jay Chou），1979年1月18日出生于台湾省新北市，祖籍福建省泉州市永春县，中国台湾流行乐男歌手、音乐人、演员、导演、编剧，毕业于淡江中学。
    <h4 id="zhuanji">专辑发布</h4>
    <p>2000年发行首张个人专辑《Jay》。2001年发行的专辑《范特西》奠定其融合中西方音乐的风格。2002年举行“The One”世界巡回演唱会 [1]  。2003年成为美国《时代周刊》封面人物 [2]  。2004年获得世界音乐大奖中国区最畅销艺人奖 [267]  。2005年凭借动作片《头文字D》获得金马奖、金像奖最佳新人奖 [3]  。2006年起连续三年获得世界音乐大奖中国区最畅销艺人奖 [4]  。2007年自编自导的文艺片《不能说的秘密》获得金马奖年度台湾杰出电影奖 [5]  。
    </p>
    <p>
2008年凭借歌曲《青花瓷》获得第19届金曲奖最佳作曲人奖。2009年入选美国CNN评出的“25位亚洲最具影响力人物” [6]  ，同年凭借专辑《魔杰座》获得第20届金曲奖最佳国语男歌手奖 [7]  。2010年入选美国《Fast Company》评出的“全球百大创意人物”。2011年凭借专辑《跨时代》再度获得金曲奖最佳国语男歌手奖，并且第四次获得金曲奖最佳国语专辑奖；同年主演好莱坞电影《青蜂侠》。2012年登福布斯中国名人榜榜首 [8]  。2014年发行华语乐坛首张数字音乐专辑《哎呦，不错哦》。2021年在央视春晚演唱歌曲《Mojito》 [94]  。</p>
    <h4>个人生活</h4>
    演艺事业外，他还涉足商业、设计等领域。2007年成立杰威尔有限公司 [10]  。2011年担任华硕笔电设计师，并入股香港文化传信集团 [11]  。
周杰伦热心公益慈善，多次向中国内地灾区捐款捐物。2008年捐款援建希望小学 [12]  。2014年担任中国禁毒宣传形象大使。
<a href="#fanhui">返回顶部</a>  <!--锚点链接--->
   
  </body>
</html>

```

## 13.table表格

```html
<table>
    <tr>
        <td></td>
         <td></td>
    </tr>
    <tr>
        <td></td>
         <td></td>
    </tr>
</table>

```

#### table属性

|    属性     |            说明            |
| :---------: | :------------------------: |
|    width    |            宽度            |
|   height    |            高度            |
|   border    |            边框            |
| bordercolor |          边框颜色          |
|   bgcolor   |          背景颜色          |
| cellspacing |  单元格与单元格之间的间距  |
| cellpadding |     单元格与内容的空隙     |
|    align    | 对齐方式 left right center |

#### tr属性

> valign属性会与table 的cellpadding有冲突

|  属性   |                说明                |
| :-----: | :--------------------------------: |
| height  |                高度                |
| bgcolor |              背景颜色              |
|  align  | 文字水平对齐 left 或right或 center |
| valign  | 文字垂直对齐 top 或middle或 bottom |

#### td属性

|  属性   |                说明                |
| :-----: | :--------------------------------: |
| weight  |                宽度                |
| height  |                高度                |
| bgcolor |              背景颜色              |
|  align  | 文字水平对齐 left 或right或 center |
| valign  | 文字垂直对齐 top 或middle或 bottom |

> td:如果一个单元格设置了宽度，会影响 整列的宽度
> td:如果一个单元格设置了高度，会影响 整行的高度

#### 表格合并

Colspan=所要合并的单元格的**列数**必须给td
Rowspan=所要合并的单元格的**行数**必须给td

## 14.表单标签

> 作用：收集用户信息

```html
<form method="get或者post" action="向何处发送表达数据">
    <input>
    A : 属性 type 定义输入框的类型
        1. 文本框 type="text" 密码框type="passowrd"
        2.提交框 type="submit" 和<button>提交按钮</button> 一样
        3. 按钮狂 type="button" 单纯的按钮
        4. 重置框 type="reset" 清空的效果
    B  属性 placeholder 描述输入字段预期的简短的提示信息，兼容到IE8以上
    C 属性  name必须设置，否则在提交表单时，用户在其中输入的数据不会被发送给服务器
    D  属性 value
</form>
```

```
<from action=""></form> 在该form标签内书写的获取用户的数据都会被form标签提交到后端

```

action：控制数据提交的后端的路径，给哪个服务器提交数据

1. 什么都不写，默认朝当前页面所在的url提交数据
2. 写全路径 ：https://www.baicu.com朝百度服务器提交
3. 只写路径后缀 action="./ndex/"，自动识别出当前服务器的IP和port拼接到前面 host:port/index/

```html
<!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Lightly-HTML-Project</title>
  </head>
  <body>

      <form action="http://www.jd.com" method="POST">
        用户信息：<input type="text" placeholder="请输入您的用户名" name="UserName">
        <br>
        密码： <input type="password" placeholder="请输入密码" name="Password">
        <br>
        <input type="submit" value="登陆">
        <!---提交信息到 action指定的地址--->
        <br>
        <button type="submit" >登陆</button>

        <input type="reset" value="重新输入一个遍">
        <button type="reset">请重新输入</button>

        <input type="button" value="aaaa">
      </form>
  </body>
</html>

```

#### 12.1单选框radio

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>表单进阶-单选框</h1>
    <div>你对京东的满意如何</div>
    <div>
        <!-- <input type="radio" name="aaa" checked="checked">非常满意 -->
        <input type="radio" name="aaa" checked>非常满意
  
    </div>
    <div>
        <input type="radio"  name="aaa">满意
  
    </div>
    <div>
        <input type="radio"  name="aaa">不满意
  
    </div>

    <div>
        <div>你的性别</div>
        <div>
            <input type="radio" name="bbb" id="man">
            <label for="man">男</label>
        </div>
        <div>
            <input type="radio" name="bbb" id="female">
            <label for="female">女</label>
        </div>
    </div>
  
</body>
</html>
```

#### 12.2复选框checkbox

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>表单进阶-复选框</h1>
    <div>你的 兴趣爱好是什么</div>
    <div>
        <div>
            <input type="checkbox" name="aaa">抽烟
            <input type="checkbox" name="aaa" checked="checked">喝酒
            <input type="checkbox" name="aaa" checked>烫头
        </div>
    </div>
    <div>你的擅长的技术是什么</div>
    <div>
        <div>
            <input type="checkbox" name="bbb" id="JS">
            <label for="JS">JS</label>
            <input type="checkbox" name="bbb" id="CSS">
            <label for="CSS">CSS</label>
            <input type="checkbox" name="bbb" id="HTML">
            <label for="HTML">HTML</label>
        </div>
    </div>
</body>
</html>
```

#### 12.3上传文件file

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div>
        <div>请截图说明</div>
        <div>
            <!-- 上传文件 -->
            <input type="file" name="" id="">
        </div>
    </div>
    <div>
        <div>图片按钮-代替提交按钮（input type="submit"）</div>
        <form action="">
  
            <input type="image" src="./IMG/submit.jpg">
        </form>
    </div>


    <div>
        <h1>隐藏按钮</h1>
        <input type="hidden" name="" id="" value="带给后端的个人信息">
    </div>


    <div>
        <div>禁用disabled，只读readonly</div>
        <div>
            <button disabled="disabled">注册</button>
            <input type="radio"  disabled>不满意
            <br>
            <input type="text" disabled value="1212121">
            <input type="text" readonly value="521">
        </div>
    </div>
</body>
</html>
```

#### 12.4下拉菜单select

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div>下拉菜单</div>
    <div>
        <div>你的收货地址</div>
        <!-- select支持的属性
            1. size 显示几个
            2. mutiple 选多个
        -->

        <!-- option支持的属性
            1. value 提供给后端需要的value值
            2. selected ，默认选中
        -->
        <select size="2"> 
            <option  value="辽宁">辽宁</option>
            <option >陕西</option>
            <option selected>河北衡水</option>
        </select>
    </div>
    <div>
        <select size="3" multiple> 
            <option >桌子</option>
            <option >椅子</option>
            <option >电脑</option>
            <option >凳子</option>
        </select>
    </div>
</body>
</html>
```

#### 12.5文本域textarea

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
<style>
    textarea{
        width: 300px;
        height: 300px;
        resize: none;/*重新设置大小 vertical, horizontal, both, none*/
    }
</style>
</head>
<body>
    <h1>多行文本输入框-文本域</h1>
    <div>
        <!--  
            placeholder：提示文字
            默认的value值：写在双标签的内部，注意空格问题

  
        -->
        <!-- <input type="text" name="" id=""> -->
        <textarea  cols="10" rows="10" placeholder="请输入你的意见">1131</textarea>
    </div>
</body>
</html>
```

#### 12.6字段集fieldset

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        fieldset{
            border: 1px solid blue;
            /* width: 200px;
            height: 200px; */
        }
        legend{
            border: 1px solid rebeccapurple;
            text-align: right;
  
        }
    </style>
</head>
<body>
    <h1>字段集</h1>
    <fieldset>
        <legend>性别</legend>
        <input type="radio" name="aa" id="">男
        <br>
        <input type="radio" name="aa" id="">女
    </fieldset>
    <fieldset>
        <legend>性别</legend>
        <input type="checkbox" name="bb" id="">抽烟
        <br>
        <input type="checkbox" name="bb" id="">喝酒
    </fieldset>
</body>
</html>
```
## HTML5新增

![image.png](https://img-blog.csdnimg.cn/img_convert/cc2cdc1081cda912f34b157918dfa345.png)

![image.png](https://img-blog.csdnimg.cn/img_convert/e04d764cc2c42417e8b2016bf175a3b1.png)

![image.png](https://img-blog.csdnimg.cn/img_convert/491b3a9ff20ee22e511c25623e03f627.png)

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
            margin: 0;
            padding: 0;

        }
        html,body{
            height: 100%;
        }
        header,footer{
            height: 50px;
            line-height: 50px;
            text-align: center;
            background-color: orange;
        }
        section{
            height: calc(100% - 100px);
        }
        aside,nav{
            background-color: #cccccc;
            width: 100px;
            height: 100%;
            float: left;
        }
        main{
            float: left;
            width: calc(100% - 200px);
            height: 100%;
        }
        aside p{
            font-size: 10px;
            color: white;
        }
        main .article1{
            height: 60%;
        }
        main .article2{
            height: 40%;
        }
    </style>
</head>
<body>
    <header>header</header>
    <section>
        <nav>
            <figure>nav</figure>
            <ul>
                <li>131</li>
                <li>1231</li>
                <li>112</li>
            </ul>
        </nav>
        <main>
            <article class="article1">
                <header>artivel-header</header>
                <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Molestias, illo iure impedit hic quos at distinctio temporibus expedita placeat sit nulla tenetur animi, odio asperiores, quae facilis. In, nulla deserunt?</p>
                <footer>articel-footer</footer>
            </article>
            <article  class="article2">
                <header>artivel-header</header>
                <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Molestias, illo iure impedit hic quos at distinctio temporibus expedita placeat sit nulla tenetur animi, odio asperiores, quae facilis. In, nulla deserunt?</p>
                <footer>articel-footer</footer>
            </article>
        </main>
        <aside>
            <figure>aside</figure>
            <p class="aside_p">Lorem, ipsum dolor sit amet consectetur adipisicing elit. Amet praesentium eum, expedita laboriosam cumque illum voluptates corporis molestiae inventore laborum! Saepe, ipsum asperiores soluta vero perspiciatis provident esse minima tenetur.</p>
        </aside>
    </section>
    <footer>footer</footer>
  
</body>
</html>
```

**音视频标签**

![image.png](https://img-blog.csdnimg.cn/img_convert/d82449e701bdbe056de7d6db8fc97979.png)

**音频**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <audio src="./jiejie.wav" controls  loop autoplay muted></audio>
    <br>
    <audio src="./Letting Go.m4a" controls autoplay></audio>
    <!-- controls出现控制栏 
        loop 循环
        autoplay自动播放
        muted静音播放
    -->
</body>
</html>
```

**视频**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        /* video{
            width: 500px;
            height: 600px;
        } */
    </style>
</head>
<body>
    <video src="./movie.mp4" controls poster="./poster.jpg" ></video>
    <!-- autoplay 自动播放
        loop 循环
        controls 控制条  
        muuted 静音

        poster 出现海报
        width height
    -->
</body>
</html>
```

**增强表单- input**

| 语法                  | 说明                      |
| --------------------- | ------------------------- |
| Type="color"          | 生成一个颜色选择的表单    |
| Type="tel"            | 唤起拨号盘表单            |
| Type="search"         | 产生一个搜索意义的表单    |
| Type="range"          | 产生一个滑动条表单        |
| Type="number"         | 产生一个数值表单          |
| Type="email"          | 限制用户必须输入email类型 |
| Type="url"            | 限制用户必须输入url类型   |
| Type="date"           | 限制用户必须输入日期      |
| Type="month"          | 限制用户必须输入月类型    |
| Type="week"           | 限制用户必须输入周类型    |
| Type="time"           | 限制用户必须输入时间类型  |
| Type="datetime-local" | 限制用户必须输入本地时间  |

**数据列表**

![image.png](https://img-blog.csdnimg.cn/img_convert/60e66df9138525fcf2bc4b2269a9718c.png)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <input type="text" list="mylist">
  
    <datalist id="mylist">
        <option value="手镯"></option>
        <option value="手表"></option>
        <option value="手环"></option>
        <option value="手机"></option>
    </datalist>

</body>

</html>
```

**属性**

![image.png](https://img-blog.csdnimg.cn/img_convert/318b50f884e30aad6c4d1302ce42ab08.png)