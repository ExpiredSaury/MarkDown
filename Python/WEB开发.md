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
*****************************************************************

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

#### 5、命令行与pycharm创建区别

1. 命令行创建不会自动创建templates文件夹，需要自己手动创建而pycharm会自动创建，并且还要在配置文件中配置对应的路径

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
```

```python
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

```

```python
"""
意味着用命令行创建django项目的时候，不单单需要手动创建templates文件夹，还需要在配置文件中配置路径
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

#### **引入**

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

#### ==**静态文件配置**==

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
    
    
# form 默认是get请求数据
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
request.POST #获取用户post请求提交 的普通数据（键值对），不包含文件
	request.POST.get()  #只获取列表最后一个元素
    request.POST.getlist() #直接将列表所有元素取出
    
request.GET   # 获取url问好后面携带的参数
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

 **在前期使用django提交post请求时，需要去配置文件中注释掉一行代码**

```python

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

django不能创建库，需要自己手动创建，并指定

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

==orm不会创建库，只能创建到表的层面。需要自己手动敲命令创建库==

#### 1、创建模型表

```python
ORM:"""对象关系映射"""
作用：能够让一个不会用sql语句的小白，也能通过python面向对象的代码简单快捷的操作数据库
不足之处：封装成都太高，有时候sql效率偏低，需要自己写sql语句


类				     表
	
对象					记录

对象属性			记录某个字段对应的值
```

1. **第一步：先去models.py中书写一个模型类**

```python
# 应用下的models.py

class User(models.Model):
    # id int primary key auto_increment
    id = models.AutoField(parmary_key=True)
    # username varchar(32)
    username = models.CharField(max_length=32)
    # passwrod int
    password = models.IntegerField()
   

***************************************2.数据库迁移命令****************************************************
 python manage.py makemigrations  将操作记录记录到小本本上（migrations文件夹）
 python manage.py migrate   将操作真正同步到数据库中
********************************************************************************************************

```

```python
#应用下的modles.py
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

**一个Djanog项目可以有多个应用，那么多个应用之间可能会出现表明冲突的情况，那么加上前缀就可以完全避免冲突**    

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
"""直接修改代码然后执行数据库迁移的两条命令即可"""
```

![image-20221022125639587](E:\MarkDown\markdown\imgs\image-20221022125639587.png)

2. 3字段的删除

```python
直接注释掉对应的字段代码，然后执行数据迁移的两条命令即可
执行完毕后，字段对应的数据都没有了

"""
在操作models.py时，一定要细心

