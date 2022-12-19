[TOC]



---


计算机不能直接理解任何除机器语言外的语言，所以必须要把程序员所编写的程序语言翻译成机器语言，才能执行程序，程序语言翻译成机器语言的工具，被成为翻译器。

* 翻译器翻译方式有两种，一个是**编译**，一个是**解释**，两种方式之间的区别在于**翻译的时间点不同**
* 翻译器实在**代码执行之前进行编译**，生成中间代码文件
* 解释器是在**运行时进行及时解释**，并立即执行(当编译器以解释器方式运行的时候,也被称之为解释器）

### js简介

```markdown
1. js一门编程语言，可以写后端代码
2. nodejs支持js代码跑在后端服务器
Javascript是脚本语言，解释语言
javascript是一种轻量级的编程语言
JavaScript是可插入HTML页面的编程代码
Javascript插入HTML页面后，可由所有的现代浏览器执行
是一种运行在客户端的脚本语言（script是脚本的意思）
脚本语言：不需要编译，运行过程中由解释器（js引擎）逐行来进行解释并执行
可以基于Node.js技术进行服务端编程
```

### js作用

```markdown
1. 表单验证（密码强度检测）（js产生最初的目的）
2. 网页特效
3. 服务端开发(Node.js)
4.桌面程序（Electron）
5.App*Cordova)
6.控制硬件-物联网（Ruff)
7.游戏开发（cocos2d-js)

```

### 浏览器执行js简介

浏览器分成两部分**渲染引擎和js引擎**

* **渲染引擎**：用来解析HTML和CSS，俗称内核，
* **JS引擎**：也成为JS解释器，用来读取网页中JavaScript代码，对其处理后运行

**浏览器本身不会执行js代码，而是通过内置的Javascript引擎（解释器）来执行js代码，JS引擎执行官代码逐行解释每一句源码（转化为机器语言），然后通过计算机去执行，所以Javascript语言归于脚本语言，会逐行解释执行**

### js三部分组成

* **ECMAScript(javascript语法)**

  ![image.png](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219111007.png)
* **DOM（页面文档对象模型**）

  ![image.png](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219111007-1.png)
* **BOM（浏览器对象模型）**

  ![image.png](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219111007-2.png)

### JS三种书写位置

**行内，内联，外部**

1. **行内式JS**

```html
<body>
    <input type="button" value="提交" onclick="alert('你好啊')">
</body>
```

* 可以将单行或少量js代码写在HTML标签的事件属性中(以on开头的属性),如：onclick
* 注意单双引号的使用：在**HTML**中推荐使用**双引号**，**JS**中推荐使用**单引号**
* 可读性差，在HTML中编写大量的代码时，不便于阅读
* 引号易错，引号多层嵌套匹配时，容易弄混

2. **内嵌式JS**

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script>
        alert('你好');
    </script>
</head>
```

3. **外部JS文件**

```html
 <scrtpt scr="my.js"></script>
```

* 利用HTML页面代码结构化，可以把大段JS代码独立到HTML页面中
* 引用外部JS文件的script标签中间不可写代🐎

### js注释

```js
//单行注释

/*多行注释
 多行注释
*/
```

### 标识符，关键字，保留字

标识符是☞开发人员为变量，函数，参数取的名字

标识符不能是关键字或者保留字

关键字是☞JS本身已经使用了的字，不能再去用他们当变量名、方法名

包括：break case、catch、continue、dedault、do、else、finally、for、function、instanceof、new、return、switch、this、throw、try、typeof、var、void、while 、with等

保留字就是预留的关键字，意思是现在虽然还不是关键字，但是未来可能会成为关键字，同样不能使用他们当变量名和方法名

![image.png](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219111007-3.png)

### js输入输出语句

| 方法             | 说明                                               |
| ---------------- | -------------------------------------------------- |
| alert(msg)       | 浏览器弹出警示框                                   |
| console.log(msg) | 浏览器 控制台打印输出信息&#x3c;br />程序员测试用的 |
| prompt(msg)      | 浏览器弹出输入框，用户可以输入                     |

### js语法结构

```
js是以分号作为语句的结束
但是不写分号，也能够正常执行，但是它相当于没有结束符
```

### 变量

变量是用于存放数据的容器，可以通过变量名，获取数据，甚至修改数据

本质：变量是程序在内存中申请的一块用来存放数据的空间

**变量的使用：**

1. **声明变量**

```html
var name;
```

* var是一个JS关键字，用来声明变量（variable)，使用该关键字声明变量后，计算机会自动为变量分配内存空间，不需要程序员管
* name是程序员自定义的变量名，通过变量名来访问内存中分配的空间

2. **赋值**

```html
name='zhao';
```

* **=**号来把右面的值赋给左边的变量空间中，
* 变量值是程序员保存到变量空间里的值

3. **变量的初始化**

声明一个变量并赋值，称之为变量的初始化

```html
var name ='zhao'
```

```markdown

js中首次定义变量名 时需要用关键字声明：
    1.var name='json'
    esr推出的新语法:
    2.let name='json'
如果编辑器支持的 版本是5.1,则无法使用let
如果是6.0则向下兼容 var let
    
    
/*var与let区别*/
```

**4. 变量语法扩展**

1. 修改变量

```html
var name='zhao';
name='lisi';
```

2. 声明多个变量

同时声明多个变量，只需要写一个var,多个变量名之间用英文逗号隔开

```html
var name='zhangsan',age=18,gender='male';
```

3. 声明变量特殊情况

```html
1.声明不赋值
var sex;
console.log(sex);/*Undefined*/
2.不声明，不赋值；
console.log(name);/*报错*/
3.不声明直接赋值
qq=110
console.log(qq);
```

**5.命名规范**

* 由字母（A-Za-z)，数字（0-9），下划线（_)，美元符号($)组成
* 严格区分大小写，var app ;和 var App;是两个变量
* 不能以数字开头
* 不能是关键字，保留字
* 变量名必须有意义，做到见名知意
* 遵循驼峰命名法，
* 推荐翻译网站：有道，爱词霸

### 数据类型

在计算机中，不同的数据类型所需要占用的存储空间是不同的，为了便于把数据分成所需要内存大小不同的数据，充分利用存储空间，便定义了不同的数据类型

变量是用来存储值的所在处，他们有名字和数据类型，变量的数据类型决定了如何将代表这些值的位存储到计算机内存中，**JavaScript是一种弱类型或者说是动态语言**，这意味着不用提前声明变量的类型，在程序中运行过程中，类型会被自动确定

```html
var age =10;  //这是数字型
var sex ='female';  //字符串类型
```

**==在代码运行时，变量的数据类型是由JS引擎 根据=号右边变量值的数据类型来判断的，运行完毕后，变量就确定了数据类型==**

JavaScript拥有动态类型，同时也意味着相同的变量可作用不同的类型

#### 数字型Number

```js
 var x=5;    //数字类型
var x='hello';  //字符串类型
```

1. **数据类型分类**

* 简单数据类型(Number,String,Boolean,Undefined,Null)
* 复杂数据类型（object)

| 简单数据类型 | 说明                                           | 默认值    |
| ------------ | ---------------------------------------------- | --------- |
| Number       | 数字型包含整形值和浮点型值                     | 0         |
| Boolean      | 布尔值类型 true,fales等价于1和0                | false     |
| String       | 字符串类型，带引号                             | ""        |
| Undefined    | var a;声明了变量a但是没有赋值，此时a=undefined | undefined |
| Null         | var a =null;  声明了变量a为空值                | null      |

2.数字型

```js
//八进制  0表示八进制
var num1=010;
console.log(num1);//控制台结果为8    默认是是十进制，010八进制转为十进制就是8
// 十六进制  0x
var num2=0xa; //结果为10

```

3. 数字类型范围

JavaScript中数的最大值和最小值

```js
alert(Number.MAX_VALUE));  //1.7976931348623157e+308
alert(Number.MIN_VALUE)); //5e-324
```

**特殊值：**

```js
alert(Infinity);
alert(-Infinity);
alert(NaN);
```

* Infinity:代表无穷大，大于任何数值
* -Infinity:代表无穷小，小于任何值
* NaN （Not a number） ,代表一个非数值

**isNaN()方法用来判断非数字，并返回True或者False，如果是数字返回False，非数字返回True**

```js
var age=20;
var isOk=isNaN(age);
console.log(isOk) //false
```

#### **字符串String**

字符串可以是引号中的任意文本，语法是双引号和单引号

**引号嵌套**：JS可以用**单引号嵌套双引号**，或者**双引号嵌套单引号（外双内单，外单内双）**

1. **字符串转义符：**

转义符都是 `\ `开头的

| 转义符 | 解释说明                 |
| ------ | ------------------------ |
| \n     | 换行符，n是newline的意思 |
| \\\    | 斜杠\                    |
| `\'`   | ' 单引号                 |
| `\"`   | “ 双引号                 |
| \t     | tab缩进                  |
| \b     | 空格，b是blank的意思     |

