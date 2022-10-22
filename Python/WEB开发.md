## 纯手撸web框架

```python
# HTTP协议
"""
网络协议
HTTP协议： 数据传输是明文
HTTPS协议：数据传输是密文
websocket协议:数据传输是密文

四大特性 :
	1.基于请求响应
	2.基于TCP/IP，作用于应用层之上的协议
	3.无状态
	4.短/无链接

数据格式:
	请求首行
	请求头
	\r\n
	请求体
	
	
响应状态码：
	1xx
	2xx   200
	3xx
	4xx   403 403
	5xx   500
	
"""
```



```python
# -*- coding: UTF-8 -*- 
# @Date ：2022/10/17 15:47

import socket

server = socket.socket()

server.bind(('127.0.0.1', 8888))
server.listen(5)

while True:
    conn, addr = server.accept()
    data = conn.recv(1024).decode('utf-8')
    # print(data)
    conn.send(b'HTTP/1.1 200 OK\r\n\r\n')
    current_path = data.split(' ')[1]   #拿到用户输入的后缀，
    # print(current_path)
    if current_path=='/index':   #根据后缀进行判断
        conn.send(b'index')
    elif  current_path=='/login':
        conn.send(b'login')
    elif current_path=='/study':
        with open(r'费曼学习法.html','rb') as fp:
            conn.send(fp.read())

    conn.send(b'hello web')
    conn.close()
    
    
 # 不足之处
	1. 代码重复，（服务端代码所有人都要重复写）
	2. 手动处理HTTP格式的数据，并且只能拿到url后缀，其他数据获取繁琐（数据格式一样，处理的代码也大致一样，重复写）
    3.并发问题
```

## 基于wsgiref模块（web服务网关接口）

```python
# -*- coding: UTF-8 -*- 
# @Date ：2022/10/17 16:14


from wsgiref.simple_server import make_server


def run(env, response):
    """

    :param env: 请求相关的所有数据
    :param response:响应相关的所有数据
    :return:返回给浏览器的数据
    """
    print(env)  # 大字典   wsgiref模块帮你处理好http格式的数据的数据，封装成了字典，操作更加方便
    # 从env中取
    response('200 OK', [])  # 响应首行+响应头
    current_path = env.get('PATH_INFO')
    if current_path == '/index':
        return [b'index']
    elif current_path == '/login':
        return [b'login']
    return [b'404 error']


if __name__ == '__main__':
    server = make_server('127.0.0.1', 8888, run)
    '''会实时监听127.0.0.1：8888地址，只要有客户端来，都会交给run函数处理（触发run函数的运行）'''
    server.serve_forever()  # 启动服务端
```

```python
# -*- coding: UTF-8 -*- 
# @Date ：2022/10/17 16:14


from wsgiref.simple_server import make_server


def index(env):
    return 'index'


def login(env):
    return 'login'


def error(env):
    return '404 error'


urls = [
    ('/index', index),
    ('/login', login),
]


def run(env, response):
    """

    :param env: 请求相关的所有数据
    :param response:响应相关的所有数据
    :return:返回给浏览器的数据
    """
    print(env)  # 大字典   wsgiref模块帮你处理好http格式的数据的数据，封装成了字典，操作更加方便
    # 从env中取
    response('200 OK', [])  # 响应首行+响应头
    current_path = env.get('PATH_INFO')
    # if current_path == '/index':
    #     return [b'index']
    # elif current_path == '/login':
    #     return [b'login']
    # return [b'404 error']

    # 定义变量，存储匹配到的函数名
    func = None
    for url in urls:
        if current_path == url[0]:
            # 将url对应的函数名赋值给func
            func = url[1]
            break  # 匹配到一个之后，立刻结束当前循环
    # 判断func是否有值
    if func:
        res = func(env)
    else:
        res = error(env)
    return [res.encode('utf-8')]


if __name__ == '__main__':
    server = make_server('127.0.0.1', 8888, run)
    '''会实时监听127.0.0.1：8888地址，只要有客户端来，都会交给run函数处理（触发run函数的运行）'''
    server.serve_forever()  # 启动服务端
```

