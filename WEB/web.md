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
|    😃     |     表情 最后一位的数字5，可以是任意的数字，表情也就不同     |

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

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1659339509056/1eda175407ca402bafdea0bafc6880fc.png)

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

### **6.2空余空间**

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

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1659339509056/f49fd70abe94477cafa4710824af9eb9.png)

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

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1659339509056/cb91516a1bba491793ca10fbe46f64e3.png)

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

# HTML5+CSS3新增

### HTML5新增

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1659339509056/81b110b60af14606a0af15cf86e774f3.png)

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1659339509056/1a5690d7cbeb4769b0e372743f6c9fc6.png)

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1659339509056/c4838c1c4ae1477582cce0a9d71dfb17.png)

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

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1659339509056/57e4aafcae8349238a4914ddcde4035a.png)

音频

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

视频

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

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1659339509056/caa0e425cde149968c5084318af0ce96.png)

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

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1659339509056/58e5e023d04f42c894e7991703ff4e46.png)

### CSS3新增

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1659339509056/1f2ac70ffa12443491d2c6141b1eb7a1.png)

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

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1659339509056/27f8b1d0346a42b8b23af463a0d9e165.png)

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

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1659339509056/2a35e7543bda4a13b42acb0969bcfb7a.png)

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

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1659339509056/dca788ab4ae7437ea7bdf56c7a3cb7dc.png)

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

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1659339509056/f0930bb8fca8414b8acb93009260a882.png)

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

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/14697/1659339509056/2cdf08fb154841fdb48c3e8679039ff5.png)

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