2. **字符串长度**

字符串是由若干个字符组成的，这些字符的数量就是字符串的长度，通过字符串的`length`可以获取整个字符串的长度

```js
var str='my sdnafaf';
console.log(str.length);
```

3. 字符串拼接

* 多个字符串之间可以使用 `+`进行拼接， 其拼接方式：字符串 + 任何类型的数据 =拼接后的新字符串
* 拼接前会把与字符串相加的任何类型转成字符串，再拼接成一个新的字符串

```js
concole.log('zhao' + 18);
var age=18;
console.log('kangyi' + age + '岁');
```

+ **+ 号口诀：数值相加，字符相连**

```js
var age = prompt('请输入年龄');
var str = '今年'+ age + '岁了';
alert(str);
```

* **模板字符串**

```js
# 模板字符串不仅可以定义多行文本还可以实现格式化字符串操作
var name = 'zhao'
var age = 18
var res= `
	my name is ${name} my age is ${age}
`
res
```

| 方法                      | 说明               |
| ------------------------- | ------------------ |
| .length                   | 返回长度           |
| .trim()                   | 移除空白           |
| .trimLeft()               | 移除左边空白       |
| .trimRight()              | 移除右边空白       |
| .charAt(n)                | 返回第n个字符      |
| .concat(value,......)     | 拼接               |
| .indexOf(substring,start) | 子序列位置         |
| .substring(from,to)       | 根据索引获取子序列 |
| .slice(start,end)         | 切片               |
| .toLowerCase()            | 小写               |
| .toUpperCase()            | 大写               |
| .split(dilimiter,limit)   | 分割               |

```js
var name = 'zhaoDSB'
undefined
name.length
7
var name1 = '  zhaoDSB   '
undefined
name1.trim()   //不能加括号指定去除的内容
'zhaoDSB'
name1.trimLeft()
'zhaoDSB   '
name1.trimRight()
'  zhaoDSB'
var name2 = '$$jason$$'
undefined
name.charAt(3)
'o'
name.indexOf('a')
2
name.substring(0,4)
'zhao'
name.slice(0,5)
'zhaoD'
name.slice(0,4)
'zhao'
name.substring(0,-1)  //不识别负数
''
name.slice(0,-1);   //推荐使用slice
'zhaoDS'
var demo = 'dlaDLjLLJUB666dsER'
undefined
demo.toLowerCase()
'dladljlljub666dser'
demo.toUpperCase()
'DLADLJLLJUB666DSER'
var test = 'tank|zhao|liao|'
undefined
name.split('|')
['zhaoDSB']
test.split('|',10)   //第二个参数不是限制切割字符的个数，而是获取切割之后元素的个数
(4) ['tank', 'zhao', 'liao', '']
test.split('|')
(4) ['tank', 'zhao', 'liao', '']
test.concat(name)   //js是弱类型语言，内部会转成相同的数据类型做操作
'tank|zhao|liao|zhaoDSB'



//python代码
l=[1,2,3,4]
res=('|').join(1)  //直接报错  列表里的数据必须是字符串类型
print(res)
```



#### 布尔型Boolean

布尔类型只有true 和 false，其中true表示真（对）， false表示假（错）

```python
"""
1.python中布尔值是首字母大写
    True
    False
2. js中布尔值是全小写
	true
	false
# 布尔值是flase的有:
	空字符串 、0、null、undefined、Nan
"""
```

#### Undefined和Null

一个声明后没有赋值，会有一个默认值undefined

一个声明变量给null值，里面存的值为空

#### typeof检测数据类型

typeof用来获取变量的数据类型



#### 字面量

字面量（literal）是用于表达源代码中一个固定值的表示法，通俗讲，就是字面量如何让表达这个值

* 数字字面量：8，9，10
* 字符串字面量：'html','前端'
* 布尔字面量：true false

#### 数据类型转换

使用表单、prompt获取过来的数据默认字符串类型的，此时不能直接简单的进行加减法，而需要转换变量的数据类型，通俗讲，就是把一种数据类型的变量转成另外一种数据类型

**1.转为字符串类型**

| 方式              | 说明                         | 案例                               |
| ----------------- | ---------------------------- | ---------------------------------- |
| toString()        | 转成字符串                   | var  num=1; alert(num.toString()); |
| String() 强制转换 | 转成字符串                   | var num =1; alert(String(num));    |
| 加号拼接字符串    | 和字符串拼接的结果都是字符串 | var num=1; alert(num+'字符串');    |

2.**转为数字型（重点）**

| 方式                   | 说明                             | 案例                  |
| ---------------------- | -------------------------------- | --------------------- |
| parselnt(string)函数   | 将string类型转换为整数数值型     | parseInt('12.3') 取整 |
| parseFloat(string)函数 | 将string类型转换为整浮点数数值型 | parseFloatt('12.3')   |
| Number()强制转换函数   | 将string类型转为数值型           | Number('12')          |
| js 隐式转换( -  *  /)  | 利用算术运算隐式转换为数值型     | '12'-0                |

```js
 console.log(parseInt('123px'));//截取到123
        console.log(parseInt('12.2'))//取整
        console.log(parseFloat('12.002'));
        console.log(Number('123.15'));
   console.log(parseFloat("132.21px")); //截取到132.21
  console.log('12'-0); //先把'12'转成整型12
        console.log('123'-'120');
```

3. **转换为布尔类型**

| 方式          | 说明                 | 案例             |
| ------------- | -------------------- | ---------------- |
| Boolean()函数 | 其他类型转换为布尔值 | Boolean('True'); |

* 代表空、否定的值会被转换为false，如：''、0、NaN、null、undefined

**小案例：**

1. 要求页面中弹出输入框，输入出生年份后，计算出我们的年龄

```js
       var year=prompt("输入年份：")
        var age=2022-year;
        alert('您的年龄是' + age +'岁');

```

2. 计算两个数的值，用户输入第一个值后，弹出第二个框输入第二个值，最后通过弹出的窗口计算两次输入值相加的结果

```js
      var num1 = prompt('请输入第一个数字：');
      var num3 = prompt('请输入第二个数字：');
      alert(parseInt(num1)+parseInt(num3));
```

3. ```js
    var name=prompt("请输入姓名：");
           var age=prompt('请输入年龄：');
           var sex=prompt('请输入性别：');
           alert('您的姓名是：'+name+'\n'+'您的年龄是：'+age+'\n'+'您的性别是：'+sex);
   ```

### 运算符

运算符(operator)也被称为**操作符**，用于实现赋值，比较，执行数运算等功能的符号

1. **算术运算符**

| 运算符 | 描述 |
| ------ | ---- |
| +      | 加   |
| -      | 减   |
| *      | 乘   |
| /      | 除   |
| %      | 取余 |

浮点数进行运算会出现精度问题

**表达式和返回值**

表达式：由数字、运算符、变量等以求和的数的有意义排列方法所得的组合，简单讲就是由数字、运算符、变量等组成的式子

**表达式最终都会有一个结果，返回给我们，称为返回值**

2. **递增递减运算符**

如果需要反复给数字添加变量添加或减去1，可以使用**递增（++）和递减（--）**运算符来完成。

在JavaScript中，递增（++）和递减（--）既可以放在变量前面，也可以放在变量后面，**放在变量后面称为后置递增（递减）运算符，放在变量前面，称为前置递增（递减）运算符**

**注意：递增递减必须和变量配合使用**

**2.1前置递增运算符**

**>: 先自加，后返回值**