* **根据功能的不同拆分成不同的py文件**

```python
"""
urls.py  路由与视图函数的对应关系
views.py 视图函数（主要后端业务逻辑）
templates文件夹 专门用来存储html文件
"""

#按照功能的不同，只需要在urls.py中书写对应关系，然后去views.py中书写业务逻辑
```

![image-20221020093951059](E:\MarkDown\markdown\imgs\image-20221020093951059.png)

![image-20221020094002008](E:\MarkDown\markdown\imgs\image-20221020094002008.png)

![image-20221020094021483](E:\MarkDown\markdown\imgs\image-20221020094021483.png)

## 动静态网页

```python
"""

静态网页
	页面上的数据是直接写死的，一直不会变
动态网页
	数据是实时获取的
	eg:
		1.后端获取当前时间展示到html页面上
		2.数据是从数据库中获取的展示到html页面上
		
"""
```

* **后端获取当前时间**

```python
# views.py
import datetime
def get_time(env):
    current_time = datetime.datetime.now().strftime('%Y-%m-%d %X')
    # 如何把后端获取到的数据“传递”给html文件?
    with open('templates/gettime.html', 'r', encoding='utf-8') as f:
        data = f.read()
    data = data.replace('dafadfadfaf', current_time)  # 后端将html页面处理好之后再返回个前端
    return data
```

* **将一个字典传递给html文件，并且可以在文件上方便快捷的操作字典数据**

```python
# views.py

from jinja2 import Template


def get_dict(env):
    user_dic = {
        'username': 'zhao',
        'age': 18,
        'hobby':'JayChou',
    }
    with open('templates/get_dict.html', 'r', encoding='utf-8') as f:
        data = f.read()
    tmp = Template(data)
    res = tmp.render(user=user_dic)  # 给这个页面传递了一个值，页面上通过变量名user就能够拿到user_dict
    return res
```

* **后端获取数据库中数据展示到前端页面**

```python
#views.py
def get_user(env):
    # 去数据库中获取数据，传递给html 页面，借助于模板语法，发送给浏览器
    conn = pymysql.connect(
        host='localhost',
        port=3306,
        user='root',
        passwd='123.com',
        db='后端链接',
        charset='utf8',
        autocommit=True  # 自动提交
    )

    cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)
    sql = 'select * from userinfo'
    arrect_rows = cursor.execute(sql)
    data_list = cursor.fetchall()
    # 将获取到的数据文件传递给html文件
    with  open('templates/get_data.html', 'r', encoding='utf-8') as f:
        data = f.read()
    tmp=Template(data)
    res=tmp.render(user_list=data_list)
    return res

```

## 模板语法之jinja2模块

```python
"""模板语法在后端起作用"""

# 模板语法(非常贴近python语法)
{{ user }}
{{ user.get('username') }}
{{ user.age }}
{{ user['hobby'] }}


{% for user in user_list %}
        <tr>
            <td>{{user.id}}</td>
            <td>{{user.username}}</td>
            <td>{{user.password}}</td>
            <td>{{user.hobby}}</td>
        </tr>
{% endfor %}
```

![image-20221020101009731](E:\MarkDown\markdown\imgs\image-20221020101009731.png)

![image-20221020104139128](E:\MarkDown\markdown\imgs\image-20221020104139128.png)

## 自定义简易版本web框架请求流程图

![image-20221020104944880](E:\MarkDown\markdown\imgs\image-20221020104944880.png)

```python
"""
wsgiref模块
	1.请求来的时候解析http格式的数据，封装成大字典
	2.响应走的时候给数据打包成符合http格式，再返回给浏览器
"""
```

## Python三大主流web框架

