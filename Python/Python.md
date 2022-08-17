## Python基础语法
****
>涵盖Python基础语法，掌握基础的编程能力
>
>****
#### 认识Python
>Python是一种**面向对象**的**解释型计算机程序设计语言**，由吉多.范罗苏姆开发，第一个公开发行版发布于1991年，它常被成为**胶水语言**，能够把其他语言制作的各种模块（尤其是C/C++），恒轻松的联结在一起。
1. **Python优点**
   
   1. 简单易学
   2. 免费开源
      >Python**开源**，开发者可以自由的下载，阅读，甚至是修改Python源代码
   4. 丰富的第三方库
      >Python具有本身且丰富而且**强大的库**，而且由于Python的**开源性**，第三方库也非常多，例如：在web开发有django.flask,Tornado，爬虫scrapy，科学计算numpy.pandas等等
   6. 可以移植
      >由于Python是开源的，他已经被移植到了大多数平台下面，例如：Windows，MacOS，Linux，Andorid,iOS等等
   7. 面向对象
      >Python既**支持面向过程，又支持面向对象**，这样编程就更加灵活
2. **Python缺点**
   1. 运行速度慢
      >C程序相比非常慢，因为**Python是解释型语言**，代码在执行时会一行一行的翻译成CPU能够理解的机器码，这个翻译过程非常的耗时，所以很慢，而C程序时运行前直接编译成CPU能执行的机器码，所以**相对Python而言，C语言执行非常快**
   2. 代码不能加密
      >要发布你写的程序，实际上时发布源代码，而是**解释型的语言**，则必须把源代码发布出去
   3. 强制的缩进
      >Python有非常**严格的缩进语法**，只要缩进错误程序立马崩溃
   4. GIL全局解释器锁
      >在任意时刻，只有一个线程在解释器中运行，对Python虚拟机的访问由全局解释器锁（GIL）来控制，正式这个锁能够保证同一时刻只有一个线程在运行，遇到i/o阻塞的时候会释放（GIL）所以Python的多线程并不是真正的多线程，而是CPU执行速度非常快，让人感觉不到GIL的存在。（GIL）会在Python高级阶段讲解


    ![](1.png)
                
                                   学了Python能干什么


#### 掌握Python注释
>注释是编写程序时，写程序的人给人一句、程序段、函数等的**解释或提示**
注释的作用：
        注释可以起到一个备注的作用，这个方法函数，变量到底是干嘛的，如果没有注释时间长了即使是自己写的代码，可能都不知道这个代码到底是干嘛的，所以注释起到的作用就是方便自己查看写过的代码，别人来接收你的代码能看懂，简单来将就是提高程序代码的可读性，以便于以后的参看、修改
Python中**单行注释用#号，**#号右边就是注释的内容，Python解析器遇到#号就会当作注释，不会去解析#号后面的内容
**多行注释  '''代码....... '''**
****
#### 掌握Python变量的定义与命名规则


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
****
#### 掌握Python基本操作符    
* 算数运算符：+  -  *  /   **（指数） %（取余） //（相除取整）


定两个变量a=7,y=3
|算数运算符|作用描述|示例|
| :----: |:----:| :---: |
|+ 加法|算术加法 |a+b=10|
|-减法|算数减法|a-b=4|
|*乘法|算数乘法|x*y=21|
|**指数 |左边的数是底数，右边的数是指数| a* *b=343|
|%取余|x%y x除以y的余数|a%b=1|
|/除法|x/y结果包含小数点后面的数|a/b=2.33333333335|
|//相除取整|x//y 结果是忽略小数点后面的小数位，只保留整数位|a//b=2|

* 比较运算符：==（等于） !=（不等于）  >   <     >=     <=  

返回布尔型 True False

|比较运算符|名称|示例|结果描述|
|:----:|:----:| :---: |:----:|
|== |等于 |x==y|如果x恰好等于y，则为真|
|!=|不等于|x!=|如果x恰好不等于y 则为真|
|>|大于|x>y|如果x(左侧参数)大于y(右边参数),则为真|
|<|小于|x< y|如果x(左侧参数)小于y(右边参数),则为真|
|>=|大于或等于|x>=y|如果x(左侧参数)大于或者等于y(右边参数),则为真|
|<+|小于或者等于|x<=y|如果x(左侧参数)小于或者等于y(右边参数),则为真|

