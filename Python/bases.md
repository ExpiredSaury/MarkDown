` 提示：涵盖Python基础语法，掌握基础的编程能力`

[toc]



# 01、python初识

Python是一种面向对象的解释型计算机程序设计语言，由吉多.范罗苏姆开发，第一个公开发行版发布于1991年，它常被成为胶水语言，能够把其他语言制作的各种模块（尤其是C/C++），恒轻松的联结在一起。
**Python优点**

* 免费开源
  Python开源，开发者可以自由的下载，阅读，甚至是修改Python源代码

* 丰富的第三方库
  Python具有本身且丰富而且强大的库，而且由于Python的开源性，第三方库也非常多，例如：在web开发有django.flask,Tornado，爬虫scrapy，科学计算numpy.pandas等等

* 可以移植
  由于Python是开源的，他已经被移植到了大多数平台下面，例如：Windows，MacOS，Linux，Andorid,iOS等等

* 面向对象
  Python既支持面向过程，又支持面向对象，这样编程就更加灵活

**Python缺点**

* **运行速度慢**
  C程序相比非常慢，因为Python是解释型语言，代码在执行时会一行一行的翻译成CPU能够理解的机器码，这个翻译过程非常的耗时，所以很慢，而C程序时运行前直接编译成CPU能执行的机器码，所以相对Python而言，C语言执行非常快

* **代码不能加密**
  要发布你写的程序，实际上时发布源代码，而是解释型的语言，则必须把源代码发布出去

* **强制的缩进**
  Python有非常**严格的缩进语法**，只要缩进错误程序立马崩溃

* **GIL全局解释器锁**
  在任意时刻，只有一个线程在解释器中运行，对Python虚拟机的访问由全局解释器锁（GIL）来控制，正式这个锁能够保证同一时刻只有一个线程在运行，遇到i/o阻塞的时候会释放（GIL）所以Python的多线程并不是真正的多线程，而是CPU执行速度非常快，让人感觉不到GIL的存在。（GIL）会在Python高级阶段讲解

#### 1.1、掌握Python注释

---

 


* 注释是编写程序时，写程序的人给人一句、程序段、函数等的**解释或提示**
* 注释的作用：
  注释可以起到一个备注的作用，这个方法函数，变量到底是干嘛的，如果没有注释时间长了即使是自己写的代码，可能都不知道这个代码到底是干嘛的，所以注释起到的作用就是方便自己查看写过的代码，别人来接收你的代码能看懂，简单来将就是提高程序代码的可读性，以便于以后的参看、修改
* Python中**单行注释用#号，**#号右边就是注释的内容，Python解析器遇到#号就会当作注释，不会去解析#号后面的内容
  **多行注释**

```python
"""
多行注释
"""
```



#### 1.2、掌握Python变量的定义与命名规则

* **变量=存储的数据**
* **变量是一段有名字的连续的存储的空间，我们可以通过定义变量来申请并命名这样的存储空间，并通过变量的名字来使用这段存储空间**
* **变量是程序中临时存放数据的场所**
  Python是一种**强类型的语言，赋值变量时候不需要指定数据类型，给这个变量赋值什么数据类型，这个变量就是什么类型**

```python
#x就是变量，当x=2结果就是6 x=10结果就是30
x=2
y=x*3
print(y)
```

**命名规则**：
变量必须以字母（a-z A-Z）下划线(_)开头，其他字符可以是字母数字下划线 ，变量区分大小写，Python关键字不能作变量名

```python
_Name='张三'
name='刘德华'
print(_Name,name)
```

**命名规范**：

* 简明知意，尽量使用有语义的单词命名，如使用Password用作密码，username用作用户名
* 小驼峰式命名法：第一个单词首字母小写其他单词首字母大写如userName
* 大驼峰式命名法：全部单词首字母都用大写，如UserName
* 下划线命名法：每个单词用_下划线连接，如user_name

---

#### 1.3、掌握Python基本操作符

* 算数运算符：+  -  *  /   **（指数） %（取余） //（相除取整）

定两个变量a=7,y=3

| 算数运算符 |                    作用描述                     |       示例        |
| :--------: | :---------------------------------------------: | :---------------: |
|   + 加法   |                    算术加法                     |      a+b=10       |
|   -减法    |                    算数减法                     |       a-b=4       |
|   *乘法    |                    算数乘法                     |      x*y=21       |
|   **指数   |         左边的数是底数，右边的数是指数          |     a* *b=343     |
|   %取余    |                x%y x除以y的余数                 |       a%b=1       |
|   /除法    |            x/y结果包含小数点后面的数            | a/b=2.33333333335 |
| //相除取整 | x//y 结果是忽略小数点后面的小数位，只保留整数位 |      a//b=2       |

* 比较运算符：==（等于） !=（不等于）  >   &#x3c;     >=     &#x3c;=

返回布尔型 True False

| 比较运算符 |     名称     |   示例    |                   结果描述                    |
| :--------: | :----------: | :-------: | :-------------------------------------------: |
|     ==     |     等于     |   x==y    |            如果x恰好等于y，则为真             |
|     !=     |    不等于    |    x!=    |            如果x恰好不等于y 则为真            |
|     >      |     大于     |    x>y    |     如果x(左侧参数)大于y(右边参数),则为真     |
|   &#x3c;   |     小于     | x&#x3c; y |     如果x(左侧参数)小于y(右边参数),则为真     |
|     >=     |  大于或等于  |   x>=y    | 如果x(左侧参数)大于或者等于y(右边参数),则为真 |
|  &#x3c;+   | 小于或者等于 | x&#x3c;=y | 如果x(左侧参数)小于或者等于y(右边参数),则为真 |

* 逻辑运算符  and  or not

| 逻辑运算符 |  实例   |                      结果描述                      |
| :--------: | :-----: | :------------------------------------------------: |
|    and     | x and y | x,y同为真，则结果为真，如果有一个为假，则结果为假  |
|     or     | x or y  |  x, y有一个为真，结果就为真，全部为假，则结果为假  |
|    not     |  not x  | 取反，如果x为真，则结果为假，如果x为假，则结果为真 |

```python
a=1
b=2
c=3
d=5
print(a>b and d>c) #结果为假
print(a>b or  d>c)#结果为真
print(not a>d)  #结果为真
```

**优先级**：（）-->not-->and-->or

```python
print(2>1 and 1<4 or 2<3 and 9>6 or 2<4 and 3<2)  #True
```

| 赋值运算符 |    作用描述    |           结果描述            |
| :--------: | :------------: | :---------------------------: |
|     =      |   赋值运算符   | 将=号右边的值赋值给左边的变量 |
|     +=     | 加法赋值运算符 |        c+=a等效于c=c+a        |
|     -=     | 减法赋值运算符 |        c-=a等效于c=c-a        |
|     *=     | 乘法赋值运算符 |        c*=a等效于c=c*a        |
|     /=     | 除法赋值运算符 |        c/=a等效于c=c/a        |
|     %=     | 取模赋值运算符 |        c%=a等效于c=c%a        |
|    **=     |  幂赋值运算符  |      c* *=a等效于c=c* *a      |
|    //=     | 取整赋值运算符 |       c//=a等效于c=c//a       |

```python
a,b,c,d=23,10,18,3
a+=3
print(a)
```

---

#### 1.4、掌握Python输入输出

**输出    %占位符**

* Python 有一个简单的字符格式化方法，使用%做占位符，%后面跟的是变量的类型 %s %d
* 在输出时，若果有\n,此时\n后的内容会在另外一行显示    换行

```python
name ='张三'
classPro='北京大学'
age =22
print('我的名字是%s：来自%s 今年%d岁了'%(name,classPro,age))

me ='我的'
classP='清华附中一年级3班'
age1=7
print('%s名字是小明，来自%s,我今年%d岁了'%(me,classP,age1))

print('我可以\n换行吗')

```

* %c 字符
* %s 通过str()字符串转换来格式化
* %d 有符号十进制整数
* %u 无符号十进制整数
* %f浮点数

```python
print('\n')

print('-------------test---------------')
print('\n')
ID='老夫子'
QQ=66666666
number=13467204515
address='广州市白云区'
print('========================================')
print('姓名：%s'%ID)
print('QQ: %d'%QQ)
print('手机号码：%d'%number)
print('公司地址：%s'%address)
print('========================================')
print('\n')
```

**格式化输出另外一种方式**

```python
print('---------格式化输出另外一种方式.format------------')
print('姓名：{}'.format(ID))
print('QQ：{}'.format(QQ))
print('手机号码：{}'.format(number))
print('公司地址：{}'.format(address))
```

**输入 input**

Python中提供input 方法来获取键盘输入
**input接收的键盘输入都是str类型**，如果接收数字类型需要将str转成int

```python
name=input('请输入姓名')
age2=int(input('请输入年龄：'))
qq=input('请输入QQNumber：')
phone=input('请输入手机号：')
addr=input('请输入地址：')
print('姓名：%s 年龄是%d'%(name,age2))
print('QQ：{}'.format(qq))
print('手机号码：{}'.format(phone))
print('公司地址：{}'.format(addr))

```

---

# 02、Python流程控制

* Python条件语句是通过一条或多条语句的执行结果【True/False】来决定执行的代码块
* 就是**计算机执行代码的顺序**
* 流程控制：对计算机代码执行的顺序进行有效的管理，只有流程控制才能实现在开发当中的业务逻辑
* **流程控制的分类**：
  * 顺序流程  ：就是代码一种自上而下的执行结构，也是python默认的流程
  * 选择流程/分支流程：根据在某一步的判断，有选择的去执行相应的逻辑的一种结构
    * 单分支   if 条件表达式：      if
    * 双分支   if 条件表达式：    if  else:
    * 多分支    if 条件表达式： if   elif 条件表达式：   elif 条件式：     else:
    * **条件表达式**:比较运算符/逻辑运算符/符合运算符
  * 循环流程： 在一定的条件下，一直重复的去执行某段代码的逻辑【事情】
    * while 条件表达式：
    * for 变量 in  可迭代的集合对象

---

#### 2.1、单分支

```python
score = 80
if score<=60: #满足条件就会输出打印提示
    print('成绩不太理想，继续加油')
    pass  #空语句
print('语句运行结束')
```

---

#### 2.2、双分支

```python
if score>60:
    print('成绩不错')
else:
    print("成绩不合格，继续努力")
```

---

#### 2.3、多分支

* 特征：只要满足其中一个分支，就会退出本层if 语句结构【必定会执行其中一个分支】
* 至少有2种情况可以选择
* elif后面必须嘚写上条件和语句
* else 是选配，根据实际的情况来填写

```python
grade=int(input('请输入成绩：'))
if grade>90:
    print('您的成绩是A等')
    pass
elif grade>=80:
    print('您的成绩是B等')
    pass
elif grade >=70:
    print('您的等级是C等')
    pass
elif grade >=60:
    print('您的等级是D等')
    pass
else:
    print('你个废物蛋')
print('程序运行结束.........')

```

---

#### 2.4、if-else 嵌套

* 一个场景需要分阶段或者层次，做出不同的处理
* 要执行的if语句一定要外部的if 语句满足条件才可以，

```python
xuefen=int(input('请输入你的学分'))
if xuefen >10:
    grade = int(input('请输入你的成绩'))
    if grade>=80:
        print('你可以升班了。。。恭喜您')
        pass
    else:
        print('很遗憾，您的成绩不达标....')
        pass
    pass
else:
    print('您的表现也太差了吧0')
```

---



#### 2.5、while循环

> 语法特点
>
> 1.有初始值
>
> 2.条件表达式
>
> 3.变量【循环体内计数变量】的自增自减，否则会造成死循环

* 使用条件：循环的次数不确定，是依靠循环条件来结束
* 目的： 为了将相似或者相同的代码操作变得更加简洁，使得代码可以使用重复利用

---

#### 2.6、for循环

* 遍历操作，依次的取集合容器中的每个值

```python
tage='我是一个中国人'#字符串类型本身就是一个字符类型的集合
for item in tage:
     print(item)
     pass
#range 此函数可以生成一个数据集合的列表
#range(起始值：结束值：步长）步长不能为0
sum=0
for data in range(1,101):#数据做包含，右边不包含1-100
   sum+=data
   pass
   #print(data,end=' ')
print(sum)
print('sum=%d'%sum)
# print('sum={}'.format(sum))
#
for message in range(50,201):
     if message%2==0:
         print('%d是偶数'%message)
         pass
     else:
         print('{}是奇数'.format(message))

# break 代表中断结束，满足条件直接结束本层循环
# continue 结束本次循环，继续进行下次循环，  （当continue条件满足的时候，本次循环剩下的语句将不在执行，后面的循环继续）
#这两个关键字只能用在循环中
```

**while:适用于对未知的循环次数 【用于判断】**
**for ：适应于已知的循环次数【可迭代对象遍历】**

#### 2.7、案例体现

**99乘法表用for实现**

```python
#九九乘法表
for i in range(1,10):
    for j in range(1,i+1):
        print('%d*%d=%d'%(i,j,i*j),end=' ')
    print()#控制换行

print('--------------------------------------------------------')
print('\n')
```

**案例 输入1-100之间的数据**

```python
#输入1-100之间的数据
index =1#定义变量
while index<=100:
   print(index)
   index+=1
   pass
```

**案例猜拳游戏**

```python
#案例猜拳游戏
import random
count =1
while count<=10:
    person =int(input('请出拳：【0 石头 1 剪刀2布】\n'))
    computer=random.randint(0,2)#随机数范围
    if person==0 and computer==1:
        print('你真厉害')
        pass
    elif person==1 and computer==2:
        print('你可真棒')
        pass
    elif person==2 and computer==0:
        print('你真厉害')
        pass
    elif person==computer:
        print('不错嘛，竟然平手')
        pass
    else:
        print('你输了')
        pass
    count+=1
```

**九九乘法表**

```python
#打印九九乘法表
i=1
while i<=9:
    j=1
    while j<=i:
        print('%d*%d=%2d'%(i,j,i*j),end="  ")
        j+=1
        pass
    print()
    i+=1
    pass
```

**直角三角形**

```python
#直角三角形
row=9
while row>=1:
    j=1
    while j<=row:
        print('*',end=' ')
        j+=1
        pass
    print()
    row-=1
    pass
```

**等腰三角形**

```python
#等腰三角形
i=1
while i<=10:
    j1=1
    while j1<=10-i:#控制打印没空格的数量
        print(' ',end='')
        j1+=1
        pass
    k=1
    while k<=2*i-1:#控制打印*号
        print('*',end='')
        k+=1
    print()
    i+=1
    pass
```

**猜年龄小游戏**

> 有三点要求：
> 1.允许用户最多尝试3次
> 2.每尝试3次后，如果还没有猜对，就问用户是否还想继续玩，如果回答Y或y,就让继续猜3次，以此往后，如果回答N或n，就退出程序
> 3.如果猜对了，直接退出

```python
# # import random
#
# # for i in range(3):
#     # computer = random.randint(20, 22)
#     # person = int(input('请输入年龄：'))
#     # if person == computer:
#     #     print('恭喜您答对了')
#     #     break
#     #
#     # else:
#     #     print('猜错了')
#     #     zimu=input('是否继续：继续请输入Y或者y  结束请输入N或n')
#     #     if zimu=='Y' or zimu =='y':
#     #         pass
#     #     elif zimu=='N' or zimu=='n':
#     #         exit()
 time =0
 count=3
 while time<=3:
     age =int(input('请输入您要猜的年龄：'))
     if age ==25:
         print('您猜对了：')
         break
     elif age >25:
         print('您猜大了')#     else:
         print('您猜小了')
     time+=1
     if time==3:
         choose=input('还想继续吗？Y/N')
         if choose == 'Y' or choose=='y':
             time=0#重置为初始值
         elif choose=='N' or choose=='n':
             exit()
         else:
             print('请输入正确的符号..谢谢配合')

```

**小王身高1.75，体重80.5kg,请根据BMI公式，【体重除以身高的平方】，帮助小王计算他的BMI指数，并根据BMI指数**

> 低于18.5 过轻
> 18.5-25 正常
> 25-28 过重
> 28-32 肥胖
> 高于32 严重肥胖
> 用if-elif 判断并打印出来

```python
high=1.75
weight=80.5
BMI=weight/(high*high)
print('BMI的数据是{}'.format(BMI))
if BMI<18.5:
    print('过轻')
elif 18.5<=BMI<25:
    print('正常')
elif 25<=BMI<28:
    print('过重')
elif 28<=BMI<32:
    print('肥胖')
else:
    print('严重肥胖')
## Python数据类型
>掌握Python的基本数据类型
* 数字
  * int(有符号整数)
  * long（长整数）(Python3版本取消了)
  * float（浮点型）
  * complex（复数）
  * bool（布尔值）
    * 返回 True False
* 字符串
* 字典
* 元组
* 列表
```python
a=[]
print(type(a)) #type方法可以查看变量的类型
b=10
print(type(b))
```

---

# 03、Python数据类型

* 序列:一组按顺序排列的值，【数据集合】
* 序列优点：支持索引和切片操作
* 特征:第一个正索引为0，指向的是左端，  第一个索引为负数，指向的是右端
* 切片是指截取字符串中的某一段内容，
* 切片使用语法：[起始下标：结束下标：步长] 切片截取的内容不包含结束下标对应的数据，步长指的是隔几个下标获取一个字符
* 下标会越界，切片不会：

#### 字符串

> **不可变的字符序列**

* **字符串驻留机制：**

仅仅保存一份相同且不可变字符串的方法，不同的值被存放 在字符串的驻留池中，Python的驻留机制对相同的字符串只保留一份拷贝，后续创建相同字符串时，不会开辟新空间，而是把该字符窜的地址赋给新创建的变量

在需要进行字符串**拼接时建议使用str类型的join方法**，而非+，因为join()方法是先计算出所有字符中的长度，然后再拷贝，只new一次对象，效率要比‘+’效率高

```python
a = 'python'
b = "Python"
c = '''Python'''
print(a, id(a))
print(b, id(b))
print(c, id(c))

"""结果如下"""
python 2090039786608
Python 2089748023280
Python 2089748023280
```

* **查询操作的方法**

```python
"""index() rindex()  find()  rfind()"""
s='hello,hello'
print(s.index('lo'))#从头找到的第一次出现的
print(s.find('lo'))#从头找到的第一次出现的
print(s.rindex('lo'))#从头开始找，找最后一次出现的
print(s.rfind('lo'))#从头开始找，找最后一次出现的

# print(s.index('k'))#index()方法找不到会报错
#find()方法找不到会返回-1
print(s.find('k'))


"""结果如下"""
3
3
9
9
-1
```

* 大小写转换

```python
s = 'hello,Python'
q = s.upper()  # 转成大写后，会产生一个新的字符串对象
print(q, id(q))
print(s, id(s))
print(s.lower(), id(s.lower()))  # 转换小写后也会转成一个新的对象
print(s, id(s))
s2 = 'hello,Python'
print(s2.swapcase())  # 所有的大写转小写，所有的小写转大写
print(s2.title())   # 把每个单词的第一个字符转成大写，把每个单词剩余的字符串转成小写
print(s2.capitalize()) # 第一个字符转大写，剩余的单词 转成小写

```

* **字符串内容对齐的操作方法**

| 方法名   | 作用                                                         |
| -------- | ------------------------------------------------------------ |
| center() | 居中对齐，第1个参数指定宽度，第2个参数指定填充符，第2个参数是可选的，默认是空格，如果设置宽度小于实际宽度则返回原字符串 |
| ljust()  | 左对齐，第1个参数指定宽度，第2个参数指定填充符，第2个参数是可选的，默认是空格，如果设置的宽度小于实际的宽度则返回原字符串 |
| rjust()  | 右对齐，第1个参数指定宽度，第2个参数指定填充符，第2个参数是可选的，默认是空格，如果设置的宽度小于实际的宽度则返回原字符串 |
| zfill()  | 右对齐，左边用0填充，该方法返回只接受一个参数，用于指定字符串的宽度，如果指定的宽度小于等于字符串 长度，返回字符串本身 |

```python
print(s2.center(20, '*'))
"""左对齐"""
print(s2.ljust(20, '*'))
print(s2.ljust(20))
"""右对齐"""
print(s2.rjust(20,'*'))
print(s2.rjust(20))
print(s2.rjust(8))
"""右对齐，使用0填充"""
print(s2.zfill(20))
print(s2.zfill(10))
print('-8910'.zfill(8))