```js
var age=10;
++age;  //类似于age =age+1;
var kig=10;
console.log(++kig + 10);//21
```

**2.2后置递增运算符**

**>: 先返回原值，后自加**

```js
var num= 10;
console.log(num++ + 10); //20
```

```js
var a  =10;
++a; //++a=11 a=11
var b = ++a + 2; //  a = 12 ++a=12  
console.log(b); //14

var c = 10;
c++; //c++=10 c=11
var d = c++ + 2 ; //c++= 11 c=12  
console.log(d); //13

var e=10;
var f = e++ + ++e; //e++ =10 e=11  ++e=12  e=12
console.log(f)//22
```

3. **比较运算符**

| 运算符名称    | 说明                         | 案例      | 结果  |
| ------------- | ---------------------------- | --------- | ----- |
| >             | 大于                         | 1>2       | false |
| &#x3c;        | 小于                         | 1&#x3c;2  | true  |
| >=            | 大于等于                     | 2>=2      | true  |
| &#x3c;=       | 小于等于                     | 3&#x3c;=2 | false |
| ==            | 判断等号(会转型)             | 37==37    | true  |
| !=            | 不等号                       | 32!=32    | false |
| `===` ` !===` | 全等，要求值和数据类型都一致 | 37==="37" | false |

`==`会默认转换数据类型,会把字符串转为整型

```js
console.log(18==18); //true
console.log(18=='18'); //true
```

4.**逻辑运算符**

逻辑运算符是用布尔值运算的运算符，其返回值也是布尔型，

常用于多个条件的判断

![image.png](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219111007-4.png)

逻辑与两边都是真才为true，否则返回false

逻辑或有一个为真就为 true

逻辑非也叫取反符，用来取一个布尔值相反的值，

4.1**短路运算（逻辑中断）**

**原理**：当有多个表达式（值），左边表达式值可以确定结果时，右边不继续运算右边表达式的值

**逻辑与短路运算**：左边返回值为假，右边就不运算，直接返回false；如果左边返回值为真，那么就要看右边是否也为真，右边为真就返回true,右边返回假就返回false

**逻辑或短路运算**：如果左边返回真，就直接返回true，右边不再运算；如果左边返回假，那么要看右边返回的值，右边返回真就返回true，右边返回值为假就返回false

5 .赋值运算符

| 赋值运算符 | 说明                 | 案例                          |
| ---------- | -------------------- | ----------------------------- |
| =          | 直接赋值             | var name="zhao"               |
| +=、 -=    | 加、减一个数后再赋值 | var age=10;&#x3c;br />age+=5; |
| *=、/=、%= | 乘、除、取模后再赋值 | var age=2;&#x3c;br />age*=5;  |

6. **运算符优先级**

![image.png](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219115108.png)

![image.png](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219111007-5.png)

* 一元运算符里面的逻辑非优先级很高
* 逻辑与比逻辑或优先级高

### 流程控制

顺序结构：按代码的先后顺序，依次执行

分支结构：根据不同的条件，执行不同的路径代码，得到不同的结果

循环结构

#### 分支流程控制if语法结构

* 单分支

条件为真，则执行语句，否则什么也不做

```js
if (表达式){
            //执行语句
}

```

* 双分支

条件为真执行语句1，否则执行语句2

```js
if (表达式){
            //执行语句1
        }else{
	//执行语句2
}


var age  =prompt("请输入年龄：")
        if(age>=18){            //比较运算符可以自动转换类型
            alert('可以进入网吧');
        }else{
	alert("未成年不可进入");
}

```

**案例：**

判断闰年:能被4整除且不能被100整除的为闰年，或者能够被400整除的就是闰年

```js
    var year= prompt('请输入年份：')
    if(year%4==0 && year%100!=0  || year%400==0){
        alert('您输入的年份时闰年')
    }else{
        alert('您输入的年份不是闰年')
    }
```

多分支

```js
if(表达式1){
	//语句1;
}else if(表达式2){
	//语句2;
}else if(表达式3){
	//语句3;
}else{
	//最终的语句;
}

```

案例：

要求：接受用户输入的分数，根据分数输出对应的字母ABCDE

其中：

1. 90分以上（包含90）输出：A
2. 80（含）--90（不含）输出：B
3. 70（含）--80（不含）输出：C
4. 60（含）--70（不含）输出：D
5. 60分以下（不含）输出：E

```js
        var grade = prompt('请输入成绩：')
        if(grade >=90){
            alert('A');
        }else if(grade >=80){
            alert('B');
        }else if(grade >=70){
            alert('C');
        }else if(grade>=60){
            alert('D');
        }else{
            alert('E')
        }

```

#### 三元表达式：

由三元运算符组成的式子

**语法结构**：`条件表达式 ? 表达式1 : 表达式2`

**执行思路**：如果条件表达式为真，则返回表达式1，如果表达式结果为假，则返回表达式2

```js
var age=10;
var result = age>12? 'No' :'Yes';
console.log(result);
```

案例：用户输入0-59的数字，若数字小于10，则前面补0.比如01，09，如果数字大于10，则不需要补，比如20

```js
var time=prompt('请输入0-59的一个数字')
var result= time<10 ? '0' + time : time;
console.log(result)
```

#### 分支流程控制switch语句

switch也是多分枝语句，用于基于不同的条件来执行不同的代码，当要针对变量设置一系列的特定值的选项时，可以用switch

##### 语法结构

```js
switch(表达式){
    case value1:
        执行语句1;
        break;
    case value2:
        执行语句2;
        break;
     .....
    default:
        执行最后的语句;
}
```

执行思路：利用表达式的值和case后面的选项值相匹配，如果匹配上，就执行该case里面的语句，如果没匹配上，那么执行default里面的语句

```js
switch (2){
    case 1:
        console.log('这是1');break;
    case 2:
        console.log('这是2');break;
    case 3:
        console.log('这是3');break;
    default:
        console.log('没有匹配到结果')
}
```

案例：

用户在弹出的 输入框输入一个水果，如果有就弹出该水果的价格，如果没有就弹出 '没有此水果'

```js
var fruit = prompt('请输入一个水果名字：');
switch( fruit){
    case '橘子':
        alert('3.5/斤');
        break;
    case '苹果':
        alert('2.8/斤');
        break;
    case '榴莲':
        alert('10/斤');
        break;
    case '桃子':
        alert('5.1/斤');
        break;
    default:
        alert('没有'+fruit +'这个水果');
}
```

* **switch和if else if 区别：**



![image.png](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219115013.png)

#### 循环

* 许多有规律性的重复操作
* 在程序中，一组被重复执行 的语句被称为循环体，能否继续重复执行，取决于循环的终止条件，由循环体及循环的终止条件组成的语句，被称之为循环语句

##### for循环

把代码重复若干次，通常跟计数有关。

```js
for(初始化变量;条件表达式;操作表达式){
	//循环体
}
```

初始化变量：就是用var 声明的一个普通变量，通常用于作为计数器使用,**整个循环里只执行一次**

条件表达式：就是用来决定每一次循环是否继续执行，就是**终止条件**

操作表达式：就是每次循环**最后的执行代码**，经常用于我们计数器变量**进行的递增或递减**

```js
for(let i =1;i<=100;i++){
	console.log(i);
}
```

**执行过程**：

1. 先执行计数器变量，var i =1 ,但是这句话在for循环里只执行一次，.
2. 然后去i&#x3c;=100里判断是否满足条件，如果满足，就去执行循环体，不满足就退出循环
3. 最后去执行 i++   i++单独写的代码，递增，  第一轮结束
   第二轮：
4. 接着去执行i&#x3c;=100,如果满足，就去执行循环体，不满足就退出循环
5. 继续去执行 i++
6. ......

**案例**：

求1-100之间所有整数累加和

```js
var sum=0;
for(var i =1;i<=100;i++){
    sum+=i;
}
alert(sum);
```

求1-100平均值

```js
var sum=0;
var average=0;
for (var i =1; i<=100;i++){
    sum+=i;
}
average=sum/100;
alert(average);
```

1-100所有整数偶数的和，奇数的和