```python
"""
django
	特点：大而全，自带的功能特别多
	不足之处：
		有时候过于笨重，
flask
	特点：小而精，自带的功能特别少
	第三方的模块特别特别多，如果将flask第三方的模块加起来完全可以盖过django，并且越来越像djando
	不足之处：
		比较依赖于第三方的开发者
		
tornado
	特点：异步非阻塞，支持高并发
	可以开发游戏服务器
"""
A:socket部分
B:路由与视图函数关系（路由匹配）
C:模板语法

# django:	
    A用的是wsgiref模块
    B用的是自己的
    C用的是自己的（没有jinjia2好用，但是也方便）
 
# flask:
	A:用的是werkzeug（内部还是wsgiref模块）
    B:自己写的
    C:用的jinja2
    
# tornado:
    ABC都是自己写的
```

## 注意事项

```python
# 让计算机能够正常启动django项目
	1.计算机名称不能出现中文
    2.一个pycharm窗口只开一个项目
    3.项目名里所有的文件尽量不要出现中文
    4.python解释器使用3.10版本
    	如果项目报错，点击最后一个报错信息，去源码中把逗号删掉
#djando版本问题
	1.x 2.x 3.x 4.x

#django安装
pip install django==4.0
如果安装了其他版本，无需自己卸载
直接重新装，会自动卸载原来版本，安装新的版本

#验证django是否安装成功
	终端中输入django-admin看看有没有反应
```

## django基本操作

#### 1、**命令行操作**

---

1. 1.==创建django项目==

```python

"""先切换到对应的盘，然后再创建"""
django-admin startproject 项目名

	项目名文件夹
    	manage.py
    	项目名文件夹
        	__init__.py
            settings.py
            urls.py
            wsgi.py
            asgi.py
```

---

1. 2.==启动django项目==



```python
 """
    一定要先切换到项目目录下
    cd /项目名
"""
    
python manage.py runserver
```

![image-20221021095003878](E:\MarkDown\markdown\imgs\image-20221021095003878.png)

![image-20221021094805831](E:\MarkDown\markdown\imgs\image-20221021094805831.png)

---

1 .3.==创建应用==

```python
python manage.py startapp app01
```

![image-20221021095924761](E:\MarkDown\markdown\imgs\image-20221021095924761.png)

#### 2、pycharm操作

---

2. 1.==创建django项目==



```python
file---new project  ,选择左侧第二个Django即可
```

![image-20221021101334625](E:\MarkDown\markdown\imgs\image-20221021101334625.png)

![image-20221021101652876](E:\MarkDown\markdown\imgs\image-20221021101652876.png)

---

2. 2.==启动django项目==



```python
# 2. 启动
	（1）用命令启动   python manage.py runserver
    （2）点击绿色小箭头
```



![image-20221021102142570](E:\MarkDown\markdown\imgs\image-20221021102142570.png)

![image-20221021102225879](E:\MarkDown\markdown\imgs\image-20221021102225879.png)

---

2. 3.==创建应用==



```python
# 3. 创建应用
	（1）pycharm提供的终端直接输入命令 python manage.py startapp app01
     (2)tools----run manage.py task提示
```



![image-20221021102406290](E:\MarkDown\markdown\imgs\image-20221021102406290.png)

- 有自动提示功能

![image-20221021102504743](E:\MarkDown\markdown\imgs\image-20221021102504743.png)

![image-20221021102553348](E:\MarkDown\markdown\imgs\image-20221021102553348.png)

---

2. 4.==还可以修改端口号==



![image-20221021102801948](E:\MarkDown\markdown\imgs\image-20221021102801948.png)

![image-20221021102835863](E:\MarkDown\markdown\imgs\image-20221021102835863.png)

![image-20221021102901754](E:\MarkDown\markdown\imgs\image-20221021102901754.png)

#### 3、应用