"""结果如下"""
****hello,Python****
hello,Python********
hello,Python        
********hello,Python
        hello,Python
hello,Python
00000000hello,Python
hello,Python
-0008910#用0填充至8位

```

* **字符串分割 操作的方法**

**split()**:从字符串的左边开始分割，默认的分隔符时空格字符串，返回的值时一个列表

以通过参数step指定分割字符串的分割数

通过参数maxsplit指定分割字符串时的最大分割次数，在经过了最大分割之后，剩余的子串会单独作为一部分

**rsplit()**:跟上述有一样，只不过是从右边开始分割

```python
s='hello world python'
lst=s.split()
print(lst)
s1='hello|world|python'

print(s1.split(sep='|',maxsplit=1))

"""rsplit()从右侧开始分割"""
print(s.rsplit())
print(s1.rsplit('|',maxsplit=1))

"""结果如下"""
['hello', 'world', 'python']
['hello', 'world|python']
['hello', 'world', 'python']
['hello|world', 'python']
```

* **判断字符串操作方法**

| 方法名         | 作用                                                         |
| -------------- | ------------------------------------------------------------ |
| isidentifier() | 判断指定的字符串是不是合法的标识符                           |
| isspace()      | 判断指定的字符串是否全部由空白字符组成（回车换行，水平指标符） |
| isalpha()      | 判断指定的字符串是否全部由字母组成                           |
| isdecimal()    | 判断指定的字符串是否全部由十进制的数字组成                   |
| isnumeric()    | 判断指定的字符串是否全部由数字组成                           |
| isalnum()      | 判断指定的字符串是否全部由字母和数字组成                     |
| isdigit()      | 判断是否是数字                                               |

print(str.startswith('I'))#返回布尔型 判断是否以什么什么开始
print(str.endswith('i'))#判断是否以什么什么结尾

```python
s='hello,pytohn'
print('1',s.isidentifier())
print('2','hello'.isidentifier())
print('3','\t'.isalpha())
print('4','\t'.isspace())
print('5','python'.isalpha())
print('6','132'.isdecimal())
print('7','132死'.isdecimal())
print('8','456'.isnumeric())
print('9','abc1'.isalnum())
print('10','张三ada1'.isalnum())

"""结果如下"""
1 False
2 True
3 False
4 True
5 True
6 True
7 False
8 True
9 True
10 True
```

* **字符串替换**

| 方法名    | 作用                                                         |
| --------- | ------------------------------------------------------------ |
| replace() | 第一个参数指定被替换的子串，第2个参数指定替换字串的字符串，该方法返回替换后得到的字符串，，替换前的字符串不发生变化，调用该方法时可以通过第3个参数指定最大替换次数 |

```python
s='hello,python'
print(s.replace('python','Java'))
s2='hello python python python'
print(s2.replace('python','Java',2))


"""结果如下"""
hello,Java
hello Java Java python

```

* **字符串比较操作**

```python
"""
运算符：> >=  <  <=   ==   !=
比较规则：首先比较两个字符串中的第一个字符，如果相等则继续往下比较，直到两个字符串中的字符串不相等时，其比较结果就是两个字符串的比较结果，两个字符串中的所有后续字符将不再继续比较

比较原理：
	两个字符进行比较，比较的是(ordinal value)原始值，调用内置函数ord可以得到指定字符的ordinal value。与内置函数ord对应的内置函数是chr，调用内置函数chr时指定ordinal value可以得到其对应的字符
"""
```

```python
print('apple' > 'app')
print('apple' > 'banana')#False 相当于 97>98 返回False
print(ord('a'),ord('b'))


print(ord('张'))
print(chr(24352))
print(chr(97),chr(98))
"""
== 与 is 的区别
    ==比较的是 value
    is比较的是 di是否相等
"""
a=b='python'
c='python'
print(a==b)
print(b==c)
print(a is b)
print(c is b)
print(id(a))
print(id(b))
print(id(c))

"""结果如下"""
True
False
97 98
24352
张
a b
True
True
True
True
2163358525104
2163358525104
2163358525104
```



* **字符串合并**

| 方法名 | 作用                                   |
| ------ | -------------------------------------- |
| join() | 将列表或元组中的字符串合并成一个字符串 |

```python
s=['hello','pytohn','java']
print('|'.join(s))
t=('hello','world')
print('*'.join(t))
print('*'.join('python'))

"""结果如下"""
hello|pytohn|java
hello*world
p*y*t*h*o*n
```

* **格式化字符串3中方法**

**（1）%作占位符**

```python
name='李四'
age=18
print('%s的今年%s岁'%(name,age))
```

**（2）{}作占位符**

```python
name='李四'
age=18
print('{}的今年{}岁'.foramt(name,age))
```

**（3）f-string**

```python
name='李四'
age=18
print(f'{name}的今年{age}岁')
```

**宽度精度的设置**

```python
print("%d"%99)
print('%10d'%100)#10表示宽度
print('%.3f'%3.1415926)#.3表示小数点后三位
print("%10.3f"%3.1415926)#总宽度为10，小数点后有3位
print('{0:.3}'.format(3.1415926))#.3表示一共3位
print('{:.3f}'.format(3.1415926))#.3f表示3位小数
print('{:10.3f}'.format(3.14152949))#同时设置宽度和精度，一共10位，小数点后有3位

"""结果如下"""
99
       100
3.142
     3.142
3.14
3.142
     3.142

```

* **去除空格**

```python
a='         python     '
print(a.strip())#去除两边的空格
print(a.lstrip())#去除左边空格
print(a.rstrip())#去除右边空格
#赋值字符串
b=a
print(b)
print(id(a))#查看内存地址
print(id(b))
```

* **索引切片**

```python
st='hello world'
print(st[0])
print(str[::-1])#逆序输出
print(st[2:5])#从下标2开始不包含5【左闭右开】
print(st[2:])#从第三个字符到最后
print(st[0:3])#从0开始可以省略0
print(st[:3])
```

* **编码与解码**

```python
s = '天涯共此时'
#编码
print(s.encode(encoding='GBK'))  # GBK编码格式中，一个中文占两个字节
print(s.encode(encoding='UTF-8'))  # 在UTF-8编码格式中，一个中文占三个字节
#解码
byte=s.encode(encoding='GBK')
print(byte.decode('GBK'))
```



#### 列表

* 特点：
  '''
  支持增删改查
  列表中的数据是可以变化的，【数据项可以变化，内存地址不会改变】
  用[]来表示列表类型，数据项之间用逗号来分割， 注意：数据项可以说任意的数据类型
  '''

```python
list=[]#空列表
print(list)
print(len(list))#获取列表对象 中数据个数
str='lodsa'
print(len(str))
```

* **查找**

```python
listA=['abcd',123,456,798,True,12.255]
print(listA)
print(listA[0])#找出第一个元素
print(listA[1:3])
print(listA[3:])
print(listA[::-1])#倒叙
print(listA*3)#输出多次列表中数据【复制多次】
```

* **添加**

```python
print('------------------增加---------------------')
listA.append('Jay')
listA.append([1,1,2,'dfgs',{"al":12}])
print(listA)
listA.insert(1,'插入的元素')#插入操作，需要指定插入位置
# reData=list(range(10))#强制转换list对象
listA.extend([12,45,484])#扩展，批量添加
print(listA)
```

* **修改**

```python
print('-----------------修改--------------')
print('修改之前',listA)
listA[0]='Peter'
print('修改之后',listA)
```

* **删除**

```python
print('----------------------删除---------------')
del listA[0] #删除列表中第一个元素
print(listA)
del listA[1:3]#批量删除
print(listA)
listA.remove(798)#指出指定元素
print(listA)
listA.pop(0)#移除第一个元素 #根据下标来移除
print(listA)

```

* **列表生成式**

```python
lst=[i*i for i in range(6)]
print(lst)
```



---

#### 元组

* 元组是**不可变序列**，在创建后不能做任何修改，没有增删改操作
* 用()来创建元组类型，数据项用逗号来分割
* 可以是任何类型
* 当元组中**只有一个元素时，要加上逗号**，不然解释器会当作本身的类型处理
* 同样支持切片操作

```python
t=(10,[10,30,40],9)
print(t)
print(type(t))
print(t[0],type(t[0]),id(t[0]))
print(t[1],type(t[1]),id(t[1]))
print(t[2],type(t[2]),id(t[2]))
#t[1]=100报错，元祖是不允许修改元素的
"""由于[10,30,40]是列表，而列表是可变序列，所以可以向列表中添加爱元素，而列表的内存地址不变"""
t[1].append(100)#向列表中添加元素
print(t,id(t[1]))

"""结果如下"""
#(10, [10, 30, 40], 9)
#<class 'tuple'>
#10 <class 'int'> 2775649288720
#[10, 30, 40] <class 'list'> 2775650645440
#9 <class 'int'> 2775649288688
#(10, [10, 30, 40, 100], 9) 2775650645440

```

* **创建**

```python
"""第一种方式：使用()"""
t=('pytohn','hello',454)
print(type(t))
"""第二种方式创建使用内置函数tuple()"""
t=typle(('hello',45,'world'))
```

* **查找**

```python
tupleA=()#空列表
tupleA=('abcd',123,12.55,[123,45,'45hf'])
print(tupleA)
print(tupleA[2])
print(tupleA[1:3])
print(tupleA[::-1])
print(tupleA[::-2])
print(tupleA[-2:-1:])#倒着取下标， -2到-1区间的数据
print(tupleA[-4:-1])
```

* **对元组中的列表元素进行修改**

```python
tupleA[3][0]=250#对元组中的列表类型的数据进行修改

tupleB=tuple(range(10))
print(tupleB.count(9))
```

* **遍历**

```python
t=('python',20,[10,20,2])
for item in t:
	print(item)
```

---

#### 字典

* 字典不是序列类型，没有下标的概念，是一个**无序**的键值集合，是内置的高级数据类型
* 不可变序列
* 字典可以存储任意对象
* 字典以键值对的形式创建的{key:value}利用大括号包裹
* 字典中某个元素时，是根据键 ，值 字典的每个元素由2部分组成：键：值
* 访问值的安全方法**get()方法**，在我们不确定字典中是否存在某个键而又想获取其值时，可以使用get()方法，还可以设置默认的值
* **字典创建**

```python
"""最常用方式：使用花括号"""
socres={'张三':100,'李四':520,'王五':51}
"""使用内置函数dict()"""
dict(name='jack',age=20)
```

* **获取元素**

如果查找的键不存在，使用dictA[]的方式会报错，

但是使用.get()的方式不会报错，会返回一个None

```python
dictA={"pro":"艺术专业","school":'上海戏剧学院'}
print(dictA['pro'])
print(dictA.get('pro'))
```

* **键的判断**

存在返回True

不存在返回False

```python
dictA={"pro":"艺术专业","school":'上海戏剧学院'}
print('pro' in dictA)
print('pro' not in dictA)
```

* **添加，修改，获取字典数据**

```python
dictA={"pro":"艺术专业","school":'上海戏剧学院'}
dictA['name']='李易峰'
dictA['age']=29
print(dictA)
print(len(dictA))
print(dictA['name'])#通过键获取值
dictA['name']='成龙'#修改键对应的值
print(dictA)
print(dictA.keys())#获取所有的键
print(dictA.values())#获取所有的值
print(dictA.items())#获取所有的键和值
for key,value in dictA.items():
    # print(item)
    print('%s值==%s'%(key,value))

dictA.update({'age':32})#更新  存在就更新，不存在就添加
dictA.update({"height":1.78})#添加
print(dictA)

```

* **删除**

```python
del dictA['name']
dictA.pop('age')
print(dictA)
dictA.clear()#清空字典元素
```

* **内置函数zip()**：
  * 用于将可迭代的对象作为参数，将对象中对应的元素打包成一个元组，然后返回由这些元组组成的列表

```python
items = ['Fruit', 'Books', 'Others']
prices = [96, 88, 65]
lst=zip(items,prices)
print(list(lst))
```



* **字典生成式**

  

```python
items = ['Fruit', 'Books', 'Others']
prices = [96, 88, 65]
d = {item.upper(): price for item, price in zip(items, prices)}
print(d)

```



---

#### 共有方法

```python
#+  合并
strA='人生苦短'
strB='我用Python'
print(strA+strB)
List_A=list(range(10))
list_B=list(range(10,21))
print(List_A+list_B)
#  * 复制
print(strA*3,end=' ')
print(List_A*3)

#  in 判断对象是否存在
print('我' in strA)
print(10 in  list_B)
```

#### 不可变序列与可变序列

不可变序列：字符串、元组

* 不可变序列：没有增、删、改操作，



```python
"""不可变：元组，字符串"""
s='hello'
print(id(s))#2213691636720
s=s+'world'
print(id(s))#2213691680432

"""对象地址发生改变所以是不可变序列"""
```

可变序列：列表、字典

* 可变序列：可以对序列执行增、删、改操作，**对象地址不发生改变**

```python
"""可变：列表，字典"""
lst=[10,20]
print(id(lst))#2250871125440
lst.append(100)
print(id(lst))#2250871125440
"""
添加元素后对象地址不变，所以是可变序列
"""
```

#### set集合

* set 不支持索引切片，是一个**无序**的且**不重复**的容器
* 类似于字典，但是只有Key,没有value
* 与列表、字典一样都属于可变类型的序列

* 创建

```python
"""第一种方式使用{}"""
t={20,20,202,30,202,70}
print(t)#{202, 20, 70, 30},集合中的元素不允许重复