执行迁移命令之前一定要检查一下代码
"""
```

#### 3、数据的增删改查

3. 1查数据

```python
#views.py
def userlist(request):
    # 查询出user用户表里的所有数据.
    # 方式一
    # data = models.User.objects.filter()
    # print(data)

    # 方式二：
    user_queryset = models.User.objects.all()
    # print(data)
    return render(request, 'userlist.html', locals())
```



```python
# views.py
from app02 import  models

#res=models.User.objects.filter(username=username)
# user_obj=res[0]

"""
返回值先看成是一个列表套数据对象的格式
也支持索引取值，切片操作，但是不支持负数索引
同样也不推荐使用索引方式取值
"""

 # 下面这条代码等价于 select * from user where username='zhao';
user_obj=models.User.objects.filter(username=username).first()

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
#第一种方法  create()
        from app02 import models
        res = models.User.objects.create(username=username, password=password)
        # 返回值就是当前被创建的对象本身
        
        print(res, res.username, res.password)
        
        
 # 第二种方法  对象.save()
        from app02 import models
   		 #生成一个类对象
        user_obj = models.User(username=username, password=password)
        #对象调用save方法
        user_obj.save()#保存数据
```

**注册功能**

```python
# views.py
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

3. 3 修改数据

```python
  # 修改数据方式一
        # 获取到用户想要删除 的数据id值
   	 	delete_id = request.GET('user_id')
     	models.User.objects.filter(id=edit_id).update(username=username, password=password)
        """
        将filter查询出来的列表中所有对象全部更新         批量更新
        只修改被修改的字段
        """
  # 修改数据方式二
		edit_obj = models.User.objects.filter(id=edit_id).first()
        edit_obj.username = username
        edit_obj.password = password
        edit_obj.save()
        """
        方式二当字段特别多的时候效率很低
        从头到尾将数据的所有字段全部更新一遍，无论该字段是否被修改
        """
```

3. 4删除数据

```python
# 删除用户
def delete_user(request):
    # 获取到用户想要删除 的数据id值
    delete_id = request.GET('user_id')

    # 直接去数据库找到对应的数据删除即可
    models.User.objects.filter(id=delete_id).delete()
    """
    批量删除
    """
    #跳转到展示页面
    return redirect('/userlist/')

真正删除数据是要有二次确认的，
删除数据内部其实不是真正的删除，我们会给数据添加一个标识字段用来标识当前数据是否被删除，如果删除了，仅仅是将一个字段修改了一个状态
```



**==示例==：**

```python
#先将数据库中的数据全部展示到前端，然后给一个数据两个按钮，一个编辑，一个删除

#编辑功能
	#点击编辑按钮，朝后端发送编辑数据的请求
   """如何告诉后端用户想要编辑哪条数据？
   		将编辑按钮所在的那一行数据的主键值发送给后端
   			利用url问号后面携带参数的方式
   """ 
    #后端查询出用户想要编辑的数据对象，展示到前端页面，供用户查看和编辑
 #删除功能
	将数据从前端页面删除
```

* 数据展示到前端页面

```html
<!-- userlist.html---> 

<h2 class="text-center">数据展示</h2>
<div class="container">
    <div class="row">
        <div class="col-md-8 col-offset-2">
            <table class="table table-striped table-hover ">
                <thead>
                <tr>
                    <th>ID</th>
                    <th>username</th>
                    <th>password</th>
                    <th class="text-center">action</th>
                </tr>
                </thead>
                <tbody>
                {% for user_obj in user_queryset %}
                    <tr>
                        <td>{{ user_obj.id }}</td>
                        <td>{{ user_obj.username }}</td>
                        <td>{{ user_obj.password }}</td>
                        <td class="text-center">
                            <a href="/edit_user/?user_id={{ user_obj.id }}" class="btn btn-primary btn-xs">编辑</a>
                            <a href="" class="btn btn-danger btn-xs">删除</a>
                        </td>
                    </tr>

                {% endfor %}

                </tbody>
            </table>
        </div>
    </div>
</div>
```

```python
# views.py

# 展示用户列表

def userlist(request):
    # 查询出user用户表里的所有数据.
    # 方式一
    # data = models.User.objects.filter()
    # print(data)

    # 方式二：
    user_queryset = models.User.objects.all()
    # print(data)
    return render(request, 'userlist.html', locals())
```

* 编辑用户数据

```html
<!-- edit_user.html--->

<h1 class="text-center">编辑</h1>
<div class="container">
    <div class="row">
        <div class="col-md-offset-2 col-md-8">
            <form action="" method="post">
                <p>username:<input type="text" name="username" class="form-control" value="{{ edit_obj.username }}"></p>
                <p>password:<input type="password" name="password" class="form-control" value="{{ edit_obj.password }}"></p>
                <input type="submit" class="btn btn-info btn-block" value="编辑">

            </form>
        </div>
    </div>
</div>
```

```python
#views.py
from app03 import models
def edit_user(request):
    # 获取url问号后面的参数
    edit_id = request.GET.get('user_id')
    # 查询当前用户想要编辑的数据对象
    edit_obj = models.User.objects.filter(id=edit_id).first()

    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')

        # 去数据库中修改对应的数据内容

        # 修改数据方式一
        # models.User.objects.filter(id=edit_id).update(username=username, password=password)
        """
        将filter查询出来的列表中所有对象全部更新         批量更新
        只修改被修改的字段
        """
        # 修改数据方式二
        edit_obj.username = username
        edit_obj.password = password
        edit_obj.save()
        """
        方式二当字段特别多的时候效率很低
        从头到尾将数据的所有字段全部更新一遍，无论该字段是否被修改
        """

        # 跳转到数据的展示页面
        return redirect('/userlist/')

    # 将数据展示到页面上
    return render(request, 'edit_user.html', locals())


```

* 删除数据

```html
<!-- userlist.html--->  

<a href="/delete_user/user_id={{ user_obj.id }}" class="btn btn-danger btn-xs">删除</a>
```

```python
#views.py
# 删除用户
def delete_user(request):
    # 获取到用户想要删除 的数据id值
    delete_id = request.GET('user_id')

    # 直接去数据库找到对应的数据删除即可
    models.User.objects.filter(id=delete_id).delete()
    """
    批量删除
    """
    #跳转到展示页面
    return redirect('/userlist/')
```

#### 4、orm中创建表关系

```python
"""
表与表关系
	一对多
		models.ForeignKey(to='Publish', on_delete=models.CASCADE)
	多对多
		models.ManyToManyField(to='Author')
	一对一
		models.OneToOneField(to='AuthorDetail', on_delete=models.CASCADE)