```python
"""
django是一款专门用来开发app的web框架

django框架类似于一所大学（空壳子）
app就类似于大学里的各个学院 （具体功能的app）
	比如开发淘宝
		订单相关
		用户相关
		投诉相关
		创建不同的app对应不同的功能
		
一个app就是一个独立的功能模块		
"""

*********************创建的应用一定要去配置文件中注册*******************
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app02.apps.App02Config' #全写
    # 'app02'#简写
]
#创建出来的应用第一步先去配置文件中注册，

ps:在用pycharm创建项目的时候，pycharm可以帮你创建一个app并自动注册
******************************************************************

```

![image-20221021104740266](E:\MarkDown\markdown\imgs\image-20221021104740266.png)

![image-20221021104644697](E:\MarkDown\markdown\imgs\image-20221021104644697.png)

#### 4、主要文件介绍

```python
-mysite项目文件夹
	--db.sqlite3		django自带的sqlite3数据库（小型数据库，功能不多，有bug）
	--manage.py          django的入口文件
	--mysite文件夹
    	---settings.py   配置文件
        ---urls.py		 路由与视图函数对应关系（路由层）
        ---wsgi.py		 wsgiref模块
    --app01文件夹
    	---admin.py		django后台管理
        ---apps.py 		注册使用
        ---mitrations文件夹	数据库迁移记录
        ---models.py	数据库相关的 模型类（orm）
        ---tests.py		测试文件
        ---views.py     视图函数（视图层）
```

#### 5、命令函与pycharm创建区别

1. 命令行创建不会自动创建templates文件夹，需要自己手动创建而pycharm会自动创建，并且还会在配置文件中配置对应的路径

```python
# pycharm创建
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates']
        ,
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

# 命令行创建
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

"""
意味着用命令行创建django项目的时候，不单单需要手动创建templates文件夹，还需要配置我呢见中配置路径
'DIRS': [BASE_DIR / 'templates']
"""
```

## django小白必会三板斧

```python
"""
httpResponse
	返回字符串
render
	返回html文件
redirect
	重定向
        return redirect('https:www.baidu.com/')  跳转别人的网址
        return redirect('/home/')   跳转自己的网址
"""
```

```python
# views.py
from django.shortcuts import render, HttpResponse, redirect


# Create your views here.
def index(request):
    """

    :param request: 请求相关的所有数据对象
    :return:
    """
    # return HttpResponse('你好啊')
    # return render(request,'myfirst.html')#自动去templtaes文件夹下查找文件
    # return redirect('https:www.baidu.com/')
    return redirect('/home/')


def home(request):
    return HttpResponse('home')


def ab_render(request):
    user_dic = {'username': 'zhao', 'age': 18}
    # 第一种传值：更加精确，节省资源
    # return render(request, 'ab_render.html', {'data': user_dic,'date':123})
    # 第二种传值:当你要传入的数据特别多的时候
    """locals会将所在的名称空间中所有的名字全部传递给html页面"""
    return render(request,'ab_render.html',locals())
```

##   静态文件配置

```python
#登录功能

"""
将html 文件默认放在templates文件夹下
将网站所使用的静态文件默认放在static文件夹下

静态文件
	前端写好的，能够直接调用使用的都可以称为静态文件
	网站写好的js文件
	网站写好的css文件
	网站用到的图片文件
	第三方前端框架
	...
	拿来就可以直接使用的
	
"""

#django默认不会自动创建static文件，需要手动创建
一般情况下在static文件夹内会做进一步的划分处理
	-static
    	--js
        --css
        --img
        其他第三方文件
"""
在浏览器中输入url能够看到对应的资源
是因为后端提前开设了该资源的接口
如果访问不到资源，说明后端没有开设该资源的接口

"""   
```

![image-20221021173936220](E:\MarkDown\markdown\imgs\image-20221021173936220.png)

![image-20221021174520951](E:\MarkDown\markdown\imgs\image-20221021174520951.png)