* 逻辑运算符  and  or not 


|逻辑运算符|实例|结果描述|
|:----:|:----:|:----:|
|and| x and y| x,y同为真，则结果为真，如果有一个为假，则结果为假|
|or| x or y| x, y有一个为真，结果就为真，全部为假，则结果为假|
|not| not x| 取反，如果x为真，则结果为假，如果x为假，则结果为真|


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
|赋值运算符|作用描述|结果描述|
|:----:|:----:|:----:|
|=|赋值运算符|将=号右边的值赋值给左边的变量|
|+=|加法赋值运算符|c+=a等效于c=c+a|
|-=|减法赋值运算符|c-=a等效于c=c-a|
|*=|乘法赋值运算符|c*=a等效于c=c*a|
|/=|除法赋值运算符|c/=a等效于c=c/a|
|%=|取模赋值运算符|c%=a等效于c=c%a|
|**=|幂赋值运算符|c* *=a等效于c=c* *a|
|//=|取整赋值运算符|c//=a等效于c=c//a|
```python
a,b,c,d=23,10,18,3
a+=3
print(a)
```
****
#### 掌握Python输入输出
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
****
## 01、Python流程控制
* Python条件语句是通过一条或多条语句的执行结果【True/False】来决定执行的代码块
* 就是计算机执行代码的顺序
* 流程控制：对计算机代码执行的顺序进行有效的管理，只有流程控制才能实现在开发当中的业务逻辑
* 流程控制的分类：
    * 顺序流程  ：就是代码一种自上而下的执行结构，也是python默认的流程
    * 选择流程/分支流程：根据在某一步的判断，有选择的去执行相应的逻辑的一种结构
         * 单分支   if 条件表达式：
         * 双分支   if 条件表达式：     else:
         * 多分支    if 条件表达式：   elif 条件表达式：   elif 条件式：     else:
      * 条件表达式:比较运算符/逻辑运算符/符合运算符
   * 循环流程： 在一定的条件下，一直重复的去执行某段代码的逻辑【事情】
      *   while 条件表达式：
      * for 变量 in  可迭代的集合对象
****
#### 单分支
```python
score = 80
if score<=60: #满足条件就会输出打印提示
    print('成绩不太理想，继续加油')
    pass  #空语句
print('语句运行结束')
```
****
#### 双分支
```python
if score>60:
    print('成绩不错')
else:
    print("成绩不合格，继续努力")
```
****
#### 多分支
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
****
#### if-else 嵌套
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
****
## 02、循环分类 while for
#### while语法结构

>语法特点
                # 1.有初始值
                # 2.条件表达式
                # 3.变量【循环体内计数变量】的自增自减，否则会造成死循环


* 使用条件：循环的次数不确定，是依靠循环条件来结束
* 目的： 为了将相似或者相同的代码操作变得更加简洁，使得代码可以使用重复利用
****
#### for循环
*  遍历操作，依次的取集合容器中的每个值
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
>有三点要求：
1.允许用户最多尝试3次
 2.每尝试3次后，如果还没有猜对，就问用户是否还想继续玩，如果回答Y或y,就让继续猜3次，以此往后，如果回答N或n，就退出程序
 3.如果猜对了，直接退出
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
>低于18.5 过轻
18.5-25 正常
25-28 过重
28-32 肥胖
高于32 严重肥胖
用if-elif 判断并打印出来
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
****
## 03、Python数据类型
#### 字符串
* 序列:一组按顺序排列的值，【数据集合】

* Python 中存在3中内置的序列类型：
'''
字符串  列表  元组
'''
* 序列优点：支持索引和切片操作
* 特征:第一个正索引为0，指向的是左端，  第一个索引为负数，指向的是右端
* 切片是指截取字符串中的某一段内容，
* 切片使用语法：[起始下标：结束下标：步长] 切片截取的内容不包含结束下标对应的数据，步长指的是隔几个下标获取一个字符
* 下标会越界，切片不会：

```python

Test='python'
print(type(Test))
print(Test[0])#获取第一个字符
print('获取第二个字符%s'%Test[1])
for i in Test:
    print(i,end=' ')
```
```python
name='peter'
print('首字母大写：%s'%name.capitalize())
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
```python
str="I Love Python"
print(str.find('P'))#返回对应的下标 查找目标对象在序列中对应的位置 没找到返回-1
print(str.index('v'))#index()检测字符串中是否包含子字符串，返回对应下标，没找到就报错
print(str.startswith('I'))#返回布尔型 判断是否以什么什么开始
print(str.endswith('i'))#判断是否以什么什么结尾
print(str.lower())#全部转小写
print(str.upper())#全部转大写