判断表关系方式：换位思考

"""
```

```python
# 创建表关系， 先将基表创建出来，然后在添加外键字段
class Book(models.Model):
    title = models.CharField(max_length=32, verbose_name='书名')
    price = models.DecimalField(max_digits=8, decimal_places=2, verbose_name='价格')
    # 小数总共8位，小数点后占2位

    """
    图书和出版社是一对多，并且书是多的一方，所以外键字段放在书表里面
    """
    publish = models.ForeignKey(to='Publish', on_delete=models.CASCADE)  # 默认就是与出版社表的主键字段做外键关联
    """
    如果字段对应的是ForeignKey,那么orm会自动在字段的后面加_id
    如果自己手动的加了_id,那么orm还是会在后面加_id
    
    """

    """
    图书 和作者 是多对多的关系，外键字段建在任意一方即可，推荐建在查询频率较高的一方，
    
    """
    authors = models.ManyToManyField(to='Author')
    """
    authors是一个虚拟字段，主要是用来告诉orm，书籍表与作者表是多对多的关系
    让orm自动创建第三章关系表
    """


class Publish(models.Model):
    name = models.CharField(max_length=32, verbose_name='出版社')

    addr = models.CharField(max_length=32, verbose_name='地址')


class Author(models.Model):
    name = models.CharField(max_length=32, verbose_name='作者名')
    age = models.IntegerField(verbose_name='年龄')

    """
    
    作者与作者详情是一对一的关系，外键字段建立在任意放一方都可以，但是建议建立在查询频率较高的地方
    
    """
    author_detail = models.OneToOneField(to='AuthorDetail', on_delete=models.CASCADE)  # 级联删除
    """
    OneToOneFiel也会自动给字段加_id后缀，不需要自己加
    """


class AuthorDetail(models.Model):
    phone = models.BigIntegerField(verbose_name='电话')
    addr = models.CharField(max_length=32, verbose_name='作者地址')

    
    
"""

orm中如何定义表关系

publish = models.ForeignKey(to='Publish', on_delete=models.CASCADE)
author_detail = models.OneToOneField(to='AuthorDetail', on_delete=models.CASCADE) 
authors = models.ManyToManyField(to='Author')



ForeignKey与OneToOneField会自动在字段后面加_id后缀
"""
```



## Django请求周期流程图（重要）

![image-20221023130145467](E:\MarkDown\markdown\imgs\image-20221023130145467.png)



```python
# 扩展
"""
缓存数据库
	提前将数据准备好，来了直接拿就可以，提高效率和响应时间
	