* 引入了bootstrap的js文件和css文件，但是浏览器访问的时候并没有显示该有的样式，需**要开设该资源的接口**

![image-20221021174448402](E:\MarkDown\markdown\imgs\image-20221021174448402.png)

```python
**************************************************************************************
"""
当在写django项目的时候，可能会出现后端代码修改了但是前端页面没有变化的情况
	1. 在同一个窗口开了好几个django项目，一直在跑的其实是第一个django项目
	2. 浏览器缓存的问题
		右键点击检查，setting--network--disable cache 勾选上
"""
**************************************************************************************
```

![image-20221021181046077](E:\MarkDown\markdown\imgs\image-20221021181046077.png)

![](E:\MarkDown\markdown\imgs\image-20221021181009410.png)

```python
STATIC_URL = 'static/'  # 类似于访问静态网页的令牌
"""如果想要访问静态文件，就必须以static开头"""
"""
/static/bootstrap-3.4.1-dist/js/bootstrap.min.js

/static/令牌
去列表里面从上往下依次查找
    bootstrap-3.4.1-dist/js/bootstrap.min.js
    都没有的话才会报错

"""
# 静态文件配置


STATICFILES_DIRS = [
    BASE_DIR / "static",
    BASE_DIR / "static1",
]

# 静态文件动态解析
{% load static %}
    <link rel="stylesheet" href="{% static 'bootstrap-3.4.1-dist/css/bootstrap.min.css' %}">
    <script src="{% static 'bootstrap-3.4.1-dist/js/bootstrap.min.js' %}"></script>
    
    
# from 默认是get请求数据
"""
form 表单action参数
	1. 不写。默认朝当前所在的url提交数据
	2.全写
	3.只写后缀 /login/

"""

# 在前期使用django提交post请求时，需要去配置文件中注释掉一行代码
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    # 'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

## request对象

```python
request.method #返回请求方式。并且是全大写的字符串形式
request.POST #获取用户post请求提交 的普通数据，不包含文件
	request.POST.get()  #只获取列表最后一个元素
    request.POST.getlist() #直接将列表所有元素取出
request.GET   # 获取用户提交的gest请求数据
	request.GET.get()  #只获取列表最后一个元素
    request.GET.getlist()  #直接将列表所有元素取出
    
"""
get请求携带的数据是有大小数据的，大概好像只有4k左右
post请求则没有限制
"""    
def login(request):
    """
    get请求和post请求应该有不同的处理机制
    :param request:
    :return:
    """
    # print(request.method)#返回请求方式。并且是全大写的字符串形式
    # print(type(request.method))

    # if request.method == 'GET':
    #     return render(request, 'login.html')
    # elif request.method == 'POST':
    #     return HttpResponse('收到 !!!')

    if request.method == 'POST':
        return HttpResponse('收到')
    return render(request, 'login.html')
```

## pycharm链接数据库（MySQL）

pycharm可以充当很多数据库的客户端

![image-20221022100125505](E:\MarkDown\markdown\imgs\image-20221022100125505.png)



点击MySQL后,如果是**第一次使用pycharm中的MySQL**，那么需要**点击download下载对应驱动**



![image-20221022100404882](E:\MarkDown\markdown\imgs\image-20221022100404882.png)



如果提示下载失败的话，可以点击Driver，选择**MySQL for 5.1,**然后重新下载，**重新测试连接，成功**，测试链接成功后，点击apply，点击OK。！



![image-20221022102748371](E:\MarkDown\markdown\imgs\image-20221022102748371.png)

![image-20221022103600831](E:\MarkDown\markdown\imgs\image-20221022103600831.png)

​      

 **可以先通过==Navicat==添加数据**



![image-20221022103653311](E:\MarkDown\markdown\imgs\image-20221022103653311.png)



**在pycharm中点击刷新，即可看到创建的userinfo 表以及数据**



![image-20221022103754208](E:\MarkDown\markdown\imgs\image-20221022103754208.png)

![image-20221022103805763](E:\MarkDown\markdown\imgs\image-20221022103805763.png)

**通过pycharm修改数据，然后submit提交同步到数据库**

![image-20221022104045078](E:\MarkDown\markdown\imgs\image-20221022104045078.png)

**回到Navicat，刷新一下，数据就有了**

![image-20221022104129877](E:\MarkDown\markdown\imgs\image-20221022104129877.png)

## Django链接MySQL

```python
# 默认用sqlite3
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