st='hello world'
print(st[0])
print(str[::-1])#逆序输出
print(st[2:5])#从下标2开始不包含5【左闭右开】
print(st[2:])#从第三个字符到最后
print(st[0:3])#从0开始可以省略0
print(st[:3])
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
* 查找
```python

listA=['abcd',123,456,798,True,12.255]
print(listA)
print(listA[0])#找出第一个元素
print(listA[1:3])
print(listA[3:])
print(listA[::-1])#倒叙
print(listA*3)#输出多次列表中数据【复制多次】
```
* 添加
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
* 修改
```python
print('-----------------修改--------------')
print('修改之前',listA)
listA[0]='Peter'
print('修改之后',listA)
```
* 删除
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
****
#### 元组
* 元组是不可变序列，在创建后不能做任何修改
* 可变
* 用()来创建元组类型，数据项用逗号来分割
* 可以是任何类型
* 当元组中只有一个元素时，要加上逗号，不然解释器会当作整形处理
* 同样支持切片操作
  
* 查找
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
* 修改
```python

tupleA[3][0]=250#对元组中的列表类型的数据进行修改

tupleB=tuple(range(10))
print(tupleB.count(9))
```
****
#### 字典
* 字典不是序列类型，没有下标的概念，是一个无序的键值集合，是内置的高级数据类型

* 字典可以存储任意对象
* 字典以键值对的形式创建的{key:value}利用大括号包裹

* 字典中中某个元素时，时根据键 ，值 字典的每个元素由2部分组成：键：值
* 访问值的安全方法get()方法，在我们不确定字典中是否存在某个键而又想获取其值时，可以使用get()方法，还可以设置默认的值


* 添加字典数据
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
* 删除
```python

del dictA['name']
dictA.pop('age')
print(dictA)
```
****
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
****
## 04、Python函数
>掌握Python函数基础，为开发项目打下坚实的基础

#### 函数初识

* 在编写程序的过程中，有某一功能代码块出现多次，但是为了提高编写的效率以及代码的重用，所以把具有独立功能的代码块组织为一个小模块，这就是函数
* 就是一系列Python语句的组合，可以在程序中运行一次或者多次，
* 一般具有独立的功能
* 代码的复用最大化以及最小化冗余代码，整体代码结构清晰，问题局部化
* 函数定义
  >def 函数名():
       函数体【一系列语句，表示独立的功能】
  函数调用
  调用之前先定义
   函数名加（）即可调用
  函数说明文档：
   函数内容的第一行可以用字符串进行 函数说明

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
* 函数 调用
```python
pritInfo()
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
****
#### 函数参数
* 参数分类：
>必选参数，默认参数【缺省参数】，可选参数，关键字参数
* 参数：其实就是函数为了实现某项特定功能，进而为了得到实现功能所需要的数据
  为了得到外部数据
  def sum(a,b):#形式参数：只是意义上的一种参数，在定义的时候是不占内存地址的

     sum=a+b
     print(sum)
* 必选参数
>函数调用的时候，必选参数是必须要赋值的
sum(20,15)#20,15就是实际参数  实参，  实实在在的参数，是实际占用内存地址的
sum()不能这样写，需要传参

* 默认参数【缺省参数】
```python

def sum(a=1,b=11):
     print(a+b)

sum()
```
* sum(10)#在调用的时候，如果未赋值，就用定义函数时给定的默认值

* 可变参数（当参数的个数不确定时，比较灵活）
```python
def getComputer(*args):
    '''
    计算累加和
    '''
    # print(args)
    result=0
    for item in args:
        result+=item
    print(result)

getComputer(1)

getComputer(1,2,3,4)
```
* 关键字可变参数
1. **来定义
2. 在函数体内 参数关键字是一个字典类型，key是一个字符串
```python
# def Func(**kwargs):
#     print(kwargs)
# Func(name='赵颂')
# def Infofunc(*args,**kwargs): #可选参数必须在关键字可选参数之前
#     print(args)
#     print(kwargs)
# Infofunc(1,2)
# Infofunc(12,13,46,age=18)
```
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
****
#### 函数返回值
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
****
#### 函数嵌套
```python
def fun1():
    print("------------------fun1start")
    print("--------------------执行代码省略")
    print("-------------------fun1end")