"""
```

## 路由层

#### 1、路由匹配

```python
    #路由匹配
    re_path('test/',views.test),
    re_path('testadd/',views.testadd)
    """
    re_path第一个参数是正则表达式
    
	只要第一个参数正则表达式能够匹配到内容，那么就会立刻停止往下匹配，直接执行对应的视图函数
	
	在输入url会默认加斜杠，是应为django内部自动做了重定向
    """
```

![image-20221023134245558](E:\MarkDown\markdown\imgs\image-20221023134245558.png)

**django路由匹配 的时候其实可以匹配两次，第一次如果url后面没有加斜杠，django会让浏览器加斜杠在发送一次请求**

```python
# settings.py

APPEND_SLASH = False# 取消自动加斜杠，默认是True
"""取消了自动加斜杠后，再次访问浏览器就会报错"""
```

![image-20221023134109323](E:\MarkDown\markdown\imgs\image-20221023134109323.png)

```python
# urls.py

from django.contrib import admin
from django.urls import path, re_path
from app03 import  views
urlpatterns = [
    #首页
    re_path(r'^$',views.home),
    #路由匹配
    re_path(r'^test/$',views.test),
    re_path(r'^testadd/$',views.testadd),
    #尾页
    # re_path(r'',views.error),
]
```

#### 2、无名/有名分组

无名

```python
"""
分组：就是给某一个正则表达式用小括号括起来
"""

#urls.py
re_path(r'^test/(\d+)',views.test),

#views.py
def test(request,xxx):
    print(xxx)
    return HttpResponse('test')
#无名分组就是将括号内正则表达式匹配到的内容当作位置参数传递给后面的视图函数
```

有名

```python
"""
可以给正则表达式起别名
"""
#urls.py
 re_path(r'^testadd/(?P<year>\d+)', views.testadd),
    
#views.py
def testadd(request, year):
    print(year)
    return HttpResponse('testadd')

#有名分组就是将括号内正则表达式匹配到的内容当作关键字参数传递给后面的视图函数
```



```python
# 无名有名 不能混用
re_path(r'^demo/(\d+)/(?P<year>\d+)/',views.demo)
def demo(request, xx, year):
    print(xx, year)
    return HttpResponse('无名有名混用')  报错，结果报错

*************************************************************************************

# 单个分组可以使用多次
* 无名
 re_path(r'^demo/(\d+)/(\d+)/',views.demo)
 def demo(request, *args):
    print(args)
    return HttpResponse('demo')
* 有名
re_path(r'^demo/(?P<year>\d+)/(?P<month>\d+)/',views.demo)
def demo(request, *args,**kwargs):
    print(kwargs)
    return HttpResponse('demo')

```



#### 3、反向解析

```python
#通过一些方法得到一个结果，该结果可以直接访问对应的url触发视图函数

# 1.先给路由和视图函数起别名
 re_path(r'^func/',views.func,name='zs'),
    
# 2.反向解析
	#后端反向解析
    	from django.shortcuts import reverse
    	def home(request):
            print(reverse('zs'))
            return render(request,'home.html')
    #前端反向解析
    	<a href="{% url 'zs' %}">111</a>
        
        
"""别名不能出现冲突！！！"""
```

#### 4、无名有名反向解析

```python
#无名分组的反向解析
re_path(r'^index/(\d+)/',views.index,name='zs'),

#前端
<a href="{% url 'zs' 123 %}"></a>

#后端
from django.shortcuts import reverse
def home(request):
    print(reverse('zs',args=(1,)))  #/index/1/
    return render(request,'home.html')

"""
数字 一般放的是数据的主键值， 来做数据的编辑和删除
"""
```

```python
# 有名分组反向解析
	re_path(r'^func/(?P<year>\d+)/',views.func,name='li'),

#前端
    <a href="{% url 'li' year=123 %}"></a>
    #简单写法
    <a href="{% url 'li' 123 %}"></a>


#后端
    def home(request):

        print(reverse('li',kwargs={'year':123}))
        #简单写法
        # print(reverse('li', args=(123,)))
        return render(request, 'home.html')
```

#### 5、路由分发

```python
"""
Django的每一个应用都可以有自己的templates文件夹，urls.py static文件夹
正是基于上述的特点，Django能够非常好的做到分组开发，（每个人只写自己的app）