#django链接MySQL
	1.第一步:配置文件中配置
    
    DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'db7',  # 数据库名
        'USER': 'root',
        'PASSWORD': '123456789',
        'HOST': '127.0.0.1',
        'POST': 3306,
        'CHARSET': 'utf8'
    }
}

    
    2.第二步:代码声明
    django默认用到是mysqldb模块连接mysql
    但是该模块兼容性不好，需要手动改为pymysql
    
    需要告诉django不要用默认的mysqldb
    #在项目名下的init或者任意应用名下的init文件总书写以下代码
    import pymysql
	pymysql.install_as_MySQLdb()
```

![image-20221022105927612](E:\MarkDown\markdown\imgs\image-20221022105927612.png)

## Django ORM

#### 1、创建表

```python
ORM:对象关系映射
作用：能够让一个不会用sql语句的小白，也能通过python面向对象的代码简单快捷的操作数据库
不足之处：封装成都太高，有时候sql效率偏低，需要自己写sql语句


类				    表
	
对象					记录

对象属性			记录某个字段对应的值
```

1. **第一步：先去models.py中书写一个类**

```python
# 应用下的models.py

class User(models.Model):
    # id int primary key auto_increment
    id = models.AutoField(parmary_key=True)
    # username varchar(32)
    username = models.CharField(max_length=32)
    # passwrod int
    password = models.IntegerField()
   

**********************2.数据库迁移命令*********************************************************************
 python manage.py makemigrations  将操作记录记录到小本本上（migrations文件夹）
 python manage.py migrate   将操作真正同步到数据库中
********************************************************************************************************

```

```python
from django.db import models


# Create your models here.

class User(models.Model):
    # id int primary key auto_increment
    id = models.AutoField(primary_key=True,verbose_name='主键')
    # username varchar(32)
    username = models.CharField(max_length=32, verbose_name='用户名')

    """
    CharField必须指定max_length参数，不指定会直接报错
    verhose_name该参数是所有字段都有的，就是用来对字段的解释
    """
    # passwrod int
    password = models.IntegerField(verbose_name='密码')


class Author(models.Model):
    # 由于一张表中必须要有一个主键字段，并且一般情况下都叫id字段
    # 所以orm当不定义主键字段的时候，orm会自动创建一个名为id字段
    # 也就意味着后续再创建模型表的时候，如果主键字段名没有额外的叫法，那么主键字段可以省略不写

    username = models.CharField(max_length=32)
    password = models.IntegerField()
```



2. **第二步：数据库迁移命令**

```python
 python manage.py makemigrations  将操作记录记录到小本本上（migrations文件夹）
```



![image-20221022111412105](E:\MarkDown\markdown\imgs\image-20221022111412105.png)

应用文件夹下的==migrations文件夹==下多了一个==0001._initial.py==日志文件

![image-20221022111604561](E:\MarkDown\markdown\imgs\image-20221022111604561.png)

```python
 python manage.py migrate   将操作真正同步到数据库中