"""第二种方式set()"""
s=set(tange(6))
print(s)
s2=set([1,2,3,4,5,6,8,7])
print(s2)
s3=set((1,2,3,4,5,6,8,7)
print(s3)
s4=set('pytohn')
print(s4)
```

* **判断操作**

```python
"""in     not in """
s={10,20,111,220,12020,1515}
print(10 in  s)
print(7 not in s)
```

* **添加操作**

| 方法名   | 作用                 |
| -------- | -------------------- |
| add()    | 一次添加一个元素     |
| update() | 一次至少添加一个元素 |




```python
"""add()  update()"""
s={10,20,11,22,12}
s.add(80)#一次添加一个元素
print(s)
s.update({500,800,8080})#一次至少添加一个元素
print(s)
s.update([100,200,5056])
s.update((78,99,46))
print(s)
```

* **删除操作**

| 方法名    | 作用                                                   |
| --------- | ------------------------------------------------------ |
| remove()  | 一次删除一个指定元素，如果元素不存在，抛出异常KeyError |
| discard() | 一次删除一个指定元素，如果元素不存在，不抛出异常       |
| pop()     | 一次只删除一个任意元素                                 |
| clear()   | 清空集合                                               |




```python
"""remove()  discard()   pop()  clear()"""
s.remove(100000)#KeyError
print(s)
s.discard(1)
s.pop()#随机删除元素,不能指定元素
s.clear()
```

* **两个集合是否相等**

```python
s={10,20,30,40,50}
s2={10,30,40,20,50}
print(s1 == s2)#True
print(s1 != s2)#False
```

* **一个集合是否是另一个集合的子集**

```python
s1={10,20,30,40,50,60}
s2={10,20,30,40}
s3={10,20,80}
print(s2.issubset(f1))#True
print(s3.issubset(s1))#False
```

* **一个集合是否是另一个集合的超集**

```python
print(s1.issuperset(s2))#True
print(s1.issuperset(s3))#False
```

* **两个集合是否含有交集**

```python
print(s2.isdisjoint(s3))#False
有交集为False
```

* **集合的数学操作**

  * **交集**

  ```python
  s1={10,20,30,40}
  s2={20,30,50,40,60}
  print(s1.intersection(s2))
  print(s1 & s2)
  ```

  * **并集**

  ```python
  print(s1.union(s2))
  print(s1 | s2)
  ```

  * **差集**

  ```python
  print(s1.difference(s2))#a中有的b中没有的
  print(s1 - s2)
  ```

  * **对称差集**

  ```python
  print(s1.symmetric_difference(c2))
  print(s1 ^ s2)
  ```

  

* **集合生成式**

```python
"""将{}修改为[]就是列表生成式"""
```

没有元组生成式

```python
s={i*i for i in range(6)}
print(s)
```



#### 总结

|  数据结构   | 是否可变 |         是否重复          | 是否有序 |  定义符号   |
| :---------: | :------: | :-----------------------: | -------- | :---------: |
| 列表(list)  |   可变   |          可重复           | 有序     |     []      |
| 元组(tuple) |  不可变  |          可重复           | 有序     |     ()      |
| 字典(dict)  |   可变   | key不可重复，value 可重复 | 无序     | {key:value} |
|  集合(set)  |   可变   |         不可重复          | 无序     |     {}      |
| 字符串(str) |  不可变  |          可重复           | 有序     |    ''/""    |

***



# 04、Python函数

#### 4.1、函数初识

* 在编写程序的过程中，有某一功能代码块出现多次，但是为了提高编写的效率以及代码的重用，所以把具有独立功能的代码块组织为一个小模块，这就是函数
* 就是一系列Python语句的组合，可以在程序中运行一次或者多次，
* 一般具有独立的功能
* 代码的复用最大化以及最小化冗余代码，整体代码结构清晰，问题局部化
* 语法结构

```python
def 函数名(参数1，参数2，参数3):
    """文档说明"""
    函数体
    return 值
```



* 函数定义

```print
def pritInfo():
    '''
    这个函数用来打印小张信息
    '''
    #函数代码块
    print('小张的身高是%f'%1.73)
    print('小张的体重是%f'%130)
    print('小张的爱好是%s'%'周杰伦')
    print('小张的专业是%s'%'信息安全')
```

定义函数发生的事情：

1. 申请内存空间保存函数体代码
2. 将上述代码地址绑定给函数名
3. 定义函数不会执行函数体代码，但是会检测函数体语法

* 函数 调用

```python
print(pritInfo())#打印的是内存地址
pritInfo()
```

调用函数发生的事情：

1. 通过函数名找到函数的内存地址
2. 然后加括号就是在触发函数体代码的执行

```python
def foo():
    bar()
    print('from foo')
def bar():
    print('from bar')
    
foo()
"""执行过程"""
函数的使用分两个 阶段，
第一个阶段：定义
第二个阶段：调用
定义的时候只检测语法，不执行代码
在定义foo时咩有语法错误
然后再去定义bar，
最后再去调用foo()时，bar早就丢到内存中了，所以就不会报错，

```

```python
def foo():
    bar()
    print('from foo')
    
foo()

def bar():
    print('from bar')
    
"""会报错"""
	
```



* 进一步去输出不同人的信息，  通过传入参数来解决

```python
def pritInfo(name,height,weight,hobby,pro):

    #函数代码块
    print('%s的身高是%f'%(name,height))
    print('%s的体重是%f'%(name,weight))
    print('%s的爱好是%s'%(name,hobby))
    print('%s的专业是%s'%(name,pro))
```

* 调用带参数的信息

```python
pritInfo('peter',175,130,'听音乐','甜品师')
pritInfo('小李',189,140,'打游戏','软件工程师')
```

---

#### 4.2、函数参数

* 参数分类：

> 必选参数，默认参数【缺省参数】，可选参数，关键字参数

* 参数：其实就是函数为了实现某项特定功能，进而为了得到实现功能所需要的数据



  

```python

def sum(a,b):#形式参数：只是意义上的一种参数，在定义的时候是不占内存地址的

sum=a+b
print(sum)
```

* **必选参数** （位置参数）

> 函数调用的时候，必选参数是必须要赋值的
> sum(20,15)#20,15就是实际参数  实参，  实实在在的参数，是实际占用内存地址的
> sum()不能这样写，需要传参

位置实参：在函数嗲用阶段，按照从左往右的顺序依次传入值

**位置实参必须放在关键字实参前，否则报语法错误**

在调用阶段，实参（变量值）会绑定给形参（变量名）

实参与形参的绑定关系：在函数调用时生效，调用结束后解除绑定关系

* **默认参数**【缺省参数】（默认形参）

在定义函数阶段，就已经被赋值的形参，称之为默认形参

特点**：在定义阶段就已经被赋值，**意味着在调用阶段可以不用为其赋值

位置形参必须在默认形参的左边

```python
def sum(a=1,b=11):
     print(a+b)

sum()#在调用的时候，如果未赋值，就用定义函数时给定的默认值
sum(10)#10赋给了a,b还是11,结果为21
```



* **关键字参数**

关键字实参：在函数调用阶段，按照key=value的形式传入值

**位置实参必须放在关键字实参前，否则报语法错误**

指名道姓的给某个形参传值，

```python
def a(a,b):
    res=a+b
    return res
print(a(b=2,a=1))
```

* **可变参数**（当参数的个数不确定时，比较灵活）

`*形参名`：用来接受溢出的位置实参，溢出的位置实参会被`*`保存成**元组形式**



```python
def getComputer(a,b*args):
    '''
    计算累加和
    '''
    # print(args)
    result=0
    for item in args:
        result+=item
    print(result)

getComputer(1,2,3,4)

"""结果如下"""
1 2 (3,4)
```

* *可用在实参中，实参中带`*`,把`*`后的值打散成位置实参

```python
def func(x,u,z):
    print(x,u,z)
func(*[11,22,33])#相当于func(11,22,33)

def func2(x,y,*args):
    print(x,y,args)
func2(*'hello')
"""结果如下"""
11 22 33
h e ('l','l','o')
```

* 形参实参都带*

```python
def func(x,y,*args):#args=(3,4,5,6)
    print(x,y,args)
func(1,2*[3,4,5,6])#相当于func(1,2,3,4,5,6)

"""结果如下"""
1 2 (3,4,5,6)
```



* **关键字可变参数**

1. **来定义
2. 在函数体内 参数关键字是一个字典类型，key是一个字符串
3. 可变参数指的是在调用函数的时候，传入的值（实参）的个数不固定

```python
def Func(a,b**kwargs):
    print(a,b,kwargs)
Func(1,b=2,x=3,b=5)

def Infofunc(*args,**kwargs): #可选参数必须在关键字可选参数之前
     print(args)
     print(kwargs)
 Infofunc(1,2)
# Infofunc(12,13,46,age=18)
```

* **可用在实参中，实参中带`**`,把`**`后的值打散成关键字实参

```python
def func(x,y,c):
    print(x,y,c)
#func(*{'x':1,'y':2,'c':3})#func('x','y','c')
func(**{'x':1,'y':2,'c':3})#func(x=1,y=2,c=3)


"""结果如下"""
1 2 3
```

* 形参与实参都带**

```python
def func(x,y,**kwargs):
    print(x,y,kwargs)
func(**{'y':111,'x':211,'z':456,'d':789})#打散后相当于func(y=222,x=211,z=456,d=789)

"""结果如下"""
211 111 {'z':456,'d':789}
```

* 混用*与**：`*args`必须在`**kwargs`前面

```python
def fun(*args,**kwargs):
    print(args)
    print(kwargs)
fun(1,2,3,4,5,6,8,x=1,y=2,z=3)

"""结果如下"""
(1,2,3,4,5,6,8)
{'x':1,'y':2,'z':3}
```

* **命名关键字参数（了解）**

```python
def func(x,y,*,a,b):#a和b为命名关键字参数
    print(x,y)
    print(a,b)
```





**案例实现**

* 接收N 个数字，求这些参数数字的和

```python
def num(*args):
    '''
    #写函数，接收N 个数字，求这些参数数字的和
    '''
    result=0
    for  i in args:
        result+=i

    return result
a=num(1,2,2,2,2)
print(a)


```

* 找出传入的列表或元组的奇数位对应的元素，并返回一个新的列表

```python
def Func1(con):
    '''
    找出传入的列表或元组的奇数位对应的元素，并返回一个新的列表
    '''
    listA=[]
    index=1
    for i in con:
        if index%2==1:#判断奇数位
            listA.append(i)
        index+=1
    return listA
# e=Func1([1,2,45,1,1,'as',{1,25,',hs'}])
e=Func1(list(range(1,11)))
print(e)
```

* 检查传入字典的每一个value的长度，如果大于2，那么仅仅保留前两个长度的内容，并将新的内容返回给调用者，PS:字典中的数据只能是字符串或者是列表

```python
def Func2(dic):
    '''
    检查传入字典的每一个value的长度，如果大于2，那么仅仅保留前两个长度的内容，
    并将新的内容返回给调用者，PS:字典中的数据只能是字符串或者是列表
    '''
    result={}
    for key,value in dic.items():
        if len(value)>2:
            result[key]=value[:2]#向字典添加新的数据
        else:
            result[key]=value

    return result
#调用
dicObj={'name':'张三','hobby':['唱歌','跳舞','编程'],'major':'信息安全'}
print(Func2(dicObj))
```

---

#### 4.3、函数返回值

* 概念：函数执行完以后会返回一个对象，如果在函数的内部有return，就可以返回实际的值，否则返回None
* 类型：可以返回任意 类型，返回值类型应该取决于return 后面的类型
* 用途：给调用方返回数据
* 在一个函数体内可以出现多个return ，但是肯定只有一个return
* 如果在一个函数体内，执行了return,意味着就执行完成推出了，return后面的代码将不会执行

```python
#返回值

def Sum(a,b):
    sum=a+b
    return sum #将计算结果返回
result=Sum(12,15)#将返回的值赋给其他变量
print(result)

def calComputer(num):
    '''
    累加和
    '''
    listA=[]
    result=0
    i=1
    while i<=num:
        result+=i
        i+=1
        # listA.append(result)
    listA.append(result)
    return listA
message=calComputer(10)
print(type(message))

print(message)


def returnType():
    '''
    返回元组类型数据
    '''
    return 1,2,3
a =returnType()
print(type(a))
print(a)

def returnType1():
    '''
    返回字典类型
    '''
    return {"name":"成龙"}
b=returnType1()
print(type(b))
print(b)
```

---

#### 4.4、 名称空间与作用域

**（1）名称空间（namespaces）**

用来存放名字的地方是对栈区的划分

有了名称空间，就可在栈区中放相同的名字

* **1.1、内置名称空间**

==存放的名字==：是Python解释器内置的名字

```python
"""
交互模式下输入：
>>> input
<built-in function input>
>>> print
<built-in function print>
>>>

"""
```

==存活周期==：Python解释器启动则产生，Python解释器关闭则销毁



* **1.2、全局名称空间**

==存放的名字==：只要不是函数内定义的，也不是内置的，剩下的都是全局名名称空间

==存活周期==:Python文件执行则产生，Python文件运行完毕则销毁

* **1.3、局部名称空间**

==存放的名字==:在调用函数时，运行函数体代码过程中产生的函数内的名字

==存活周期==：在函数调用时存活，函数调用完毕后则销毁

* **1.4、名称空间加载顺序**

```python
"""
内置名称空间》全局名称空间》局部名称空间
"""
```

* **1.5、销毁顺序**

```python
"""
局部名称空间》全局名称空间》内置名称空间
"""
```

* **1.6、名字查找优先级**

```python
"""当前所在的位置向上一层一层查找"""

"""
如果当前在局部名称空间：
	局部名称空间》全局名称空间》内置名称空间

如果当前在全局名称空间：
	全局名称空间》内置名称空间
"""
```

**示范1：**

```python
def func():
	print(x)
x=111
func()

"""结果如下"""
111
```

**示范2**：名称空间的‘’嵌套‘’关系是==以函数定义阶段为准==，与调用位置无关

```python
x=1
def func():
    print(x)
    
def foo():
    x=222
    func()
foo()

"""结果如下"""
1
```

**示范3**：函数嵌套定义

```python
input=111
def f1():
    input=222
    def f2():
        input=333
        print(input)
    f2()
f1()
```

**示范4**：

```python
x=111
def fun():
    print(x)
    x=222
fun()
"""结果报错"""
```

**（2）作用域**

> 作用范围

* **2.1、 全局作用域**

全局作用域：内置名称空间。全局名称空间

```python
"""
全局存活
全局有效：被所有函数共享
"""
```

* **2.2、局部作用域**

局部作用域：局部名称空间的名字

```python
"""
临时存活
局部有效
"""
```

* **2.3、作用域与名字查找的优先级**

在局部作用域查找名字时，起始位置是局部作用域，所以先查找局部名称空间，没有找到，再去全局作用域查找：先查找全局名称空间，没有找到，再查找内置名称空间，最后都没有找到就会抛出异常

```python
x=100 #全局作用域的名字x
def foo():
    x=300 #局部作用域的名字x
    print(x) #在局部找x
foo()#结果为300
```

在全局作用域查找名字时，起始位置便是全局作用域，所以先查找全局名称空间，没有找到，再查找内置名称空间，最后都没有找到就会抛出异常

```python
x=100
def foo():
    x=300 #在函数调用时产生局部作用域的名字x
foo()
print(x) #在全局找x,结果为100
```

在函数内，无论嵌套多少层，都可以查看到全局作用域的名字，若要在函数内修改全局名称空间中名字的值，当值为不可变类型时，则需要用到==global==关键字

```python
x=1
def foo():
    global x #声明x为全局名称空间的名字
    x=2
foo()
print(x) #结果为2
```

当实参的值为可变类型时，函数体内对该值的修改将直接反应到原值

```python
num_list=[1,2,3]
def foo(nums):
    nums.append(5)

foo(num_list)
print(num_list)
#结果为
[1, 2, 3, 5]
```

对于嵌套多层的函数，使用==nonlocal==关键字可以将名字声明为来自外部嵌套函数定义的作用域（非全局）

```python
def  f1():
    x=2
    def f2():
        nonlocal x
        x=3
    f2() #调用f2(),修改f1作用域中名字x的值
    print(x) #在f1作用域查看x

f1()

#结果
3
```

nonlocal x会从当前函数的外层函数开始一层层去查找名字x，若是一直到最外层函数都找不到，则会抛出异常。

#### 4.5、函数嵌套

```python
#函数分类：根据函数返回值和函数参数决定
#有参数无返回值
#有参数有返回值
#无参数无返回值
#无参数有返回值
```

* **函数的嵌套调用**

```python
def max2(x,y):
    if x>y:
        return x
    else:
        return y
 def max4(a,b,c,d):
    #第一步：比较a,b，得到res1
    res1=max2(a,b)
    #第二部：比较res1,c得到res2
    res2=max2(res1,c)
    #第三步：比较res2,d得到res3
    res2=max2(res2,d)
    return res2
res=max4(1,2,3,4)
print(res)
```

* 函数的嵌套定义

```python
def f1():
    def f2():
        pass
```



---

#### 4.6、函数全局变量

* 局部变量：就是在函数内部定义的变量【作用域仅仅局限在函数的内部】
* 不同的函数，可以定义相同的局部变量，但是各自用各自的 不会产生影响.
* 局部变量的作用：为了临时的保存数据，需要在函数中定义来进行存储.
* 当全局变量和局部变量出现重复定义的时候，程序会优先执行函数定义的内部变量.
* 如果函数内部要想对全局变量进行修改，必须使用global关键字进行声明.

```python
# pro='信息安全'#全局变量
# name='周润发'
# def printInfo():
#     name='刘德华'#局部变量
#     print('{}'.format(name))
#
# def TestMethod():
#     name='peter'#局部变量
#     print(name)
# printInfo()
# TestMethod()
#
#
# def message():
#     print(pro)
#
# message()
#
pro='123'
def changGlobal():
    global pro
    pro='456'
    print(pro)
changGlobal()
```

---

#### 4.7、引用

* Python中所有值的传递，传递都不是值本身，而是值的引用，即内存地址
* 在Python中，值是靠引用来传递来的，可以用id()查看一个对象的引用是否相同.
* id是值保存在内存中那块内存地址的标识.
* **不可变类型**

```python
a=1
def func(x):
    print('x的地址{}'.format(id(x)))
    x=12
    print('修改之后的x 的地址:{}'.format(id(x)))

print('a的地址：{}'.format(id(a)))
func(a)

```

* **可变类型**

```python
li=[]
def testRenc(parms):
    print(id(parms))
    li.append([1,2,3,54])
print(id(li))
testRenc(li)
print('外部变量对象{}'.format(li))

```

* 小结

1. python中，万物皆对象，在函数调用的时候，实参传递的就是对象的引用，
2. 了解了原理之后，就可以更好的去把控，在函数内部的处理是否会影响到外部数据的变化
3. 参数传递是通过对象引用来完成  参数传递是通过对象引用来完成   参数传递是通过对象引用来完成

---

#### 4.8、函数对象和闭包

**函数对象**

可以把函数当成变量去用

func=内存地址

```python
def func():
    print('from func')

#1.可以赋值
f=func
print(f,func)

#2.可以当作参数传入
def foo(x):
    print(x)
foo(func)#foo(func的内存地址)

#3.把函数当作另外一个函数的返回值
def foo2(x):#x=func的内存地址
    return x#return func的内存地址
res=foo2(func)#foo2(func的内存地址)
print(res)

#4.当作容器类型的一个元素
#l=[func,]
#l[0]

dic={'k1':func}
print(dic)
dic['k1']()


"""结果如下"""
<function func at 0x0000018533F7FEB0> <function func at 0x0000018533F7FEB0>
<function func at 0x0000018533F7FEB0>
<function func at 0x0000018533F7FEB0>
{'k1': <function func at 0x0000018533F7FEB0>}
from func
```

**案例：**

```python
def login():
    print('登录')


def transfer():
    print('转账')


def check_balance():
    print('查询余额')


def withdraw():
    print('提现')

def register():
    print('注册')
func_dic = {
    
    '1': login,
    '2': transfer,
    '3': check_balance,
    '4': withdraw,
    '5':register
}
# func_dic['1']()
while True:
    print("""
    0 退出
    1 登录
    2 转账
    3 查询余额
    4 提现
    5 注册
    """)
    choice = input('请输入编号：').strip()
    if not choice.isdigit():
        print('请输入正确的编号')
        continue
    if choice == '0':
        break

    if choice in func_dic:
        func_dic[choice]()
    else:
        print('输入的编号不正确')

    # if choice == '1':
    #     login()
    # elif choice == '2':
    #     transfer()
    # elif choice == '3':
    #     check_balance()
    # elif choice == '4':
    #     withdraw()
    # else:
    #     print('输入的编号不正确')

```

**案例改进:**

```python
def login():
    print('登录')


def transfer():
    print('转账')


def check_balance():
    print('查询余额')


def withdraw():
    print('提现')


def register():
    print('注册')


func_dic = {
    '0': ['退出', None],
    '1': ['登录', login],
    '2': ['转账', transfer],
    '3': ['查询余额', check_balance],
    '4': ['提现', withdraw],
    '5': ['注册', register]
}
# func_dic['1']()
while True:
    for i in func_dic:
        print(i,func_dic[i][0])
    choice = input('请输入编号：').strip()
    if not choice.isdigit():
        print('请输入正确的编号')
        continue
    if choice == '0':
        break

    if choice in func_dic:
        func_dic[choice][1]()
    else:
        print('输入的编号不正确')

    # if choice == '1':
    #     login()
    # elif choice == '2':
    #     transfer()
    # elif choice == '3':
    #     check_balance()
    # elif choice == '4':
    #     withdraw()
    # else:
    #     print('输入的编号不正确')

```

---

**闭包函数**
> 1. 嵌套函数	
> 2. 内部函数引用了外部函数的变量
> 	3. 返回值是内部函数





闭包函数=名称空间与作用域+函数嵌套+函数对象

==核心点：名字的查找关系是以函数定义阶段为准==

* ''闭''函数指的是该函数的内嵌函数

```python
def f1():
    def f2():
        pass
```

* ''包''函数指的是该函数对外层函数作用域名字的引用（不是对全局作用域）

```python
"""闭包函数:名称空间与作用域+函数嵌套"""
def f1():
    x=1
    def f2():
      print(x)
    f2()
x=1111
def bar ():
    x=4444
    f1()
def foo():
    x=22222
    bar()
foo()
```

```python
"""闭包函数：函数对象"""
def f1():
    x = 333333333

    def f2():
        print(x)

    return f2#相当于把 f2丢到全局，


f = f1()#f的内存地址就是f2的内存地址
print(f)


def foo():
    x = 5555
    f()


foo()

   
"""结果如下"""
<function f1.<locals>.f2 at 0x0000013F2D2EA950>
333333333
```



---

#### 4.9、装饰器

**（1）什么是装饰器：**

* 器指的是工具，可以定义成函数

* 装饰指的是为其他事务添加额外的东西来点缀

上面两者合到一起：

* 装饰器指的是定义一个函数，该函数用来为其他函数添加额外的功能

函数装饰器分为：

* 无参装饰器和有参装饰两种，二者的实现原理一样，都是’函数嵌套+闭包+函数对象’的组合使用的产物。

**（2）为何要用装饰器**

开放封闭原则

开放：指的是对扩展功能是开放的

封闭：指的是对修改源代码是封闭的

**装饰器就是在不修改被装饰器对象的源代码以及调用方式的前提下，为被装饰对象添加新功能** 

 **（3）装饰器实现思路**

**无参装饰器**

* 方案一：失败

没有修改被装饰对象的调用方式，但是改变了源代码

```python
def index(x, y):
    start = time.time()
    time.sleep(2)
    print(' index %s %s  ' % (x, y))
    end = time.time()
    print(end - start)

index(1, 2)
# index()
```

* 方案二

```python
# 问题：没有修改被装修饰对象的源代码，也灭有修改调用方式，但是代码冗余
def index(x, y):
    time.sleep(2)
    print(' index %s %s  ' % (x, y))


start = time.time()
index(11, 22)
end = time.time()

start = time.time()
index(11, 22)
end = time.time()
```

* 方案三

 问题:解决了代码荣誉,但是函数的调用方式改变了

```python
def index(x, y):
    time.sleep(2)
    print(' index %s %s  ' % (x, y))


def wrapper():
    start = time.time()
    index(11, 22)
    end = time.time()


wrapper()
wrapper()
wrapper()
```

* 方案三的优化一: 

在方案三基础上优化代码：将Index写活了(参数写活了)

```python
def index(x, y,z):
    time.sleep(2)
    print(' index %s %s %s ' % (x, y,z))



def wrapper(*args,**kwargs):
    start=time.time()
    index(*args,**kwargs)
    stop=time.time()
    print(stop-start)
wrapper(11,22,44)
wrapper(111,y=111,z=4545)
```

* 方案三的优化二:

* 在优化一的基础上把被装饰对象写活,原来只能装饰Index

```python
def index(x, y, z):
    time.sleep(2)
    print(' index %s %s %s ' % (x, y, z))


def home(name):
    time.time()
    print('welcome %s to home page' % name)


def outer(func):  # func=index的内存地址
    # func = index
    def wrapper(*args, **kwargs):
        start = time.time()
        func(*args, **kwargs)  # index的内存地址()
        stop = time.time()
        print(stop - start)

    return wrapper


home=outer(home)
home('zhao')

# f = outer(index)  # f=outer(index的内存地址)
# f(x=1, y=2, z=3)
index = outer(index)  # 偷梁换柱.>>此时的index指向的是wrapper的内存地址
print(index)
index(x=1, y=2, z=3)
```

* 方案三的优化三

  将wrapper做的跟被装饰器一摸一样,以假乱真

```python
def index(x, y, z):
    time.sleep(2)
    print(' index %s %s %s ' % (x, y, z))


def home(name):
    time.time()
    print('welcome %s to home page' % name)
    return 1234


def outer(func):
    def wrapper(*args, **kwargs):
        start = time.time()
        res = func(*args, **kwargs)
        stop = time.time()
        print(stop - start)
        return res

    return wrapper


home = outer(home)
res = home('zhao')
print('返回值:>>>', res)
```

* 语法糖:

  在被装饰对象正上方的单独一行写 @装饰器名字

```python
# 装饰器
def timmer(func):
    def wrapper(*args, **kwargs):
        start = time.time()
        res = func(*args, **kwargs)
        stop = time.time()
        print(stop - start)
        return res

    return wrapper


@timmer  # index=timmer(index)
def index(x, y, z):
    time.sleep(2)
    print(' index %s %s %s ' % (x, y, z))


@timmer  # home = timmer(home)
def home(name):
    time.sleep(2)
    print('welcome %s to home page' % name)
    return 1234



index(x=1,y=2,z=3)
home('zhao')
```

* 偷梁换柱

即将原函数名指向的内存地址偷梁换柱,所以应该将wrapper做的跟原函数一样才行

手动的将原函数的属性值赋值给wrapper，需要一个一个的去加，太麻烦

```python
def outter(func):
   
    def wrapper(*args, **kwargs):
        #手动的将原函数的属性值赋值给wrapper
        # 函数名wrapper.__name__ =原函数.__name__
        # 函数名wrapper.__doc__ = 原函数.__doc__
        wrapper.__name__ = func.__name__
        wrapper.__doc__ = func.__doc__
        res = func(*args, **kwargs)
        return res

    return wrapper


@outter  # index=outter(index)
def index(x, y):
    """

    :param x:
    :param y:
    :return:
    """
    print('index:', x, y)


index(1, 2)
print(index.__name__)
print(index.__doc__)  # help(index)

```

自动的将原函数的所有属性值赋值给wrapper

```python
from functools import  wraps
def outter(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        #手动的将原函数的属性值赋值给wrapper
        # 函数名wrapper.__name__ =原函数.__name__
        # 函数名wrapper.__doc__ = 原函数.__doc__
        # wrapper.__name__ = func.__name__
        # wrapper.__doc__ = func.__doc__
        res = func(*args, **kwargs)
        return res

    return wrapper


@outter  # index=outter(index)
def index(x, y):
    """

    :param x:
    :param y:
    :return:
    """
    print('index:', x, y)


index(1, 2)
print(index.__name__)
print(index.__doc__)  # help(index)
```

* **无参装饰器模板**

```python
def outter(func):
    def wrapper(*args,**kwargs):
        res=func(*args,**kwargs)
        return  res
    return wrapper()
```

* 统计时间的装饰器

```python
def timmer(func):
    def wrapper(*args,**kwargs):
        start=time.time()
        res=func(*args,**kwargs)
        end=time.time()
        print(end-start)
        return  res
    return wrapper()
```



* 认证功能

```python
def auth(func):
    def wrapper(*args, **kwargs):
        name = input('your name :').strip()
        passwd = input('your password:').strip()
        if name == 'zhao' and passwd == '132':
            res = func(*args, **kwargs)
            return res
        else:
            print("your name or your password is error")

    return wrapper


@auth
def index():
    print('from index')


index()

```

* **有参装饰器模板**

```python
def 有参装饰器(x,y,z)
    def outter(func):
        def wrapper(*args,**kwargs):
            res=func(*args,**kwargs)
            return  res
        return wrapper()
@有参装饰器(1,y=2,z=3)
def 被装饰对象():
    pass
```

* 认证功能改进

```python
def auth(db_type):
    def deco(func):
        def wrapper(*args, **kwargs):
            username = input('your name:').strip()
            password = input('your paddword').strip()
            if db_type == 'file':
                print('基于文件验证')
                if username == 'zhao' and password == '133':
                    print('login successful')
                    res = func(*args, **kwargs)
                    return res
                else:
                    print('username or password  is error')
            elif db_type == 'mysql':
                print("基于数据库")
            elif db_type == 'ldap':
                print("基于ldap")
            else:
                print('不支持该db_type')

        return wrapper
    return deco


@auth(db_type='file')#@deco #index=dexo(index)
def index(x, y):
    print('index:>>%s %s' % (x, y))


@auth(db_type='mysql')
def home(name):
    print('home :>>%s' % name)


@auth(db_type='ldap')
def transfer():
    print("transfer:>>>%s" % transfer)


index(1, 2)
home('zhao')
transfer()
```

* 叠加多个装饰器分析

```python
def deco1(func1):  # func1=wrapper2的内存地址
    def wrapper1(*args, **kwargs):
        print('deco1.wrapper1')
        res1 = func1(*args, **kwargs)
        return res1

    return wrapper1


def deco2(func2):  # func2=wrapper3的内存地址
    def wrapper2(*args, **kwargs):
        print('deco1.wrapper2')
        res2 = func2(*args, **kwargs)
        return res2

    return wrapper2


def deco3(x):
    def outter(func3):  # func3=被装饰对象index函数的内存地址
        def wrapper3(*args, **kwargs):
            print('deco3.outter.wrapper3')
            res3 = func3(*args, **kwargs)
            return res3

        return wrapper3

    return outter


# 加载顺序：自下而上
@deco1  # index=deco1(wrapper2的内存地址)    ===》index=wrapper1的内存地址
@deco2  # index=deco2(wrapper3的内存地址）===》index=wrapper2的内存地址
@deco3(11)  # @outer===>@index=outer(index)===>index=wrapper3的内存地址
def index(x, y):
    print('from index %s %s' % (x, y))


print(index)

# 执行顺序：自上而下即 wrapper1>wrapper2>wrapper3
#
index(1, 2)  # wrapper1(1,2)

```



#### 4.10、迭代器

* 迭代器即用来迭代取值的工具，而迭代是重复反馈过程的活动，其目的通常是为了逼近所需的目标或结果，每一次对过程的重复称为一次“迭代”，而每一次迭代得到的结果会作为下一次迭代的初始值,单纯的重复并不是迭代

* 迭代器是用来迭代取值的工具，而涉及到把多个值循环取出来的类型有：列表、字符串、元组、字典、集合、打开文件

  

```python
l = ['zhao', 'lisi', 'zhangsan ']
i = 0
while i < len(l):
    print(l[i])
    i += 1
"""此迭代取值的方式只适用于有索引的数据类型，列表，字符串....."""
```

**而迭代器是不依赖于索引的**

**可迭代对象**：但凡内置有`__iter__`的方法的都称之为可迭代对象

通过索引的方式进行迭代取值，实现简单，但仅适用于序列类型：字符串，列表，元组。对于没有索引的字典、集合等非序列类型，必须找到一种不依赖索引来进行迭代取值的方式，这就用到了迭代器。

要想了解迭代器为何物，必须事先搞清楚一个很重要的概念：可迭代对象(Iterable)。从语法形式上讲，内置有__iter__方法的对象都是可迭代对象，字符串、列表、元组、字典、集合、打开的文件都是可迭代对象：

`可迭代对象.__iter__()`:得到迭代器对象

```python
str1=''
#str1.__iter__()
l=[]
#l.__iter__()
t=(1,)
#t.__iter__()
d={'a':1}
#d.__iter__()
set1={1,2,3}
#set1.__iter__()
with open('a.txt','w') as f:
   # f.__iter__()
	pass

```

迭代器对象：内置有`__next__`方法并且内置有`__iter__`方法的对象

目前只有文件既是可迭代对象，又是迭代器对象

`迭代器对象.__next__()`:得到迭代器的下一个值

`迭代器对象.__iter__()`:得到迭代器的本身，说白了调了就跟没调一样

```python
d = {'a': 1, 'b': 2, 'c': 3}
d_iterator = d.__iter__()
print(d_iterator.__next__())
print(d_iterator.__iter__())
print(d_iterator is d_iterator.__iter__())#True

```

**while循环**

```python
d = {'a': 1, 'b': 2, 'c': 3}
d_iterator = d.__iter__()
while True:
    try:
        print(d_iterator.__next__())
    except StopIteration :
        break

```

**for循环工作原理**

for循环又称为迭代循环，in后可以跟任意可迭代对象

```python
d = {'a': 1, 'b': 2, 'c': 3}
d_iterator = d.__iter__()

for i in d:#d本身就是可迭代对象，调用d.__iter__()后返回的还是它本身
    print(i)
```

1. `d.__iter__()`得到一个迭代器对象
2. `迭代器对象.__next__()`拿到一个返回值，然后该返回值赋值给i
3. 循环往复步骤2,直到抛出StopIteration异常for循环会捕捉异常然后结束迭代。

可迭代对象不是迭代器对象

迭代器对象是可迭代对象

#### 4.11、生成器

生成器就是迭代器

```python
def fun():
    print('第一次')
    yield 1
    print('第二次')
    yield 2
g=fun()
print(g)
print(g.__iter__())
#print(g.__next__())#会触发函数体代码的运行，然后遇到yield停下来，将yield的值当作本次调用的结果返回

#res1=g.__next__()
res1=next(g)
print(res1)
res2=g.__next__()
print(res2)
"""结果如下"""
<generator object fun at 0x000001C1F8231E00>
<generator object fun at 0x000001C1F8231E00>
第一次
1
第二次
2
"""没值 就报StopIteration"""
```

```python
next(g) 等同于g.__next__()
iter(可迭代对象)  等同于 可迭代对象.__iter__()

```

##### yield表达式（挂起）

```python
# x=yield 返回值

def dog(name):
    print("dog %s  eat something...." % name)
    while True:
        # x拿到的时yield接收到的值
        x = yield  # x='一根骨头'
        print('dog %s eat %s' % (name, x))


g = dog('dH')
# print(g)

# res=next(g)
# print(res)
# next(g)
g.send(None)  # 等同于next(g)
g.send('一根骨头')  # 为yield赋值，yield会将send过来的值交给x
# g.send('包子')
g.send(['包子', '骨头'])
# g.close()#关闭后，无法传值

```



```python
# x=yield 返回值

def dog(name):
    foodlist=[]
    print("dog %s  eat something...." % name)
    while True:
        # x拿到的时yield接收到的值
        x = yield foodlist
        print('dog %s eat %s' % (name, x))
        foodlist.append(x)


g = dog('dH')
g.send(None)
res=g.send('骨头')
print(res)
res2=g.send('包子')
print(res2)

"""结果如下"""
dog dH  eat something....
dog dH eat 骨头
['骨头']
dog dH eat 包子
['骨头', '包子']
```

```python
def func():
    print('start....')
    x=yield  1111
    print('哈哈哈')

    yield 222

g=func()
res=next(g)
print(res)


res3=g.send('xxxxx')
print(res3)

"""结果如下"""
start....
1111
哈哈哈
222
```

##### 三元表达式

```python
# 三元表达式
# 格式：  条件成立时返回的值  if 条件 else 条件不成立时返回的值
z = 1
s = 2
res = z if z > s else s
print(res)


def func(x,y):
    x=1 if x>y else 2
    print(x)
```

##### 列表生成式

```python
def func(x, y):
    x = 1 if x > y else 2
    print(x)


l = ['zhao', 'lisi', 'wan', 'egon']
new_list = []
for name in l:
    if name.endswith('n'):
        new_list.append(name)
# 列表生成式
new_l = [name for name in l if name.endswith('n')]
print(new_l)
print(new_list)

# 所有小写变成大写
res = [name.upper() for name in l]
print(res)
# 把所有的名字去掉后缀n
res2=[ name.replace('n','') for  name in l]
print(res2)

"""结果如下"""
['wan', 'egon']
['wan', 'egon']
['ZHAO', 'LISI', 'WAN', 'EGON']
['zhao', 'lisi', 'wa', 'ego']
```

**字典生成式**

```python
keys={'name','age','hobby'}
dic={key:None for key in keys}
print(dic)

items=[('name','zhao'),('age',18),('hobby','JayChou')]
res={key:value for key,value in items if key!='hobby'}
print(res)

"""结果如下"""
{'age': None, 'name': None, 'hobby': None}
{'name': 'zhao', 'age': 18}
```

##### 集合生成式

```python
keys=['name','age','gender']
set2={ name for name in keys}
print(set2,type(set2))

"""结果如下"""
{'name', 'age', 'gender'} <class 'set'>

```

##### 生成器表达式

```python
g=(i for i in range(10) if i>3)
print(g,type(g))
print(next(g))
print(next(g))
print(next(g))
print(next(g))
print(next(g))

"""结果如下"""
<generator object <genexpr> at 0x0000012FBFEC0430> <class 'generator'>
4
5
6
7
8
```

#### 4.12、二分法

```python
# 有一个按照从小到大顺序排列的数字列表，需要从中找到一个想要的数字，怎么做更高效
nums = [-3, 1, 10, 4, 77, 8, 13, 21, 43, 89]
find_mum = 13


# for num in nums:
#     if num ==find_mum:
#         print('find it')
#         break

# 二分法

def binary_search(find_num, l):
    print(l)
    if len(l)==0:
        print('找的值不存在')
        return
    # 找到列表的中间值:
    mid_index = len(l) // 29
    mid_val = l[mid_index]
    if find_mum > mid_val:
        # 接下来应该找列表的右半部分
        l=l[mid_index+1:]
        binary_search(find_mum, l)
    elif find_mum < mid_val:
        # 接下来应该找列表的左半部分
        l=l[:mid_index]
        binary_search(find_mum, l)
    else:
        print('find it ')
binary_search(find_mum,nums)
```

#### 4.13、面向过程与函数式

##### 面向过程

”面向过程“核心是“过程”二字，“过程”指的是解决问题的步骤，即先干什么再干什么......，基于面向过程开发程序就好比在设计一条流水线，是一种机械式的思维方式，这正好契合计算机的运行原理：任何程序的执行最终都需要转换成cpu的指令流水按过程调度执行，即无论采用什么语言、无论依据何种编程范式设计出的程序，最终的执行都是过程式的。

1、优点

```python
将复杂的问题流程化，进而简单化
```

2、缺点

```python
程序的可扩展性极差，
```

3、应用场景

```python
面向过程的程序设计一般用于那些功能一旦实现之后就很少需要改变的场景， 如果你只是写一些简单的脚本，去做一些一次性任务，用面向过程去实现是极好的，但如果你要处理的任务是复杂的，且需要不断迭代和维护， 那还是用面向对象最为方便。
```

##### 函数式

函数式编程并非用函数编程这么简单，而是将计算机的运算视为数学意义上的运算，比起面向过程，函数式更加注重的是执行结果而非执行的过程，代表语言有：Haskell、Erlang。而python并不是一门函数式编程语言，但是仍为我们提供了很多函数式编程好的特性，如lambda，map，reduce，filter

###### 匿名函数lambda

 对比使用def关键字创建的是有名字的函数，使用lambda关键字创建则是没有名字的函数，即匿名函数，语法如下

```python
lambda 参数1,参数2,...: expression
```

**案例：**

```python
# 1、定义
lambda x,y,z:x+y+z

#等同于
def func(x,y,z):
    return x+y+z

# 2、调用
# 方式一：
res=(lambda x,y,z:x+y+z)(1,2,3)

# 方式二：
func=lambda x,y,z:x+y+z # “匿名”的本质就是要没有名字，所以此处为匿名函数指定名字是没有意义的
res=func(1,2,3)
print(res)
```

匿名函数与有名函数有相同的作用域，但是匿名意味着引用计数为0，使用一次就释放，所以匿名函数用于临时使用一次的场景，匿名函数通常与其他函数配合使用

案例

```python
salaries={
    'siry':3000,
    'tom':7000,
    'lili':10000,
    'jack':2000
}
```

要想取得薪水的最大值和最小值，我们可以使用内置函数max和min（为了方便开发，python解释器已经为我们定义好了一系列常用的功能，称之为内置的函数，我们只需要拿来使用即可）

```python
print(max(salaries))
print(min(salaries))
#默认根据字符比较大小


"""结果如下"""
tom
jack
```

内置max和min都支持迭代器协议，工作原理都是迭代字典，取得是字典的键，因而比较的是键的最大和最小值，而我们想要的是比较值的最大值与最小值，于是做出如下改动

```python
max1=max(salaries,key=lambda k:salaries[k])
print(max1)
min1=min(salaries,key=lambda k:salaries[k])
print(min1)

"""结果如下"""
lili
jack
```

直接对字典进行排序，默认也是按照字典的键去排序的

```python
print(sorted(salaries))

"""结果如下"""
['jack', 'lili', 'siry', 'tom']
```

根据值的最大值与最小值排序

```python
res=sorted(salaries,key=lambda k:salaries[k])
print(res)

"""结果如下"""
['jack', 'siry', 'tom', 'lili']
```



* 特点
  * 使用lambda关键字创建函数
  * 没有名字的函数
  * 匿名函数冒号后面的表达式只有一个，注意：是表达式，而不是语句
  * 匿名函数自带return，而这个return的结果是表达式计算后的结果
* 缺点

  * lamdba只能是单个表达式：不是一个代码块，lamdba的设计就是为了满足简单的函数场景
  * 仅仅能封装有限的逻辑，复杂逻辑实现不了，必须使用def来处理

```python
#匿名函数
m=lambda x,y:x+y
#通过变量去调用匿名函数
print(m(23,19))
M=lambda a,b,c:a*b*c
print(M(1,2,3))

age =15
print('可以继续参军，'if age>18 else'继续上学')
C=lambda x,y:x if x>y else y
print(C(1,5))

re=(lambda  x,y:x if x<y else y)(16,12)
print(re)

Rs=lambda x:(x**2)+890
print(Rs(10))
```

###### map、filter、reduce（了解）

```python
l = ['zhoa', 'lisi', 'wangwu ']

res = map(lambda name: name + '_dsb', l)
print(res)  # 生成器
res1 = filter(lambda name: name.endswith('u'), l)
print(res1)

from functools import reduce

res3=reduce(lambda x, y: x + y, [1, 2, 3],10)
print(res3)

"""结果如下"""
<map object at 0x0000014D2CDF7E80>
<filter object at 0x0000014D2CDF7D60>
16
```

---

#### 4.13、函数的递归调用

函数不仅可以嵌套定义，还可以嵌套调用，即在调用一个函数的过程中，函数内部又调用另一个函数，而函数的递归调用指的是在调用一个函数的过程中又直接或间接地调用该函数本身

例如：
在调用f1的过程中，又调用f1，这就是直接调用函数f1本身

```python
def f1():
    print('from f1')
    f1()
f1()
```

**递归  自己调用自己**
**必须有一个明确的结束条件**

* 阶乘/通过递归实现

```python
#阶乘/通过递归实现
def factorial(n):
    '''
    阶乘
    '''
    if n==1:
        return 1
    return n*factorial(n-1)
result =factorial(5)
print(result)
```

* 通过普通函数实现阶乘

```python
#通过普通函数实现阶乘
def jiecheng(n):
    '''
    阶乘
    '''
    result=1
    for item in range(1,n+1):
        result*=item
    return result
re=jiecheng(5)
print(re)
```

```python
#模拟实现，树形结构的遍历
import os #引入文件操作模块
def findFile(file_Path):
    listRs=os.listdir(file_Path)#得到该路径下面的文件夹
    for fileItem in listRs:
        full_Path=os.path.join(file_Path,fileItem)#获取完整的文件路径
        if os.path.isdir(full_Path):#判断是否是文件夹
            findFile(full_Path)#如果是一个文件，再次去遍历
        else:
            print(fileItem)
    else:
        return
#d调用搜索文件对象
findFile('D:\\Python')
```

**回溯**：一层 一层调用下去

**递推**：满足某种结束条件，结束递归调用，然后一层一层返回

#### 4.14、内置函数 数学运算

```python
#Python语言自带的函数
print(abs(-23))#取绝对值
print(round(2.23))#近似值
print(round(12.56,1))#保留一位小数
print(pow(3,3))#次方
print(3**3)#次方
print(max(12,15,18))#返回最大值
print(min(12,15))#最小值

print(sum(range(50)))

#eval 执行表达式
a,b,c=1,2,3
print('动态生成的函数{}'.format(eval('a+b+c')))

```

---

#### 4.115、类型转换函数

```python
#chr() 数字转字符
#ord() 字符转数字
#bin()  转为二进制
#hex() 转为十六进制
#oct() 转八进制
#list()将元组转列表
print(bin(10))
print(hex(16))
tupleA=(132,2,2,23,1)
print(list(tupleA))

listA=[1,123,15,',ghdj']
print(tuple(listA))

# bytes转换
print(bytes('我喜欢Python',encoding='utf-8'))

```

---

#### 4.16、序列操作函数

```python
#sorted()函数对所有可迭代的对象进行排序操作   生成一个新的进行排序
#sort() 在原有数据基础上进行排序
#all()
#range()

list=[1,2,3,45,6]
# list.sort()#直接修改原始对象

print('-------------排序之前-----------{}'.format(list))
# varList=sorted(list)
varList=sorted(list,reverse=True)#降序排序
# varList=sorted(list,reverse=False)#升序排序
print('-------------排序之后-----------{}'.format(varList))

```

---

# 05模块与包

**模块是个啥**

模块是一系列功能的集合体，分为三大类：

1. 内置模块
2. 第三方模块
3. 自定义的模块

一个Python文件本身就是一个模块，文件名m.py，模块名叫m

`__init__.py`文件，称为包

**为何有模块**

1. 内置模块与第三方拿来就用，无需定义，可以提升开发效率
2. 自定义模块，可以将程序的各部分功能提取出来放到一个模块中，供大家共享，好处是减少代码冗余，程序组织结构清晰



#### 5.1、首次导入模块发生的三件事

* 1.执行xx.py
* 2.产生xx.py的名称空间，将xx.py运行过程中产生的名字都丢带xx的名称空间中
* 3.在当前文件中产生的有一个名字xx,该名字指向2中产生的名称空间

==首次导入完后，之后再导入都是直接引用首次导入产生的xx.py的名称空间,不会重复执行代码==

#### 5.2、import导入模块使用

```python
"""

1. 可以使用逗号分隔符在一行导入多个模块
import time,sys
2. 导入模块的规范（导入的顺序）
	1.python内置的模块
	2.第三方模块
	3.自定义模块
3. 将导入的模块起个别名(当模块名比较长的时候用as起别名)
import time  as t
4. 自定义模块命名应该采用纯小写+下划线风格
5. 可以在函数内导入模块，对比在文件开头导入模块属于全局作用域，在函数内导入的模块则属于局部的作用域。
"""
```

**一个python文件的两种用途**

* 被当作模块导入
* 被当作程序运行

一个Python文件有两种用途，一种被当主程序/脚本执行，另一种被当模块导入，为了区别同一个文件的不同用途，每个py文件都内置了`__name__`变量，该变量在py文件被当做脚本执行时赋值为“__main__”,在py文件被当做模块导入时赋值为模块名

```python
print(__name__)
#1. 当xx.py被运行时，__name__的值为__main__
#2. 当xx.py被当作模块导入时，__name__的值为'xx'

if __name__ == '__main__':
	print("文件被执行")
else:
    #被当做模块导入的时做到事情
    print("文件被导入")
```

#### 5.3、from ...import 导入

```python
"""
import导入模块在使用时必须加前缀"模块."
	优点：肯定不会与当前名称空间中的名字冲突
	缺点：加前缀显得麻烦

from ...import ...导入也发生了三件事
	1.产生一个模块的名称空间
	2.运行xx.py将运行过程中产生的名字都丢到模块的名称空间去
	3. 在当前名称空间拿到一个名字，该名字与模块名称空间中的某一个内存地址
	优点：代码更精简
	缺点：容易与当前名称空间混淆
	
	一行可以导入多个名字
	from time import time,sleep
	*：导入模块中的所有名字
	from os import *
	
"""
```

如果我们需要引用模块中的名字过多的话，可以采用上述的导入形式来达到节省代码量的效果，但是需要强调的一点是：只能在模块最顶层使用*的方式导入，在函数内则非法，并且*的方式会带来一种副作用，即我们无法搞清楚究竟从源文件中导入了哪些名字到当前位置，这极有可能与当前位置的名字产生冲突。模块的编写者可以在自己的文件中定义`__all__`变量用来控制*代表的意思

```python
#foo.py
__all__=['x','get'] #该列表中所有的元素必须是字符串类型，每个元素对应foo.py中的一个名字
x=1
def get():
    print(x)
def change():
    global x
    x=0
class Foo:
    def func(self):
       print('from the func')
```

这样我们在另外一个文件中使用*导入时，就只能导入`__all__`定义的名字了

```python
from foo import * #此时的*只代表x和get

x #可用
get() #可用
change() #不可用
Foo() #不可用
```

#### 5.4、循环导入问题

#### 5.5、模块搜索优先级

模块其实分为四个通用类别，分别是：

1、使用纯Python代码编写的py文件

2、包含一系列模块的包

3、使用C编写并链接到Python解释器中的内置模块

4、使用C或C++编译的扩展模块

**优先级**

在导入一个模块时，如果该模块已**加载到内存**中，则直接引用，否则**会优先查找内置模块**，然后按照从左到右的顺序**依次检索sys.path中定义的路径**，直到找模块对应的文件为止，否则抛出异常。

**sys.path**也被称为模块的搜索路径，它是一个列表类型



```python 
import sys
print(sys.path)#值为一个列表，存放了一系列的文件夹，其中第一个文件夹是当前执行文件所在的文件夹

"""结果如下"""
['E:\\Python\\python\\demo\\day04', 'E:\\Python\\python\\demo', 'D:\\Catalog\\JetBrains\\PyCharm 2022.2\\plugins\\python\\helpers\\pycharm_display', 'C:\\Users\\31812\\AppData\\Local\\Programs\\Python\\Python310\\python310.zip', 'C:\\Users\\31812\\AppData\\Local\\Programs\\Python\\Python310\\DLLs', 'C:\\Users\\31812\\AppData\\Local\\Programs\\Python\\Python310\\lib', 'C:\\Users\\31812\\AppData\\Local\\Programs\\Python\\Python310', 'C:\\Users\\31812\\AppData\\Local\\Programs\\Python\\Python310\\lib\\site-packages', 'D:\\Catalog\\JetBrains\\PyCharm 2022.2\\plugins\\python\\helpers\\pycharm_matplotlib_backend']

```

列表中的每个元素其实都可以当作一个目录来看：在列表中会发现有.zip或.egg结尾的文件，二者是不同形式的压缩文件，事实上Python确实支持从一个压缩文件中导入模块，我们也只需要把它们都当成目录去看即可。

...

sys.path中的第一个路径通常为空，代表执行文件所在的路径，所以在被导入模块与执行文件在同一目录下时肯定是可以正常导入的，而针对被导入的模块与执行文件在不同路径下的情况，为了确保模块对应的源文件仍可以被找到，需要将源文件foo.py所在的路径添加到sys.path中，假设foo.py所在的路为/pythoner/projects/



```python
import sys
#找foo.py，就把foo.py的文件夹添加到环境变量中
sys.path.append(r'/pythoner/projects/') 

import foo #无论foo.py在何处,我们都可以导入它了
```

**sys.modules**查看已经加载带内存中的模块

```pytohn
print('demo1' in sys.modules)
```

#### 5.6、代码规范

我们在编写py文件时，需要时刻提醒自己，该文件既是给自己用的，也有可能会被其他人使用，因而代码的可读性与易维护性显得十分重要，为此我们在编写一个模块时最好按照统一的规范去编写，如下

```python
#!/usr/bin/env python #通常只在类unix环境有效,作用是可以使用脚本名来执行，而无需直接调用解释器。

"The module is used to..." #模块的文档描述

import sys #导入模块

x=1 #定义全局变量,如果非必须,则最好使用局部变量,这样可以提高代码的易维护性,并且可以节省内存提高性能

class Foo: #定义类,并写好类的注释
    'Class Foo is used to...'
    pass

def test(): #定义函数,并写好函数的注释
    'Function test is used to…'
    pass

if __name__ == '__main__': #主程序
    test() #在被当做脚本执行时,执行此处的代码
```

#### 5.7、函数的提示类型

```python
def register(name:str,age:int,hobby:tuple)->int:# ->int表示返回值为整型，冒号跟的是提示信息
    print(name)
    print(age)
    print(hobby)
    return 111
# register(1,'aaa',[1,])
register('lisi',22,('JayChou',))
```

#### 5.8、包

1. 包就是一个包含`__init__.py`文件的文件夹
2. 包的本质就是模块的一种形式，是用来被当作模块导入

**导入包发生的三件事**

* 1.产生一个名称空间
* 2.运行包下的`__init__.py`文件，将运行过程中产生的名字都丢带1的名称空间中
* 3.在当前执行文件的名称空间中拿到一个名字xx,xx指向1的名称空间

```python
在python3中，即使包下没有__init__.py文件，import 包仍然不会报错，而在python2中，包下一定要有该文件，否则import 包报错

#2. 创建包的目的不是为了运行，而是被导入使用，记住，包只是模块的一种形式而已，包的本质就是一种模
```

**强调**

```python
1.关于包相关的导入语句也分为import和from ... import ...两种，但是无论哪种，无论在什么位置，在导入时都必须遵循一个原则：凡是在导入时带点的，点的左边都必须是一个包，否则非法。可以带有一连串的点，如import 顶级包.子包.子模块,但都必须遵循这个原则。但对于导入后，在使用时就没有这种限制了，点的左边可以是包,模块，函数，类(它们都可以用点的方式调用自己的属性)。

2、包A和包B下有同名模块也不会冲突，如A.a与B.a来自俩个命名空间

3、import导入文件时，产生名称空间中的名字来源于文件，import 包，产生的名称空间的名字同样来源于文件，即包下的__init__.py，导入包本质就是在导入该文件
```

针对包内的模块之间互相导入，导入的方式有两种

1、绝对导入：以顶级包为起始

```python
#demo下的__init__.py
from demo import versions
from demo.a.m1 import b
```

```python
import也能使用绝对导入，导入过程中同样会依次执行包下的__init__.py,只是基于import导入的结果，使用时必须加上该前缀
```

2、相对导入：.代表当前文件所在的目录，..代表当前目录的上一级目录，依此类推

```python
#demo下的__init__.py
from . import versions
```

**总结**

包的使用需要牢记三点
1、导包就是在导包下__init__.py文件
2、包内部的导入应该使用相对导入，相对导入也只能在包内部使用，而且...取上一级不能出包
3、
使用语句中的点代表的是访问属性
m.n.x ----> 向m要n，向n要x
而导入语句中的点代表的是路径分隔符
import a.b.c --> a/b/c，文件夹下a下有子文件夹b，文件夹b下有子文件或文件夹c
所以导入语句中点的左边必须是一个包

**from 包 import ***

 在使用包时同样支持from pool.futures import * ，毫无疑问`*`代表的是futures下`__init__.py`中所有的名字，通用是用变量`__all__`来控制*代表的意思

```python
#futures下的__init__.py
__all__=['process','thread']
```


---

# 06、Python面向对象

#### 6.1、oop介绍

> 面向对象编程 oop  【object oriented programming】是一种Python的编程思路

* 面向过程：
  在思考问题的时候，怎么按照步骤去实现，
  然后将问题解决拆分成若干个步骤，并将这些步骤对应成方法一步一步的最终完成功能
* 面向过程：就是一开始学习的，按照解决问题步骤去编写代码【根据业务逻辑去写代码】
* 面向对象：关注的是设计思维【找洗车店， 给钱洗车】
* 从计算机角度看：面向过程不适合做大项目
* 面向过程关注的是：怎么做
* 面向对象关注的是：谁来做

---

#### 6.2、类和对象

* 类：是一个模板，模板里包含多个函数，函数里实现一些功能
* 对象：则是根据模板创建的实例，通过实例对象可以执行类中的函数
* 类由3部分构成：

  * 类的名称：类名
  * 类的属性：一组数据
  * 类的方法：允许对进行操作的方法（行为）

> 例如：创建一个人类
> 事物的名称（类名）：人（Person）
> 属性：身高，年龄
> 方法；吃 跑.....

* 类是具有一组 相同或者相似特征【属性】和行为【方法】的一系列对象的集合

> 现实世界    计算机世界
> 行为------》方法
> 特征------》属性

* 对象：是实实在在的一个东西，类的具象化 实例化
* 类是对象的抽象化 而对象是类的实例

```python
"""
在程序中，必须要事先定义类，然后再调用类产生对象（调用类拿到的返回值就是对象）。产生对象的类与对象之间存在关联，这种关联指的是：对象可以访问到类中共有的数据与功能，所以类中的内容仍然是属于对象的，类只不过是一种节省空间、减少代码冗余的机制，面向对象编程最终的核心仍然是去使用对象。
"""
```



---

#### 6.3、定义类

> #定义类和对象
> #类名：采用大驼峰方式命名

```python
#创建对象
#对象名=类名()
'''

class 类名:
    属性
    方法

'''
#实例方法 ：
# 在类的内部，使用def关键字可以定义一个实例方法，与一般函数 定义不同，类方法必须包含参数self【self可以是其他的名字，但是这个位置必须被占用】，且为第一个参数
#属性：在类的 内部定义的变量
#定义在类里面，方法外面的属性成为类属性，定义在方法里面使用self引用的属性称之为实例属性
class Person:
    '''
    对应人的特征
    '''
    name='小勇'   #类属性
    age=22       #类属性
    '''
    对应人的行为 
    '''

    def __int__(self):
        self.name = '小赵'#实例属性
    def eat(self):#实例方法
        print('狼吞虎咽的吃')
    def run(self):#实例方法
        print('飞快地的跑')



```

类体最常见的是变量的定义和函数的定义，但其实类体可以包含任意Python代码，类体的代码在类定义阶段就会执行，因而会产生新的名称空间用来存放类中定义的名字，可以打印`Person.__dict__`来查看类这个容器内盛放的东西

```python
print(Person.__dict__)

"""结果如下"""
{'__module__': '__main__', '__doc__': '\n    对应人的特征\n    ', 'name': '小勇', 'age': 22, '__int__': <function Person.__int__ at 0x000001E768DCFEB0>, 'eat': <function Person.eat at 0x000001E76907A950>, 'run': <function Person.run at 0x000001E77A2FA9E0>, '__dict__': <attribute '__dict__' of 'Person' objects>, '__weakref__': <attribute '__weakref__' of 'Person' objects>}

```

调用类的过程称为将类实例化，拿到的返回值就是程序中的对象，或称为一个实例

```python
#创建对象【类的实例化】
xm=Person() # 每实例化一次Person类就得到一个人的对象
xm.eat()#调用函数
xm.run()
print('{}的年龄是{}'.format(xm.name,xm.age))
```



---

#### 6.4、__init__方法

```python
# 如果有n个这样对象 被实例化，那么就需要添加很多次实例属性，显然比较麻烦
class Person1:
    student='周杰伦'
    def __init__(self,name,age,sex):#魔术方法
        '''
        实例属性的声明
        '''
        self.name=name
        self.age=age
        self.sex=sex
    def run(self,name):
        print('%s 跑太快了吧'%self.name)

```

然后实例出三个人

```python
per1=Person1('zhao',18,'男')
per=Person1('lisi',22,'女')
per=Person1('wangwu',19,'男')
print(per1.age)#18
```

单拿per1的产生过程来分析，调用类会先产生一个空对象per1，然后将per1连同调用类时括号内的参数一起传给`Person1.__init__(per1,’李zhao’,18,'男')`

```python
def __init__(self,name,age,sex):
  
    self.name=name#per1.name='zhao'
    self.age=age#per1.age=18
    self.sex=sex#per1.sex='男'
```

会产生对象的名称空间，同样可以用`__dict__`查看

```python
print(per1.__dict__)

"""结果如下"""
{'name': 'zhao', 'age': 18, 'sex': '男'}
```

#### 6.5、属性访问

##### 类属性与对象属性

在类中定义的名字，都是类的属性，细说的话，类有两种属性：数据属性和函数属性，可以通过`__dict__`访问属性的值，比如`Person1.__dict__['student']`，但Python提供了专门的属性访问语法

```python
print(Person1.student )# 访问数据属性，等同于Person1.__dict__['student']
print(Person1.run)  # 访问函数属性，等同于Person1.__dict__['run']

"""结果如下"""

周杰伦
<function Person1.run at 0x0000029E9DCDA950>
```

操作对象的属性也是一样

```python
print(per1.name)  # print(per1.__dict__['name'])
print(per1.age)
res = per1.hobby = 'JayChou'  # 新增，等同于res=per1.__dict__['hobby']='JayChou'
print(res)
print(per1.hobby)
del per1.hobby  # 删除，等同于del per1.__dict__['hobby']


```

对象的名称空间里只存放着对象独有的属性，而对象们相似的属性是存放于类中的。对象在访问属性时，会优先从对象本身的`__dict__`中查找，未找到，则去类的`__dict__`中查找

---

#### 6.5、self理解

* self和对象指向同一个地址，可以认为self就是对象的引用
* 在实例化对象时，self不需要开发者传参，Python自动将对象传递给self
* self只有类中定义实例方法的时候才有意义，在调用的时候不必传入相应的参数，
* 而是由解释器自动取指向
* self的名字时可以更改的，可以定义成其他的名字，只是约定俗成的定义成了self
* self指的是类实例对象本身

```python
class Person:
    def __init__(self,pro):
        self.pro=pro
    def geteat(s,name,food):
        # print(self)
        print('self在内存中的地址%s'%(id(s)))
        print('%s喜欢吃%s,专业是：%s'%(name,food,s.pro))

zs=Person('心理学')
print('zs的内存地址%s'%(id(zs)))
zs.geteat('小王','榴莲')
```

---

#### 6.6、魔术方法

> 魔术方法： __ xxx __

```python
'''
__init__ 方法：初始化一个类，在创建实例对象为其赋值时使用
__str__方法 ： 在将对象转换成字符串str(对象)测试的时候，打印对象的信息
__new__方法 ： 创建并返回一个实例对象，调用了一次，就会得到一个对象
__class__方法 ： 获得已知对象的类（对象__class__)

__del__方法：  对象在程序运行结束后进行对象销毁的时候调用这个方法，来释放资源
'''

class Animal:
    def __init__(self,name,color):
        self.name=name
        self.color=color
        print('--------init-------')

    def __str__(self):
        return '我的名字是%s,我的颜色为%s'%(self.name,self.color)

    def __new__(cls, *args, **kwargs):
        print('--------new-------')
        return object.__new__(cls)
dog =Animal('旺财','黑色')
print(dog)


'''
__new__和__init__函数区别
__new__类的实例化方法：必须返回该实例 否则对象就创建不成功
__init__用来做数据属性的初始化工作，也可以认为是实例的构造方法，接收类的实例 self 并对其进行构造
__new__至少有一个参数是cls是代表要实例化的类，此参数在实例化时由Python解释器自动操作
__new__函数执行要早于__init__函数
'''

```

```PYTHON
class People:
    def __init__(self,name,age):
        self.name=name
        self.age=age
    def __str__(self):
        return f'{self.name}今年{self.age}岁'

p=People('zhao',18)
print(p)

"""结果如下"""
zhao今年18岁
```

```python
class People1:
    def __init__(self,name,age):
        self.name=name
        self.age=age
    def __del__(self):
        print('run.......')
People1('lisi',12)
print('==========')

"""结果如下"""
run.......
==========

```

#### 6.7、析构方法

```python
'''
当一个对象被删除或者被销毁是，python解释器会默认调用一个方法，这个方法为__del__()方法，也被成为析构方法
'''
class Animals:
    def __init__(self,name):
        self.name=name
        print('这是构造初始化方法')
    def __del__(self):
        print('当在某个作用域下面，没有被引用的情况下，解释器会自动调用此函数，来释放内存空间')
        print('这是析构方法')
        print('%s 这个对象被彻底清理了，内存空间被释放了'%self.name)


cat=Animals('猫猫')
# del cat #手动的清理删除对象
input('程序等待中......')

#当整个程序脚本执行完毕后会自动的调用_del_方法
# 当对象被手动摧毁时也会自动调用_del_方法
# 析构方法一般用于资源回收，利用_del_方法销毁对象回收内存资源

```

---

#### 6.8、封装

面向对象编程有三大特性：封装、继承、多态，其中最重要的一个特性就是封装。封装指的就是把数据与功能都整合到一起，针对封装到对象或者类中的属性，我们还可以严格控制对它们的访问，分两步实现：**隐藏与开放接口**,也就是后面出现的**私有化属性和方法**

```python
# -*- coding: UTF-8 -*-
# @Date ：2022/9/6 19:17

class School:
    school_name = 'ShanXiPoliceCollege'

    def __init__(self, nickname, addr):
        self.addr = addr
        self.nickname = nickname
        self.classes = []

    def related(self, class_obj):
        # self.classes.append(class_name)
        self.classes.append(class_obj)

    def tell_class(self):
        # 打印班级的名字
        print(self.nickname.center(60, '='))
        # 打印班级开设的课程信息
        for class_obj in self.classes:
            class_obj.tell_couse()


"""
# 创建学校
sch_obj1 = School('老男孩', '上海')
sch_obj2 = School('黑马程序员', '北京')

# 开设班级
sch_obj1.related('脱产14期')
sch_obj2.related('脱产15期')
#查看每个校区开设的班级
sch_obj1.tell_class()
sch_obj2.tell_class()
# sch_obj1.tell_classes()
"""
sch_obj1 = School('老男孩', '上海')
sch_obj2 = School('黑马程序员', '北京')
sch_obj3 = School('千锋教育', '北京')


class Class:
    def __init__(self, name):
        self.name = name
        self.course = None

    def related_course(self, course_obj):
        self.course = course_obj

    def tell_couse(self):
        print('班级名:%s' % self.name, end=' ')
        self.course.tell_info()  # 打印课程的详细信息


# 创建班级
class_obj1 = Class('脱产14期')
class_obj2 = Class('脱产15期')
class_obj3 = Class('脱产16期')
# 2.为班级关联一个课程
# class_obj1.related_course('Python全栈开发')
# class_obj2.related_course('Linux运维')
# class_obj3.related_course('MySQL数据库')


# 3.查看班级开设的课程
# class_obj1.tell_couse()
# class_obj2.tell_couse()
# class_obj3.tell_couse()

# 4. 为学校开设班级
sch_obj1.related(class_obj1)
sch_obj2.related(class_obj2)
sch_obj3.related(class_obj2)


# sch_obj2.tell_class()
# sch_obj1.tell_class()


class Course:
    def __init__(self, name, period, price):
        self.name = name
        self.period = period
        self.price = price

    def tell_info(self):
        print('<%s-%s-%s>' % (self.name, self.period, self.price))


# 三：课程
# 1.创建课程
course_obj1 = Course('Python全栈开发', '6months', 20000)
course_obj2 = Course('Linux运维', '5months', 15000)
course_obj3 = Course('MySQL数据库', '2months', 8000)
# 2.查看课程详细信息
# course_obj1.tell_info()
# course_obj2.tell_info()

# 3.为班级关联课程对象
class_obj1.related_course(course_obj1)
class_obj2.related_course(course_obj2)
class_obj3.related_course(course_obj3)

# 4.
# class_obj1.tell_couse()
# class_obj2.tell_couse()
# class_obj3.tell_couse()

sch_obj1.tell_class()
sch_obj2.tell_class()
sch_obj3.tell_class()



```

#### 6.9、继承

**Python中展现面向对象的三大类型： 封装，继承，多
态**

1. 封装：值得是把内容封装到某个地方，便于后面的使用
   他需要：
   把内容封装到某个地方，从另外一个地方去到调用被封装的内容
   对于封装来说，其实就是使用初始化构造方法将内容封装到对象中，然后通过对象直接或者self来获取被封装的内容

2. 继承：和现实生活当中的继承是一样的，也就是子可以继承父的内容【属性和行为】（爸爸有的儿子有， 相反， 儿子有的爸爸不一定有）。**继承是一种创建新类的方式，在Python中，新建的类可以继承一个或多个父类，新建的类可称为子类或派生类，父类又可称为基类或超类**

3. 所谓多态，定义时的类型和运行时的类型是不一样，此时就成为多态

##### 单继承

```python
class Animal:
    def eat(self):
        '''
        吃
        '''
        print('吃')
    def drink(self):
        '''
        喝
        '''
        print('喝')

class Dog(Animal):#继承Animal父类，  此时Dog就是子类
    def wwj(self):
        print('汪汪叫')
class Cat(Animal):
    def mmj(self):
        print('喵喵叫')
d1=Dog()
d1.eat()#继承了父类的行为
d1.wwj()
c1=Cat()
c1.drink()
c1.eat()

'''
对于面向对象的继承来说，其实就是将多个子类共有的方法提取到父类中，
子类仅仅需要继承父类而不必一一去实现
这样就可以极大提高效率，减少代码的重复编写，精简代码的层级结构 便于扩展

class 类名(父类):
    pass
'''
```

**通过类的内置属性`__bases__`可以查看类继承的所有父类**

```python
print(Dog.__bases__)

"""结果如下"""
(<class '__main__.Animal'>,)

```

---

##### 多继承

```python
class shenxian:
    def fly(self):
        print('神仙会飞')

class Monkey:
    def chitao(self):
        print('猴子喜欢吃桃子')
class SunWuKong(shenxian,Monkey):
    pass

swk=SunWuKong()
swk.fly()
swk.chitao()

#当多个父类当中存在相同方法时候，
class D:
    def eat(self):
        print('D.eat')
class C(D):
    def eat(self):
        print('C.eat')
class B(D):
    pass
class A(B,C):
    pass

a=A()
# b=B()
a.eat()
print(A.__mro__)#可以现实类的依次继承关系      查找执行顺序
#在执行eat方法时，顺序应该是
#首先到A里面去找，如果A中没有，则就继续去B类中去查找，如果B中没有，则去C中查找，
#如果C类中没有，则去D类中查找，如果还没有找到，就会报错
#A-B-C-D 也是继承的顺序

```

在Python2中有经典类与新式类之分，没有显式地继承object类的类，以及该类的子类，都是经典类，显式地继承object的类，以及该类的子类，都是新式类。而在Python3中，即使没有显式地继承object，也会**默认继承该类**，如下

```python
print(shenxian.__bases__)

"""结果如下"""
(<class 'object'>,)

```

```python
提示：object类提供了一些常用内置方法的实现，如用来在打印对象时返回字符串的内置方法__str__
```

##### 属性查找

有了继承关系，对象在查找属性时，==先从对象自己的`__dict__`中找==，如果没有则去子类中找，然后再去父类中找……

```python
>>> class Foo:
...     def f1(self):
...         print('Foo.f1')
...     def f2(self):
...         print('Foo.f2')
...         self.f1()
... 
>>> class Bar(Foo):
...     def f1(self):
...         print('Foo.f1')
... 
>>> b=Bar()
>>> b.f2()
Foo.f2
Foo.f1
"""
b.f2()会在父类Foo中找到f2，先打印Foo.f2,然后执行到self.f1(),即b.f1()，仍会按照：对象本身->类Bar->父类Foo的顺序依次找下去，在类Bar中找到f1，因而打印结果为Foo.f1
"""
```

父类如果不想让子类覆盖自己的方法，可以采用双下划线开头的方式将方法设置为私有的

```python
>>> class Foo:
...     def __f1(self): # 变形为_Foo__fa
...         print('Foo.f1') 
...     def f2(self):
...         print('Foo.f2')
...         self.__f1() # 变形为self._Foo__fa,因而只会调用自己所在的类中的方法
... 
>>> class Bar(Foo):
...     def __f1(self): # 变形为_Bar__f1
...         print('Foo.f1')
... 
>>> 
>>> b=Bar()
>>> b.f2() #在父类中找到f2方法，进而调用b._Foo__f1()方法，同样是在父类中找到该方法
Foo.f2
Foo.f1
```

##### 继承实现原理

1. 菱形问题,,或称钻石问题，有时候也被称为“死亡钻石”

![在这里插入图片描述](E:\MarkDown\markdown\imgs\714ab23f7ab9402ea8656ccf6845f3b6.png)


```python
class A(object):
    def test(self):
        print('from A')


class B(A):
    def test(self):
        print('from B')


class C(A):
    def test(self):
        print('from C')


class D(B,C):
    pass


obj = D()
obj.test() # 结果为：from B
```

2. **继承原理**

python到底是如何实现继承的呢？ 对于你定义的每一个类，Python都会计算出一个方法解析顺序(MRO)列表，该MRO列表就是一个简单的所有基类的线性顺序列表，如下

```python
>>> D.mro() # 新式类内置了mro方法可以查看线性列表的内容，经典类没有该内置该方法
[<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>]
```

**python会在MRO列表上从左到右开始查找基类,直到找到第一个匹配这个属性的类为止。**

 **MRO列表并遵循如下三条准则:**

```python
1.子类会先于父类被检查
2.多个父类会根据它们在列表中的顺序被检查
3.如果对下一个类存在两个合法的选择,选择第一个父类
```

所以obj.test()的查找顺序是，先从对象obj本身的属性里找方法test，没有找到，则**参照属性查找的发起者**(即obj)所处类D的MRO列表来依次检索，首先在类D中未找到，然后再B中找到方法test

```python
1.由对象发起的属性查找，会从对象自身的属性里检索，没有则会按照对象的类.mro()规定的顺序依次找下去，
2.由类发起的属性查找，会按照当前类.mro()规定的顺序依次找下去，
```

---

#### 6.10、重写父类方法

* 所谓重写，就是子类中，有一个和父类相同名的方法，在子类中的方法会覆盖掉子类中同名的方法，
* 为什么要重写，父类的方法已经不满足子类的需要，那么子类就可以重写父类或者完善父类方法

当父类的方法实现不能满足子类的时候，可以对方法进行重写
重写父类的方法有两种

1. 覆盖父类方法



```python
class Father:
    def smoke(self):
        print('抽芙蓉王')
    def drink(self):
        print('喝二锅头')
class Son(Father):
    #与父类的（抽烟）方法同名,这就是重写父类方法
    def smoke(self):
        print('抽中华')

#重写父类方法后，子类调用父类方法时，将调用的是子类的方法
son=Son()
son.smoke()

```

2. 扩展父类方法

方法一：**“指名道姓”地调用某一个类的函数**

```python
class Father:
    def smoke(self):
        print('抽芙蓉王')

    def drink(self):
        print('喝二锅头')


class Son(Father):
    def drink(self):
        ##调用的是函数,因而需要传入self
        Father.drink(self)


s = Son()
s.drink()
```

方法二：**super()**

```python
"""
调用super()会得到一个特殊的对象，该对象专门用来引用父类的属性，且严格按照MRO规定的顺序向后查找
"""
```

```python
class Father:
    def smoke(self):
        print('抽芙蓉王')

    def drink(self):
        print('喝二锅头')


class Son(Father):
    def drink(self):
        #调用的是绑定方法，自动传入self
        super().drink()


s = Son()
s.drink()

"""结果如下"""
喝二锅头
```

```python
class People:
    school = '清华大学'

    def __init__(self, name, sex, age):
        self.name = name
        self.sex = sex
        self.age = age


class Teacher(People):
    def __init__(self, name, sex, age, title):  # 派生
        #方法一
        People.__init__(self, name, sex, age)  # 调用的是函数,因而需要传入self
        #方法二
        super().__init__(name,age,sex) #调用的是绑定方法，自动传入self
        self.title = title
    def teach(self):
        print('%s is teaching' % self.name)
obj = Teacher('lili', 'female', 28, '高级讲师')  # 只会找自己类中的__init__，并不会自动调用父类的
print(obj.name)
```

这两种方式的区别是：方式一是跟继承没有关系的，而方式二的super()是依赖于继承的，并且即使没有直接继承关系，super()仍然会按照MRO继续往后查找

```python
#A没有继承B


class A:
    def test(self):
        super().test()


class B:
    def test(self):
        print('from B')


class C(A, B):
    pass

print(C.mro())  # 在代码层面A并不是B的子类，但从MRO列表来看，属性查找时，就是按照顺序C->A->B->object，B就相当于A的“父类”

obj = C()
obj.test()  # 属性查找的发起者是类C的对象obj，所以中途发生的属性查找都是参照C.mro()

"""结果如下"""
[<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>, <class 'object'>]
from B
```

obj.test()首先找到A下的test方法，执行super().test()会基于MRO列表(以C.mro()为准)当前所处的位置继续往后查找()，然后在B中找到了test方法并执行。

#### 6.11、组合

在一个类中以另外一个类的对象作为数据属性，称为类的组合。组合与继承都是用来解决代码的重用性问题。不同的是：继承是一种“是”的关系，比如老师是人、学生是人，当类之间有很多相同的之处，应该使用继承；而组合则是一种“有”的关系，比如老师有生日，老师有多门课程，当类之间有显著不同，并且较小的类是较大的类所需要的组件时，应该使用组合，如下示例

```python
class Course:
    def __init__(self,name,period,price):
        self.name=name
        self.period=period
        self.price=price
    def tell_info(self):
        print('<%s %s %s>' %(self.name,self.period,self.price))

class Date:
    def __init__(self,year,mon,day):
        self.year=year
        self.mon=mon
        self.day=day
    def tell_birth(self):
       print('<%s-%s-%s>' %(self.year,self.mon,self.day))

class People:
    school='清华大学'
    def __init__(self,name,sex,age):
        self.name=name
        self.sex=sex
        self.age=age

#Teacher类基于继承来重用People的代码，基于组合来重用Date类和Course类的代码
class Teacher(People): #老师是人
    def __init__(self,name,sex,age,title,year,mon,day):
        super().__init__(name,age,sex)
        self.birth=Date(year,mon,day) #老师有生日
        self.courses=[] #老师有课程，可以在实例化后，往该列表中添加Course类的对象
    def teach(self):
        print('%s is teaching' %self.name)


python=Course('python','3mons',3000.0)
linux=Course('linux','5mons',5000.0)
teacher1=Teacher('lili','female',28,'博士生导师',1990,3,23)

# teacher1有两门课程
teacher1.courses.append(python)
teacher1.courses.append(linux)

# 重用Date类的功能
teacher1.birth.tell_birth()

# 重用Course类的功能
for obj in teacher1.courses: 
    obj.tell_info()
```

#### 6.12、Mixins机制

Mixins机制指的是子类混合(mixin)不同类的功能，而这些类采用统一的命名规范（例如Mixin后缀），以此标识这些类只是用来**混合功能**的

```python
class Vehicle:  # 交通工具
    pass


class FlyableMixin:
    def fly(self):
        '''
        飞行功能相应的代码        
        '''
        print("I am flying")


class CivilAircraft(FlyableMixin, Vehicle):  # 民航飞机
    pass


class Helicopter(FlyableMixin, Vehicle):  # 直升飞机
    pass


class Car(Vehicle):  # 汽车
    pass

# ps: 采用某种规范（如命名规范）来解决具体的问题是python惯用的套路
```

可以看到，上面的CivilAircraft、Helicopter类实现了多继承，不过它继承的第一个类我们起名为FlyableMixin，而不是Flyable，这个并不影响功能，但是会告诉后来读代码的人，这个类是一个Mixin类，表示混入(mix-in)，这种命名方式就是用来明确地告诉别人（python语言惯用的手法），这个类是作为功能添加到子类中，而不是作为父类，

#### 6.13、多态

* 所谓多态，定义时的类型和运行时的类型是不一样，此时就成为多态

```python
#要想实现多态，必须有两个前提需要遵守：
'''
1.继承：多态必须放生在父类和子类之间
2.重写：子类重写父类的方法
'''
class Animal:
    '''
    基类[父类]
    '''
    def say(self):
        print('我是一个动物'*10)
class Dark(Animal):
    '''
    子类【派生类】
    '''
    def say(self):
        '''
        重写父类方法
        '''
        print('我是一个鸭子'*10)
class Dog(Animal):
    def say(self):
        print('我是一只🐕'*10)
# dark=Dark()
# dark.say()


#统一的去调用
def  commonInvoke(obj):
    obj.say()
list=[Dark(),Dog()]
for item in list:
    '''
    循环调用函数
    '''
    commonInvoke(item)

```

---

#### 6.14、类属性和实例属性

1. 类属性：就是类对象拥有的属性，它被所有类对象的实例对象所共有，类对象和实例对象可以访问
2. 实例属性：实例对象所拥有的属性，只能通过实例对象访问

```python
class Student:
    name ='黎明'#属于类属性，就是Student类对象所拥有
    def __init__(self,age):
        self.age=age #实例属性

lm=Student(18)
print(lm.name)
print(lm.age)
print(Student.name)#通过Student类对象访问name属性
# print(Student.age)#类对象不能访问实例属性

#类对象可以被类对象和实例对象共同访问使用的
#实例对象只能实例对象访问


```

---

#### 6.15、类方法和静态方法

* 类方法的第一个参数是类对象cls,通过cls引用的类对象的属性和方法
* 实例对象的第一个参数是实例对象self,通过self引用的可能是类属性，也可能是 实例属性
  ，不过在存在相同名称的类属性和实例属性的情况下，实例属性的优先级更高
* 静态方法中不需要额外定义参数，因此在静态方法中引用类属性的话，必须通过类对象来引用

```python
class People:
    country='china'
    @classmethod #类方法 用classmethod进行修饰
    def get_country(cls):
        return  cls.country#访问类属性
    @classmethod
    def change_country(cls,data):
        cls.country=data#修改列属性的值，在类方法中
    @staticmethod
    def gatData():
        return People.country
    @staticmethod
    def add(x,y):
        return x+y

print(People.gatData())
p=People()
print(p.gatData())#注意：一般情况下，我们不会通过实例对象去访问静态方法
print(People.add(1,2))
# print(People.get_country())#通过类对象去引用
# p=People()
# print('通过实例对象访问',p.get_country())
# People.change_country('英国')
# print(People.get_country())

#由于静态方法主要来存放逻辑性的代码，本身和类以及实例对象没有交互
#也就是说，在静态方法中，不会涉及到类中方法和属性的操作
#数据资源能够得到有效的充分利用


import time
class TimeTest:
    def __init__(self,hour,minute,second):
        self.hour=hour
        self.minute=minute
        self.second=second

    @staticmethod
    def showTime():
        return time.strftime('%H:%M:%S',time.localtime())
print(TimeTest.showTime())
# t=TimeTest(2,10,11)
# print(t.showTime())#没必要通过实例对象访问静态方法

```

---

#### 6.16、私有化属性(隐藏)

* 语法：

  * 两个下划线开头，声明该属性为私有，不能在类的外部被使用或直接访问
* 使用私有化属性的场景

  1. 把特定的一个属性隐藏起来，不想让类的外部进行直接调用
  2. 我想保护这个属性，不想让属性的值随意的改变，
  3. 保护这个属性，不想让派生类【子类】去继承
* 要想访问私有变量一般是写两个方法，一个访问，一个修改，由方法去控制访问

> class Person:
> __age =18 #实例一个私有化属性，属性名字前面加两个下划线
> '''

```python
#print(Person.__dict__)

class Person:
    __hobby = 'dance'

    def __init__(self):
        self.__name = '李四'  # 加两个下划线将此属性私有化,不能再外部直接访问，在类的内部可以访问
        self.age = 30
        print(self.__name)#print(self._Person__name)
        

    def __str__(self):
        return '{}的年龄是{}，喜欢{}'.format(self.__name, self.age, Person.__hobby)  # 调用私有化属性

    def change(self, hobby):
        Person.__hobby = hobby


class Student(Person):
    def printInfo(self):
        # print(self.__name)#访问不了父类中的私有属性
        print(self.age)

    pass


xl = Person()
# print(xl.name)#通过类对象，在外部访问的
print(xl)
xl.change('唱歌')  # 修改私有属性的值
print(xl)
stu = Student()
stu.printInfo()
stu.change('Rap')
print(stu)
# print(xl.hobby)  # 通过实例对象访问类属性
# print(stu.hobby)  # 通过实例对象访问类属性
# print(Person.hobby)  # 通过类对象访问类属性

```



* 小结：
  1. 私有化的【实例】属性，不能在外部直接的访问，可以在类的内部随意的使用
  2. 子类不能继承父类的私有化属性，【只能继承父类公共的属性和行为】
  3. 在属性名的前面直接加__，就可以将其私有化

* 单下划线、  双下划线、 头尾双下划线三者区别

> _xxx前面加下划线，以单下划线开头表示的是protected类型的变量，即保护类型只能允许其本身与子类进行访问，不能使用from xxx import *的方式导入
> __xxx__前后两个下划线，魔术方法，一般是python自带的，开发者不要创建这类型的方法
> xxx_后面单下划线，避免属性名与Python关键字冲突

---

#### 6.17、私有化方法(隐藏)

* 概述
  * 私有化方法跟私有化属性一样，有些重要的方法，不允许外部调用，防止子类意外重写，把普通的方法设置成私有化方法
  * 私有化方法一般是类内部调用，子类不能继承外部不能调用
* 语法

> class A:
> def __myname(self):  #在方法名前面加两个下划线
> print('小明')

```python

class Animal:
    def __eat(self):
        print('吃东西')

    def run(self):
        self.__eat()  # 在此调用私有化方法
        print('飞快地跑')


class Bird(Animal):
    pass


bird = Bird()
bird.run()

```

---

#### 6.18、Property函数

```python
#  属性函数 (property)
class Perons:
    def __init__(self):
        self.__age = 18

    def ger_age(self):#访问私有实例属性
        return self.__age

    def set_age(self, age):#修改私有实例属性
        if age < 0:
            print('年龄太小')
        else:
            self.__age = age

    #定义一个类属性，实现通过直接访问属性的形式去访问私有属性
    age=property(ger_age,set_age)
p=Perons()
print(p.age)
p.age=-1
print(p.age)

```

装饰器property，可以将类中的函数“伪装成”对象的数据属性，对象在访问该特殊属性时会触发功能的执行，然后将返回值作为本次访问的结果，例如

```python
>>> class People:
...     def __init__(self,name,weight,height):
...         self.name=name
...         self.weight=weight
...         self.height=height
...     @property
...     def bmi(self):
...         return self.weight / (self.height**2)
...
>>> obj=People('lili',75,1.85)
>>> obj.bmi #触发方法bmi的执行，将obj自动传给self，执行后返回值作为本次引用的结果
21.913805697589478
```

使用property有效地保证了属性访问的一致性。另外property还提供设置和删除属性的功能，如下

```python
>>> class Foo:
...     def __init__(self,val):
...         self.__NAME=val #将属性隐藏起来
...     @property
...     def name(self):
...         return self.__NAME
...     @name.setter
...     def name(self,value):
...         if not isinstance(value,str):  #在设定值之前进行类型检查
...             raise TypeError('%s must be str' %value)
...         self.__NAME=value #通过类型检查后,将值value存放到真实的位置self.__NAME
...     @name.deleter
...     def name(self):
...         raise PermissionError('Can not delete')
...
>>> f=Foo('lili')
>>> f.name
lili
>>> f.name='LiLi' #触发name.setter装饰器对应的函数name(f,’Egon')
>>> f.name=123 #触发name.setter对应的的函数name(f,123),抛出异常TypeError
>>> del f.name #触发name.deleter对应的函数name(f),抛出异常PermissionError
```

#### 6.19、`__new__`方法

![image.png](E:\MarkDown\markdown\imgs\9f8069f3e7366eb43814f43223bcba50.png)

* **new方法在init方法之前执行**

```python
class A(object):
    def __init__(self):
        print("__init__执行了")

    def __new__(cls, args, **kwargs):
        print("__new__执行了")
        return object.__new__(cls)  # 调用父类的new方法


a = A()  # 实例化的过程会自动调用__new__方法取创建实例

```

**运行结果如下：**

![image.png](E:\MarkDown\markdown\imgs\deb37b1e90d00c698fbd909123142976.png)

#### 6.20、单例模式

单例模式：是一种常用的软件设计模式

**目的**：确保某一个类只有一个实例存在

如果希望在某个系统中只能出现一个实例，那么单例对象就能满足要求

```python
# 创建一个单例对象,基于__new__去实现


class DataBaseClass(object):
    def __new__(cls, *args, **kwargs):
        # cls._instance=cls.__new__(cls)不能使用自身的new方法，容易造成深度递归，应该调用父类的new方法
        if not hasattr(cls, "_instance"):  # 如果不存在，就开始创建
            cls._instance = super().__new__(cls, *args, **kwargs)
        return cls._instance


db1 = DataBaseClass()
print(id(db1))
db2 = DataBaseClass()
print(id(db2))

```

#### 6.21、异常处理

**不需要在每个可能出错的地方捕获，只要在合适的层次去捕获错误就可以**

```markdown
#语法格式：
try:
	可能出错的代码块
except：
	出错后执行的代码块
else：
	没有出错的代码块
finally：
	不管有没有错都执行的代码块
```

 **try-except**

* except在捕获错误类型的时候，主要根**据具体的错误类型来捕获的**
* 用一个try可以捕获多个不同类型的异常

code 1：

```python
try:
    print(b) #这里是要捕获的代码

except NameError: #指定错误类型为NameRrror
    #捕捉到错误会在这里执行
    print("NameError")

```

code 2：

```python
try:
    print(b) #这里是要捕获的代码

except NameError as msg:
    #捕捉到错误会在这里执行
    print(msg)

```

code 3：

```python
try:
   
    li=[1,2,54,78] #这里是要捕获的代码
    print(li[10])
    a=10/0
except NameError as msg:
    #捕捉到错误会在这里执行
    print(msg)

except IndexError as msg:
    print(msg)
except ZeroDivisionError as msg:
    print(msg)

```

code 4：

```python
try:
    a=10/0
except Exception as msg:
    print(msg)
  
```

code 5:

```python
def A(s):
    return 10/int(s)
def B(s):
    return A(s)*2
def main():
    try:
        B("0")
    except Exception as msg:
        print(msg)

main()
#不需要在每个可能出错的地方捕获，只要在合适的层次去捕获错误就可以
```

**try-except-else**

```python
try:
    print (aa)
except Exception as msg:
    print(msg)
else:
    print("当try里面没有错误。else才会执行")
```

**try-except-else-finally**

```python
try:
    print (aa)
except Exception as msg:
    print(msg)
else:
    print("当try里面没有错误。else才会执行")
finally:
    print("不过有没有错都执行")
```

 **自定义异常**

* 自定义异常，都要直接或者间接的继承 `Error`或 `Exception`类
* 由开发者主动抛出自定义异常，在python中使用 `raise`关键字

```python
class ToolsMyException(Exception):
    def __init__(self, leng):
        self.leng = leng

    def __str__(self):
        return "您输入的的姓名长度是"+str(self.leng)+"已经超过长度"
  
def name_Test():
    name=input("请输入姓名")
    try:
        if len(name)>5:
            raise ToolsMyException(len(name))
        else:
            print(name)
    except ToolsMyException as result:
        print(result)
    finally:
        print("执行完毕-------")

name_Test()
```

#### 6.22、反射机制

[详见相关博客](https://zhuanlan.zhihu.com/p/109336120)

#### 6.23、元类

[详见相关博客](https://zhuanlan.zhihu.com/p/109336845)

---

# 07、文件操作

 文件是操作系统提供给用户/应用程序操作硬盘的一个虚拟的概念/接口

用户/应用程序可以通过文件将数据永久保存在硬盘中

用户/应用程序直接操作的是文件，对文件进行的所有的操作，都是在向操作系统发送系统调用，然后再由操作系统将其转成具体的硬盘操作

#### 7.1、文件操作流程

* **打开文件**

打开文件，由应用系统向操作系统发起系统调用open()，操作系统打开该文件，对应一块硬盘空间，并返回一个文件对象赋值给一个变量f

```python
"""Windows路径分隔符问题"""
"""方案1 （推荐）"""
f=open(r'C:\a\b\c\d.txt',mode='rt')#r原生字符串rawstring
"""方案2"""
f=open(r'C:/a/b/c/d.txt')
```

* **操作文件（读/写）**

调用文件对象下的读/写方法，会被操作系统转换为读/写硬盘的操作

```python
f.read()
```

* **关闭文件**

向操作系统发起关闭文件的请求，回收系统资源

```python
f.close()
```

**with上下文管理**

```python
#1. 执行完代码后，with会自动执行fp.close()
with open('a.txt',mode='rt')as fp:
    	res=fp.read()#t模式会将fp.read()读出来的结果解码成unicode
        print(res)
#2. 用with打开多个文件，用逗号分割开即可
with open('a.txt','r') as f1,open('b.txt','r')as f2:
    res1=f1.read()
    res2=f2.read()
    print(res1)
    print(res2)
```

**指定操作文件的字符编码**

```python
"""
f=open(....)由操作系统打开文件，如果打开的是文本文件，会涉及到字符编码问题，如果没有为open指定编码，那么打开文本文件的默认编码是操作系统说了算的，
操作系统会用自己默认的编码去打开文件，在windos下是gbk，在Linux下是utf-8
"""
```



#### 7.2、文件的操作模式

**控制文件读写操作的模式**

```python
"""
	r（默认的）只读
	w:只写
	a:只追加写
"""
```

* **r模式使用**

```python
# r只读模式: 在文件不存在时则报错,文件存在文件内指针直接跳到文件开头
 with open('a.txt',mode='r',encoding='utf-8') as f:
     res=f.read() # 会将文件的内容由硬盘全部读入内存，赋值给res

# 小练习：实现用户认证功能
 inp_name=input('请输入你的名字: ').strip()
 inp_pwd=input('请输入你的密码: ').strip()
 with open(r'db.txt',mode='r',encoding='utf-8') as f:
     for line in f:
         # 把用户输入的名字与密码与读出内容做比对
         u,p=line.strip('\n').split(':')
         if inp_name == u and inp_pwd == p:
             print('登录成功')
             break
     else:
         print('账号名或者密码错误')
```

* **w模式的使用**

```python
# w只写模式: 在文件不存在时会创建空文档,文件存在会清空文件,文件指针跑到文件开头
with open('b.txt',mode='w',encoding='utf-8') as f:
    f.write('你好\n')
    f.write('我好\n') 
    f.write('大家好\n')
    f.write('111\n222\n333\n')
#强调：
# 1 在文件不关闭的情况下,连续的写入，后写的内容一定跟在前写内容的后面
# 2 如果重新以w模式打开文件，则会清空文件内容

#案例：w模式用来创建全新的文件
#文件的copy工具
src_file=input("源文件路径》》").strip()
dsc_file=input("源文件路径》》").strip()
with open(r'{}'.format(src_file),mode='rt',encoding='utf-8')as f1,open(r'{}'.format(dsc_file),mode='wt',encoding='utf-8')as f2:
    res=f1.read()
    f2.write(res)
```

* **a模式的使用**

```python
# a只追加写模式: 在文件不存在时会创建空文档,文件存在会将文件指针直接移动到文件末尾
 with open('c.txt',mode='a',encoding='utf-8') as f:
     f.write('44444\n')
     f.write('55555\n')
#强调 w 模式与 a 模式的异同：
# 1 相同点：在打开的文件不关闭的情况下，连续的写入，新写的内容总会跟在前写的内容之后
# 2 不同点：以 a 模式重新打开文件，不会清空原文件内容，会将文件指针直接移动到文件末尾，新写的内容永远写在最后

# 小练习：实现注册功能:把用户名和密码添加至数据库
 name=input('username>>>: ').strip()
 pwd=input('password>>>: ').strip()
 with open('db1.txt',mode='a',encoding='utf-8') as f:
     info='%s:%s\n' %(name,pwd)
     f.write(info)
```

* **+模式的使用**

```python
# r+ w+ a+ :可读可写
#在平时工作中，我们只单纯使用r/w/a，要么只读，要么只写，一般不用可读可写的模式
```

**控制文件读写内容的模式**

```python
大前提: tb模式均不能单独使用,必须与r/w/a之一结合使用
t（默认的）：文本模式
    1. 读写文件都是以字符串为单位的
    2. 只能针对文本文件
    3. 必须指定encoding参数
b：二进制模式:
   1.读写文件都是以bytes/二进制为单位的
   2. 可以针对所有文件
   3. 一定不能指定encoding参数
```

* **t模式的使用**

```python
# t 模式：如果我们指定的文件打开模式为r/w/a，其实默认就是rt/wt/at
 with open('a.txt',mode='rt',encoding='utf-8') as f:
     res=f.read() 
     print(type(res)) # 输出结果为：<class 'str'>

 with open('a.txt',mode='wt',encoding='utf-8') as f:
     s='abc'
     f.write(s) # 写入的也必须是字符串类型

 #强调：t 模式只能用于操作文本文件,无论读写，都应该以字符串为单位，而存取硬盘本质都是二进制的形式，当指定 t 模式时，内部帮我们做了编码与解码
```

* **b模式的使用**

```python
# b: 读写都是以二进制位单位
 with open('1.mp4',mode='rb') as f:
     data=f.read()
     print(type(data)) # 输出结果为：<class 'bytes'>

 with open('a.txt',mode='wb') as f:
     msg="你好"
     res=msg.encode('utf-8') # res为bytes类型
     f.write(res) # 在b模式下写入文件的只能是bytes类型

#强调：b模式对比t模式
1、在操作纯文本文件方面t模式帮我们省去了编码与解码的环节，b模式则需要手动编码与解码，所以此时t模式更为方便
2、针对非文本文件（如图片、视频、音频等）只能使用b模式

# 小练习： 编写拷贝工具
src_file=input('源文件路径: ').strip()
dst_file=input('目标文件路径: ').strip()
with open(r'%s' %src_file,mode='rb') as read_f,open(r'%s' %dst_file,mode='wb') as write_f:
    for line in read_f:
        # print(line)
        write_f.write(line)
```

#### 7.3、操作文件的方法

**重点**

```python
# 读操作
f.read()  # 读取所有内容,执行完该操作后，文件指针会移动到文件末尾
f.readline()  # 读取一行内容,光标移动到第二行首部
f.readlines()  # 读取每一行内容,存放于列表中

# 强调：
# f.read()与f.readlines()都是将内容一次性读入内容，如果内容过大会导致内存溢出，若还想将内容全读入内存，则必须分多次读入，有两种实现方式：
# 方式一
with open('a.txt',mode='rt',encoding='utf-8') as f:
    for line in f:
        print(line) # 同一时刻只读入一行内容到内存中

# 方式二
with open('1.mp4',mode='rb') as f:
    while True:
        data=f.read(1024) # 同一时刻只读入1024个Bytes到内存中
        if len(data) == 0:
            break
        print(data)

# 写操作
f.write('1111\n222\n')  # 针对文本模式的写,需要自己写换行符
f.write('1111\n222\n'.encode('utf-8'))  # 针对b模式的写,需要自己写换行符
f.writelines(['333\n','444\n'])  # 文件模式
f.writelines([bytes('333\n',encoding='utf-8'),'444\n'.encode('utf-8')]) #b模式
```

**了解**

```python
f.readable()  # 文件是否可读
f.writable()  # 文件是否可读
f.closed  # 文件是否关闭
f.encoding  # 如果文件打开模式为b,则没有该属性
f.flush()  # 立刻将文件内容从内存刷到硬盘
f.name
```

#### 7.4、主动移动文件内指针移动

```python
#大前提:文件内指针的移动都是Bytes为单位的,唯一例外的是t模式下的read(n),n以字符为单位
with open('a.txt',mode='rt',encoding='utf-8') as f:
     data=f.read(3) # 读取3个字符


with open('a.txt',mode='rb') as f:
     data=f.read(3) # 读取3个Bytes


# 之前文件内指针的移动都是由读/写操作而被动触发的，若想读取文件某一特定位置的数据，则则需要用f.seek方法主动控制文件内指针的移动，详细用法如下：
# f.seek(指针移动的字节数,模式控制): 
# 模式控制:
# 0: 默认的模式,该模式代表指针移动的字节数是以文件开头为参照的
# 1: 该模式代表指针移动的字节数是以当前所在的位置为参照的
# 2: 该模式代表指针移动的字节数是以文件末尾的位置为参照的
# 强调:其中0模式可以在t或者b模式使用,而1跟2模式只能在b模式下用
```

* **0模式**

只有0模式可以在t下使用，1，2必须在b模式下使用

```python
# a.txt用utf-8编码，内容如下（abc各占1个字节，中文“你好”各占3个字节）
abc你好

# 0模式的使用
with open('a.txt',mode='rt',encoding='utf-8') as f:
    f.seek(3,0)     # 参照文件开头移动了3个字节
    print(f.tell()) # 查看当前文件指针距离文件开头的位置，输出结果为3
    print(f.read()) # 从第3个字节的位置读到文件末尾，输出结果为：你好
    # 注意：由于在t模式下，会将读取的内容自动解码，所以必须保证读取的内容是一个完整中文数据，否则解码失败

with open('a.txt',mode='rb') as f:
    f.seek(6,0)
    print(f.read().decode('utf-8')) #输出结果为: 好
```

* **1模式**

```python
# 1模式的使用
with open('a.txt',mode='rb') as f:
    f.seek(3,1) # 从当前位置往后移动3个字节，而此时的当前位置就是文件开头
    print(f.tell()) # 输出结果为：3
    f.seek(4,1)     # 从当前位置往后移动4个字节，而此时的当前位置为3
    print(f.tell()) # 输出结果为：7
```

* **2模式**

```python
# a.txt用utf-8编码，内容如下（abc各占1个字节，中文“你好”各占3个字节）
abc你好

# 2模式的使用
with open('a.txt',mode='rb') as f:
    f.seek(0,2)     # 参照文件末尾移动0个字节， 即直接跳到文件末尾
    print(f.tell()) # 输出结果为：9
    f.seek(-3,2)     # 参照文件末尾往前移动了3个字节
    print(f.read().decode('utf-8')) # 输出结果为：好

# 小练习：实现动态查看最新一条日志的效果
#exe.py:
	with open('access.log','a',encoding='utf-8')as fp:
    fp.write('202120202020200 收款2000元\n')
#access_log.py:
    import time
    with open('access.log',mode='rb') as f:
        f.seek(0,2)#将指针移到末尾
        while True:
            line=f.readline()
            if len(line) == 0:
                # 没有内容
                time.sleep(0.5)
            else:
                print(line.decode('utf-8'),end='')
 """每执行一次exe.py，access_log就会检测到数据，并打印出来"""
```

#### 7.5文件的修改

```python
# 文件a.txt内容如下
张一蛋     山东    179    49    12344234523
李二蛋     河北    163    57    13913453521
王全蛋     山西    153    62    18651433422

# 执行操作
with open('a.txt',mode='r+t',encoding='utf-8') as f:
    f.seek(9)
    f.write('<妇女主任>')

# 文件修改后的内容如下
张一蛋<妇女主任> 179    49    12344234523
李二蛋     河北    163    57    13913453521
王全蛋     山西    153    62    18651433422

# 强调：
# 1、硬盘空间是无法修改的,硬盘中数据的更新都是用新内容覆盖旧内容
# 2、内存中的数据是可以修改的
```

**文件对应的是硬盘空间,硬盘不能修改对应着文件本质也不能修改, 那我们看到文件的内容可以修改,是如何实现的呢? 大致的思路是将硬盘中文件内容读入内存,然后在内存中修改完毕后再覆盖回硬盘** 

具体的实现方式分为两种:

* 方式一

```python
# 实现思路：将文件内容发一次性全部读入内存,然后在内存中修改完毕后再覆盖写回原文件
# 优点: 在文件修改过程中同一份数据只有一份
# 缺点: 会过多地占用内存
with open('db.txt',mode='rt',encoding='utf-8') as f:
    data=f.read()

with open('db.txt',mode='wt',encoding='utf-8') as f:
    f.write(data.replace('zhao','SB'))
```

* 方式二

```python
# 实现思路：以读的方式打开原文件,以写的方式打开一个临时文件,一行行读取原文件内容,修改完后写入临时文件...,删掉原文件,将临时文件重命名原文件名
# 优点: 不会占用过多的内存
# 缺点: 在文件修改过程中同一份数据存了两份
import os

with open('db.txt',mode='rt',encoding='utf-8') as read_f,\
        open('.db.txt.swap',mode='wt',encoding='utf-8') as wrife_f:
    for line in read_f:
        wrife_f.write(line.replace('SB','kevin'))

os.remove('db.txt')
os.rename('.db.txt.swap','db.txt')
```

#### 7.6垃圾回收机制

解释器在执行到定义变量的语法时，会申请内存空间来存放变量的值，而内存的容量是有限的，这就涉及到变量值所占用内存空间的回收问题，当一个变量值没有用了（简称垃圾）就应该将其占用的内存给回收掉，那什么样的变量值是没有用的呢？

单从逻辑层面分析，我们定义变量将变量值存起来的目的是为了以后取出来使用，而取得变量值需要通过其绑定的直接引用（如x=10，10被x直接引用）或间接引用（如l=[x,]，x=10，10被x直接引用，而被容器类型l间接引用），所以当一个变量值不再绑定任何引用时，我们就无法再访问到该变量值了，该变量值自然就是没有用的，就应该被当成一个垃圾回收。

毫无疑问，内存空间的申请与回收都是非常耗费精力的事情，而且存在很大的危险性，稍有不慎就有可能引发内存溢出问题，好在Cpython解释器提供了自动的垃圾回收机制来帮我们解决了这件事。

**什么是GC**

垃圾回收机制（简称GC）是Python解释器自带一种机，专门用来回收不可用的变量值所占用的内存空间

**为什么用垃圾回收机制**

程序运行过程中会申请大量的内存空间，而对于一些无用的内存空间如果不及时清理的话会导致内存使用殆尽（内存溢出），导致程序崩溃，因此管理内存是一件重要且繁杂的事情，而python解释器自带的垃圾回收机制把程序员从繁杂的内存管理中解放出来。

**垃圾回收机制原理分析**

Python的GC模块主要运用了“引用计数”（reference counting）来跟踪和回收垃圾。在引用计数的基础上，还可以通过“标记-清除”（mark and sweep）解决容器对象可能产生的循环引用的问题，并且通过“分代回收”（generation collection）以空间换取时间的方式来进一步提高垃圾回收的效率。







**（一）引用计数**

引用计数就是：变量值被变量名关联的次数

如：age=18

变量值18被关联了一个变量名age，称之为引用计数为1

==引用计数增加==：

age=18 （此时，变量值18的引用计数为1）

m=age （把age的内存地址给了m，此时，m,age都关联了18，所以变量值18的引用计数为2）

==引用计数减少==：

age=10（名字age先与值18解除关联，再与3建立了关联，变量值18的引用计数为1）

del m（del的意思是解除变量名x与变量值18的关联关系，此时，变量18的引用计数为0）

值18的引用计数一旦变为0，其占用的内存地址就应该被解释器的垃圾回收机制回收

```python
a=12  #直接引用
b=a   #间接引用
```

```python
"""循环引用"""
l1=[111,]
l2=[222,]
l1.append(l2)#l1=[值111的内存地址，l2列表的内存地址]
l2.append(l1)#l2=[值222的内存地址，l1列表的内存地址]

print(id(l1[1]))
print(id(l2))

print(id(l2[1]))
print(id(l1))

print(l2)
print(l1[1])

"""结果如下"""
2006853601024
2006853601024

2006561499520
2006561499520

[222, [111, [...]]]
[222, [111, [...]]]
```

[垃圾回收机制](https://zhuanlan.zhihu.com/p/108683483)

# 08、正则表达式

> 学习正则表达式操作字符串
> re模块是用C语言写的没匹配速度非常快
> 其中compile函数根据一个模式字符串和可选的标志参数生成一个正则表达式对象，该对象拥有一系列方法用于正则表大会匹配和替换，re模块也提供了与这下方法功能完全一致的函数，这些函数适用一个模式字符串做为他们的第一个参数

**re.macth方法**

* re.math  尝试**从字符串起始位置匹配**，返回match对象，，否则返回None，适用group()获取匹配成功的字符串
  * 语法：re.match(pattern,string,flags)

|  参数   |                             描述                             |
| :-----: | :----------------------------------------------------------: |
| pattern |                       匹配的正则表达式                       |
| string  |                        要匹配的字符串                        |
|  flags  | 标志位，用于控制正则表达式的匹配方式:如:是否匹配大小写，多行匹配 |

```python
import re 
str='Python is the best language in the world'
result= re.match('P',str)
print(type(result))#<class 're.Match'>
print(result.group())
```

**标志位**

* 如果使用多个标志位，使用|分割，如：re.I|re.M

| 修饰符 |                             描述                             |
| :----: | :----------------------------------------------------------: |
|  re.I  |                     适匹配对大小写不敏感                     |
|  re.L  |                       做本地化识别匹配                       |
|  re.M  |                     多行匹配，影响^ 和$                      |
|  re.S  |                使.匹配包括换行在内的所有字符                 |
|  re.U  |    根据Unicode字符集解析字符，这个标志影响\w，\W ,\b，\B     |
|  re.X  | 该标识符通过给予你更灵活的格式以便于你将正则表达式写得更易于理解。 |

```python
import re 
strData='Python is the best language in the world\
gslfjgldsjgls'
#result= re.match('p',strData,re.I|re.M)#第三个参数 忽略大小写
#print(type(result))#<class 're.Match'>
#print(result.group())
res=re.match('(.*?)is(.*?)',strData,re.I)
print(res.group(1))
print(res.group(2))

```

**常用匹配规则**

|  符号   |           匹配规则           |
| :-----: | :--------------------------: |
| .（点） |  匹配任意1个字符除了换行符   |
|  [abc]  |      匹配abc中任意一个       |
|   \d    |       匹配一个数字0-9        |
|   \D    |          匹配非数字          |
|   \s    |    匹配空白 即空格 tab键     |
|   \S    |          匹配非空格          |
|   \w    | 匹配单词字符 即a-z A-Z 0-9 _ |
|   \W    |        匹配非单词字符        |

**匹配字符数量**

| 符号  |                     匹配规则                      |
| :---: | :-----------------------------------------------: |
|   *   |    匹配前一个字符出现0次或者无限次，即可有可无    |
|   +   |   匹配前一个字符出现1次或者无限次，即至少有1次    |
|  \?   | 匹配前一个字符出现1次或者0次，即要么有1次要么没有 |
|  {m}  |               匹配前一个字符出现m次               |
| {m,}  |             匹配前一个字符至少出现m次             |
| {m,n} |           匹配前一个字符出现从m次到n次            |

#### 8.1、限定匹配数量规则

```python
import re

# * 匹配前一个字符出现0次或者无限次
res=re.match('[a-z][a-z]*','MyPython',re.I)
print(res.group())

# + 匹配前一个字符1次或者无限次  至少一次
res=re.match('[a-zA-Z]+[\w]*','mynAMEDCeisz848s_')
print(res.group())

# ? 匹配前一个字符0次或者1次
res=re.match('[a-zA-Z]+[\d]?','mkohjhjgu8jg8')
print(res.group())

# {min,max} 匹配前一个从min到max次   min max必须是非负整数
#{count}精确匹配次数   {count,}没有限制
res=re.match('\d{4,}','46145')
if res:
    print('匹配成功{}'.format(res.group()))

#匹配邮箱  格式：xxxxxx@163.com
res=re.match('[a-zA-Z0-9]{6,11}@163.com','318129549@163.com')
print(res.group())


```

#### 8.2、原生字符串

```python
# path="D:\\1_zhao_File\\1_MarkDown\MarkDown学习使用篇"
# print(path )
import re


#原生字符串  r
print(re.match(r'c:\\a.text','c:\\a.text').group())


#匹配开头结尾
#^ 匹配字符串开头
#$ 匹配字符串结尾
# res=re.match('^p.*','python is language')
res=re.match('^p[\w]{5}','python is language')
print(res.group())
res=re.match('[\w]{5,15}@[\w]{2,5}.com$','318129549@qq.com')
print(res.group())

```

#### 8.3、分组匹配

```python
#  | 匹配左右任意一个表达式  从左往右
import  re

res=re.match('[\w]*|100','100')
print(res.group())

 # (ab)分组匹配  将括号中字符作为一个分组
res=re.match('([0-9]*)-(\d*)','123456-464651561')
print(res.group())
print(res.group(1))
print(res.group(2))

# \num 的使用
# htmlTag='<html><h1>Python核心编程</h1></html>'
# res1=re.match(r'<(.+)>(.+)>(.+)</\2></\1>',htmlTag)
# print(res1.group(1))


#  分组 别名的使用 (?P<名字>)
data='<div><h1>www.baidu.com</h1></div>'
res=re.match(r'<(?P<div>\w*)><(?P<h1>\w*)>(?P<data>.*)</\w*></\w*>',data)

print(res.group())

```

#### 8.4、编译函数compile

```python
# re.compile 方法
'''
compile将正则表达式模式编译成一个正则表达式对象
reg=re.compile(pattern)
result=reg.match(string)
等效于result=re.match(pattern,string)
使用re.compile和保持所产生的正则表达式对象重用效率更高
'''
import re

#compile 可以把字符串编译成字节码
#优点：在使用正则表达式进行match时，python会将字符串转为正则表达式对象
# 而如果使用compile，只需要转换一次即可，以后在使用模式对象的话无需重复转换

data='1364'
pattern=re.compile('.*')
#使用pattern对象
res=pattern.match(data)
print(res.group())


#re.search方法
#search在全文中匹配一次，匹配到就返回
data='我爱我伟大的祖国，I love China,China is a great country'
rees=re.search('China',data)
print(rees)
print(rees.span())
print(rees.group())
# print(data[21])

#re.findall方法 匹配所有，返回一个列表，

data='华为牛逼是华人的骄傲'
# res =re.findall('华.',data)
# print(res)
pattern=re.compile('华.')
res=pattern.findall(data)
print(res)


# re.sub方法 实现目标搜索和替换
data1='Pythons是很受欢迎的编程语言'
pattern='[a-zA-Z]+' #字符集范围  +代表 前导字符模式出现1从以上
res=re.sub(pattern,'C#',data1)
resn=re.subn(pattern,'C#',data1)
print(res)
print(resn)
#re.subn 完成目标的的搜索和替换 还返回被替换的数量，以元组的形式返回

#re.split  是新分割字符串
data='百度，腾讯，阿里，华为，360，字节跳动'
print(re.split(',',data))

```

#### 8.5贪婪模式和非贪婪模式

```python
'''
python 中默认是贪婪的，总是贪婪的匹配尽可能多的字符，非贪婪相反，总是尝试匹配尽可能少的字符
在  ” * ？ + {m,n}"后面加上 ? 使贪婪变成非贪婪

'''
#贪婪
import  re
res=re.match('[\d]{6,9}','111222333')
print(res.group())


#非贪婪
res=re.match('[\d]{6,9}?','111222333')
print(res.group())


content='asdfbsdbdsabsd'
# pattern=re.compile('a.*b')# 贪婪
pattern=re.compile('a.*?b')#非贪婪
res=pattern.search(content)
print(res.group())
#0710-49

```xxxxxxxxxx23 1'''2python 中默认是贪婪的，总是贪婪的匹配尽可能多的字符，非贪婪相反，总是尝试匹配尽可能少的字符3在  ” * ？ + {m,n}"后面加上 ? 使贪婪变成非贪婪45'''6#贪婪7import  re8res=re.match('[\d]{6,9}','111222333')9print(res.group())101112#非贪婪13res=re.match('[\d]{6,9}?','111222333')14print(res.group())151617content='asdfbsdbdsabsd'18# pattern=re.compile('a.*b')# 贪婪19pattern=re.compile('a.*?b')#非贪婪20res=pattern.search(content)21print(res.group())22#0710-4923python
```xxxxxxxxxx24 1'''2python 中默认是贪婪的，总是贪婪的匹配尽可能多的字符，非贪婪相反，总是尝试匹配尽可能少的字符3在  ” * ？ + {m,n}"后面加上 ? 使贪婪变成非贪婪45'''6#贪婪7import  re8res=re.match('[\d]{6,9}','111222333')9print(res.group())101112#非贪婪13res=re.match('[\d]{6,9}?','111222333')14print(res.group())151617content='asdfbsdbdsabsd'18# pattern=re.compile('a.*b')# 贪婪19pattern=re.compile('a.*?b')#非贪婪20res=pattern.search(content)21print(res.group())22#0710-492324```xxxxxxxxxx23 1'''2python 中默认是贪婪的，总是贪婪的匹配尽可能多的字符，非贪婪相反，总是尝试匹配尽可能少的字符3在  ” * ？ + {m,n}"后面加上 ? 使贪婪变成非贪婪45'''6#贪婪7import  re8res=re.match('[\d]{6,9}','111222333')9print(res.group())101112#非贪婪13res=re.match('[\d]{6,9}?','111222333')14print(res.group())151617content='asdfbsdbdsabsd'18# pattern=re.compile('a.*b')# 贪婪19pattern=re.compile('a.*?b')#非贪婪20res=pattern.search(content)21print(res.group())22#0710-4923pythonpython
```xxxxxxxxxx26 1'''2python 中默认是贪婪的，总是贪婪的匹配尽可能多的字符，非贪婪相反，总是尝试匹配尽可能少的字符3在  ” * ？ + {m,n}"后面加上 ? 使贪婪变成非贪婪45'''6#贪婪7import  re8res=re.match('[\d]{6,9}','111222333')9print(res.group())101112#非贪婪13res=re.match('[\d]{6,9}?','111222333')14print(res.group())151617content='asdfbsdbdsabsd'18# pattern=re.compile('a.*b')# 贪婪19pattern=re.compile('a.*?b')#非贪婪20res=pattern.search(content)21print(res.group())22#0710-492324```xxxxxxxxxx23 1'''2python 中默认是贪婪的，总是贪婪的匹配尽可能多的字符，非贪婪相反，总是尝试匹配尽可能少的字符3在  ” * ？ + {m,n}"后面加上 ? 使贪婪变成非贪婪45'''6#贪婪7import  re8res=re.match('[\d]{6,9}','111222333')9print(res.group())101112#非贪婪13res=re.match('[\d]{6,9}?','111222333')14print(res.group())151617content='asdfbsdbdsabsd'18# pattern=re.compile('a.*b')# 贪婪19pattern=re.compile('a.*?b')#非贪婪20res=pattern.search(content)21print(res.group())22#0710-4923python25```xxxxxxxxxx24 1'''2python 中默认是贪婪的，总是贪婪的匹配尽可能多的字符，非贪婪相反，总是尝试匹配尽可能少的字符3在  ” * ？ + {m,n}"后面加上 ? 使贪婪变成非贪婪45'''6#贪婪7import  re8res=re.match('[\d]{6,9}','111222333')9print(res.group())101112#非贪婪13res=re.match('[\d]{6,9}?','111222333')14print(res.group())151617content='asdfbsdbdsabsd'18# pattern=re.compile('a.*b')# 贪婪19pattern=re.compile('a.*?b')#非贪婪20res=pattern.search(content)21print(res.group())22#0710-492324```xxxxxxxxxx23 1'''2python 中默认是贪婪的，总是贪婪的匹配尽可能多的字符，非贪婪相反，总是尝试匹配尽可能少的字符3在  ” * ？ + {m,n}"后面加上 ? 使贪婪变成非贪婪45'''6#贪婪7import  re8res=re.match('[\d]{6,9}','111222333')9print(res.group())101112#非贪婪13res=re.match('[\d]{6,9}?','111222333')14print(res.group())151617content='asdfbsdbdsabsd'18# pattern=re.compile('a.*b')# 贪婪19pattern=re.compile('a.*?b')#非贪婪20res=pattern.search(content)21print(res.group())22#0710-4923pythonpython26```xxxxxxxxxx25 1'''2python 中默认是贪婪的，总是贪婪的匹配尽可能多的字符，非贪婪相反，总是尝试匹配尽可能少的字符3在  ” * ？ + {m,n}"后面加上 ? 使贪婪变成非贪婪45'''6#贪婪7import  re8res=re.match('[\d]{6,9}','111222333')9print(res.group())101112#非贪婪13res=re.match('[\d]{6,9}?','111222333')14print(res.group())151617content='asdfbsdbdsabsd'18# pattern=re.compile('a.*b')# 贪婪19pattern=re.compile('a.*?b')#非贪婪20res=pattern.search(content)21print(res.group())22#0710-492324```xxxxxxxxxx23 1'''2python 中默认是贪婪的，总是贪婪的匹配尽可能多的字符，非贪婪相反，总是尝试匹配尽可能少的字符3在  ” * ？ + {m,n}"后面加上 ? 使贪婪变成非贪婪45'''6#贪婪7import  re8res=re.match('[\d]{6,9}','111222333')9print(res.group())101112#非贪婪13res=re.match('[\d]{6,9}?','111222333')14print(res.group())151617content='asdfbsdbdsabsd'18# pattern=re.compile('a.*b')# 贪婪19pattern=re.compile('a.*?b')#非贪婪20res=pattern.search(content)21print(res.group())22#0710-4923python25```xxxxxxxxxx24 1'''2python 中默认是贪婪的，总是贪婪的匹配尽可能多的字符，非贪婪相反，总是尝试匹配尽可能少的字符3在  ” * ？ + {m,n}"后面加上 ? 使贪婪变成非贪婪45'''6#贪婪7import  re8res=re.match('[\d]{6,9}','111222333')9print(res.group())101112#非贪婪13res=re.match('[\d]{6,9}?','111222333')14print(res.group())151617content='asdfbsdbdsabsd'18# pattern=re.compile('a.*b')# 贪婪19pattern=re.compile('a.*?b')#非贪婪20res=pattern.search(content)21print(res.group())22#0710-492324```xxxxxxxxxx23 1'''2python 中默认是贪婪的，总是贪婪的匹配尽可能多的字符，非贪婪相反，总是尝试匹配尽可能少的字符3在  ” * ？ + {m,n}"后面加上 ? 使贪婪变成非贪婪45'''6#贪婪7import  re8res=re.match('[\d]{6,9}','111222333')9print(res.group())101112#非贪婪13res=re.match('[\d]{6,9}?','111222333')14print(res.group())151617content='asdfbsdbdsabsd'18# pattern=re.compile('a.*b')# 贪婪19pattern=re.compile('a.*?b')#非贪婪20res=pattern.search(content)21print(res.group())22#0710-4923pythonpythonpythonpython