def fun2():
    print('------------------fun2start')
    #调用第一个函数
    fun1()
    print("---------------------fun2end")
fun2()

#函数分类：根据函数返回值和函数参数决定
#有参数无返回值
#有参数有返回值
#无参数无返回值
#无参数有返回值
```
****
#### 函数全局变量

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
****
#### 引用
* 在Python中，值是靠引用来传递来的，可以用id()查看一个对象的引用是否相同.
* id是值保存在内存中那块内存地址的标识.

* 不可变类型
```python
a=1
def func(x):
    print('x的地址{}'.format(id(x)))
    x=12
    print('修改之后的x 的地址:{}'.format(id(x)))

print('a的地址：{}'.format(id(a)))
func(a)

```

* 可变类型
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
****
#### 匿名函数

* 语法
>lambda 参数1、参数2、参数3：表达式

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
****
#### 递归函数
> 如果一个函数在内部不调用其他函数，而是调用自己本身，这个函数就是递归函数

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
#### 内置函数 数学运算
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
****
#### 类型转换函数
```python
#chr() 数字转字符
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
****
#### 序列操作函数
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
****
#### set集合
```python
#set 不支持索引切片，是一个无序的且不重复的容器
#类似于字典，但是只有Key,没有value



set1={1,2,3}
set1.add(123)#添加操作
set1.clear() #清空

print(set1)
List1=['1','2','24']
set2=set(List1)
print(set2)
re=set1.difference(set2)# 差集， a中有的b中没有的
print(re)
print(set1-set2)#差集
print(set1.intersection(set2))#交集
print(set1&set2)#交集
print(set1.union(set2)) #并集
print(set1 | set2)#并集
#pop 就是从集合中拿数据并且同时删除
print(set1)
set1.pop()
print(set1)
set1.discard(3)#指定移除元素
print(set1)

#update两个集合
set1.update((set2))
print(set1)
```
****
## 05、Python面向对象
>介绍Python面向对象开发，为开发项目打下坚实的基础
#### oop介绍
>面向对象编程 oop  【object oriented programming】是一种Python的编程思路

* 面向过程：
在思考问题的时候，怎么按照步骤去实现，
然后将问题解决拆分成若干个步骤，并将这些步骤对应成方法一步一步的最终完成功能
* 面向过程：就是一开始学习的，按照解决问题步骤去编写代码【根据业务逻辑去写代码】
* 面向对象：关注的是设计思维【找洗车店， 给钱洗车】


* 从计算机角度看：面向过程不适合做大项目
* 面向过程关注的是：怎么做
* 面向对象关注的是：谁来做
****
#### 类和对象

* 类：是一个模板，模板里包含多个函数，函数里实现一些功能

* 对象：则是根据模板创建的实例，通过实例对象可以执行类中的函数

* 类由3部分构成：
  * 类的名称：类名
  *   类的属性：一组数据
  * 类的方法：允许对进行操作的方法（行为）

>例如：创建一个人类
 事物的名称（类名）：人（Person）
 属性：身高，年龄
 方法；吃 跑.....

* 类是具有一组 相同或者相似特征【属性】和行为【方法】的一系列对象的集合

 >现实世界    计算机世界
行为------》方法
特征------》属性

* 对象：是实实在在的一个东西，类的具象化 实例化
* 类是对象的抽象化 而对象是类的实例
****
#### 定义类
 >#定义类和对象
#类名：采用大驼峰方式命名


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


#创建对象【类的实例化】
xm=Person()
xm.eat()#调用函数
xm.run()
print('{}的年龄是{}'.format(xm.name,xm.age))
```
****
#### __init__方法
```python
# 如果有n个这样对象 被实例化，那么就需要添加很多次实例属性，显然比较麻烦
class Person1:
    def __init__(self):#魔术方法
        '''
        实例属性的声明
        '''
        self.name='小勇'
        self.age=22
        self.sex='男生'
    def run(self):
        print('跑太快了吧')