当一个django项目中的url特别多，总路由urls.py代码就非常冗余，不易于维护，
这个时候就可以利用路由分发，减轻总路由压力


利用路由分发之后，总路由不再干路由与视图函数的直接对应关系
而是做一个分发处理
	识别当前url属于哪个应用下的，直接分发给对应的应用去处理
	
"""


#总路由

from app01 import urls as app01_urls
from app02 import urls as app02_urls
from django.urls import path, re_path, include
urlpatterns = [
    # 1.第一种方式 路由分发
    #re_path(r'^app01/', include(app01_urls)),  # 只要re_path前缀是app01开头的，全部交给app01处理
    #re_path(r'^app02/', include(app02_urls)),   # 只要re_path前缀是app02开头的，全部交给app02处理
    
    # 2.第二宗方式 路由分发，不用导模块，
    re_path(r'^app01/',include('app01.urls')),
    re_path(r'^app02/',include('app02.urls'))
    #总路由里面的re_path不能加$结尾
]




#子路由

#app01下urls.py
from django.urls import path, re_path
from app01 import views

urlpatterns = [
    re_path(r'^register/',views.register)
]
#app02下urls.py
from django.urls import path, re_path
from app02 import views

urlpatterns = [
    re_path(r'^register/',views.register)
]

```

#### 6、名称空间

```python
# 当多个应用出现了相同的别名，反向解析不能自动识别应用前缀

#名称空间
	#总路由
        path('app01/', include('app01.urls', namespace='app01')),
        path('app02/', include('app02.urls', namespace='app02'))
        
     #解析的时候
    	#后端
        	#app01下的urls.py
            urlpatterns = [re_path(r'^register/',views.register,name='reg')]
            #app02下的urls.py
            urlpatterns = [re_path(r'^register/',views.register,name='reg')]

        #前端
        	{% url 'app01:reg' %}
			{% url 'app02:reg' %}
            
            
#只要保证名字不冲突，就没有必要使用名称空间

"""
一般情况下，有多个app的时候，在起别名时会加上app的前缀
这样就能确保多个app之间名字不冲突

"""

#加前缀
urlpatterns = [
    re_path(r'^register/',views.register,name='app02_reg')
]
urlpatterns = [
    re_path(r'^register/',views.register,name='app01_reg')
]

```



#### 7、伪静态

```python
"""
将一个动态网页为伪装成静态网页

伪装的目的在于增大网站的 seo查询力度
并且增加搜友引擎收藏本网站的概率

搜索引擎本质上就是一个巨大的爬虫引擎


	
"""
```

## 虚拟环境

```python
"""

在正常开发中，会给每一个项目配置一个该项目独有的解释器资源
该环境内只有该项目用到的模块，用不到的一概不装



虚拟环境
	每创建一个虚拟环境就类似于重新下载了一个纯净的python解释器，但是虚拟机环境不要创建太多，是需要消耗硬盘空间的
	
在开发中会给每一个项目配置一个requirements.txt文件
里面书写该项目用到的所有模块即版本
只需要输入一条命令即可一键安装所有模块即版本
    pip install pipreqs
    pipreqs . --encoding=utf8 --force

"""
```

```python
--encoding=utf8 ：为使用utf8编码

--force ：强制执行，当 生成目录下的requirements.txt存在时覆盖 

. /: 在哪个文件生成requirements.txt 文件
```



## Django版本区别

```python
"""
1.django1.x路由层使用url方法
	django2.x 3.x 4.x 路由层使用path方法
	url()第一个参数支持正则
	path()第一个参数不支持正则，写生么就匹配什么
	
	
	不习惯使用path 也可以改成re_path()，只是需要导入，
	from django.urls import re_path
	就可以使用正则
	
	
	re_path 等价于1.x 里面的 url
	