```js
var sum_a=0;
var sum_a=0;
var sum_b=0;
for(var i =1;i<=100;i++){
    if(i%2==0){
        sum_a+=i;
    }else{
        sum_b+=i;
    }  
}
alert('偶数和：'+sum_a);
alert('奇数和：'+sum_b);
```

求1-100之间所有能被3整除的数字之和

```js
var sum=0;
for(var i =1;i<=100;i++){
    if(i%3==0){
        sum+=i;
    } 
}
alert('被3整除的和'+sum)

```

用户输入班级人数，之后依次输入每个学生的成绩，最后打印出该班级总的成绩以及平均分

```js
var sum=0;

var numbers=prompt('班级总人数：')
for (var i =1;i<=numbers;i++){
    var stu =prompt('第'+i+'个学生成绩为');
     var score=parseFloat(stu);
    sum+=score;
}
alert('总成绩为：'+sum);
alert('平均成绩为：'+ sum/numbers);
```

打印五行五列♥

```js
var str='';
for(var i=1;i<=5;i++){
    for(var j=1;j<=5;j++){
        str=str+'♥';
   
    }
    str+='\n'
}
console.log(str);
```

打印倒三角

```js
var str='';
for(var i=1;i<=10;i++){
    for(var j=10;j>=i;j--){
        str=str+'⭐';
    }
    str=str+'\n';
}
console.log(str);
```

```js
var str='';
for(var i=1;i<=10;i++){
    for(var j=i;j<=10;j++){
        str+='⭐';
    }
    str+='\n';
}
console.log(str);
```

打印正三角形

```js
var str='';
for(var i=1;i<=10;i++){
    for(var j=10;j<=i;j--){
        str+='⭐';
    }
    str+='\n';
}
console.log(str);
```

九九乘法表

```js
var str='';
for(var i =1;i<10;i++){
    for(var j=1;j<=i;j++){
  
        str+=j +'x'+i+'='+i*j;
        str+='\t';
   
    }
    str+='\n';
}
console.log(str);
```

##### 断点调试

![image-20221013090135443](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219114941.png)

##### while循环

当....的时候

* 执行思路：当条件表达式结果为true，则执行循环体，否则退出
* 里面也应该有计数器，初始化变量
* 里面应该有操作表达式，完成计数器自增或者自减，防止死循环

```js
while(条件表达式){
    //循环体
}
```

```js
var i=1;
while(i<=100){
    //循环体
    console.log(i);
    i++;
}
```

1-100和

```js
var sum=0;
var j=1;
while(j<=100){
    sum+=j;
    j++
}
console.log(sum);
```

弹出一个提示框：你爱我吗？，如果输入我爱你，就提示结束，否则一直提示

```js
var  message=prompt('你爱我吗?');
while(message !== '我爱你'){
    message=prompt('你爱我吗？');
}
alert('我也爱你!♥');
```

##### do while循环

```js
do{
	//循环体
}while (条件表达式)
```

执行思路：跟while不同的是do while先执行一次循环体，在判断条件，如果条件表达式结果为真，则继续执行循环体，否则退出循环体

**do while至少执行一次循环体代码**

```js
do{
	consoloe.log('heool are you?);
	i++;
}while(i<=100)
```

```js
var i=1;
do{
    console.log('今年'+i+'岁了');
    i++;
}while(i<=100)
```

```js
do{
    var mes=prompt('你爱我吗？');
}while(mes!=='我爱你')
alert('我也爱你♥')
```

##### continue关键字

用于立即跳出本次循环，继续下一次循环，

```js
for(var i=1;i<=5;i++){
    if(i==3){
        continue;
    }
    console.log(i);
}
```

1-100之间，除了能被7整除的数之和

```js
var sum=0;
for(var i=1;i<=100;i++){
    if(i%7==0){
        continue;
    }
    sum+=i;
}
console.log(sum);
```

##### break关键字

立即跳出整个循环，循环结束

### 数组(Array)

数组：一组数据的集合，存储在单个变量下的优雅方式

* 数组里的数据用逗号隔开，
* 数组里的数据称为数组元素
* 数组中可以存放**任意类型**的数据

**利用new创建数组**

```js
var arr = new Arrary() //创建另一个空数组
```

**利用数字字面量创建数组**

```js
var 数组名 = [];   //创建空数组
var 数组名 =['小白','小黑','蜘蛛'];//创建带初始值的数组
```

##### 获取数组元素

1. 数组的索引

**索引（下标）**：用来访问数组元素的序号（数组下标从0开始）

可以通过**索引**来访问设置修改对应的数组元素，可以通过  **数组名[索引]**  的形式获取数组中的元素

```js
var arr=[1,2,15,421,3130];
alert(arr[1]); //获取第二个元素
```

##### 数组遍历

遍历：就是把数组中每个元素从头到尾都访问一遍

```js
var arr = [1,2,15,45,'zhangsan'];
for (var i=0;i<5;i++){
    console.log(arr[i]);
}
```

##### 数组的长度

使用  **数组名.length**  可以访问数组元素的个数（数组长度）

```js
var arr = [1,2,15,45,'zhangsan'];
for (var i=0;i<arr.length;i++){
    console.log(arr[i]);
}
```

案例：求数组[2.6,1.7,4]里面所有元素的和，以及平均值

```js
var sum=0;
var arr = [2.6,1.7,4];
for (var i=0;i<arr.length;i++){
    sum+=arr[i];
}
console.log(sum);
console.log(sum/arr.length);
```

数组中最大值

```js
var arr =[20,2,54,5461,121,1,3,13,456,4,970];
var max=arr[0];
for (var i=1;i<arr.length;i++){
    if (arr[i]>max){
        max=arr[i];
    }
}
console.log(max);
```

数组转换为分割字符串

```js
var arr =['zhao','lisi','yiming','haihong','xuan'];
var str='';
for (var i=0;i<arr.length;i++){
    str+=arr[i];
    str+='|';
}
console.log(str);
```

##### 数组新增元素

1. 通过修改length长度新增数组元素

* 可以通过修改length长度来实现数组扩容的目的
* length属性是可读写的

```js
var arr = ['refd','a','f'];
arr.length=7;
console.log(arr);
console.log(arr[4]);
```

其余索引号3，4，5，6的空间没有给值，就是声明变量为赋值，默认值就是undefined

2. 通过修改数组索引新增数组元素

* 可以通过修改数组索引的方式追加数组元素

```js
var arr=['red',1,2];
arr[3]='hello';
console.log(arr);
arr[4]='world';
console.log(arr.length);
arr[0]='black';  //替换原来的数组元素
console.log(arr);
```

不要给数组名直接赋值，否则会替换掉数组里的所有元素

```js
var arr=[1,2,15];
arr='zhao';   //替换数组所有的元素
console.log(arr);
```

新建一个数组，里面存放10个整数（1-10）

```js
var arr=[];
for (var i=0;i<10;i++){
     arr[i]=i+1;
}
console.log(arr);
```

要求把数组中大于等于10的元素筛选出来，放入新的数组

```js
var arr=[2,12,1210,31,45,12,10,200,5];
var j=0;
var newArr=[];
for (var i=0;i<arr.length;i++){
    if (arr[i]>=10){
        //新数组下标应该从0开始，。依次递增
        newArr[j]=arr[i];
        j++;
    }
}
console.log(newArr);
```

```js
var arr=[2,12,1210,31,45,12,10,200,5];

var newArr=[];
for (var i=0;i<arr.length;i++){
    if (arr[i]>=10){
        //新数组下标应该从0开始，。依次递增
        newArr[newArry.length]=arr[i];
  
    }
}
console.log(newArr);
```

删除指定数组元素

将指定数组中的0去掉后，形成一个不包含0的新数组

```js
        var arr = [2, 0, 3, 45, 461, 2, 3, 0, 0, 25, 7];
        var newArry = [];
        for (var i = 0; i < arr.length; i++) {
            if (arr[i] != 0) {
                newArry[newArry.length] = arr[i];
            }
        }
```

反转数组，内容都反过来存放

```js
        var arr=[1,32,121,0,1214,'dog'];
        var newArry=[];
        for (var i= arr.length-1; i>=0;i--){
            newArry[newArry.length]=arr[i]
        }
        console.log(newArry);

```

冒泡排序：是一种算法，把一系列的数据按照一定的顺序进行**两两比较**排列显示（从小到大或从大到小）