xy=Person1()
# xy.run()
print(xy.age)
```
****
#### self理解
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
****
#### 魔术方法
>魔术方法： __ xxx __

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
****
#### 析构方法
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
****
#### 单继承
**Python中展现面向对象的三大类型： 封装，继承，多
态**

1. 封装：值得是把内容封装到某个地方，便于后面的使用
 他需要：
    把内容封装到某个地方，从另外一个地方去到调用被封装的内容
    对于封装来说，其实就是使用初始化构造方法将内容封装到对象中，然后通过对象直接或者self来获取被封装的内容

2. 继承：和现实生活当中的继承是一样的，也就是子可以继承父的内容【属性和行为】（爸爸有的儿子有， 相反， 儿子有的爸爸不一定有），
3. 所谓多态，定义时的类型和运行时的类型是不一样，此时就成为多态
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
****
#### 多继承
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
#在执行eaet方法时，顺序应该是
#首先到A里面去找，如果A中没有，则就继续去B类中去查找，如果B中没有，则去C中查找，
#如果C类中没有，则去D类中查找，如果还没有找到，就会报错
#A-B-C-D 也是继承的顺序

```
****
#### 重写父类方法
* 所谓重写，就是子类中，有一个和父类相同名的方法，在子类中的方法会覆盖掉子类中同名的方法，
* 为什么要重写，父类的方法已经不满足子类的需要，那么子类就可以重写父类或者完善父类方法
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
****
#### 多态
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
****
#### 类属性和实例属性
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
****
#### 类方法和静态方法
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
****
#### 私有化属性

* 语法：
    * 两个下划线开头，声明该属性为私有，不能在类的外部被使用或直接访问
* 使用私有化属性的场景
    1. 把特定的一个属性隐藏起来，不想让类的外部进行直接调用
    2. 我想保护这个属性，不想让属性的值随意的改变，
    3. 保护这个属性，不想让派生类【子类】去继承

* 要想访问私有变量一般是写两个方法，一个访问，一个修改，由方法去控制访问
>class Person:
    __age =18 #实例一个私有化属性，属性名字前面加两个下划线