2.虽然path不支持正则，但是内部支持5中转换器
path('index/<int:id>',views.index)将第二个路由里面的内容先转成整型，以关键字的形式传递给后面的视图函数
def index(request,id):
	print(id,type(id))
	return HttpResponse('index')

str,匹配除了路径分隔符，（/）之外的非空字符串，这是默认形式
int,匹配正整数，除了0
slug,匹配字母，数字，以及横杠，下滑线等组成的字符串
uuid，匹配格式化的uuid，如 0212451-4578-451w-5a6e-4611313
path，匹配任何非空字符串，包含了路径分隔符（/）


3.除了有默认的五个转换器之外，还支持自定义转换器

4.模型层，1.x外键默认都是级联更新删除的,但是2.x,3.x,4.x都是需要手动添加参数
models.OneToOneField(to='AuthorDetail', on_delete=models.CASCADE)
"""
```

## 视图层

#### 1、三板斧

```python
"""
HttpResponse
	返回字符串类型
render
	返回html页面，并且返回给浏览器之前还可以给html文件传值
redirect
	重定向
"""
# 视图函数必须要返回一个HttpResponse对象，研究三者的源码即可得出结论


# render简单内部原理
def inder(request):
    from django.template import Template,Context
    res=Template('<h1>{{ user }}</h1>')
    con=Context({'user':{'username':'zhao','password':123}})
    ret=res.render(con)
    print(ret)
    return HttpResponse(ret)
```



#### 2、JsonResponse对象

```python
"""
json格式数据有什么用？：
	前后端数据交互的时候需要使用json作为过渡，实现跨语言传输数据

前端系列化                      python系列化
	JSON.stringify()			json.dumps()
	JSON.parse()				json.loads()
"""

from django.http import JsonResponse


def ab_json(request):
    user_dic = {'username': '我是谁', 'password': 123, 'hobby': 'JayChou'}
    l=[111,222,33,444]
    # 先转成json格式字符串
    # json_str=json.dumps(user_dic,ensure_ascii=False)
    # #将该字符串返回
    # return HttpResponse(json_str)

    # return JsonResponse(user_dic, json_dumps_params={'ensure_ascii': False})
    return JsonResponse(l,safe=False)
默认只能序列化字典，序列化其他需要加safe参数
```

#### 3、form表单上传文件及后端如何操作

```python
"""
from 表单上传文件类型的数据
	1. method必须指定post
	2.enctype必须换成form-data
"""

def ab_file(request):
    if request.method == 'POST':
        print(request.POST)  # 只能获取普通的键值对数据
        print(request.FILES)  # 获取文件数据
        file_obj = request.FILES.get('myfile')
        print(file_obj.name)
        with open(file_obj.name, 'wb') as f:
            for line in file_obj:  # chunks方法加与不加都是一样的效果
                f.write(line)
    return render(request, 'form.html')

```

#### 4、request对象方法

```python
"""
request.method
request.GET
request.POST
request.FILES

request.path		#只能获取路由
request.path_info   #只能获取路由
request.get_full_path() #能够获取路由以及问号后面的参数
request.body   #原生的浏览器发过来的二进制数据

"""
print(request.path)#/app01/ab_file/
print(request.path_info)#/app01/ab_file/
print(request.get_full_path())#/app01/ab_file/?username=lisi
```

#### 5、FBV与CBV

```python
#视图函数既可以是函数也可以是类

#FBV
	#FBV路由
    path('index/',views.index),
    #views.py
    def index(request):
        return HttpResponse('index')

#CBV
    #CBV路由  
    path('login/',views.MyLogin.as_view())
    
    #views.py
    class MyLogin(View):
    def get(self, request):
        return render(request,'form.html')

    def post(self, request):
        return HttpResponse('post方法')

    """
    CBV特点：能够根据请求方式的不同直接匹配到对应的方法执行
    """
```