```js
 var arr = [5, 4, 3, 2, 1];
        for (var i = 0; i <= arr.length - 1; i++) {  //外层循环管趟数
            for (var j = 0; j < arr.length - i - 1; j++) {//里层循环管每趟交换的次数
                if (arr[j] > arr[j + 1]) {
                    var temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;

                }
            }

        }
```

##### 其他方法

```python
var l =[1,2,3,4,5,6,7,8,10]
undefined
l.length
9
l.push(9)   //尾部添加元素
10
l.pop()   //删除最后一个值
9
l.unshift(123) //头部插入元素
10
l
(10) [123, 1, 2, 3, 4, 5, 6, 7, 8, 10]
l.shift()     //头部移除元素
123
l
(9) [1, 2, 3, 4, 5, 6, 7, 8, 10]
l.slice(0,4)   //切片
(4) [1, 2, 3, 4]
l.reverse()    //反转
(9) [10, 8, 7, 6, 5, 4, 3, 2, 1]
l.join('&')    //拼接，  跟python正好相反
'10&8&7&6&5&4&3&2&1'
l
(9) [10, 8, 7, 6, 5, 4, 3, 2, 1]
var j=l.join('^')
undefined
j
'10^8^7^6^5^4^3^2^1'
l.concat([12121,4545,4551,511,21313,])  //连接数组，跟python里的extend相似
(14) [10, 8, 7, 6, 5, 4, 3, 2, 1, 12121, 4545, 4551, 511, 21313]
l
(9) [10, 8, 7, 6, 5, 4, 3, 2, 1]
l.sort()   //排序
(9) [1, 10, 2, 3, 4, 5, 6, 7, 8]
```

```python
var ll=[111,222,333,444,555]
undefined
ll.forEach(function(value){console.log (value)},ll)
VM933:1 111   //一个参数就是数组里面每一个元素对象
VM933:1 222 
VM933:1 333
VM933:1 444
VM933:1 555
undefined
ll.forEach(function(value,index){console.log(value,index)},ll)
VM1084:1 111 0   //两个参数就是  元素+元素索引
VM1084:1 222 1
VM1084:1 333 2
VM1084:1 444 3
VM1084:1 555 4
undefined
ll.forEach(function(value,index,arr){console.log(value,index,arr)},ll)  //三个参数就是 元素+元素索引+元素的数据来源
VM1189:1 111 0 (5) [111, 222, 333, 444, 555]
VM1189:1 222 1 (5) [111, 222, 333, 444, 555]
VM1189:1 333 2 (5) [111, 222, 333, 444, 555]
VM1189:1 444 3 (5) [111, 222, 333, 444, 555]
VM1189:1 555 4 (5) [111, 222, 333, 444, 555]
```

```python
ll.splice(0,3)  //两个参数，第一个是起始位置，第二个参数是删除的个数
(3) [111, 222, 333]
ll
(2) [444, 555]
ll.splice(0,1,777)   //先删除，后增加
[444]
ll
(2) [777, 555]
ll.splice(0,1,[111,222,333,444])
[777]
ll
(2) [Array(4), 555]0: (4) [111, 222, 333, 444]1: 555length: 2[[Prototype]]: Array(0)
```

```python
var ll =[11,22,33,44,55,66]
undefined
ll
(6) [11, 22, 33, 44, 55, 66]
ll.map(function(value){console.log(value)},ll)
VM1785:1 11
VM1785:1 22
VM1785:1 33
VM1785:1 44
VM1785:1 55
VM1785:1 66
(6) [undefined, undefined, undefined, undefined, undefined, undefined]
ll.map(function(value,indexedDB){return value*2},ll)
(6) [22, 44, 66, 88, 110, 132]
ll.map(function(value,indexed,arr){return value*2},ll)
(6) [22, 44, 66, 88, 110, 132]
```



### 函数

函数就是一段被重复执行调用的代码块。

#### 函数的使用

分两步：声明函数，调用函数

1. **声明函数**

```js
function myfunc(){
	//函数体
}
```

* **function**是声明函数的关键字
* 函数表达式(匿名函数)

```js
var 变量名=function(){
	console.log('我是匿名函数');
}
```

2. **调用函数**
   
   ```
   函数名()  //通过调用函数名来执行函数体代码
   ```

* 调用函数的时候**不要忘了加小括号**

注意：声明函数本省不会执行代码。只有调用的时候才会执行函数体的代码

3. **函数的封装**

* 函数的封装就是把一个或者多个功能通过**函数的方式封装起来**，对外只提供一个简单的的函数名

```js
 //函数声明
         function getSum(){
            var sum=0;
            for(var i=0;i<=100;i++){
                sum+=i;
            }
            console.log(sum);
         }
         //调用函数
         getSum();
```

#### 函数的参数

在声明函数时，可以在函数名称后面的小括号中添加一些参数，这些参数称为形参，而在调用函数时，同样也需要传递响应的参数，被称为实参。

```js
function 函数名(形参1,形参2){

}
函数名(实参1,实参2);
```

* 形参是接收实参的
* 函数的参数可以有，可以没有

```js
  function getSum(num1,num2){
            console.log(num1+num2);
        }
   getSum(1,2);
```

#### 函数的返回值 return

```js
function 函数名(){
	rerturn 需要返回的结果;

}
```

* 遇到return ，就把后面的结果，返回给函数的调用者

```js
function getsum(){

	return 6696;
}
getsum();
```

* return之后的代码就不会被执行
* return只能返回一个值，如果用逗号隔开，以最后一个为准
* 如果有return则返回return后面的值
* 没有return则返回undefined
* 想返回多个可以写成数组的形式

```js
function index(){
	return [666,444,888];
}
```

#### arguments的使用

当我们不知道有多少个参数传递的时候，可以调用arguments来获取

JavaScript中，arguments实际上时当前函数的一个内制对象，所有函数都内置了一个arguments对象，arguemnts对象中存储了传递的所有是实参

```js
 function getSum(){
            console.log(arguments);  //里面存储了所有传递过来的实参
            console.log(arguments.length);
            console.log(arguments[1]);
        }
        getSum(1,2);
```

arguments展示形式是一个伪数组，因此它可以遍历，伪数组具有以下特点：

* 具有length属性
* 按索引方式存储数据
* 不具有数组的push pop等方法

案例：利用函数求任意个数的最大值

```js
function getMax(){
            var max=arguments[0];
            for (var i=1;i<arguments.length;i++){
                if (arguments[i]>max){
                    max=arguments[i];
                }
            }
            return max;
        }
        console.log(getMax(1,2,3));
        console.log(getMax(1,2,3,4564,456411));
        console.log(getMax(101215,15141,11));
```

#### 匿名函数

> 没有名字的函数

```js
var res=function(){
    console.log('啊哈哈')
}
```

#### 箭头函数

```js
var func1 =v => v;//箭头左边的是形参，右边的是返回值
等价于
var func1 =function(v){
    return v
}


var func2 =(arg1,arg2) => arg1+arg2
等价于
var func1 =function(arg1,arg2){
    return arg1+arg2
}
```

#### 调用另一个函数

```js
function fn1(){
	console.log(11);
	fn2();
}
function fn2(){
	console.log(22);
}
```

用户输入年份，输出当前年份2月份的天数

如果是闰年，2月份时29天，如果是平年，则2月份是28天

```js
function backDay() {
            var year = prompt('请输入年份：')
            if (isRunyear(year)) {
                alert('当前年份是闰年，2月份有29天');
            } else {
                alert('当前年份是平年，2月份有28天');
            }
        }


        function isRunyear() {
            var flag = false;
            if (year % 4 == 0 && year % 100 != 0 || year % 400 == 0) {
                flage = true;
            }
            return flag;
        }
```

#### 作用域

> 跟Python一样

就是变量在某个范围内起作用，目的是为了提高程序的可读性，最重要的是减少冲突

局部作用域：在函数内部就是局部作用域，只在函数内部起效果

全局作用域：整个script标签或者是一个单独的js文件

变量的作用域：根据作用域的不同，变量分为全局变量和局部变量

在全局作用域下声明的变量叫**全局变量（在函数外部定义的变量）**