'''

```python




class Person:
    __hobby = 'dance'

    def __init__(self):
        self.__name = '李四'  # 加两个下划线将此属性私有化,不能再外部直接访问，在类的内部可以访问
        self.age = 30

    def __str__(self):
        return '{}的年龄是{}，喜欢{}'.format(self.__name, self.age, Person.__hobby)  # 调用私有化属性

    def change(self, hobby):
        Person.__hobby = hobby


class Studeng(Person):
    def printInfo(self):
        # print(self.__name)#访问不了父类中的私有属性
        print(self.age)

    pass


xl = Person()
# print(xl.name)#通过类对象，在外部访问的
print(xl)
xl.change('唱歌')  # 修改私有属性的值
print(xl)
stu = Studeng()
stu.printInfo()
stu.change('Rap')
print(stu)
# print(xl.hobby)  # 通过实例对象访问类属性
# print(stu.hobby)  # 通过实例对象访问类属性
# print(Person.hobby)  # 通过类对象访问类属性


```
'''
* 小结：
    1. 私有化的【实例】属性，不能在外部直接的访问，可以在类的内部随意的使用
    2. 子类不能继承父类的私有化属性，【只能继承父类公共的属性和行为】
    3. 在属性名的前面直接加__，就可以将其私有化
    '''
* 单下划线  双下划线， 头尾双下划线
>_xxx前面加下划线，以单下划线开头表示的是protected类型的变量，即保护类型只能允许其本身与子类进行访问，不能使用from xxx import *的方式导入
__xxx__前后两个下划线，魔术方法，一般是python自带的，开发者不要创建这类型的方法
xxx_后面单下划线，避免属性名与Python关键字冲突
****
#### 私有化方法
* 概述
    * 私有化方法跟私有化属性一样，有些重要的方法，不允许外部调用，防止子类意外重写，把普通的方法设置成私有化方法
    * 私有化方法一般是类内部调用，子类不能继承外部不能调用
* 语法
> class A:
    def __myname(self):  #在方法名前面加两个下划线
        print('小明')


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
****
#### Property函数

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


## 06、文件操作与垃圾回收机制
>学习使用Python操作文件，了解Python的垃圾回收机制
* 文件操作：
  1. 打开文件：open()
  2. WriteFile=open("./Test.text", mode="w",encoding="utf-8")
* 读/写操作
  1. WriteFile.write("在苍茫的大海上")
  2. WriteFile.write("你好中国")
* 关闭文件
  1. WriteFile.close()
  

**以二进制的形式写数据**
```python

'''
fobj=open("./Test1.text","wb")# str--->bytes
fobj.write("在乌云和大海之间".encode("utf-8"))
fobj.close()
'''

```

**二进制形式追加**

```python
'''
#追加不需要encode()
#fobj=open("Test.text","a")# 追加数据
#fobj.write("在乌云和大海之间\r\n")
#fobj.write("海燕像黑色的闪电\r\n")
fobj=open("./Test1.text","ab")# str--->bytes
fobj.write("今天诗兴大发\n".encode("utf-8"))
fobj.close()
'''
```
**读文件数据**
```python
#fobj=open('./Test.text','r')
fobj=open('./Test.text','rb')
data=fobj.read()
print(data)
print(data.decode("utf-8"))#读取所有数据
#print(fobj.read(12))#读取两个数据
#print(fobj.readline())#读取一行数据
#print(fobj.readlines())#读取所有行，返回列表
fobj.close()#文件对象关闭掉
```

* with语句，不管在处理文件过程中是否发生异常，都能够保证with语句执行完毕后已经关闭打开的文件句柄
* with上下文管理对象
* 优点：自动释放关联的对象
```python
with open("withfile.text",'r') as f:
    #f.write("123木头人")
    print(f.read())
```
* read r r+ rb rb+
* r r+ 只读， 适用普通的读取场景
* rb rb+ 适用 文件 ，图片，视频，音频这样的文件读取
* write w w+ wb wb+ a ab
* w wb+ w+ 每次都会创建文件
* 二进制读写的时候注意编码问题，默认情况下，写入文件编码是gbk
* a ab a+在原有文件的末尾追加，【文件指针的末尾】，不会每次创建新的文件



## 07、正则表达式
>学习正则表达式操作字符串
> re模块是用C语言写的没匹配速度非常快
其中compile函数根据一个模式字符串和可选的标志参数生成一个正则表达式对象，该对象拥有一系列方法用于正则表大会匹配和替换，re模块也提供了与这下方法功能完全一致的函数，这些函数适用一个模式字符串做为他们的第一个参数

**re.macth方法** 

* re.math  尝试**从字符串起始位置匹配**，返回match对象，，否则返回None，适用group()获取匹配成功的字符串
    * 语法：re.match(pattern,string,flags)


|参数|描述|
|:----:|:----:|
|pattern|匹配的正则表达式|
|string|要匹配的字符串|
|flags|标志位，用于控制正则表达式的匹配方式:如:是否匹配大小写，多行匹配|

```python
import re 
str='Python is the best language in the world'
result= re.match('P',str)
print(type(result))#<class 're.Match'>
print(result.group())
```

**标志位**

* 如果使用多个标志位，使用|分割，如：re.I|re.M
  
|修饰符|描述|
|:----:|:----:|
|re.I|适匹配对大小写不敏感|
|re.L |做本地化识别匹配|
|re.M |多行匹配，影响^ 和$|
|re.S|使.匹配包括换行在内的所有字符|
|re.U|根据Unicode字符集解析字符，这个标志影响\w，\W ,\b，\B|
|re.X|该标识符通过给予你更灵活的格式以便于你将正则表达式写得更易于理解。|
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
|符号|匹配规则|
|:----:|:----:|
|.（点）|匹配任意1个字符除了换行符|
|[abc]|匹配abc中任意一个|
|\d |匹配一个数字0-9|
|\D |匹配非数字|
|\s |匹配空白 即空格 tab键|
|\S |匹配非空格|
|\w |匹配单词字符 即a-z A-Z 0-9 _|
|\W |匹配非单词字符|

**匹配字符数量**
|符号|匹配规则|
|:----:|:----:|
|*|匹配前一个字符出现0次或者无限次，即可有可无|
|+|匹配前一个字符出现1次或者无限次，即至少有1次|
|\?|匹配前一个字符出现1次或者0次，即要么有1次要么没有|
|{m}|匹配前一个字符出现m次|
|{m,}|匹配前一个字符至少出现m次|
|{m,n}|匹配前一个字符出现从m次到n次|

#### 限定匹配数量规则
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

#### 原生字符串
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
#### 分组匹配
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

#### 编译函数
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
#### 贪婪模式和非贪婪模式
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

```

##	08、模块