```

![image-20221022112226431](E:\MarkDown\markdown\imgs\image-20221022112226431.png)

**数据库迁移的两条命令输入完成后，此时数据库中会出现很多张表**



![image-20221022115814598](E:\MarkDown\markdown\imgs\image-20221022115814598.png)

**一个Djanog项目可以有多个应用，那么多个应用之间可能会出现表明冲突的情况，那么加上前缀就可以完全避免冲突**        `app02_user`

![image-20221022120128278](E:\MarkDown\markdown\imgs\image-20221022120128278.png)

```python
"""
只要修改了models.py中跟数据库相关的代码，就必须重新执行数据迁移的两条命令
"""
```

#### 2、字段的增删改查

2. 1 **字段的增加**

```python
"""
1.可以在终端内直接给出默认值
	age=models.IntegerField(verbose_name='年龄')
2.该字段可以为空
	 info = models.CharField(max_length=32,verbose_name='信息', null=True)
3.直接给字段设置默认值 
    hobby = models.CharField(max_length=32,verbose_name='爱好', default='play')
"""
```

**终端内直接给默认值操作如下图**

![image-20221022124444920](E:\MarkDown\markdown\imgs\image-20221022124444920.png)

==切记==，**只要动了数据库相关的代码就必须执行数据库迁移的两条命令**

```python
******************************************************************
python manage.py makemigrations
python manage.py migrate
******************************************************************
```

2. 2字段的修改

```python
直接修改代码然后执行数据库迁移的两条命令即可
```

![image-20221022125639587](E:\MarkDown\markdown\imgs\image-20221022125639587.png)

2. 3字段的删除

```python
直接注释掉对应的字段代码，然后执行数据迁移的两条命令即可
执行完毕后，字段对应的数据都没有了

"""
在操作models.py时，一定要细心
千万不要注释一些字段
执行迁移命令之前一定要检查一下代码
"""
```

#### 3、数据的增删改查

3. 1查数据

```python
# views.py
from app02 import  models
res=models.User.objects.filter(username=username)
"""
返回值先看成是一个列表套数据对象的格式
也支持索引取值，切片操作，但是不支持负数索引
同样也不推荐使用索引方式取值
"""
 # 下面这条代码等价于 select * from user where username='zhao';
user_obj=models.User.objects.filter(username=username).first()
# user_obj=res[0]
print(user_obj)
print(user_obj.username)
print(user_obj.password)

filter括号内可以携带多个参数，参数与参数之间默认是and关系
可以把filter联想成MySQL中的where

user_obj=models.User.objects.filter(username=username,password=password).first()
#select * from user where username='zhao' and password=132;
```

**登录功能**

```python
# views.py
def login(request):
   
    if request.method == 'POST':
        
        # 获取用户的用户名和密码，然后利用orm操作数据库，校验数据是否正确
        username = request.POST.get('username')
        password = request.POST.get('password')
        # 去数据库中查询数据
        from app02 import models
        # select * from user where username='zhao';
        user_obj = models.User.objects.filter(username=username).first()

        # < QuerySet[ < User: Userobject(1) >] >  [数据对象1，数据对象2......]
        # print(res)
        
        # user_obj=res[0]
        # print(user_obj)
        # print(user_obj.username)
        # print(user_obj.password)
        if user_obj:
            # 比对密码是否一致
            if password == user_obj.password:
                return HttpResponse('登录成功')
            else:
                return HttpResponse('密码错误')
        else:
            return HttpResponse('用户不存在')




    return render(request, 'login.html')
```

3. 2 增加数据

```python
#第一种方法
        from app02 import models
        res = models.User.objects.create(username=username, password=password)
        # 返回值就是当前被创建的对象本身
        
        print(res, res.username, res.password)
        
        
 # 第二种方法
        from app02 import models
        user_obj = models.User(username=username, password=password)
        user_obj.save()#保存数据
```

**注册功能**

```python
def register(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')

        # 直接获取用户数据存入数据库
        from app02 import models
        res = models.User.objects.create(username=username, password=password)
        # 返回值就是当前被创建的对象本身
        print(res, res.username, res.password)

    # 先给用户返回已给注册页面
    return render(request, 'register.html')
```