在局部作用域下声明的变量叫做**局部变量（在函数内部定义的变量）**

* 全局变量只有浏览器关闭才会销毁，比较占内存资源
* 局部变量当程序执行完就会销毁，比较省j资源
* 局部变量只能在函数**内部**使用
* 全局变量在代码**任何位置**都可以使用
* 函数的形参实际上也是局部变量

**作用域链**

* 只要是代码，就至少有一个作用域
* 写在函数内部的局部作用域
* 如果函数中还有函数，那么在这个作用域中就可以诞生一个作用域
* 根据在内部函数可以访问外部函数变量的这种机制，用链式查找决定哪些数据能被内部函数访问，就称为作用域链

```js
var num=10;
function fun(){
	var num=20;
	function fn(){
	console.log(num);  //20
	}
	fn();
}
fun();
```

```js
var city ='Beijing';
function Bar(){
    console.log(city);
}
function f(){
    var city='ShanHai';
    return Bar;
}
var res=f();
res();    //Beijing
```

```js
//闭包
var city="Beijing";
function f(){
    var city="ShanHai";
    function inner(){
        console.log(city);
    }
    return inner;
}
var res=f();
res();
```



### 预解析

JavaScript代码是由浏览器中的JavaScript解析器来执行的，JavaScript解析器在运行JavaScript代码的时候分两步：**预解析和代码执行**

按照代码书写的顺序从上往下执行

预解析会把所有的var 还有function提升到当前作用域的最前面

预解析分为**变量预解析**（变量提升）和**函数预解析**（函数提升）

* 变量提升就是把所有的变量声明提升到当前的作用域最前面， 不提升赋值操作

![image.png](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219111007-8.png)![image.png](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219111007-9.png)

* 函数提升就是把所有的函数声明提升到当前作用域的最前面，不调用函数

```js
 fn();
 function fn(){
      console.log(11);
} 

        /*相当于
        function fn(){
            console.log(11);
        }
        fn()
        */
```

预解析案例：

```js
var num=10;
fun();
function fun(){
	console.log(num);
	var num=20;
}

//相当于执行了以下操作
var num;
function fun(){
	var num;
	console.log(num);  //undefined
	num=20;
}
num=10;
fun();
```

```js
var num = 10;
function fn(){
	console.log=(num);
	var num=20;
	console.log(num);
}
fn();
//相当于执行以下代码
var num;
function fn(){
	var num;
	console.log=(num); //undefined
	num=20;
	console.log(num); //20
}
num=10;
fn();
```

```js
var a =18;
f1();
function f1(){
	var b=9;
	console.log(a);
	console.log(b);
	var a='123';
}
//相当于以下代码
var a;
function f1(){
	var b;
	var a;
	b=9;
	console.log(a); //undefined
	console.log(b);//9

    a='123';
}
a=18;
f1();
```

```js
f1();
console.log(c);
console.log(b);
console.log(a);
function f1(){
	var a=b=c=9;
	console.log(a);
	console.log(b);
	concole.log(c);
}
//相当于以下代码
function f1(){
	var a;
	a=b=c9;


	console.log(a); //9
	console.log(b);//9
	concole.log(c);//9
}
f1();
console.log(c);//9
console.log(b);//9
console.log(a);//报错
```

### 对象

万物皆对象，对象是一个具体的事务，看的见摸得到的实物，例如：一本书、一辆车、一个人，可以是对象，一个数据库，一张网页，一个与远程服务器的连接也可以是对象

在JavaScript中，对象是一组无序的相关属性和方法的集合，所i有事物都是对象，例如：字符串、数值、数组、函数等

对象是 由**属性**和**方法**祖成的

* 属性：事物的**特征**，在对象中用**属性**来表示
* 方法：事物的**行为**，在对象中用**方法**来表示

保存一个值时，可以使用**变量**，保存多个值（一组值）时，可以使用**数组**，但是要保存一个人的完成信息就需要用到**对象**

##### 创建对象三种方式

1. ==利用字面量创建对象==

**对象字面量**：就是花括号{}里面包含了表达这个具体事物（对象）的属性和方法

```js
var obj={
	name:'张三',
	age:18,
	sex:'男',
	myfun:function(){
		console.log('hello');
	}
}
```

* 里面的属性或者方法，我们可以采取键值对的形式，键 属性名：值 属性值
* 多个属性或者方法中间用逗号隔开
* 调用对象的属性，采用**对象名.属性名 (对象的属性名)**
* 也可以采用**对象['属性名']**来调用

```js
console.log(obj.name);
console.log(obj['name']);
```

* 调用对象的方法  **对象名.方法名()**

```js
obj.myfun();
```

2. ==利用new Object()创建对象==

```js
var obj=new Object()  //创建了一个空对象
//添加属性
obj.name='李四';
obj.gender='女';
//添加方法
obj.myfun=function(){
	console.log('zhao');
}
//调用
console.log(obj.name);
console.log(obj.gender);
console.log(obj.myfun);
```

3. ==利用构造函数来创建对象==

构造函数：是一种特殊的函数，主要是用来初始化对象，即为对象成员变量赋初始值，它总与nuew运算符一起使用，我们可以把对象中一些公共属性和方法抽取出来，封装到这个函数里面

```js
       function 构造函数名(){
        this.属性=值;
        this.方法=function(){}
       }
       new 构造函数名();
```

```js
        function Star(name, age, sex) {
            this.name = name;
            this.age = age;
            this.sex = sex;
        }
        var result=new Star('黎明', 45, '男');//调用函数返回的是一个对象
        // console.log(typeof result);
        console.log(result.name);
        console.log(resulr.sex);
        var result1=new Star('张学友',50,'男');
        console.log(result1.name);
        console.log(resulr1.age);
```

* 构造函数首字母要大写
* 构造函数不需要return ，就可以返回结果
* 调用构造函数必须使用**new**
* 属性和方法前面必须添加**this**
* 通过new关键字创建对象的过程称为**对象实例化**

new关键字执行过程：

1. new构造函数可以在内存中创建了一个空的对象
2. this就会指向刚才创建的空对象
3. 执行构造里面的的代码，给空对象添加属性和方法
4. 返回这个对象，（所以构造函数不需要return)

##### 遍历对象

for..in语句用于对数组或者对象的属性和方法进行循环操作

```js
for (变量 in  对象){

}
```

```js
 for (var key in obj){
            console.log(key); //输出属性名
            console.log(obj[key]);//输出属性值
        }
```

##### 内置对象

* JavaScript中对象分为3种：自定义对象、内置对象、浏览器对象
* 前两种是JS基础内容，属于ECMAScript ;第三个浏览器对象属于我们JS独有的
* 内置对象就是JS语言自带的一些对象，供开发者使用，并提供了一些常用的或者是最基本功能（属性和方法）
* JavaScript中常用的内置对象：Math、Date、Array、String等

###### 查阅文档

[MDN](https://developer.mozilla.org/zh-CN/)

###### Math对象

[Math.max()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/max)

* 返回给定的一组数字中的最大值。如果给定的参数中至少有一个参数无法被转换成数字，则会返回 [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN)
* 如果没有参数，则结果为 - [`Infinity`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Infinity)

```js
        console.log(Math.PI);//圆周率
        console.log(Math.max(1,88,99));//最大值
        console.log(Math.max());//-infinity -的无穷大
        console.log(Math.max(1,2,55,'lisi')); //NaN
```

**封装自己的Math对象**

```js
        var myMath={
            PI:3.141592653,
            max:function(){
                var max=arguments[0];
                for (var i =1;i<arguments.length;i++){
                    if(arguments[i]>max){
                        max=arguments[i];
                    }
                }
                return max;
            },
            min:function(){
                var min=arguments[0];
                for (var i =1;i<arguments.length;i++){
                    if(arguments[i]<min){
                        min=arguments[i];
                    }
                }
                return min;
            }

        }
        console.log(myMath.PI,myMath.max(1,88,99));
```

其他方法

```js
        //绝对值方法
        console.log(Math.abs(-1));
        console.log(Math.abs('-1'));//隐式转换，会把字符串型-1转成数字型
        console.log(Math.abs('lisi'));//NaN

        //三个取整方法
        //1.Math.floor 向下取整，往最小了取整
        console.log(Math.floor(1.1));//1
        console.log(Math.floor(1.9));//1
        //2.Math.ceil 向上取整，往最大了取整
        console.log(Math.ceil(1.1));//2
        console.log(Math.ceil(1.9));//2
        //3.四舍五入 Math.round()
        console.log(Math.round(1.1));//1
        console.log(Math.round(1.5));//2
        console.log(Math.round(1.9));//2
        console.log(Math.round(-1.1));//-1

```



[Math.random()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/random)

**`Math.random()`** 函数返回一个浮点数， 伪随机数在范围从**0 到**小于 **1** ，也就是说，从 0（包括 0）往上，但是不包括 1（排除 1）

得到大于等于0.小于1的随机数

```js
function getRandom() {
	return Math.random();
}
```

###### Date对象

是一个构造函数，必须使用new来调用创建日期对象



```python
# 时间对象具体方法
let d6=new Date();			
d6.getDate()  			#获取日期
d6.getDay()				#获取星期
d6.getMonth()			#获取月份（0-11）
d6.getFullYear()		#获取完成年份
d6.getHours()			#获取小时
d6.getMinutes()			#获取分钟
d6.getSeconds()         #获取秒
d6.getMilliseconds()    #获取毫秒
d6.getTime()            #时间戳
```



#### JSON对象

```python
"""
在Python中序列化反序列化
dumps 序列化
loads 反序列化


在js中也有序列化和反序列化
JSON.stringify()                  dumps
JSON.parse()                      loads
"""
let d1={'name':'zhao','age':18}
let res=JSON.stringify(d1)
'{"name":"zhao","age":18}'
JSON.parse(res)
{name: 'zhao', age: 18}
```



![image.png](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219114934.png)

```python
Supports the following objects and types by default:
    +-------------------+---------------+
    | Python            | JSON          |
    +===================+===============+
    | dict              | object        |
    +-------------------+---------------+
    | list, tuple       | array         |
    +-------------------+---------------+
    | str               | string        |
    +-------------------+---------------+
    | int, float        | number        |
    +-------------------+---------------+
    | True              | true          |
    +-------------------+---------------+
    | False             | false         |
    +-------------------+---------------+
    | None              | null          |
    +-------------------+---------------+
```

#### RegExp对象

```python
"""
python中使用正则，需要借助re模块
Js中需要创建正则对象
"""
#第一种，有点麻烦
let reg1=new RegExp('^[a-zA-Z][a-zA-Z0-9]{5,11}')
#第二种
let reg2=/^[a-zA-Z][a-zA-Z0-9]{5,11}/

#匹配内容
reg1.test('lisizhangsan')
reg2.test('zhangang')
#获取字符串里的字面
let ss='zhaosong so  sosfaf asn'
ss.match(/s/)  #拿到第一个停止
['s', index: 4, input: 'zhaosong so  sosfaf asn', groups: undefined]
ss.match(/s/g) #全局匹配，g就代表全局模式
(5) ['s', 's', 's', 's', 's']

let reg5 =/undefined/
undefined
reg5.test('zhao')
false
reg5.test()
true
```

### BOM与DOM操作 

```python
"""
BOM:浏览器对象模型 Brower Object Model
	js代码操作浏览器
DOM：文档对象模型  Document Object Model
	js代码操作标签
"""
```

### BOM操作

```python
#window对象：指代的就是浏览器窗口
window.innerHeight  浏览器窗口的高度
736
window.innerWidth   浏览器窗口的宽度
970

#新建窗口打开页面，第二个参数写空即可，第三个参数写新建窗口的大小和位置
window.open('https://www.csdn.net/','','height=400px,width=400px，top=400px,left=400px')

window.close()  关闭当前页面

#父子页面通信window.opener()

```

#### window子对象

```python
window.navigator.appName
'Netscape'  #网景

window.navigator.appVersion
'5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.0.0 Safari/537.36'
window.navigator.userAgent  #用来标识当前是否是一个浏览器.
'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/106.0.0.0 Safari/537.36'
"""
防爬措施
1. 校验当前请求的发起者是否是一个浏览器
爬虫：在代码中加上user-agent配置即可， 就可以UA伪装
"""

window.navigator.platform  #平台
'Win32'

# 如果时window的子对象，那么window可以省略不写
navigator.appVersion
```

#### history对象

```python
window.history.back()  #回退到上一页
window.history.forward()#前进到下一页
#对应的就是浏览器左上方的两个箭头

```

#### location对象(掌握)

```python
window.location.href  #获取当前页面的url
window.location.herf='url'  #跳转到指定的url
window.location.reload() #刷新页面（浏览器左上方的刷新小圆圈）
```

#### 弹出框

* 警告框

  ```js
  alert('你不要过来啊')
  ```
* 确认框

  ```js
  comfirm('你确定吗')  #点击取消，控制台显示false，点击确定，控制台显示true
  ```
* 提示框

```python
prompt('请输入')#控制台显示输入的内容
```

#### 计时器相关

* 过几秒钟后触发` setTimeout()`
* 每隔一段时间触发一次（循环）`setInterval()`

```js
 <script>
        function fun1(){
            alert(123)
        }
        let t = setTimeout(fun1,3000)  //毫秒为单位，3秒后自动执行fun1函数
        clearTimeout(t)  //取消定时任务，如果要清除定时任务，需要提前用变量指代定时任务


        function fun2(){
            alert(123)
        }
        function show(){
            let t = setInterval(fun2,3000) //每隔三秒弹出一次
            function inner(){
                clearInterval(t)  //清除定时器
            }
            setTimeout(inner,9000)  //9秒后触发
        }
        show()
     
      
</script>
```

### DOM操作

```python
DOM树的概念
所有的标签都可以称之为节点
Javascript可以通过DOM创建动态的HTML
Javascript能够改变页面中的所有HTML元素
Javascript能够改变页面中的所有HTML属性
Javascript能够改变页面中的所有CSS样式
Javascript能够改变页面中的所有事件做出反应


DOM操作 操作的是标签，而一个html页面上的标签有很多
1. 先学如何查找标签
2. 再学DOM操作标签

DOM需要用关键字document
""
```

#### 查找标签

* 直接查找（掌握）

```python
"""
id查找
类查找
标签查找
"""
#下面三个方法的返回值是不一样的
document.getElementById('d2')
document.getElementsByClassName('c1')
document.getElementsByTagName('div')


let divEle =document.getElementsByTagName('div')[0]
divEle
"""
当用变量指代标签对象的时候，一般情况下写成xxxEle
"""
```

* 进阶查找（熟悉）

```python
let pELe=document.getElementsByClassName('c1')[0]
undefined
pELe.parentElement  #拿父节点
<div id="d2">…</div>
pELe.parentElement.parentElement
<body>…</body>
pELe.parentElement.parentElement.parentElement
<html lang="en"><head>…</head><body>…</body></html>


let divEle =document.getElementById('d2')
undefined
divEle.children #获取所有的子标签

divEle.children[0]   //获取第一个子标签
<div>div1>div</div>
divEle.firstChild
"div1 "
divEle.firstElementChild  //第一个子标签
<div>div1>div</div>
divEle.lastElementChild   //最后一个子标签
<p>div1>p2</p>
divEle.nextElementSibling  #同级别下面第一个
<div>div1div2</div>
divEle.previousElementSibling  #同级别上面第一个
<div>H人类咯</div>
```

#### 节点操作

```python
"""
1. 通过DOM操作动态的创建img标签
2. 并给img加属性
3. 最后将标签添加到文本中
"""
```

```js
let imgEle=document.createElement('img')  #创建标签

imgEle.src='../tou.jpg'   #给标签设置默认的属性
'../tou.jpg'

imgEle.username='zhao'#自定义的属性没办法以点的方式设置
'zhao'

imgEle.setAttribute('name','zhao')#既可以设置自定义的属性，也可设置默认的属性
undefined
imgEle
<img src="../tou.jpg" name="zhao">
imgEle.setAttribute('title','头像')
undefined
imgEle
<img src="../tou.jpg" name="zhao" title="头像">
let divELe= document.getElementById('d1')
undefined


divELe.appendChild(imgEle)#标签内部添加元素（尾部追加）
<img src="../tou.jpg" name="zhao" title="头像">
```

```js
/*
创建a标签
设置属性
设置文本
添加到标签内部，添加到指定的标签上面
*/
let aELe=document.createElement('a')
undefined
aELe.href='http:www.baidu.com/'
'http:www.baidu.com/'
aELe
<a href="http:www.baidu.com/"></a>
aELe.innerText='别摸我'   #给标签设置文本内容
'别摸我'
aELe
<a href="http:www.baidu.com/">别摸我</a>
let divEle=document.getElementById('d1')
undefined
let pEle=document.getElementById('d2')
undefined
divEle.insertBefore(aELe,pEle)  #添加标签内容指定位置添加（a标签添加到div内部，p标签之前）
<a href="http:www.baidu.com/">别摸我</a>
```

**补充**

```python
appendChild()
removeChild()
replaceChild()

setAttribute() 设置属性
getAttribute() 获取属性
removeAttrive()  移除属性


# innerText与innerHTML

divEle.innerText         #获取到标签内部所有的文本
'别摸我\n\ndiv>p\n\ndiv>span'
divEle.innerHTML       #内部文本和标签都拿到
'\n        <a href="http:www.baidu.com/">别摸我</a><p id="d2">div>p</p>\n        <span>div>span</span>\n    '
divEle.innerText='hahahah'
'hahahah'
divEle.innerHTML='sb'
'sb'
divEle.innerText='<h1>二蛋</h1>'  #不识别HTML标签
'<h1>二蛋</h1>'
divEle.innerHTML='<h1>二蛋</h1>'   #识别HTML标签
'<h1>二蛋</h1>'
```

#### 获取值操作

```js
//获取用户数据集标签内部的数据
let selectEle =document.getElementById('d2')
selectEle.value
'111'
selectEle.value
'222'
selectEle.value
'333
let inputEle = document.getElementById('d1')
inputEle.value
"sxcd"

//如何上传用户输入的文件
let fileEle =document.getElementById('d3')
undefined

fileEle.value  //无法获取到文件数据，得到是文件路径
'C:\\fakepath\\d160681e16c2782af151165d1202dd45.jpeg'

fileEle.files[0] //获取文件数据
File {name: 'd160681e16c2782af151165d1202dd45.jpeg', lastModified: 1661066246763, lastModifiedDate: Sun Aug 21 2022 15:17:26 GMT+0800 (中国标准时间), webkitRelativePath: '', size: 13627, …}

fileEle.files  //返回数组，【文件对象，文件对象.......】
FileList {0: File, length: 1}
```

#### class、 css操作

```js
let divEle =document.getElementById('d1')
undefined
divEle.classList  //获取标签所有的类属性
DOMTokenList(3) ['c1', 'bg_green', 'bg_red', value: 'c1 bg_green bg_red']
divEle.classList.remove('bg_red') //移除某个类属性
undefined
divEle.classList.add('bg_red')  //添加类属性
undefined
divEle.classList.contains('c1')  //验证是否包含某个类属性
true
divEle.classList.contains('c2')
false
divEle.classList.toggle('bg_red')//有则删除，无则添加
false
divEle.classList.toggle('bg_red')
true
divEle.classList.toggle('bg_red')
false
divEle.classList.toggle('bg_red')
true


//DOM 操作标签样式，统一先用style

let pELe=document.getElementsByTagName('p')[0]
undefined
pELe.style.color='red'
'red'
pELe.style.fontSize='20px'
'20px'
pELe.style.backgroundColor='#eee'
'#eee'
pELe.style.border='3px solid red'
'3px solid red'

//会将css中横杠或者下划线去掉，然后将后面的单词变成大写
//background-color变成backgroundColor
```

#### 原生JS事件绑定

```js
"""
到达某个事先设定的条件，自动触发的动作
"""
//绑定事件的两种方式
<button onclick="fun1()">点击</button>
<button id="d1">点击</button>

<script>
    //第一种绑定事件方式
    function fun1(){
    alert(1231)
}
//第二种
let btnEle =document.getElementById('d1');
btnEle.onclick=function(){
    alert(222)
}
</script>

//script标签既可以放在head内，也可以放在body最底部,一般放在body最底部




//等待浏览器窗口加载完毕再执行代码
    <script>
        window.onload=function(){
             //第一种绑定事件方式
        function fun1(){
            alert(1231)
        }
        //第二种
        let btnEle =document.getElementById('d1');
        btnEle.onclick=function(){
            alert(222)
        }
        }
    </script>
```

* 开关灯示例

```html
<style>
        .c1{
            width: 400px;
            height: 400px;
            border-radius: 50%;
        }
        .bg_green{
            background-color: green;
        }
        .bg_red{
            background-color: red;
        }
    </style>
</head>
<body>
    <div id="d1" class="c1 bg_green bg_red"></div>
    <button id="d2">变色</button>

    <script>
        let btnEle=document.getElementById('d1')
        let divEle=document.getElementById('d2')
        btnEle.onclick=function(){  //绑定点击事件
             //动态的修改类属性
             divEle.classList.toggle('bg_red')
        }
    </script>
```

* input框获取焦点失去焦点案例

```html
<input type="text" value="您好" id="d1">

    <script>
        let inEle=document.getElementById('d1');
        //获取焦点事件
        inEle.onfocus=function(){
            //将input框内部的文本去除
            inEle.value=''
            // 点value就是获取，等号赋值是设置

        }
        //失去焦点事件
        inEle.onblur=function(){
            //给input标签重新赋值
            inEle.value ='不好'
        }
    </script>
```

* 实时展示当前时间

```html
<body>
    <input type="text" id="d1" style="display: block; height: 40px;width: 200px">
    <button id="d2">开始</button>
    <button id="d3">结束</button>

    <script>
        //定义一个全局变量
        let t=null;
        let inputELe = document.getElementById('d1')
        let startBtnEle = document.getElementById('d2')
        let endtBtnEle = document.getElementById('d3')
        //1.访问页面之后将访问的时间添加到input框中
        //2.动态展示当前时间
        //页面上加两个按钮。一个开始，一个结束。
        function showTime() {
            let currentTime = new Date();
            inputELe.value = currentTime.toLocaleString()

        }
        // showTime()
        // setInterval(showTime,1000)
        startBtnEle.onclick = function () {
            //限制定时器只能开一个
            if(!t){
                t=setInterval(showTime, 1000)  //每点击一次就会开设一个定时器，而t只指代最后一个
            }
        }
        endtBtnEle.onclick = function () {
            clearInterval(t)
            //清楚定时器的时候，因该把t重置为空
            t=null
        }
    </script>
</body>
```

* 省市联动

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
    <select  name="" id="d1">
        <option value="" selected disabled>--请选择--</option>
    </select>
    <select  name="" id="d2"></select>

    <script>
        let proEle=document.getElementById('d1')
        let cityEle=document.getElementById('d2')
        //模拟省市数据
        data={'河北省':['衡水','邯郸','唐山'],
        '北京':['东城区','朝阳区'],
        '山东':['青岛市','潍坊市'],
        '上海':['浦东新区','黄浦区'],
        '深圳':['南山区','宝安区','福田区']
    };
    //for循环获取省
    for (let key in data){
        //将省信息做成一个option标签，添加到select框中
        //1.创建option标签
        let opEle=document.createElement('option')
        //2.设置文本
        opEle.innerText=key
        //设置value值
        opEle.value=key
        //将创建好的option添加到slect中
        proEle.appendChild(opEle)
    }
    //文本域变化事件 (change事件）
    proEle.onchange=function(){
        //获取到用户选择的省
        let currentPro=proEle.value
        //获取对应的市信息
        let currentCityList=data[currentPro]
        //清空市select中所有的option
        let ss='<option disabled selected>请选择</option>'
        cityEle.innerHTML=ss
        //for循环所有的市渲染到第二个select中
        for (let i=0; i<currentCityList.length;i++){
            let currentCity=currentCityList[i]
            //1.创建option标签
            let opEle=document.createElement('option')
            //2.设置文本
            opEle.innerText=currentCity
            //设置value值
            opEle.value=currentCity
            //将创建好的option添加到slect中
            cityEle.appendChild(opEle)
    }

    }
    </script>
</body>
</html>
