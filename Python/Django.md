[toc]



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

* 代码优化

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

**前端获取后端结果**

![image-20221028155919554](E:\MarkDown\markdown\imgs\image-20221028155919554.png)

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

**前端获取数据库传过来的值**

```html
<div class="container">
    <div class="row">
        <div class="col-md-8 col-md-offset-2">
            <h1 class="text-center">用户数据</h1>
            <table class="table table-hover table-striped">
                <thead>
                <tr>
                    <th>ID</th>
                    <th>username</th>
                    <th>password</th>
                    <th>hobby</th>
                </tr>
                </thead>
                <tbody>
                {% for user in user_list %}
                <tr>
                    <td>{{user.id}}</td>
                    <td>{{user.username}}</td>
                    <td>{{user.password}}</td>
                    <td>{{user.hobby}}</td>
                </tr>
                {% endfor %}
                </tbody>
            </table>

        </div>
    </div>
</div>
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

### 1、**命令行操作**

---

##### 1. 1.==创建django项目==

```python

"""先切换到对应的盘，然后再创建"""
django-admin startproject 项目名

#目录结构

    ├── mysite #项⽬⽬录
    |
    │  ├── __init__.py #包标志
    │  ├── settings.py #项⽬配置⽂件
    │  ├── urls.py #路由映射表
    │  └── wsgi.py #wsgi接⼝
    └── manage.py #项⽬管理命令
    
manage.py:是Django⽤于管理本项⽬的命令⾏⼯具，之后进⾏站点运⾏，数据库⾃动⽣成等都是通过本⽂件完成。
```

![image-20221105205022192](E:/MarkDown/markdown/imgs/image-20221105205022192.png)

---

##### 1. 2.==启动django项目==



```python
 """
    一定要先切换到项目目录下
    cd /项目名
"""
    
python manage.py runserver
python manage.py runserver 9000 #可以指定端口

```

测试服务器默认端⼝是8000，仅限于本地连接。打开浏览器输⼊：

```python
http://localhost:8000 
# 或者
http://127.0.0.1:8000 
```

如果要让远程客户端连接需要修改配置⽂件，其中0.0.0.0:9000是可选的，0.0.0.0 说明任何ip都可以访问。

```markdown
# 修改setting.py中的这⼀⾏
ALLOWED_HOSTS = ['*']
```



可以看到自己的网站,就表示运行启动成功！

![image-20221021095003878](E:\MarkDown\markdown\imgs\image-20221021095003878.png)

![image-20221021094805831](E:\MarkDown\markdown\imgs\image-20221021094805831.png)

---

##### 1. 3.==创建应用==

⼀个django项⽬中可以包含多个应⽤,可以使⽤以下命令建⽴应⽤：

```python
python manage.py startapp app01
```

![image-20221021095924761](E:\MarkDown\markdown\imgs\image-20221021095924761.png)

修改项⽬的配置⽂件setting.py

```markdown
INSTALLED_APPS = [
 'django.contrib.admin',
 'django.contrib.auth',
 'django.contrib.contenttypes',
 'django.contrib.sessions',
 'django.contrib.messages',
 'django.contrib.staticfiles',
 'app01.apps.App02Config'  # 安装⾃⼰的应⽤,全写
  # 'app01'#简写
  
]
```



### 2、pycharm操作

---

##### 2. 1.==创建django项目==



```python
file---new project  ,选择左侧第二个Django即可
```

![image-20221021101334625](E:\MarkDown\markdown\imgs\image-20221021101334625.png)

![image-20221021101652876](E:\MarkDown\markdown\imgs\image-20221021101652876.png)

##### 2. 2.==启动django项目==



```python
# 2. 启动
	（1）用命令启动   python manage.py runserver
    （2）点击绿色小箭头
```



![image-20221021102142570](E:\MarkDown\markdown\imgs\image-20221021102142570.png)

**如下图所示**：**启动成功**

![image-20221021102225879](E:\MarkDown\markdown\imgs\image-20221021102225879.png)

---

##### 2. 3.==创建应用==



```python
# 3. 创建应用
	（1）pycharm提供的终端直接输入命令 python manage.py startapp app01
     (2)tools----run manage.py task提示
```

---

![image-20221021102406290](E:\MarkDown\markdown\imgs\image-20221021102406290.png)

- 有自动提示功能

![image-20221021102504743](E:\MarkDown\markdown\imgs\image-20221021102504743.png)

**如下图所示：应用创建成功**

![image-20221021102553348](E:\MarkDown\markdown\imgs\image-20221021102553348.png)

---

##### 2. 4.==还可以修改端口号==



![image-20221021102801948](E:\MarkDown\markdown\imgs\image-20221021102801948.png)

![image-20221021102835863](E:\MarkDown\markdown\imgs\image-20221021102835863.png)

**如下图所示：端口修改成功！**

![image-20221021102901754](E:\MarkDown\markdown\imgs\image-20221021102901754.png)

### 3、应用

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
***********************************************************

```

![image-20221028172420141](E:\MarkDown\markdown\imgs\image-20221028172420141.png)

![image-20221021104644697](E:\MarkDown\markdown\imgs\image-20221021104644697.png)

### 4、主要文件介绍

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

**项目配置文件：**

```markdown
# 项⽬根⽬录 manage.py所在⽬录
BASE_DIR =BASE_DIR = Path(__file__).resolve().parent.parent  # 当前项目路径

# 调试模式
DEBUG = True  # 上线之后改为False

# 允许访问的主机
ALLOWED_HOSTS = []  # 上线之后可以写*

# 注册的app（app就是功能模块)
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app02.apps.App02Config'  # 全写
    # 'app02'#简写
]


# html文件存放路径配置
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

# 数据库配置
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'db1',  # 数据库名
        'USER': 'root',
        'PASSWORD': '123456',
        'HOST': '127.0.0.1',
        'POST': 3306,
        'CHARSET': 'utf8'
    }
}

# 数据库默认是sqlite3
DATABASES = {
 'default': {
 'ENGINE': 'django.db.backends.sqlite3',
 'NAME': os.path.join(BASE_DIR, 'db.sqlite3'), 
 }
}

# 国际化
LANGUAGE_CODE = 'zh-hans' #语⾔编码
TIME_ZONE = 'Asia/Shanghai' #时区
```





### 5、命令行与pycharm创建区别

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

### **引入**

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

### ==**静态文件配置**==

* 网站通常需要提供额外的文件，如图像、JavaScript或CSS。在Django中，我们将这些文件称为“`静态文件`”

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

方法1：

<link rel="stylesheet" href="/static/bootstrap-3.4.1-dist/css/bootstrap.min.css">   直接跟static后面的路径即可，这里的static并不是我们创建的目录名称，而是令牌名字

# 静态文件动态解析
{% load static %}
    <link rel="stylesheet" href="{% static 'bootstrap-3.4.1-dist/css/bootstrap.min.css' %}">
    <script src="{% static 'bootstrap-3.4.1-dist/js/bootstrap.min.js' %}"></script>

方法2：动态解析static令牌，这样的好处是当我们需要更换令牌的名字的时候，不需要每个html文件更改了，只需要在setting.py里面更改即可，会动态的解析到静态文件的名称。语法如下：

{% load static %}
<link rel="stylesheet" href="{% static "bootstrap-3.4.1-dist/css/bootstrap.min.css" %} ">    
    
# form 默认是get请求数据
"""
form 表单action参数
	1. 不写。默认朝当前所在的url提交数据
	2.全写
	3.只写后缀 /login/

"""

# 在前期使用django提交post请求时，需要去配置文件中注释掉一行代码
'django.middleware.csrf.CsrfViewMiddleware'


#settings.py
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

### 静态文件css，js样式加载不出来解决

```python
在要编辑的html文件中，删除掉第一行的<!DOCTYPE html>就可以解决
```



## pycharm链接数据库（MySQL）

pycharm可以充当很多数据库的客户端

![image-20221022100125505](E:\MarkDown\markdown\imgs\image-20221022100125505.png)



点击MySQL后,如果是**第一次使用pycharm中的MySQL**，那么需要**点击download下载对应驱动**



![image-20221022100404882](E:\MarkDown\markdown\imgs\image-20221022100404882.png)



==如果提示下载失败的话，可以点击Driver，选择**MySQL for 5.1,**然后重新下载，**重新测试连接，成功**，测试链接成功后，点击apply，点击OK。！==



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

### 1、创建模型表

```python
ORM:"""对象关系映射"""
作用：能够让一个不会用sql语句的小白，也能通过python面向对象的代码简单快捷的操作数据库
不足之处：封装成都太高，有时候sql效率偏低，需要自己写sql语句


类				     表
	
对象					记录

对象属性			记录某个字段对应的值
```

1. **第一步：先去应用下的models.py中书写一个模型类**

```python
# 应用下的models.py

class User(models.Model):
    # id int primary key auto_increment
    id = models.AutoField(parmary_key=True)
    # username varchar(32)
    username = models.CharField(max_length=32)
    # passwrod int
    password = models.IntegerField()
   

************************************2.数据库迁移命令**************************************************
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
 python manage.py makemigrations   #⽣成数据库迁移⽂件
```



![image-20221022111412105](E:\MarkDown\markdown\imgs\image-20221022111412105.png)

应用文件夹下的==migrations文件夹==下多了一个==0001._initial.py==日志文件

![image-20221022111604561](E:\MarkDown\markdown\imgs\image-20221022111604561.png)

```python
 python manage.py migrate  #⽣成数据库表 
#将操作真正同步到数据库中
```

![image-20221022112226431](E:\MarkDown\markdown\imgs\image-20221022112226431.png)

**数据库迁移的两条命令输入完成后，此时数据库中会出现很多张表**



![image-20221022115814598](E:\MarkDown\markdown\imgs\image-20221022115814598.png)

**一个Djanog项目可以有多个应用，那么多个应用之间可能会出现表名冲突的情况，那么加上前缀就可以完全避免冲突**    

![image-20221022120128278](E:\MarkDown\markdown\imgs\image-20221022120128278.png)

```python
"""
只要修改了models.py中跟数据库相关的代码，就必须重新执行数据迁移的两条命令
"""
```

### 2、字段的增删改查

##### 2. 1、 **字段的增加**

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

##### 2. 2、字段的修改

```python
"""直接修改代码然后执行数据库迁移的两条命令即可"""
```

![image-20221022125639587](E:\MarkDown\markdown\imgs\image-20221022125639587.png)

##### 2. 3、字段的删除

```python
直接注释掉对应的字段代码，然后执行数据迁移的两条命令即可
执行完毕后，字段对应的数据都没有了

"""
在操作models.py时，一定要细心

执行迁移命令之前一定要检查一下代码
"""

```

### 3、数据的增删改查

##### 3. 1、 查数据

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

#####   3. 2 、增加数据

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

##### 3. 3 、修改数据

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

##### 3. 4、 删除数据

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



##### **==3.5、示例==：**

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
                            <a href="/delete_user/user_id={{ user_obj.id }}" class="btn btn-danger btn-xs">删除</a>
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
    delete_id = request.GET.get('user_id')

    # 直接去数据库找到对应的数据删除即可
    models.User.objects.filter(id=delete_id).delete()
    """
    批量删除
    """
    #跳转到展示页面
    return redirect('/userlist/')
```

#### 4、orm中创建表关系（==外键==）

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

## ==路由层==

当⽤户在您的Web应⽤程序上发出⻚⾯请求时，Django会获取url中请求路径（端 ⼝之后的部分），然后通过urls.py⽂件查找与路径相匹配的视图，然后返回HTML 响应或404未找到的错误（如果未找到）。在urls.py中，最重要的是 **`urlpatterns` 列表。这是您定义URL和视图之间映射的地⽅。映射是URL模式中的path对象**

### 1、路由匹配

```python
from django.conf.urls import patterns, include
from app import views
urlpatterns = [
 path('hello/', views.hello, name='hello'),
 path('blog/', include('blog.urls')),
]
```

==path对象有四个参数==

*  模式串：匹配⽤户请求路径的字符串（和flask⼀样） 

* 视图函数：匹配上⽤户指定请求路径后调⽤的是视图函数名 kwargs：

* 可选参数，需要额外传递的参数，是⼀个字典 

* 名称（name）：给路由命名，在代码中可以使⽤name进⾏反向解析（由 name获取⽤户请求路劲）。

  ---

另外，如果path中模式串如果不能满⾜你路由规则，还可以使⽤re_path对象， re_path对象中模式串是正则表达式，其他三个参数和path对象⼀致.

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

**在path中使⽤<参数名>表示所传参数，视图函数中的参数名必须和**==**<>中参数名⼀ 致**==，参数可以是以下类型：

```python
str：如果没有指定参数类型，默认是字符串类型。字符串参数可以匹配除/和
空字符外的其他字符串
int：匹配0和正整数，视图函数的参数将得到⼀个整型值
slug：匹配由数字、字⺟、-和_组成的字符串参数
path：匹配任何⾮空字符串，包括/。
```

* **string**

```python
# 总路由
urlpatterns = [
    path('admin/', admin.site.urls),
    # 路由分发，根路由中包含子路由
    # blog/ 路由前缀
    path('blog/', include('App.urls')),
]
# 子路由
    # string
    path('change/<name>/', views.change, name='change'),
    # path第三个参数name是路由的名字，与视图函数参数name无关

# views.py
def change(request, name):
    return HttpResponse(name)
```

效果图:

![image-20221105214456492](E:/MarkDown/markdown/imgs/image-20221105214456492.png)

* **int**

```python
# 子路由
urlpatterns = [
    # int
    path('show/<int:age>/', views.show, name='show')

]

# views.py
def show(request, age):
    print(type(age))
    return HttpResponse(str(age))

```

效果图：

![image-20221105210631000](E:/MarkDown/markdown/imgs/image-20221105210631000.png)



* **slug**

```python
# 子路由
path('list/<slug:name>/', views.list, name='list')
    
# views.py
def list(request,name):
    print(name,type(name))
    return HttpResponse(name)
```

效果如图：

![image-20221105211639543](E:/MarkDown/markdown/imgs/image-20221105211639543.png)

* **path**

```python
# 子路由

	# path,如果有多个参数，path类型必须在最后一个 
path('access/<path:path>/',views.access,name='access')
    
# views.py
def access(request, path):
    # path 可以包含任何字符,包括 /
    return HttpResponse(path)

```

效果如图:



![image-20221105212419964](E:/MarkDown/markdown/imgs/image-20221105212419964.png)

---

Django检查url模式之前会移除模式前的`/`，所以url模式前⾯的`/`可以不写，但如果 在地址栏⾥请求的时候不带尾斜杠，则会引起重定向，重定向到带尾斜杠的地址， 所以请求的时候要带尾斜杠。

![image-20221023134245558](E:\MarkDown\markdown\imgs\image-20221023134245558.png)

**django路由匹配 的时候其实可以匹配两次，第一次如果url后面没有加斜杠，django会让浏览器加斜杠在发送一次请求**

```python
# settings.py

APPEND_SLASH = False# 取消自动加斜杠，默认是True
"""取消了自动加斜杠后，再次访问浏览器就会报错"""
```

![image-20221023134109323](E:\MarkDown\markdown\imgs\image-20221023134109323.png)

---

`re_path`中模式包含了⼀ 个上尖括号(`^`)和⼀个美元符号(`$`)。这些都是正则符号，**上尖括号表示从字符串开 头匹配**。**美元符号表示匹配字符串的结尾**，这两个符号和到⼀起就表示模式必须完 全匹配路径，⽽不是包含⼀部分。⽐如对于模式：

`r'^hello/$' `如果不包含美元符，也就是` r'^hello/' `，任何以/hello/的url都可以 匹配，例如：/hello/world/、/hello/1/2/等，同样如果不以上尖括号开头，则任何 以hello/做结尾的url都可以匹配。

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

==模式匹配的时候要注意：== 

* Django从上往下进⾏模式匹配，⼀旦匹配成功就不会往下继续匹配了 ⼀个视图函数可以有多个模式匹配 
* 模式前⾯不需要加`/ `
* 如果匹配不上，则会引起异常，Django会调⽤错误处理视图处理（关闭调试模式）

---



### 2、无名/有名分组

在re_path中，（）部分是正则的组， django在进⾏url匹配时，就会⾃动把匹配 成功的内容，作为参数传递给视图函数。



**无名**

* 无名分组就是将括号内正则表达式匹配到的内容当作位==**置参数**==传递给后面的视图函数

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
#
```

**有名**

* 有名分组就是将括号内正则表达式匹配到的内容当作==**关键字参数**==传递给后面的视图函数

```python
"""
可以给正则表达式起别名

对正则表达式分组进⾏命名
"""
#urls.py
 re_path(r'^testadd/(?P<year>\d+)', views.testadd),
    
#views.py
def testadd(request, year):
    print(year)
    return HttpResponse('testadd')


```

**匹配/分组算法：**

* 在⼀个匹配模式中要么使⽤命名分组，要么使⽤⽆命名分组，不能同时使⽤ 
* 请求的URL被看做是⼀个普通的Python 字符串， URLconf在其上查找并匹 配。进⾏匹配时将不包括GET或POST请求⽅式的参数以及域名。换句话讲， 对同⼀个URL的⽆论是POST请求、GET请求、或HEAD请求⽅法等等 —— 都 将路由到相同的函数。 
* 每个捕获的参数都作为⼀个普通的Python 字符串传递给视图，⽆论正则表达 式使⽤的是什么匹配⽅式。

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



### 3、反向解析

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

### 4、无名有名反向解析

```python
#无名分组的反向解析
re_path(r'^index/(\d+)/',views.index,name='zs'),

#前端
<a href="{% url 'zs' 1 %}"></a>

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

### 5、路由分发

```python
"""
Django的每一个应用都可以有自己的templates文件夹，urls.py static文件夹
正是基于上述的特点，Django能够非常好的做到多人分组开发，（每个人只写自己的app）


当一个django项目中的url特别多，总路由urls.py代码就非常冗余，不易于维护，
这个时候就可以利用路由分发，减轻总路由压力


利用路由分发之后，总路由不再干路由与视图函数的直接对应关系
而是做一个分发处理
	识别当前url属于哪个应用下的，直接分发给对应的应用去处理
	
"""


"""总路由"""

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


"""子路由"""

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

### 6、名称空间

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



### 7、伪静态

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

## ==视图层==

视图响应的过程：

 当⽤户从浏览器发起⼀次请求时，⾸先django获取⽤户的请求，然后通过路由 （urls）将请求分配到指定的视图函数。视图函数负责进⾏相应的业务处理，处 理完毕后把结果（可能是json、html等）浏览器

### 1、三板斧

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



### 2、JsonResponse对象

`JsonResponse` 是`HttpResponse`的⼦类，⽤于向客户端返回`json`数据。⼀般⽤于` ajax`请求。它的`content-type`缺省值为：`application/json `

```python
"""
json格式数据有什么用？：
	前后端数据交互的时候需要使用json作为过渡，实现跨语言传输数据

前端系列化                      python系列化
	JSON.stringify()			json.dumps()
	JSON.parse()				json.loads()
"""
Django后端给前端返回json格式的数据
	1. 手动利用json模块
    2.利用JsonResponse
```

* **方式一:  json模块**

```python
#方式一:json模块
def ab_json(request):
    user_dic = {'username': '我是谁', 'password': 123, 'hobby': 'JayChou'}

    # 先转成json格式字符串
    # json_str=json.dumps(user_dic,ensure_ascii=False)
    # #将该字符串返回
    # return HttpResponse(json_str)
```

* **方式二 :JsonResponse对象**

```python
 #方式二 :JsonResponse对象
from django.http import JsonResponse
def ab_json(request):
    user_dic = {'username': '我是谁', 'password': 123, 'hobby': 'JayChou'}
    l=[111,222,33,444]
    # return JsonResponse(user_dic, json_dumps_params={'ensure_ascii': False})
    return JsonResponse(l,safe=False)

默认只能序列化字典，序列化其他需要加safe参数
```



### 3、form表单上传文件及后端如何操作

```python
"""
from 表单上传文件类型的数据
	1. method必须指定post
	2.enctype必须换成form-data
"""

<form action="" method="post" enctype="multipart/form-data">
    <p>username:<input type="text" name="username"></p>
    <p>file:<input type="file" name="myfile"></p>
    <input type="submit">

</form>


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

### 4、request对象

HttpRequest是从web服务器传递过来的请求对象，经过Django框架封装产⽣的， 封装了原始的Http请求

* 服务器接收到http请求后，django框架会⾃动根据服务器传递的环境变量创建 HttpRequest对象 
* 视图的第⼀个参数必须是HttpRequest类型的对象 
* 在django.http模块中定义了HttpRequest对象的API 
* 使⽤HttpRequest对象的不同属性值，可以获取请求中多种信息

| 属性        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| contenttype | 请求的mime类型                                               |
| GET         | ⼀个类似于字典的QueryDict对象，包含get请求⽅式的所有参 数，也就是“?”后⾯的内容 |
| POST        | ⼀个类似于字典的QueryDict对象，包含post请求⽅式的所有参 数   |
| COOKIES     | ⼀个标准的Python字典，包含所有的cookie，键和值都为字符串     |
| SESSION     | ⼀个类似于字典的对象，表示当前的会话，只有当Django启⽤会 话的⽀持时才可⽤ |
| PATH        | ⼀个字符串，表示请求的⻚⾯的完整路径，不包含域名             |
| method      | ⼀个字符串，表示请求使⽤的HTTP⽅法，常⽤值包括：GET、 POST， |
| FILES       | ⼀个类似于字典的QueryDict对象，包含所有的上传⽂件            |
| META        | 请求的请求头的源信息（请求头中的键值对）                     |
| encoding    | 字符编码                                                     |
| scheme      | 协议                                                         |

META中常用的键：

| 键           | 说明       |
| ------------ | ---------- |
| HTTP_REFERER | 来源⻚⾯   |
| REMOTE_ADDR  | 客户端ip   |
| REMOTE_HOST  | 客户端主机 |

下⾯是常⽤的⽅法:

| 方法名               | 说明                    |
| -------------------- | ----------------------- |
| get_host()           | 获取主机名+端⼝         |
| get_full_path()      | 获取请求路径+查询字符串 |
| is_ajax()            | 如果是ajax请求返回True  |
| build_absolute_uri() | 完整的url               |



```python
"""
request.method
request.GET
request.POST
request.FILES
request.is_ajax() 判断当前请求是否是ajax请求，返回布尔值

request.path		#只能获取路由
request.path_info   #只能获取路由
request.get_full_path() #能够获取路由以及问号后面的参数
request.body   #原生的浏览器发过来的二进制数据

"""
print(request.path)#/app01/ab_file/
print(request.path_info)#/app01/ab_file/
print(request.get_full_path())#/app01/ab_file/?username=lisi
```
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
```
```python
# 子路由urls.py

 # path,如果有多个参数，path类型必须在最后一个
path('access/<path:path>/', views.access, name='access'),



#views.py

def access(request, path):
    # path 可以包含任何字符,包括 /

    # 获取请求方法
    print(request.method)
    # 获取请求路径
    print(request.path)
    # 其他请求属性
    print(request.META)
    # 客户端地址
    print(request.META.get('REMOTE_ADDR'))
    # 来源页面
    print(request.META.get('HTTP_REFERER'))

    # 常用方法
    print(request.get_host())  # 127.0.0.1:8000
    print(request.get_full_path())
    print(request.build_absolute_uri())

    # 获取请求参数的字典  QueryDict转成dict
    print(request.GET.dict())  # {'name': '12'}
    return HttpResponse(path)

```
![在这里插入图片描述](E:/MarkDown/markdown/imgs/4a4702a214cd46e3a904897c1fccb1f8.png)

```python
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

### 5、响应对象

每⼀个视图函数必须返回⼀个响应对象，HttResponse对象由程序员创建并返回。

| 属性         | 说明               |
| ------------ | ------------------ |
| content      | 字节字符串         |
| charset      | 字符编码           |
| status_code  | http状态码         |
| content_type | 指定输出的MIME类型 |



```python
# urls.py
	path('response/', views.handle_response, name='response'),
    
    
 # views.py   
    def handle_response(request):
    res = HttpResponse('响应对象')
    res.content = b'good bye'
 	res.charset = "utf-8"
    res.content_type = 'text/html'
    res.status_code = 400
    return None
```



![image-20221107224043800](E:/MarkDown/markdown/imgs/image-20221107224043800.png)

### 6、FBV与CBV

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
        #只要是有处理业务逻辑的视图函数，形参里面都应该有request
        def get(self, request):
            return render(request,'form.html')

        def post(self, request):
            return HttpResponse('post方法')

    """
    CBV特点：能够根据请求方式的不同直接匹配到对应的方法执行
    """
```

### 6、CBV源码剖析

## ==模版层==

### 1、模板语法传值

* 变量相关：{{ }}

* 逻辑相关：{% %}

```html
#templates文件夹下的login.html
<body>
<p>{{ n }}</p>
<p>{{ f }}</p>
<p>{{ s }}</p>
<p>{{ b }}</p>
<p>{{ l }}</p>
<p>{{ d }}</p>
<p>{{ t }}</p>
<p>{{ se }}</p>
<p>传递函数名会自动加括号调用， 但是模板语法不支持给函数传额外的参数:{{ func }}</p>

<p>传递类的时候也会自动加括号调用（实例化）:{{ MyClass }}</p>
<p>{{ obj }}</p>
<p>内部能够自动判断出当前的变量是否可以加括号调用，如果可以就会自动加括号执行， 一般情况针对的是函数名和类名</p>

<p>{{ obj.get_self }}</p>
<p>{{ obj.get_class }}</p>
<p>{{ obj.get_func }}</p>

    
<p>{{ d.username }}</p>
<p>{{ l.0 }}</p>
<p>{{ d.hobby.3.info }}</p>
</body>



Django模板语法的取值就固定格式，只能采用"句点符"

可以点索引，可以点键，两者也可以混用

```

```python
#views.py

def index(request):
    # 模板语法可以传递的后端数据类型
    n = 123
    f = 11.1
    s = '1231'
    b = True
    l = ['123', '45', '哈哈']
    t = (111, 22, 33, 44)
   	d = {'usernaem': 'lisi', 'age': 18,'hobby':[111,222,333,{'info':'NB'}]}
    se = {'123', '133', 'ef'}

    def func():
        print('我i被执行了')
        return '函数'

    class MyClass(object):
        def get_self(self):
            return 'self'

        @staticmethod
        def get_func(self):
            return 'func'

        @classmethod
        def get_class(cls):
            return 'cls'
        
        #对象被展示到html页面上，就类似于执行答应操作，也会触发__str__方法
        def __str__(self):
            return '__str__'

    obj = MyClass()

    return render(request, 'index.html', locals())
    # return render(request, 'index.html', {})#一个个传

```

### 2、过滤器

**（过滤器最多只能有两个参数）**

```python
"""
类似于模板语法内置的 内置方法
django内置有60多个过滤器

"""
#基本语法
{{数据|过滤器:参数}}
|length
|default
|filesizeformat  文件大小
|date:'Y-m-d H:i:s'
|slice:'0:6:2'
|truncatechars(包含三个点)
|tucncatewords（不包含三个点，按空格切）
|add  数字加，字符串拼接
|cut
|join
|safe


# 转义
    #前端
    |safe
    #后端
    from django.utils.safestring import  mark_safe
    res=mark_safe('<h1>静静</h1>')
"""
前端代码不一定非要在前端页面书写，也可以先在后端写好，然后传递给前端页面
"""
```

* 常间过滤器

```html
<h1>过滤器</h1>
<p>统计长度:{{ s|length }}</p>
<p>默认值:{{ b|default:'nothing!!!' }}</p>
<p>文件大小:{{ file_size|filesizeformat }}</p>
<p>日期格式化:{{ current_time|date:'Y-m-d H:i:s' }}</p>
<p>切片操作:{{ l|slice:'0:4:2' }}</p>
<p>切取字符:{{ info|truncatechars:9 }}</p>
<p>切取单词(按照空格切):{{ word|truncatewords:9 }}</p>
<p>切取中文(按照空格切):{{ info|truncatewords:9 }}</p>
<p>移除特定的字符:{{ msg|cut:" " }} </p>
<p>拼接:{{ l|join:'$' }}</p>
<p>拼接操作（加法）:{{ n|add:10 }}</p>
<p>拼接操作（加法）:{{ s|add:msg }}</p>
<p>取消转义:{{ hhh }}</p>
<p>转义:{{ hhh|safe }}</p>
<p>转义:{{ sss|safe }}</p>
<p>转义:{{ res }}</p>
```

```python
def index(request):
    # 模板语法可以传递的后端数据类型
    n = 123
    f = 11.1
    s = '1231'
    b = True
    l = ['123', '45', '哈哈', '1414141']
    t = (111, 22, 33, 44)
    d = {'usernaem': 'lisi', 'age': 18, 'hobby': [111, 222, 333, {'info': 'NB'}]}
    se = {'123', '133', 'ef'}
    file_size = 123131211

    def func():
        print('我i被执行了')
        return '函数'

    class MyClass(object):
        def get_self(self):
            return 'self'

        @staticmethod
        def get_func(self):
            return 'func'

        @classmethod
        def get_class(cls):
            return 'cls'

        # 对象被展示到html页面上，就类似于执行答应操作，也会触发__str__方法
        def __str__(self):
            return '__str__'

    obj = MyClass()

    file_size = 123131211
    current_time = datetime.datetime.now()
    info = '奥克兰房间里卡机分厘卡即使对方考虑按时间可了收到v不过他的风格公司v的是v打发发士大夫阿发发发是歌舞团和经济仍维持v房间啊'


    word='my name is  zhao my age is 18 and i  am from china'
    msg='i love you '
    hhh='<h1>蛇姐</h1>'
    sss='<script>alert(123)</script>'

    from django.utils.safestring import  mark_safe

    res=mark_safe('<h1>静静</h1>')

    return render(request, 'index.html', locals())

    # return render(request, 'index.html', {})#一个个传
```



### 3、模板语法之标签

*  for循环

```python
# for循环
{% for foo in l %}
    #<p>{{ forloop }}</p>
     <p>{{ foo }}</p>  # 一个个元素
{% endfor %}

"""
{'parentloop': {}, 'counter0': 0, 'counter': 1, 'revcounter': 4, 'revcounter0': 3, 'first': True, 'last': False}
"""
```

* if判断

```python
#if判断
{% if b %}
    <p>buddy</p>
{% elif s %}
    <p>ssss</p>
{% else %}
    <p>guy</p>
{% endif %}
```

* for与if混合使用

```python
#for与if混合使用
{% for foo in l %}
    {% if forloop.first %}
        <p>第一次循环</p>
    {% elif forloop.last %}
        <p>最后一次循环</p>
    {% else %}
        <p>{{ foo }}</p>
    {% endif %}
{% empty %}
    <p>for循环可迭代对象内部没有元素，根本无法循环</p>
{% endfor %}
```

* 处理字典方法

```python
#处理字典其他方法
{% for key in d.keys %}
    <p>{{ key }}</p>

{% endfor %}
{% for value in d.values %}
    <p>{{ value }}</p>
{% endfor %}
{% for item in d.items %}
    <p>{{ item }}</p>
  
```

* with起别名

```python
 #with起别名
{% with  d.hobby.3.info as zs %}
    <p>{{ zs }}</p>
    在with语法内就可以通过as后面的别名快速的使用到前面非常复杂的数据
    <p>{{ d.hobby.3.info }}</p>
{% endwith %}
```

### 4、自定义过滤器，标签以及inclusion_tag

* 自定义过滤器

```python
 
    """
    三步走：
    	1. 必须要在应用下创建一个名字必须叫templatetags文件夹
    	2.在该文件夹内创建任意名称的py文件
    	3.在该py文件内必须先书写两句话
            from django import template

            register = template.Library()
    """

    
{% load tag %}
<p>{{ n|tag:666 }}</p>


@register.filter(name='tag')
def my_sum(v1, v2):
    return v1 + v2
```

* 自定义标签

```python
标签多个参数彼此之间空格隔开
<p>{% plus 'zhao' 123 456 789 %}</p>


# 自定义标签(参数可以有多个)
@register.simple_tag(name='plus')
def index(a, b, c, d):
    return '%s-%s-%s-%s' % (a, b, c, d)
```

* 自定义inclusion_tag

```python
内部原理
	先定义一个方法
    在页面上调用该方法，并且可以传值
    该方法会生成一些数据然后传递给一个html页面
    之后将渲染好的结果放到调用的位置
    
# 自定义inclusion_tag
#mytag.py
@register.inclusion_tag('left_menu.html')
def left(n):
    data = ['第{}项'.format(i) for i in range(n)]
    # 第一种
    # return {'data':data}
    # 第二种
    return locals()  # 把data传递给left_menu.html    


#left_menu.html
<ul>
    {% for datum in data %}
        <li>{{ datum }}</li>

    {% endfor %}

</ul>

#index.html
{% left 10 %}


#当html页面某一个地方的页面需要传参数能够动态的渲染出来，并且在多个页面上都需要使用该局部，那么就考虑该局部页面做成inclusion_tag形式
(bbs中会使用到)
```

![image-20221024224003718](E:\MarkDown\markdown\imgs\image-20221024224003718.png)

![image-20221024224030160](E:\MarkDown\markdown\imgs\image-20221024224030160.png)

![image-20221024224053678](E:\MarkDown\markdown\imgs\image-20221024224053678.png)

### 5、模板的继承

```python
"""
同一个html页面，想要重复使用大部分样式，只是局部修改
"""


#模板的继承，先选好要继承的模板页面
{% extends 'home.html' %}

#继承了之后子页面跟模板页面长得一摸一样，需要在模板页面上提前规划可以被修改的区域
                    {% block 名字 %}


                    {% endblock %}

```

![image-20221025153951035](E:\MarkDown\markdown\imgs\image-20221025153951035.png)

```python
#然后子页面可以声明想要修改哪块划定了的区域
{% block 名字 %}
	子页面内容
    
    子版页面除了可以写自己的代码之外，还可以继续使用模板的内容
    {{ block.super }} 
{% endblock %}
```



![image-20221025152808944](E:\MarkDown\markdown\imgs\image-20221025152808944.png)

```python
        
#一般情况下模板页面上应该至少有三块可以被修改的区域
 1. css区域
 2. html区域
 3. js区域
    
    
每一个子页面都可以有自己独有的html代码，css代码，js代码
```

![image-20221025153413108](E:\MarkDown\markdown\imgs\image-20221025153413108.png)

![image-20221025153715030](E:\MarkDown\markdown\imgs\image-20221025153715030.png)

* **一般情况下，模板页面上划定的区域越多，那么该模板的扩展性就越高，但是如果太多，就还不如自己写**

### 6、模板的导入



```python
"""
将页面的某一个局部当作模块形式
哪个地方需要就可以直接导入使用
"""

{% include 'demo.html' %}"
```

![image-20221025155047637](E:\MarkDown\markdown\imgs\image-20221025155047637.png)

![image-20221025155207250](E:\MarkDown\markdown\imgs\image-20221025155207250.png)

## ==模型层==



### 1、==测试脚本==

```python
"""
当你只是想想测试django中的某一个 py文件内容，那么你可以不用书写前后端交互的形式，而是直接写一个测试脚本即可

脚本代码无论写在应用下的tests.py还是写在自己单独开设的py文件都可以
"""

#测试环境准备
	# 1.去manage.py中拷贝部分代码，
    #2.
        #import django
        #django.setup()

    
    
#tests.py    
import os

def main():
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'day06.settings')
    import django
    django.setup()
    
	#在代码块的下main就可以测试django里的单个py文件了
    from app01 import models
    ......
if __name__ == '__main__':
    main()
```

### 2、==单表操作==

#### 2. 1、表准备

```python
"""
Django自带的sqlite3对日期格式不是很敏感，处理的时候容易出错
"""
class User(models.Model):
    name = models.CharField(max_length=32, verbose_name='姓名')
    age = models.IntegerField()
    register_time = models.DateField()  # 年月日
    """
    DateTime     
    DateTimeField
        两个重要参数，
                    auto_now   :每次操纵数据的时候，该字段会自动将 当前时间更新
                    auto_now_add  :在创建数据的时候，会自动将当前创建时间记录下来，之后只要不修改，那么就一直不变
                    
    """
    
```

#### 2. 2、操作

```python
#增
1.create()
2.对象.save()

#查
1.all()  #查所有
2.filter() #筛选条件，括号内多个参数之间逗号隔开，并且默认是and的关系
3.get() #筛选条件，条件不存在直接报错，不推荐使用

#改
1.update  #queryset对象帮你封装的批量更新
2.对象.save()

#删
1.delete() #queryset对象帮你封装的批量删除
2.对象.delete()

"""在实际项目中，数据是不可能真正删除的，一般情况下都用一个字段来标记是否删除"""
```



```python
#tests.py

from django.test import TestCase

# Create your tests here.
import os


def main():
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'day06.settings')
    import django
    django.setup()

    from app01 import models
    增
    #方法一
    # models.User.objects.create(name='zhao',age=18,register_time='2002-1-21')
   
	#方法二
	# import datetime
    # ctime = datetime.datetime.now()
    # user_dic = models.User(name='lisi', age=19, register_time=ctime)
    # user_dic.save()
    
     删
    #方法一
    # res = models.User.objects.filter(pk=2).delete()
    # print(res)
    """
        pk会 自动查找到当前表的主键字段，指代的就是当前表的主键字段
        用了pk就不需要知道当前表的主键字段到底叫什么了
        
    """
	#方法二
    # user_obj=models.User.objects.filter(pk=1).first()
    # user_obj.delete()
    
    修改
    # models.User.objects.filter(pk=2).update(name='张三')
    
    #user_obj = models.User.objects.get(pk=4)
    #user_obj.name='lisi'
    #user_obj.save()
    """
    get方法返回的直接就是当前数据对象
    但是不推荐使用，
        一旦数据不存在，该方法直接报错
        而filter不会报错，返回空
    """
   
```

#### 2.3、常见13种单表查询方法

```python
all()  #查询所有数据
	 models.User.objects.all()
    
filter()  #带有过滤条件的查询
	models.User.objects.filter()
    models.User.objects.filter(pk=2)
    
get()  #直接拿数据对象，但是条件不存在会报错
	user_pbj = models.User.objects.get(pk=4)

first()	#拿queryset第一个元素
	user_obj=models.User.objects.filter(pk=1).first()
    models.User.objects.all().first()

last()	#拿queryset最后一个元素
	models.User.objects.all().last()

values() #可以指定获取的数据字段  返回结果：列表套字典的形式 select name,age from ...
	models.User.objects.values('name')
    
values_list() #返回结果：列表套元组的形式
	 res=models.User.objects.values_list('name','age')
    
distinct()  #去重一定要是一摸一样的数据才能去重，如果带有主键那么肯定不一样
	res=models.User.objects.values('name','age').distinct()
    
order_by() #排序
	res=models.User.objects.order_by('age')  #默认升序
    res=models.User.objects.order_by('-age') #降序
    
reverse() #反转  反转的前提是已经排过序了 order_by().reverse()
	res=models.User.objects.order_by('age').reverse()
    
count() #统计当前数据的个数
	res=models.User.objects.count()
    
exclude() #排除...
	res=models.User.objects.exclude(name='zhao')

exists()  #判断是否存在,返回布尔值
	res=models.User.objects.exists()
    res=models.User.objects.filter(pk=2).exists()
	
```

代码：

```python
values()
    # res = models.User.objects.values('name')
    # print(res)
    # values_list()
    
values_list()
    # res = models.User.objects.values_list('name', 'age')
    # print(res.query)  # 查看内部封装的sql语句
    """
    上述查看sql语句的方式，只能用于queryset对象
    只有queryset对象才能够.query查看内部的sql语句
    """
    
order_by()
    # res=models.User.objects.order_by('age')
    # print(res)
    
reverse()    
    res=models.User.objects.order_by('age').reverse()
    print(res)
    
count()    
    res=models.User.objects.count()
    print(res)
    
exclude    
    # models.User.objects.exclude(name='zhao')
    # res = models.User.objects.all().first()
    # print(res)
```



### 3、Django终端打印SQL语句

```python
#方式一，只能用于queryset对象
    res = models.User.objects.values_list('name', 'age')
    print(res.query)  # 查看内部封装的sql语句
    """
    上述查看sql语句的方式，只能用于queryset对象
    只有queryset对象才能够.query查看内部的sql语句
    """
    
#方式二,所有的sql语句都能看
    #去配置文件中配置以下代码
    
    LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'console':{
            'level':'DEBUG',
            'class':'logging.StreamHandler',
        },
    },
    'loggers': {
        'django.db.backends': {
            'handlers': ['console'],
            'propagate': True,
            'level':'DEBUG',
        },
    }
}
```



![image-20221026180126520](E:\MarkDown\markdown\imgs\image-20221026180126520.png)

### 4、==神奇的双下划线==

```python
"""神奇的双下划线"""
#年龄大于18
res=models.User.objects.filter(age__gt=18)

#年龄小于18
res=models.User.objects.filter(age__lt=18)

#年龄小于等于18
res=models.User.objects.filter(age__lte=18)

#年龄大于等于18
res=models.User.objects.filter(age__gte=18)

# 年龄在18 获取 32获取40
res=models.User.objects.filter(age__in=[18,32,40])

#年龄在18到40岁之间
res=models.User.objects.filter(age__range=[18,40])
print(res)

# 查询名利里含有o的数据 模糊查询   区分大小写
res = models.User.objects.filter(name__contains='o')
print(res)

# 忽略大小写
res=models.User.objects.filter(name__icontains='p') #'ignore'

# 以什么开头
res = models.User.objects.filter(name__startswith='j')

# 以什么结尾
res = models.User.objects.filter(name__endswith='j')


res=models.User.objects.filter(register_time__month='1')
res=models.User.objects.filter(register_time__day='12')
res=models.User.objects.filter(register_time__week_day='1')
res=models.User.objects.filter(register_time__year='2000')
```

### 5、==多表操作==

#### 5. 1、表准备

```python
from django.db import models

# Create your models here.

class Book(models.Model):
    title = models.CharField(max_length=32)
    price = models.DecimalField(max_digits=8, decimal_places=2)
    publish_data = models.DateField(auto_now_add=True)
    # 一对多
    publish = models.ForeignKey(to='Publish', on_delete=models.CASCADE)

    # 多对多
    author = models.ManyToManyField(to='Author')


class Publish(models.Model):
    name = models.CharField(max_length=32)
    addr = models.CharField(max_length=32)
    email = models.EmailField()  # 本质还是varchar(254)


class Author(models.Model):
    name = models.CharField(max_length=32)
    age = models.IntegerField()

    # 一对一
    author_detail = models.OneToOneField(to='AuthorDetail', on_delete=models.CASCADE)


class AuthorDetail(models.Model):
    phone = models.BigIntegerField()  # 电话号码用BigIntegerField()或者用CharFiled()
    addr = models.CharField(max_length=32)

    
    
"""切记执行表的迁移命令"""    
```

#### 5. 2、一对多外键增删改

```python
 #tests.py
    # 一对多的外键增删改查
    # 增
    # 1直接写实际字段 id
    # models.Book.objects.create(title='西游记', price=81.8, publish_id=1)
    # models.Book.objects.create(title='聊斋', price=61.8, publish_id=2)
    # models.Book.objects.create(title='老子', price=66.8, publish_id=1)
    
    # 2虚拟字段  对象
    # publish_obj = models.Publish.objects.filter(pk=2).first()
    # models.Book.objects.create(title='红楼梦',price=80,publish=publish_obj)

    # 删
    # models.Publish.objects.filter(pk=1).delete()  # 级联删除

    # 修改
    # models.Book.objects.filter(pk=1).update(publish_id=2)
    # publish_obj = models.Publish.objects.filter(pk=1).first()
    # models.Book.objects.filter(pk=1).update(publish=publish_obj)

```

#### 5. 3、多对多外键增删改

```python 
add  给第三张关系表添加数据，括号内既可以传数字，也可以传对象，并且都支持多个

remove  括号内既可以传数字，也可以传对象，并且都支持多个

set	  括号内必须传一个可迭代对象，括号内既可以传数字，也可以传对象，并且都支持多个，   先删除后新增

clear 清空对应的关系数据 括号内不加任何参数   
```



```python
#tests.py

    """多对多 增删改查  就是在操作第三张表"""

    # 如何给数据添加作者
    # book_obj = models.Book.objects.filter(pk=2).first()
    # print(book_obj.author) #就类似于你已经找到了第三张关系表了
    # book_obj.author.add(1)  # 书籍id为1的书籍绑定一个主键为1的作者
    # book_obj.author.add(2,3)

    # author_obj = models.Author.objects.filter(pk=1).first()
    # author_obj1 = models.Author.objects.filter(pk=2).first()
    # author_obj2 = models.Author.objects.filter(pk=3).first()
    # book_obj.author.add(author_obj)
    # book_obj.author.add(author_obj1,author_obj2)
    """
    add 给第三张关系表添加数据，括号内既可以传数字，也可以传对象，并且都支持多个
    """

    # 删
    book_obj = models.Book.objects.filter(pk=2).first()

    # book_obj.author.remove(2)
    # book_obj.author.remove(1,3)

    # author_obj = models.Author.objects.filter(pk=1).first()
    # author_obj1 = models.Author.objects.filter(pk=2).first()
    # book_obj.author.remove(author_obj,author_obj1)
    """
    remove 括号内既可以传数字，也可以传对象，并且都支持多个
    """

    # 修改
    # book_obj.author.set([1, 3])  # 括号内必须给一个可迭代对象
    
    # author_obj=models.Author.objects.filter(pk=2).first()
    # author_obj1=models.Author.objects.filter(pk=3).first()
    # book_obj.author.set([author_obj,author_obj1])
    """
    set 括号内必须传一个可迭代对象，括号内既可以传数字，也可以传对象，并且都支持多个，
        先删除后新增
    """

    # 清空
    # 在第三张表中，清空某个书籍与作者的绑定关系
    book_obj.author.clear()
    """
    clear 括号内不加任何参数
    """
    
```

#### 5. 4、正反向概念

```python

外键字段在我手上，那么我查你就是正向
外键字段不在我手上，我查你就是反向

book>>>外键字段在书那（正向）>>>publish
pupblish >>>外键字段在书那（反向）>>>book


"""
正向查询按字段
反向查询按表名小写
"""
```

#### 5. 5、多表查询

##### 1. 子查询（基于对象的跨表查询）

```python
   # 基于对象的跨表查询
    # 1.查询书籍主键为1的出版社
    # book_obj = models.Book.objects.filter(pk=1).first()
    # # 书查出版社》》正向
    # res = book_obj.publish
    # print(res.name)
    # print(res.addr)

    # 2.查询书籍主键为1的作者
    # book_obj = models.Book.objects.filter(pk=1).first()
    # 书查作者 正向
    # res=book_obj.author   #app01.Author.None
    # res=book_obj.author.all()#<QuerySet [<Author: Author object (1)>, <Author: Author object (3)>]>
    # print(res)

    # 3查询作者zhao的电话号码
    # author_obj = models.Author.objects.filter(name='zhao').first()
    # res = author_obj.author_detail
    # print(res.phone)
    # print(res.addr)


    """
    在书写orm语句的时候，跟写sql 语句是一样的
    不要企图一次性写完，如果比较复杂，就写一点看一点
    
    正向什么时候需要加.all()????
        当结果可能有多个，就需要加.all()
        如果是一个则直接拿到数据对象
     """
   
    # 4查询出版社是东方出版社出版的书
    # publish_obj = models.Publish.objects.filter(name='东方出版社').first()
    # 出版社查书 反向
    # res = publish_obj.book_set.all()
    # print(res)

    # 5.查询作者是zhao写过的书
    # author_obj = models.Author.objects.filter(name='zhao').first()
    # 作者查书，，反向
    # ress = author_obj.book_set.all()
    # print(ress)

    # 6手机号是110的作者姓名
    # author_detail_obj = models.AuthorDetail.objects.filter(phone=110).first()
    # res=author_detail_obj.author
    # print(res.name)

    
 """
    基于对象
        反向查询的时候    
            当你的查询结果可以有多个的时候，就必须加_set.all()
            当结果只有一个的时候，就不许需要加_set.all()
        
 """
```

##### 2.联表查询（基于双下划线的跨表查询）

```python
  """基于双下划线的跨表查询"""
    
    # 1.查询zhao的手机号
    # res = models.Author.objects.filter(name='zhao').values('author_detail__phone')
    # print(res)
    """反向"""
    # res = models.AuthorDetail.objects.filter(author__name='zhao')  # 拿作者姓名是zhao的作者详情
    # res = models.AuthorDetail.objects.filter(author__name='zhao').values('phone','author__name')  # 拿作者姓名是zhao的作者详情
    # print(res)

    # 2查询书籍主键为1的出版社名称和书的名称
    # res = models.Book.objects.filter(pk=1).values('title', 'publish__name')
    # print(res)
    """反向"""
    # res=models.Publish.objects.filter(book__pk=1).values('name','book__title')
    # print(res)

    # 3查询书籍主键为1的作者姓名
    # res=models.Book.objects.filter(pk=1).values('author__name')
    # print(res)
    """反向"""
    # res=models.Author.objects.filter(book__pk=1).values('name')
    # print(res)

    # 4查询书籍主键是1的作者的手机号
    # book author authordetail
    # res = models.Book.objects.filter(pk=1).values('author__author_detail__phone')
    # print(res)  
    """反向"""
    # res1=models.Author.objects.filter(book__id=1).values('author_detail__phone')
    # print(res1)
```

### 6、聚合查询

```python
"""聚合查询   aggregate"""

"""聚合函数通常都是配合分组一起使用的,也可以单独使用，需要借助于aggregate"""

from django.db.models import Max, Min, Sum, Count, Avg

# 只要是跟数据库相关的模块，
	#基本上都在，django.db.models里面,
    #如果没有那么就应该在django.db里面
    
# 统计书的平均价格
# res=models.Book.objects.aggregate(Avg('price'))
# print(res)
res = models.Book.objects.aggregate(Max('price'), Min('price'), Sum('price'), Avg('price'))
print(res)
```

### 7、分组查询

```p
"""分组查询 annotate"""
"""
    MySQL分组之后默认只能获取到分组的依据，组内其他字段都无法直接获取了， 
        严格模式
            ONLY_FULL_GROUP_BY
    
    
"""
# 1统计每一本书的作者个数
from django.db.models import Max, Min, Sum, Count, Avg
# res = models.Book.objects.annotate()  # models后面点什么，就是按什么分组
# res = models.Book.objects.annotate(author_number=Count('author__id')).values('title','author_number')
# res1 = models.Book.objects.annotate(author_number=Count('author')).values('title','author_number')
"""
author_number是自己定义的字段，用来存储统计出来的每本书对应的作者个数
"""
# print(res1)
# 2统计每个出版社卖的最便宜的书的价格
# res = models.Publish.objects.annotate(bargain=Min('book__price')).values('name','bargain')
# print(res)

# 3统计不止一个作者的图书
# 1.先按照图书分组，求每一本书对应的作者个数
# 2过滤出不止一个作者的图书
# res = models.Book.objects.annotate(author_num=Count('author')).filter(author_num__gt=1).values('title', 'author_num')

"""只要你的orm 语句得出的结果还是一个queryset对象那么就可以无限制的点queryset方法"""
# print(res)

# 4查询每个作者出版的书的总价格
# res = models.Author.objects.annotate(sum_price=Sum('book__price')).values('name', 'sum_price')
# print(res)

"""
如果向按照指定的字段分组
    models.Book.objects.values('price').annotate()
"""

"""
如果分组查询报错，
    需要修改数据严格模式
"""
```

### 8、F与Q查询

#### 8. 1、F查询

```python
 """F查询"""
      """能够直接获取到表中某个字段对应的数据"""
        
    # 1.查询卖出数大于库存数的书籍
  
    from django.db.models import F
    res = models.Book.objects.filter(maichu__gt=F('kucun'))
    print(res)

    # 2将所有书籍的价格提升50块
    res = models.Book.objects.update(price=F('price') + 50)
    print(res)

    # 3将所有数的名称后面加上爆款两个字
    """
    在操作字符类型的数据的时候，F不能够直接做到字符串的拼接
    """
    from django.db.models import Value
    from django.db.models.functions import Concat
    res = models.Book.objects.update(title=Concat(F('title'), Value('爆款')))
    print(res)
```

#### 8. 2、Q查询

```python
 """Q查询"""
    """能够改变多个查询条件之间的关系：与或非"""
    
    
    from django.db.models import Q
    # 1查询卖出数大于200或者价格小于50的书籍
    # res = models.Book.objects.filter(Q(maichu__gt=200),Q(price__lt=50))
    # print(res)
    """Q包裹，逗号分割还是and关系，"""
    # res = models.Book.objects.filter(Q(maichu__gt=200)|Q(price__lt=50)) #| or关系
    """竖杠就是or关系"""
    # res = models.Book.objects.filter(~Q(maichu__gt=200) | Q(price__lt=50))  # ~ not关系
    """~就是not关系"""
    # print(res)

    
    """Q高阶用法  能够将查询条件变成字符串的形式"""
    q = Q()
    q.connector='or'
    q.children.append(('maichu__gt', 200))
    q.children.append(('price__lt', 50))
    res = models.Book.objects.filter(q)  # filter内支持直接放q对象，默认还是and关系
    print(res)
```



###  9、Django中开启事务

```python
"""
MySQL中：
    事务
        ACID
            A:原子性	不可分割的最小单位
            C:一致性	跟原子性是相辅相成的
            I:隔离性	事务之间互相不干扰
            D:持久性	事务一旦确认永久生效

        事务的回滚
            roollback
        事务的提交
            commit

"""

#django中如何开启事务
    from django.db import transaction
    try:
        with transaction.atomic():
            # sql语句
            # with代码块内书写的所有orm操作都是属于同一个事务
            pass
    except Exception as e:
        print(e)
```



### 10、orm中常用字段及参数

```python
AutoField	
	主键字段 primary_key=True
CharField	
	max_length字符长度  verbose字符的注释  对应到数据库是varchar

IntegerField  	    int
BigIntegerField  	bigint

DecimalField  
	max_digits=8, decimal_places=2
    
EmailField  varchar(254)

DateField         date
DateTimeField	  datetime
	auto_now :每次修改数据都会自动更新当前时间
    auto_now_add:只在创建数据的时候记录时间，后续不会修改

    
BooleanField  布尔类型
	该字段传布尔值(False/True)，数据库里存0/1
    
TextField   文本类型
	该字段可以存大段内容（文章、博客....），没有字数限制
FileField
    	upload_to='/data'  给该字段传一个文件对象，会自动将文件保存到/data目录下，然后将文件路径保存到数据库中/data/a.txt


"""其他字段参考博客"""
https://www.cnblogs.com/Dominic-Ji/p/9203990.html






```

* Django还支持自定义字段类型

```python
class MyCharField(models.Field):
    def __init__(self, max_length, *args, **kwargs):
        self.max_length = max_length
        super().__init__(max_length=max_length, *args, **kwargs)#一定要是关键字的形式传入

    def db_type(self, connection):
        """
        返回真正的数据类型及各种约束条件
        :param connection:
        :return:
        """
        return 'char(%s)' % self.max_length
   

#自定义字段使用
myfield=MyCharField(max_length=16,null=True)
```

### 11、数据库查询优化

```python
only与defer  正好相反
select_related与prefetch_related  跟跨表操作有关
```



```python
"""

orm语句的特点：惰性查询，如果仅仅只是书写了orm语句，在后面没有用到该语句所查询出来的参数，那么orm 会自动识别，直接不执行
"""
```

#### **only**

```python
    res=models.Book.objects.only('title')
    # res=models.Book.objects.all()
    # print(res)
    for i in res:
        # print(i.title) #点击only括号内的字段，不会走数据库
        print(i.price) #点击only括号外的字段，会重新走数据库查询而all不需要
```

![image-20221029164727597](E:\MarkDown\markdown\imgs\image-20221029164727597.png)



---

![image-20221029164841977](E:\MarkDown\markdown\imgs\image-20221029164841977.png)

#### **defer**

```python
#tests.py

res=models.Book.objects.defer('title')
    for i in res:
        print(i.price)
        """defer与only刚好相反
                defer括号内放到字段不在查询出来的对象里面，查询该字段需要重新走数据
                而如果查询的是非括号内的字段，则不需要走数据库
            """
```



![image-20221029164310790](E:\MarkDown\markdown\imgs\image-20221029164310790.png)

---

![image-20221029164520153](E:\MarkDown\markdown\imgs\image-20221029164520153.png)

---

#### select_related

```python
    res = models.Book.objects.select_related('publish')  # INNER JOIN
    """
    select_related内部直接将book表与publish表连接起来，然后一次性将大表里面的所有数据全部封装给查询出来的对象
    这个时候对象无论是点击book表的数据还是publish数据都无需再走数据查询了
    
    
    select_related括号内只能放外键字段  
        只能放一对一 ，一对多，
        不能放多对多
    
    """
    # print(res)
    for i in res:
        print(i.publish.name)
    

```



#### prefetch_related

```python
    res=models.Book.objects.prefetch_related('publish')  #子查询
    """
    prefetch_related该方法内部是子查询
        将子查询查询出来的所有结果也封装到对象中，
        给你的感觉好像也是一次查询数据库
    """
    for i in res:
        print(i.publish.name)

```

## 图书管理系统搭建

## choices参数（数据库字段设计常见）

当数据可以被列举完，能够供用户选择的时候，能够考虑用choices参数，好比性别，成绩，学历，婚否等等

```python
#models.py

from django.db import models


# Create your models here.

class User(models.Model):
    username = models.CharField(max_length=32)
    age = models.IntegerField()
    # 性别
    gender_choices = (
        (1, '男'),
        (2, '女'),
        (3, '其他'),
    )
    gender = models.IntegerField(choices=gender_choices)
    """
    该gender字段存的还是数字，但是如果存的数字再上面元组列举的范围之内，
    那么就可以获取到数字对应的真正内容
    
    1 gender如果字段存的数字不在上述元组列举的范围内容
        
    2 如果在，获取对应的中文信息
    """
    score_choices = (
        ('A', '优秀'),
        ('B', '良好'),
        ('C', '及格'),
        ('D', '不合格'),
    )
    # 保证字段类型跟列举出来的元组的第一个数据类型一致即可
    score = models.CharField(max_length=32, choices=score_choices, null=True)
```

```python
#tests.py

from django.test import TestCase

# Create your tests here.
import os
import sys


def main():
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'day08.settings')
    import django
    django.setup()
    from app01 import models
    #存
    # models.User.objects.create(score='A', username='zhao', age=19, gender=1)
    # models.User.objects.create(score='B', username='xiaoyu', age=18, gender=2)
    # models.User.objects.create(score='C', username='张三', age=22, gender=3)
    """存的时候没有列举出来的数字也能存"""
    # models.User.objects.create(score='D', username='tony', age=50, gender=4)

    # 取
    user_obj = models.User.objects.filter(pk=1).first()
    print(user_obj.gender)
    # 只要是choices参数字段，如果想要获取对应信息，固定写法，get_字段名_display()
    print(user_obj.get_gender_display())
    print(user_obj.get_score_display())
    user_obj2 = models.User.objects.filter(pk=4).first()
    # 如果没有对应关系，那么字段是什么就展示什么
    print(user_obj2.score)
    print(user_obj2.get_gender_display())
    print(user_obj2.get_score_display())



if __name__ == '__main__':
    main()
```

## MTV与MVC模型

```python
# MTV:django是MTV模型
M: 	  models 
T:    templates
V:    views
# MVC:django本质也是MVC
M:	  models
V:    views
C:    controler(urls.py)
```



## 多对多关系的三种创建方式

* ==全自动==

```python
#全自动:利用orm自动帮我们创建第三张关系表
class Book(models.Model):
    name=models.CharField(max_length=32)
    authors=models.ManyToManyField(to='Author')
class Author(models.Model):
    name=models.CharField(max_length=32)

    """
   优点：代码不需要自己写，还支持orm提供的第三张关系表的方法:(add,remove,clear,set)
   缺点：第三张关系表扩展性极差，（不能额外的添加字段）
    """
    


```

* 纯手动

```python
class Book(models.Model):
    name=models.CharField(max_length=32)
    
class Author(models.Model):
    name=models.CharField(max_length=32)
    
class BookToAuthor(models.Model):
    book_id=models.ForeignKey(to='Book',on_delete=models.CASCADE)
    author_id=models.ForeignKey(to='Author',on_delete=models.CASCADE)
    
    """
    优点:第三张关系表完全取决于自己进行额外的拓展
    缺点:需要写代码较多，不能使用orm提供的简单方法:(add,remove,clear)
    
    
    不建议使用该方式
    """
```



* ==半自动==

```python
class Book(models.Model):
    name = models.CharField(max_length=32)
    authors = models.ManyToManyField(to='Author',
                                     through='BookToAuthor',
                                     through_fields=('book', 'author')
                                     )


class Author(models.Model):
    name = models.CharField(max_length=32)


class BookToAuthor(models.Model):
    book = models.ForeignKey(to='Book', on_delete=models.CASCADE)
    author = models.ForeignKey(to='Author', on_delete=models.CASCADE)

    
   """through_fields字段先后顺序:
        第三张表查询对应的表，需要用到哪个字段就把哪个字段放前面，
        也就是说，当前表是谁，就把对应的关联字段放前面
        
        
        
        
        半自动可以使用orm的正反向查询，但是没有办法使用add,remove,clear,set这四个方法
    """


# 总结:
    只需要掌握全自动和半自动，半自动扩展性高，一般都采用半自动，
```

## ==Ajax（重点）==

* **异步提交**
* **局部刷新**

```python
"""

案例：github注册示例，
	动态获取用户名，实时的跟后端确认并实时展示到前端（局部刷新）

发送请求方式 
	1 浏览器地址栏直接输入url回车     GET请求
	2 a标签href属性				   GET请求
	3 form表单					POST请求/GET请求
	4 ajax						 POST请求/GET请求
	
"""
```

### 1、简介

AJAX（Asynchronous Javascript And XML）翻译成中文就是“异步的Javascript和XML”。即使用Javascript语言与服务器进行异步交互，传输的数据为XML（当然，传输的数据不只是XML）。

**AJAX 不是新的编程语言，而是一种使用现有标准的新方法。**

==AJAX 最大的优点是在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容。（这一特点给用户的感受是在不知不觉中完成请求和响应过程）==

AJAX 不需要任何浏览器插件，但需要用户允许JavaScript在浏览器上执行。

- 同步交互：客户端发出一个请求后，需要等待服务器响应结束后，才能发出第二个请求；
- 异步交互：客户端发出一个请求后，无需等待服务器响应结束，就可以发出第二个请求。

```python
Ajax只学习jQuery封装好之后的版本，所以在前端页面使用ajax的时候需要确保导入jQuery
```

### 2、Ajax语法

```python
案例引入：

"""
页面有三个input框
    在前两个框中输入数字，点击按钮，朝后端发送到ajax请求
    后端计算出结果，再返回给前端动态展示到第三个input框
    （整个过程不允许有刷新，也不能再前端计算）

"""
```

```python
# index.html

<body>
<input type="text" id="d1">+
<input type="text" id="d2">=
<input type="text" id="d3">
<p>
    <button id="btn">Click</button>
</p>
<script>
    //给按钮绑定点击事件
    $('#btn').click(function () {
        //朝后端发送ajax请求
        $.ajax({

            //1 指定朝哪个后端发送ajax请求
            url: '',//不写朝当前地址提交,跟action三种书写方式一致
            //2 请求方式
            type: 'post',//不指定，默认就是get
            //3 数据
            data: {'d1': $('#d1').val(), 'd2': $('#d2').val()},
            //4 回调函数:当后端给你返回结果的时候会自动触发 args接收后端的返回结果
            datatyp: true, //会自动反序列化
            success: function (args) {
                $('#d3').val(args)//通过DOM操作，动态渲染到第三个Input框
                console.log(typeof (args))
                
            }
        })


    })
</script>
</body>

"""
针对后端，如果是用HttpResponse返回的json数据，前端回调函数不会自动反序列化，
如果后端直接用的JsonResponse返回的数据，回调函数会自动反序列化


HttpResponse解决方式
	1 自己再前端用JSON.parse()
	2.在ajax里配置一个参数
		dataType:'JSON'  
    """

```

**当利用ajax进行前后端交互的时候，后端无论返回什么，都只会被回调函数接收，而不再影响这个浏览器页面**。

```python
# views.py

from django.shortcuts import render, HttpResponse

from django.http import JsonResponse

# Create your views here.
def ab_ajax(request):
    if request.method == 'POST':
        # print(request.POST)
        # d1 = request.POST.get('d1')
        # d2 = request.POST.get('d2')
        # # 先转成整型
        # d3 = int(d1) + int(d2)
        # print(d3)
        d={'code':100,'msg':666}
        # return HttpResponse(d3)
        # return HttpResponse(json.dumps(d))
        return HttpResponse(JsonResponse(d))
    return render(request, 'index.html')
```

```python
# urls.py

from django.contrib import admin
from django.urls import path
from app01 import  views
urlpatterns = [
    path('admin/', admin.site.urls),
    #ajax相关
    path('ab_ajax/',views.ab_ajax)
]
```

### 3、前后端传输数据的编码格式(contentType)

```python
# 主要研究 post请求数据的编码格式

"""
get请求数据就是直接放在url后面的  
	url?username=zhao&password=123
"""

#可以朝后端发送post的请求方式
	"""
	1. form表单
	2. Ajax请求
	"""
    
# 前后端传输数据的编码格式
	"""
	urlencoded
	fromdata
	json
	"""
```

#### 3.0、代码

```python
# index.html

<form action="" method="post" >
    <p>username:<input type="text" name="username" id="" class="form-control"></p>
    <p>password:<input type="text" name="password" id="" class="form-control"></p>
    <p>file:<input type="file" name="file" id=""></p>
    <input type="submit" name="" id="" class="btn btn-primary">
    <input type="button" name="" id="d1" class="btn btn-danger" value="click">
</form>


<script>
    $('#d1').click(function (){
        $.ajax({
            utl:'',
            type:'post',
            data:{'username':'tony','age':19},
            success:function (args){

        }
        })
    })
</script>
```

```python
# views.py

from django.shortcuts import render

# Create your views here.
def index(request):
    if request.method =='POST':
        print(request.POST)
        print(request.FILES)
    return render(request,'index.html')
```

```python
# urls.py 

from django.contrib import admin
from django.urls import path
from app01 import  views
urlpatterns = [
    path('admin/', admin.site.urls),
    #前后端编码格式研究
    path('index/',views.index)
]

```



#### 3.1、form表单

```python
默认的编码格式是urlencoded
数据格式：username=zhao&password=123
django后端针对符合urlencoded编码格式的数据会自动的帮你解析封装到request.POST中
	username=zhao&password=123  >>> request.POST
   
如果把编码格式改成formdata，那么针对普通的键值对还是解析到request.POST中而将文件解析到request.FILES中


form表单没有办法发送json格式
```

* **默认的编码格式**

![image-20221102200546457](E:/MarkDown/markdown/imgs/image-20221102200546457.png)

* **数据格式**

![image-20221102201124294](E:/MarkDown/markdown/imgs/image-20221102201124294.png)

![image-20221102201322322](E:/MarkDown/markdown/imgs/image-20221102201322322.png)



* **django后端针对符合urlencoded编码格式的数据会自动的帮你解析封装到request.POST中**



![image-20221102202334553](E:/MarkDown/markdown/imgs/image-20221102202334553.png)

![image-20221102203015897](E:/MarkDown/markdown/imgs/image-20221102203015897.png)



* **如果把编码格式改成formdata，那么针对普通的键值对还是解析到request.POST中,而将文件解析到request.FILES中**



![image-20221102202407309](E:/MarkDown/markdown/imgs/image-20221102202407309.png)

![image-20221102202618109](E:/MarkDown/markdown/imgs/image-20221102202618109.png)

![image-20221102202655337](E:/MarkDown/markdown/imgs/image-20221102202655337.png)

#### 3.2、ajax

```python
默认编码格式也是urlencoded
数据格式也是:username=tony&age=19
django后端针对符合urlencoded编码格式的数据会自动的帮你解析封装到request.POST中
	username=zhao&password=123  >>> request.POST
```

![image-20221102204855809](E:/MarkDown/markdown/imgs/image-20221102204855809.png)



* **编码格式**

  

![image-20221102203718139](E:/MarkDown/markdown/imgs/image-20221102203718139.png)



* **数据格式**



![image-20221102203825877](E:/MarkDown/markdown/imgs/image-20221102203825877.png)



* **django后端针对符合urlencoded编码格式的数据会自动的帮你解析封装到request.POST中**



![image-20221102204038587](E:/MarkDown/markdown/imgs/image-20221102204038587.png)

### 4、ajax发送json格式数据

==前后端传输数据的时候一定要确保**编码格式**跟**数据格式**是**一致**的==

```python
"""
前后端传输数据的时候一定要确保编码格式跟数据真正的格式是一致的

{"username":"zhao","age":19} 在request.POST里肯定找不到
"""

request对象补充
	request.is_ajax()
	判断当前请求是否是ajax请求，返回布尔值
    
django对json格式的数据不会做任何的处理

```

**代码:**

```html
<!--ab_json.html-->

<body>
<button class="btn btn-danger" id="d1">click me !!</button>

<script>
    $('#d1').click(function () {
        $.ajax({
            url: '',
            type: 'post',
            data: JSON.stringify({'username': 'zhao', 'age': 19}),
            contentType: 'application/json',//指定编码格式
            success: function () {


            }
        })
    })
</script>
</body>
```

```python
# views.py

def ab_json(request):
    # print(request.is_ajax())  # 判断当前请求是否是ajax请求，返回布尔值
    if request.is_ajax():
        # print(request.POST)
        # print(request.body)
        # 针对json格式数据需要自己手动处理
        json_bytes = request.body  # 二进制数据
        # json_str = json_bytes.decode('utf-8')  # 将二进制转成字符串
        # json_dic = json.loads(json_str)
        """json.loads括号内如果传入了一个二进制数据，那么内部可以自动解码再反序列化"""
        json_dic = json.loads(json_bytes)
        print(json_dic, type(json_dic))

    return render(request, 'ab_json.html')
```



```python
# urls.py

from django.contrib import admin
from django.urls import path
from app01 import  views
urlpatterns = [
    path('admin/', admin.site.urls),

    #ajax发送json格式的数据
    path('ab_json/',views.ab_json)
]
```

==ajax发送json格式数据需要注意以下几点==：

1. `contentType`参数指定成`application/json`
2. 确保是真正的json格式数据,`JSON.stringify()`
3. django后端不会自动处理json格式，需要去`request.body`中获取数据并处理

---

* **指定编码格式**

![image-20221102205741523](E:/MarkDown/markdown/imgs/image-20221102205741523.png)

![image-20221102205908090](E:/MarkDown/markdown/imgs/image-20221102205908090.png)

* 但是此时要发送的数据并不是json格式,**需要转成json格式**
* 前端使用`JSON.stringify()`将想要发送的数据转成json格式的数据

```python
data: JSON.stringify({'username': 'zhao', 'age': 19}),
```



![image-20221102210141395](E:/MarkDown/markdown/imgs/image-20221102210141395.png)

* 查看json格式数据

```python
{"username":"zhao","age":19}
```

![image-20221102210354092](E:/MarkDown/markdown/imgs/image-20221102210354092.png)

* 在`request.POST`中找不到数据

![image-20221102210714716](E:/MarkDown/markdown/imgs/image-20221102210714716.png)

* 去`request.body`中获取数据，转成`json`格式

![image-20221103171457707](E:/MarkDown/markdown/imgs/image-20221103171457707.png)

```python
json.loads() 括号内如果传入了一个二进制数据，那么内部可以 自动解码再反序列化
```

![image-20221102213336984](E:/MarkDown/markdown/imgs/image-20221102213336984.png)

### 5、ajax发送文件数据

```python
"""ajax发送文件需要借助于js内置对象FormData"""

```

```html
<p>username:<input type="text" id="d1"></p>
<p>password:<input type="text" id="d2"></p>
<p><input type="file" id="d3"></p>
<button class="btn btn-info" id="d4">Click Me !!!</button>


<script>

    //点击按钮朝后端发送普通键值对和文件数据
    $('#d4').on('click', function () {
        //1. 需要先利用FormData生成一个内置对象
        let formDataObj = new FormData();
        //2. 添加普通的键值对
        formDataObj.append('username', $('#d1').val());
        formDataObj.append('password', $('#d2').val());
        //3. 添加文件对象
        formDataObj.append('myfile', $('#d3')[0].files[0])
        //4. 将对象基于ajax发送给后端
        $.ajax({
            url: '',
            type: 'post',
            data: formDataObj,//直接将对象放在data后面即可
            //ajax发送文件必须指定两个参数
            contentType: false, //不需要使用任何编码，django后端能自动识别formdata对象
            processData: false, //告诉浏览器不要对数据进行任何处理

            success: function (args) {

            }
        })
    })
</script>
```

```python
# views.py

def ab_file(request):
    if request.is_ajax():
        if request.method == 'POST':
            print(request.POST)
            print(request.FILES)
    return render(request, 'ab_file.html')

```

```python
# urls.py

from django.contrib import admin
from django.urls import path
from app01 import views

urlpatterns = [
    path('admin/', admin.site.urls),

    # ajax发送文件
    path('ab_file/', views.ab_file),
]
```

![image-20221102221448232](E:/MarkDown/markdown/imgs/image-20221102221448232.png)

#### **总结：**

1. ajax发文件需要利用内置对象`FormData`

```html
       let formDataObj = new FormData();
        //2. 添加普通的键值对
        formDataObj.append('username', $('#d1').val());
        formDataObj.append('password', $('#d2').val());
        //3. 添加文件对象
        formDataObj.append('myfile', $('#d3')[0].files[0])
```

2. 需要指定两个关键性参数

```python
contentType: false, //不需要使用任何编码，django后端能自动识别formdata对象
processData: false, //告诉浏览器不要对数据进行任何处理
```

3. django后端能自动识别`FormData`对象，能够将内部的普通键值自动封装到`request.POST`中，文件数据能==自动解析封装==到`request.FILES`中

### 6、Ajax结合sweetalert实现删除按钮的二次确认



```html
<style>
        div.sweet-alert h2 {
            padding-top: 15px;
        }
</style>


<h1 class="text-center">数据展示</h1>
<table class="table table-hover table-striped">
    <thead>
    <tr>
        <th>ID</th>
        <th>username</th>
        <th>age</th>
        <th>gender</th>
        <th>describe</th>
    </tr>
    </thead>
    <tbody>
    {% for user_obj in user_queryset %}
        <tr>
            <td>{{ user_obj.pk }}</td>
            <td>{{ user_obj.age }}</td>
            <td>{{ user_obj.username }}</td>
            <td>{{ user_obj.gender }}</td>
            <td>
                <button class="btn btn-toolbar btn-xs">编辑</button>
                <!--注意，绑定ajax事件在for循环中不能加id，每for循环一次出现一个按钮，
                  如果绑定id就意味着for循环后出现的按钮id值一致，使用class=del
                    我们需要实现用户点击删除按钮，后端能够知道用户到底要删那条数据，
                    后端怎么知道？主键。
                   自定义属性

                   ----->

                <button class="btn btn-danger btn-xs del" delete_id="{{ user_obj.pk }}">删除</button>
            </td>

        </tr>
    {% endfor %}

    </tbody>
</table>


<script>
    $('.del').on('click', function () {
        //先将当前标签对象存储，用变量指代当前被点击对象
        let currentBtn = $(this);
        //二次确认弹框
        swal({
                title: "确定删除？?",
                text: "确定要删吗！！!",
                type: "warning",
                showCancelButton: true,    //延时效果
                confirmButtonClass: "btn-danger",
                confirmButtonText: "我就要删!",
                cancelButtonText: "算了 算了 不删了",
                closeOnConfirm: false,
                closeOnCancel: false
            },
            //isConfirm 判断用户有没有点击二次确认删除按钮
            function (isConfirm) {
                if (isConfirm) {
                    // 朝后端发送ajax请求删除数据之后，后端判断是否有数据，再谈下面的提示框，
                    $.ajax({
                        // 向当前页面发送ajax请求，并携带需要产出数据的主键值,传递主键值第一种方式
                        {#url:'/delete/user/' + currentBtn.attr('delete_id'),#}
                        // 传递主键值第二种方式，放在请求体中
                        url: '/delete/user',
                        type: 'post',
                        data: {'delete_id': currentBtn.attr('delete_id')},
                        success: function (args) {
                            //判断响应状态码做不同的处理。
                            if (args.code === 1000) {
                                swal("已删除!", args.msg, "success");
                                // 2.利用DOM操作，动态刷新
                                // 当前端点击删除，后端找到标签所在行，通过DOM操作删除此行，
                                // delete_id 上一个标签是td,再上一个标签为tr,需要删的是当前标签delete_id所在的这一行。

                                // currentBtn指代的是当前被操作对象，parent()拿到父标签，两个parent()拿到父标签的父标签
                                currentBtn.parent().parent().remove()  //实时刷新
                            } else {
                                swal("出现问题", "..", "info");
                            }
                        }
                    })

                } else {
                    swal("您已取消", "...........", "error");
                }
            });
    })
</script>
```

```python
# views.py

def delete_user(request):
    """
    前后端在使用ajax进行交互的时候，后端通常给ajax回调函数返回一个字典格式的数据
    字典返回到前端就是一个自定义对象，前端可以通过.的方式拿到想要的数据
    :param request:
    :return:
    """
    if request.is_ajax():
        if request.method == 'POST':
            # code:1000 为响应状态码
            back_dic = {'code': 1000, 'msg': ''}
            # 取到前端返回用户想要删除数据的主键值
            delete_id = request.POST.get('delete_id')
            models.User.objects.filter(pk=delete_id).delete()
            back_dic['msg'] = '数据已删除'
            # 需要告诉前端操作的结果
            return JsonResponse(back_dic)

```

效果图：

![image-20221104224139637](E:/MarkDown/markdown/imgs/image-20221104224139637.png)

## Django自带的序列化组件(为drf做铺垫)

(drf：django restframework)

```python
#在前端获取到、后端用户表里所有的数据，并且是列表套字典的格式
```

```python
# views.py

from django.http import JsonResponse

def ab_ser(request):
    user_queryset = models.User.objects.all()
    user_list = []
    for user_obj in user_queryset:
        tmp = {
            'pk': user_obj.pk,
            'username': user_obj.username,
            'age': user_obj.age,
            'gender': user_obj.get_gender_display()
        }
        user_list.append(tmp)
    return JsonResponse(user_list, safe=False)
```

```html
<body>
{% for user_obj in user_queryset %}
    <p>{{ user_obj }}</p>
{% endfor %}

</body>
```

**前端显示结果：**

![image-20221103175900052](E:/MarkDown/markdown/imgs/image-20221103175900052.png)

```python
"""
[
  {"pk": 1,"username": "zhao","age": 19,"gender": "male"},
  {"pk": 2,"username": "lisi","age": 20,"gender": "female"},
  {"pk": 3,"username": "wangwu","age": 18,"gender": "others"},
  {"pk": 4,"username": "tony","age": 22,"gender": 4}
]

"""

前后端分离项目
	作为后端开发，只需要写代码将数据返回
    能够序列化返回给前端即可
    	再写一个接口文档，告诉前端每个字段代表的意思即可
```

### 借助于内置序列化模块`serializers`

```python
from django.core import serializers


def ab_ser(request):
    user_queryset = models.User.objects.all()
    # user_list = []
    # for user_obj in user_queryset:
    #     tmp = {
    #         'pk': user_obj.pk,
    #         'username': user_obj.username,
    #         'age': user_obj.age,
    #         'gender': user_obj.get_gender_display()
    #     }
    #     user_list.append(tmp)
    # return JsonResponse(user_list, safe=False)

    # 序列化
    res = serializers.serialize('json', user_queryset)  # 自动将数据变成json格式的数据，并且内部非常的全面
    return HttpResponse(res)
```

效果：

```python
[
  {
    "model": "app01.user",
    "pk": 1,
    "fields": {"username": "zhao","age": 19,"gender": 1}
  },
  {
    "model": "app01.user",
    "pk": 2,
    "fields": {"username": "lisi","age": 20,"gender": 2}
  },
  {
    "model": "app01.user",
    "pk": 3,
    "fields": {"username": "wangwu","age": 18,"gender": 3}
  },
  {
    "model": "app01.user",
    "pk": 4,
    "fields": {"username": "tony","age": 22,"gender": 4}
  }
]

#后端开发写接口就是利用序列化组件渲染数据，然后写一个接口文档，
```

![image-20221104205336198](E:/MarkDown/markdown/imgs/image-20221104205336198.png)

## 批量插入    bulk_create()

```python
# urls.py

from django.contrib import admin
from django.urls import path
from app01 import views

urlpatterns = [
    path('admin/', admin.site.urls),

    #批量插入
    path('ab_pl',views.ab_pl)
]

```

```python
#views.py

def ab_pl(request):
    # 先给Book表插入一千条数据
    for i in range(1000):
        models.Book.objects.create(title='第%s本书' % i)

    # 再将所有的数据查询并展示到前端页面
    book_queryset = models.Book.objects.all()
    return render(request,'ab_pl.html',locals())
```

```python
# models.py

class Book(models.Model):
    title = models.CharField(max_length=32)

```

```html
{% for book_obj in book_queryset %}
    <p>{{ book_obj.title }}</p>
{% endfor %}
```

上述代码插入数据，需要一定时间，效率低下

### bulk_create()

```python
def ab_pl(request):
    
    """批量插入"""
    book_list = []
    for i in range(1000):
        book_obj = models.Book(title='第%s本书' % i)
        book_list.append(book_obj)
        
    models.Book.objects.bulk_create(book_list)  
    return render(request, 'ab_pl.html', locals())


"""
    当想要批量插入数据时候，使用orm提供的bulk_create()能够大大的减少操作时间

"""
```

## 自定义分页器

### 1、分页推导

* 1. queryset对象支持切片操作

* 2. 确定用户访问的页码  url?page=1

  ```python
  current_page=request.GET.get('page',1)
  ```

* 3. 前端获取到 的都是字符串数据，需要类型转换

  ```python
      current_page = request.GET.get('page', 1)  
      try:
          current_page = int(current_page)
      except Exception:
          current_page = 1
  ```

* 4. 规划每页展示多少条数据

  ```python
  per_page_num=10
  ```

* 5. 切片的起始位置和终止位置

  ```python
  start_page =(current_page - 1 ) * per_page_num
  end_page=current_page * per_page_num
  ```

* 6. 当前数据的总条数

  ```python
  book_queryset.count()
  ```

* 7. 确定多少页码才能展示完所有的数据

  ```python
  * 利用python内置函数`divmod()`
  * page_couny,more =divmod(all_count,per_page_num)
  * if more:
    * page_count += 1
  ```

* 8. 前端没有`range`方法

  ```python
  # 前端代码不一定非要在前端书写，也可以在后端生成，传递给前端
  
  
  for i in range(current_page - 5, current_page + 6):
    if xxx == i:
     	page_html += '<li class="active"><a href="?page=%s">%s</a></li>' % (i, i)
    else:
      page_html += '<li><a href="?page=%s">%s</a></li>' % (i, i)
  ```

 * 9. 针对展示的页码需要规划好需要展示多少个页码

   ```python
   # 在制作页面的个数的时候，一般都是奇数个，    符合中国人对称美的标准
   
   当前页减5
   当前页加6
   
   current_page - 5, current_page + 6
   
   可以给标签加选中的样式，高亮显示
   
   ```

* 10. 针对页码小于6的情况，需要做处理，不能再减，否则页码出现负数

  ```python
  if current_page < 6:
          current_page = 6
  ```

**自定义分页器推导代码如下：**

```python
def ab_pl(request):
    """分页"""

    # 想访问那一页
    current_page = request.GET.get('page', 1)  # 如果获取不到当前页码就展示第一页
   
	# 数据类型转换
    try:
        current_page = int(current_page)
    except Exception:
        current_page = 1

    # 每页展示多少条
    per_page_num = 10
    
    # 起始位置
    start_page = (current_page - 1) * per_page_num
    
    # 终止位置
    end_page = current_page * per_page_num

    book_list = models.Book.objects.all()
    # 计算出需要多少页
    all_count = book_list.count()
    page_count, more = divmod(all_count, per_page_num)
    if more:
        page_count += 1
    page_html = ''
    xxx=current_page
    if current_page < 6:
        current_page = 6

    for i in range(current_page - 5, current_page + 6):
        if xxx == i:
            page_html += '<li class="active"><a href="?page=%s">%s</a></li>' % (i, i)
        else:
            page_html += '<li><a href="?page=%s">%s</a></li>' % (i, i)
    book_queryset = book_list[start_page:end_page]
    return render(request, 'ab_pl.html', locals())


"""
per_page_num = 10
current_page             start_page             end_page
    1                       0                       10
    2                       10                      20  
    3                       20                      30
    4                       30                      40



per_page_num = 5
current_page             start_page             end_page
    1                       0                       5
    2                       5                       10
    3                       10                      15
    4                       15                      20
    


start_page =(current_page - 1 ) * per_page_num
end_page=current_page * per_page_num
"""
```

```html
<body>
{% for book_obj in book_queryset %}
    <p>{{ book_obj.title }}</p>

{% endfor %}

<nav aria-label="Page navigation">
    <ul class="pagination">
        <li>
            <a href="#" aria-label="Previous">
                <span aria-hidden="true">&laquo;</span>
            </a>
        </li>
        {{ page_html|safe }}
        
        {#    <li><a href="?page=1">1</a></li>#}
        {#    <li><a href="?page=2">2</a></li>#}
        {#    <li><a href="?page=3">3</a></li>#}
        {#    <li><a href="#">4</a></li>#}
        {#    <li><a href="#">5</a></li>#}
        
        <li>
            <a href="#" aria-label="Next">
                <span aria-hidden="true">&raquo;</span>
            </a>
        </li>
    </ul>
</nav>
</body>
```

效果图:



![image-20221104223106449](E:/MarkDown/markdown/imgs/image-20221104223106449.png)

### 2、分页器代码封装

```python
"""

当需要使用非django内置的第三方功能或者组件代码的时候
一般会在项目根目录下创建一个名为 utils文件夹，在该文件夹内对模块进行功能性划分
		utils也可以在应用下创建
"""
```

```python
utils文件夹下mypage.py文件

class Pagination(object):
    def __init__(self, current_page, all_count, per_page_num=2, pager_count=11):
        """
        封装分页相关数据
        :param current_page: 当前页
        :param all_count:    数据库中的数据总条数
        :param per_page_num: 每页显示的数据条数
        :param pager_count:  最多显示的页码个数
        """
        try:
            current_page = int(current_page)
        except Exception as e:
            current_page = 1
 
        if current_page < 1:
            current_page = 1
 
        self.current_page = current_page
 
        self.all_count = all_count
        self.per_page_num = per_page_num
 
        # 总页码
        all_pager, tmp = divmod(all_count, per_page_num)
        if tmp:
            all_pager += 1
        self.all_pager = all_pager
 
        self.pager_count = pager_count
        self.pager_count_half = int((pager_count - 1) / 2)
 
    @property
    def start(self):
        return (self.current_page - 1) * self.per_page_num
 
    @property
    def end(self):
        return self.current_page * self.per_page_num
 
    def page_html(self):
        # 如果总页码 < 11个：
        if self.all_pager <= self.pager_count:
            pager_start = 1
            pager_end = self.all_pager + 1
        # 总页码  > 11
        else:
            # 当前页如果<=页面上最多显示11/2个页码
            if self.current_page <= self.pager_count_half:
                pager_start = 1
                pager_end = self.pager_count + 1
 
            # 当前页大于5
            else:
                # 页码翻到最后
                if (self.current_page + self.pager_count_half) > self.all_pager:
                    pager_end = self.all_pager + 1
                    pager_start = self.all_pager - self.pager_count + 1
                else:
                    pager_start = self.current_page - self.pager_count_half
                    pager_end = self.current_page + self.pager_count_half + 1
 
        page_html_list = []
        # 添加前面的nav和ul标签
        page_html_list.append('''
                    <nav aria-label='Page navigation>'
                    <ul class='pagination'>
                ''')
        first_page = '<li><a href="?page=%s">首页</a></li>' % (1)
        page_html_list.append(first_page)
 
        if self.current_page <= 1:
            prev_page = '<li class="disabled"><a href="#">上一页</a></li>'
        else:
            prev_page = '<li><a href="?page=%s">上一页</a></li>' % (self.current_page - 1,)
 
        page_html_list.append(prev_page)
 
        for i in range(pager_start, pager_end):
            if i == self.current_page:
                temp = '<li class="active"><a href="?page=%s">%s</a></li>' % (i, i,)
            else:
                temp = '<li><a href="?page=%s">%s</a></li>' % (i, i,)
            page_html_list.append(temp)
 
        if self.current_page >= self.all_pager:
            next_page = '<li class="disabled"><a href="#">下一页</a></li>'
        else:
            next_page = '<li><a href="?page=%s">下一页</a></li>' % (self.current_page + 1,)
        page_html_list.append(next_page)
 
        last_page = '<li><a href="?page=%s">尾页</a></li>' % (self.all_pager,)
        page_html_list.append(last_page)
        # 尾部添加标签
        page_html_list.append('''
                                           </nav>
                                           </ul>
                                       ''')
        return ''.join(page_html_list)
```

**后端**

```python
# views.py

def page(request):
    book_queryset = models.Book.objects.all()
    current_page = request.GET.get('page', 1)
    all_count = book_queryset.count()
    # 1. 实例化，传值生成对象
    page_obj = Pagination(current_page=current_page, all_count=all_count)
    # 2. 直接对总数居进行切片操作
    page_queryset = book_queryset[page_obj.start:page_obj.end]
    # 3. 将page_queryset page_obj传递到页面，
    return render(request, 'page.html', locals())
```

**前端**

自定义分页器是基于bootstrap样式来的，需要提前导入js、css文件

```html
<body>
{% for book_obj in page_queryset %}
    <p>{{ book_obj.title }}</p>

{% endfor %}

{# 利用自定义分页器直接显示分页器样式 #}
{{ page_obj.page_html|safe }}
</body>
```

效果图:



![image-20221104223029555](E:/MarkDown/markdown/imgs/image-20221104223029555.png)

## 校验性组件：froms组件

### 1、前戏

```python
"""
写一个注册功能，获取用户名和密码，利用form表单提交数据
在后端判断用户名和密码是否符合一定条件
	用户名中不能含有金瓶梅
	密码不能少于三位

将提示信息展示到前端页面
"""

# 前端
<form action="" method="post">
    <p>username:
        <input type="text" name="username">
        <span style="color: red">{{ back_dic.username }}</span>
    </p>

    <p>password:
        <input type="password" name="password">
        <span style="color: red">{{ back_dic.password }}</span>
    </p>
    <input type="submit" name="" id="" class="btn btn-info">

</form>

# 后端
def ab_form(request):
    back_dic = {'username': '', 'password': ''}
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')
        if '金瓶梅' in username:
            back_dic['username'] = '用户名不符合社会主义核心价值观'
        if len(password) <= 3:
            back_dic['password'] = '密码太短不安全！！'
            
    """
    无论是get请求还是post请求，只不过是get请求的时候，字典值都是空的，而post请求之后，字典有可能有值
    """
    
    return render(request, 'ab_form.html', locals())



"""
上述代码的三个环节
1.手动书写前端获取用户数据的html代码		渲染html代码
2.后端对用户数据进行校验				   校验数据
3.不符合要求的数据进行前端提示			 展示提示信息



forms组件
	能够完成的事情
		1.渲染html代码
		2.校验数据
		3.展示提示信息
		
为什么数据校验非要去后端，不能在前端利用js直接完成呢？
	数据校验前端可有可无
	但是后端必须要有！！！
	
	因为前端校验若不惊风，可以直接修改或者利用爬虫程序绕过前端页面直接朝后端提交数据
	
	
"""
```

### 2、基本使用

```python
# views.py

from django import forms

class MyForm(forms.Form):
    # username最小3位，最大8位，字符串类型
    username = forms.CharField(min_length=3, max_length=8)
    # password最小3位，最大8位，字符串类型
    password = forms.CharField(min_length=3, max_length=8)
    # email字段必须符合邮箱格式 xxx@xx.com
    email = forms.EmailField()

```

### 3、校验数据

> 测试环境的准备除了可以拷贝代码在test.py文件中书写之外，还可以使用pycharm中已经准备好了一个测试环境

```python
from app01 import views
# 1. 将带校验的数据组织成字典的形式传入即可
form_obj=views.MyForm({'username':'zhao','password':'123','email':'123'})
# 2. 判断数据是否合法， 该方法只有在所有数据都合法的情况下才会返回True
form_obj.is_valid()
Out[6]: False
# 3. 查看所有校验通过的数据（符合校验规则的数据）
form_obj.cleaned_data
Out[7]: {'username': 'zhao', 'password': '123'}
# 4. 查看所有不符合校验规则以及不符合原因
form_obj.errors
Out[8]: {'email': ['Enter a valid email address.']}
```

![image-20221106210111924](E:/MarkDown/markdown/imgs/image-20221106210111924.png)

```python
# 5. 校验数据只校验类中出现的字段，多传不影响，多传的字段直接忽略
form_obj=views.MyForm({'username':'zhao','password':'123','email':'123@qq.com','hobby':'JayChou'})
form_obj.is_valid()
Out[13]: True

form_obj.cleaned_data
Out[15]: {'username': 'zhao', 'password': '123', 'email': '123@qq.com'}
form_obj.errors
Out[16]: {}
```

```python
# . 校验数据 默认情况下，类里的所有字段都必须给值，

form_obj=views.MyForm({'username':'zhao','password':'123'})
form_obj.is_valid()
Out[18]: False
form_obj.cleaned_data
Out[19]: {'username': 'zhao', 'password': '123'}
form_obj.errors
Out[20]: {'email': ['This field is required.']}
```

**校验数据的时候，==默认情况下==数据可以多传，但是绝不能少传，**

### 4、渲染标签

**froms组件只会自动渲染用户输入的标签(input, select ,radio,checkbox,)，不会渲染提交按钮**

#### 4.1、先在后端生成一个**空对象**

```python
# views.py

from django import forms

class MyForm(forms.Form):
    # username最小3位，最大8位，字符串类型
    username = forms.CharField(min_length=3, max_length=8)
    # password最小3位，最大8位，字符串类型
    password = forms.CharField(min_length=3, max_length=8)
    # email字段必须符合邮箱格式 xxx@xx.com
    email = forms.EmailField()



def index(request):
    # 1.先产生一个空对象
    form_obj = MyForm()
    # 2.直接将该空对象传递给html页面
    return render(request, 'index.html', locals())



# urls.py
urlpatterns = [
    path('index/', views.index, name='index')
]

#前端利用空对象做操作
<form action="" method="post">
    <p>第一种渲染方式:代码书写极少，封装成度太高不便于后续拓展，一般只在本地测试使用</p>
    {{ form_obj.as_p }}
    {{ form_obj.as_ul }}
    {{ form_obj.as_table }}

    <p>第二种渲染方式：可扩展性非常强，但是书写代码太多</p>
    <p>{{ form_obj.username.label }}:{{ form_obj.username }}</p>
    <p>{{ form_obj.password.label }}:{{ form_obj.password }}</p>
    <p>{{ form_obj.email.label }}:{{ form_obj.email }}</p>

    <p>第三种渲染方式：代码精简，扩展性也高</p>
    {% for form in form_obj %}
        <p>{{ form.label }}:{{ form }}</p>

    {% endfor %}

</form>


# label属性默认展示的是类中定义的字段首字母大写的形式，也可以自己修改，直接给类中字段对象加label属性即可
```

#### 4.2、第一种渲染方式

```html
<body>
<form action="" method="post">
    <p>第一种渲染方式:代码书写极少，封装成度太高不便于后续拓展，一般只在本地测试使用</p>
    {{ form_obj.as_p }}
</form>
</body>
```

**自动生成字段名和Input框**

![image-20221106211541541](E:/MarkDown/markdown/imgs/image-20221106211541541.png)

```html
<form action="" method="post">
    <p>第一种渲染方式:代码书写极少，封装成度太高不便于后续拓展，一般只在本地测试使用</p>
    {{ form_obj.as_ul }}
</form>
```

![image-20221106212046568](E:/MarkDown/markdown/imgs/image-20221106212046568.png)

```html
<form action="" method="post">
    <p>第一种渲染方式:代码书写极少，封装成度太高不便于后续拓展，一般只在本地测试使用</p>
    {{ form_obj.as_table }}
</form>
```

![image-20221106212158865](E:/MarkDown/markdown/imgs/image-20221106212158865.png)

#### 4.3、第二种渲染方式

```python
<p>{{ form_obj.username }}</p>
# form_obj是类对象，而username是类里的属性，
```

![image-20221106213211520](E:/MarkDown/markdown/imgs/image-20221106213211520.png)

```html
 <p>{{ form_obj.username.label }}{{ form_obj.username }}</p>
```

![image-20221106212855862](E:/MarkDown/markdown/imgs/image-20221106212855862.png)

```python
#可以在类里面修改代码，展示input框前面的注释信息，
username = forms.CharField(min_length=3, max_length=8,label='用户名')
```

![image-20221106212959362](E:/MarkDown/markdown/imgs/image-20221106212959362.png)

```html
<form action="" method="post">
    <p>第二种渲染方式：可扩展性非常强，但是书写代码太多</p>
    <p>{{ form_obj.username.label }}:{{ form_obj.username }}</p>
    <p>{{ form_obj.password.label }}:{{ form_obj.password }}</p>
    <p>{{ form_obj.email.label }}:{{ form_obj.email }}</p>
</form>
```

![image-20221106213434732](E:/MarkDown/markdown/imgs/image-20221106213434732.png)

#### 4.4、第三种渲染方式

```html
<form action="" method="post">
    <p>第三种渲染方式：</p>
    {% for form in form_obj %}
        <p>{{ form }}</p>
    {% endfor %}
</form>


```

![image-20221106213615001](E:/MarkDown/markdown/imgs/image-20221106213615001.png)

上图所示,`{{form}}`等价于第二种渲染方式的`{{form_obj.username}}`,那么直接`.label`就可以得到`input`框前面的**注释**,

```html
<form action="" method="post">

    <p>第三种渲染方式：代码精简，扩展性也高</p>
    {% for form in form_obj %}
        <p>{{ form.label }}:{{ form }}</p>

    {% endfor %}

</form>
```

![image-20221106214118737](E:/MarkDown/markdown/imgs/image-20221106214118737.png)

### 5、展示错误信息

前端

```python
<form action="" method="post">
    {% for form in form_obj %}
        <p>{{ form.label }}:{{ form }}
            <span style="color:red">{{ form.errors }}</span>
        </p>
    {% endfor %}
    <input type="submit" class="btn btn-info">
</form>
```

后端

```python
class MyForm(forms.Form):
    # username最小3位，最大8位，字符串类型
    username = forms.CharField(min_length=3, max_length=8, label='用户名')
    # password最小3位，最大8位，字符串类型
    password = forms.CharField(min_length=3, max_length=8, label='密码')
    # email字段必须符合邮箱格式 xxx@xx.com
    email = forms.EmailField(label='邮箱')

    
def index(request):
    # 1.先产生一个空对象
    form_obj = MyForm()
    if request.method == 'POST':
        # 获取用户数据并校验
        """
        username=request.POST.get('username')
        如果数据太多，
        获取数据太繁琐
        校验数据需要构造字典的格式传入才行
        ps:但是request.POST可以看成就是一个字典
        """
        # 3校验数据
        form_obj = MyForm(request.POST)
        # 4.判断数据是否合法
        if form_obj.is_valid():
            # 5如果合法，操作数据库存储数据
            return HttpResponse('OK')
        # 6不合法，有错误
        else:
            # 将错误信息展示到前端
            pass

    # 2.直接将该空对象传递给html页面
    return render(request, 'index.html', locals())
```

---

* **浏览器会自动检验数据，但是前端检验若不惊风，如下图:**

---

![image-20221107190747441](E:/MarkDown/markdown/imgs/image-20221107190747441.png)

---

![image-20221107191036994](E:/MarkDown/markdown/imgs/image-20221107191036994.png)

---

![image-20221107191023281](E:/MarkDown/markdown/imgs/image-20221107191023281.png)

* **如何让浏览器不自动校验**

```html
<form action="" method="post" novalidate>
```

![image-20221107191659967](E:/MarkDown/markdown/imgs/image-20221107191659967.png)

```python
<form action="" method="post" novalidate>
    {% for form in form_obj %}
        <p>{{ form.label }}:{{ form }}
            <span style="color:red">{{ form.errors.0 }}</span>
        </p>
    {% endfor %}
    <input type="submit" class="btn btn-info">

</form>
</body>
```

![image-20221107191923509](E:/MarkDown/markdown/imgs/image-20221107191923509.png)

前端渲染效果展示：就不会再自动生成`ul套li`的格式，而是生成一个`普通文本`

![image-20221107191810415](E:/MarkDown/markdown/imgs/image-20221107191810415.png)

但是又因为再后端做了限制，最大只能8位，所以会出现当输入到8位的时候就出现不能再输入的效果，可以通过打开检查，修改前端代码来达到目的。再次显示出前端校验的若不惊风！！

![image-20221107192646078](E:/MarkDown/markdown/imgs/image-20221107192646078.png)

```python
"""
1.必备条件：get请求和post请求传递给html页面对象变量名必须一样
2.form组件当你的数据不合法的时候，会保存你上次的数据，让你基于之前的结果进行修改，更加人性化
"""
```

* **自定义展示信息**

```python
#针对错误的提示信息可以自己定制

class MyForm(forms.Form):
    # username最小3位，最大8位，字符串类型
    username = forms.CharField(min_length=3, max_length=8, label='用户名', error_messages={
        'min_length': '用户名最少3位',
        'max_length': '用户名最大8位',
        'required': '用户名不能位空',
    })
    # password最小3位，最大8位，字符串类型
    password = forms.CharField(min_length=3, max_length=8, label='密码', error_messages={
        'min_length': '密码最少3位',
        'max_length': '密码最长8位',
        'required': '密码不能为空',
    })
    # email字段必须符合邮箱格式 xxx@xx.com
    email = forms.EmailField(label='邮箱', error_messages={
        'invalid': '邮箱格式不正确',
        'required': '邮箱不能为空',
    })
```

![image-20221107194237578](E:/MarkDown/markdown/imgs/image-20221107194237578.png)

### 6、钩子函数(HOOK)

```python
"""
>在特定的节点自动触发，完成相应操作

钩子函数在form组件中就类似于第二道关卡，能够让我们自定义校验规则

form组件中有两类钩子：
	1. 局部钩子
		当你需要给单个字段增加校验规则的时候可以使用
	2. 全局钩子
		当你需要给多个字段增加校验规则的时候可以使用
"""
#实际案例

# 1. 校验用户名中不能有666   只是校验username字段，用局部钩子

# 2. 校验密码和确认密码是否一致  passwrod和confirm两个字段，用全局钩子

```

```python
from django import forms


class MyForm(forms.Form):
    # username最小3位，最大8位，字符串类型
    username = forms.CharField(min_length=3, max_length=8, label='用户名', error_messages={
        'min_length': '用户名最少3位',
        'max_length': '用户名最大8位',
        'required': '用户名不能位空',
    })
    # password最小3位，最大8位，字符串类型
    password = forms.CharField(min_length=3, max_length=8, label='密码', error_messages={
        'min_length': '密码最少3位',
        'max_length': '密码最长8位',
        'required': '密码不能为空',
    })
    confirm_password = forms.CharField(min_length=3, max_length=8, label='密码确认', error_messages={
        'min_length': '确认密码最少3位',
        'max_length': '确认密码最长8位',
        'required': '确认密码不能为空',
    })
    # email字段必须符合邮箱格式 xxx@xx.com
    email = forms.EmailField(label='邮箱', error_messages={
        'invalid': '邮箱格式不正确',
        'required': '邮箱不能为空',
    })

    # 钩子函数在类里面书写方法即可
    
    # 1.局部钩子
    def clean_username(self):
        # 获取到用户名
        username = self.cleaned_data.get('username')
        if '666' in username:
            # 提示前端展示错误信息
            #报错方式一:
            self.add_error('username', '用户名中不能含有666')
             # 报错方式二
            #from django.core.exceptions import  ValidationError    
            #raise ValidationError('用户名中不能含有666')
        # 将钩子函数钩取出来的数据再放回去
        return username

    # 2.全局钩子
    def clean(self):
        password = self.cleaned_data.get('password')
        confirm_password = self.cleaned_data.get('confirm_password')
        if not confirm_password == password:
            self.add_error('confirm_password', '两次密码不一致')
        # 将钩子函数钩出来的数据全部返回
        return self.cleaned_data


def index(request):
    # 1.先产生一个空对象
    form_obj = MyForm()
    if request.method == 'POST':
        # 获取用户数据并校验
        """
        username=request.POST.get('username')
        如果数据太多，
        获取数据太繁琐
        校验数据需要构造字典的格式传入才行
        ps:但是request.POST可以看成就是一个字典
        """
        # 3校验数据
        form_obj = MyForm(request.POST)
        # 4.判断数据是否合法
        if form_obj.is_valid():
            # 5如果合法，操作数据库存储数据
            return HttpResponse('OK')
        # 6不合法，有错误
        else:
            # 将错误信息展示到前端
            pass

    # 2.直接将该空对象传递给html页面
    return render(request, 'index.html', locals())
```

![image-20221107200041886](E:/MarkDown/markdown/imgs/image-20221107200041886.png)

### 7、forms组件其他参数

创建Form类时，主要涉及到 【字段】 和 【插件】，字段用于对用户请求数据的验证，插件用于自动生成HTML;

```python
label 给字段起名字
error_messages  自定义报错信息
initial 默认值
required=False  控制字段是否必填，默认是True


"""
1.字段没有样式
2. 针对不同类型的input如何修改
	text
	password
	radio
	date
	checkbox
	...
"""

widget=forms.widgets.TextInput(attrs={'class': 'form-control c1 c2', 'username': 'zhao'})
#多个属性，直接空格隔开即可



```

#### **initial**

初始值，input框里面的初始值。

```python
class MyForm(forms.Form):
    # username最小3位，最大8位，字符串类型
    username = forms.CharField(
        min_length=3, 
        max_length=8, 
        label='用户名', 
        initial='zhao', #设置默认值
    )
    pwd = forms.CharField(min_length=6, label="密码")
```

#### **error_messages**

重写错误信息。

```python
class MyForm(forms.Form):
    # username最小3位，最大8位，字符串类型
    username = forms.CharField(
        min_length=3, 
        max_length=8, 
        label='用户名', 
        initial='zhao',
        error_messages={
            'min_length': '用户名最少3位',
            'max_length': '用户名最大8位',
            'required': '用户名不能位空',
         }
      )
      pwd = forms.CharField(min_length=6, label="密码")
```

#### widget

PasswordInput    TextInput     EmailInput

```python
class MyForm(forms.Form):
    # username最小3位，最大8位，字符串类型
    username = forms.CharField(
        min_length=3, 
        max_length=8, 
        label='用户名', 
        initial='zhao',
        error_messages={
            'min_length': '用户名最少3位',
            'max_length': '用户名最大8位',
            'required': '用户名不能位空',
        },
        widget=forms.widgets.TextInput(attrs={'class': 'form-control c1 c2', 'username': 'zhao'})
                               )
    
    # password最小3位，最大8位，字符串类型
    password = forms.CharField(
        min_length=3, 
        max_length=8, 
        label='密码', 
        error_messages={
            'min_length': '密码最少3位',
            'max_length': '密码最长8位',
            'required': '密码不能为空',
    	},
        widget=forms.widgets.PasswordInput(attrs={'class': 'form-control'})
                               )
    
    email = forms.EmailField(
        label='邮箱', 
        error_messages={
            'invalid': '邮箱格式不正确',
            'required': '邮箱不能为空',
        },
        widget=forms.widgets.EmailInput(attrs={'class': 'form-control'})
    )

```

#### RegexValidator验证器

```python
from django.forms import Form
from django.forms import widgets
from django.forms import fields
from django.core.validators import RegexValidator
 
class MyForm(Form):
    user = fields.CharField(
        validators=[RegexValidator(r'^[0-9]+$', '请输入数字'), RegexValidator(r'^159[0-9]+$', '数字必须以159开头')],
    )
```

#### required

默认是True，必填项

```python
class MyForm(forms.Form):
    # username最小3位，最大8位，字符串类型
    username = forms.CharField(
        min_length=3, 
        max_length=8, 
        label='用户名', 
        required=False, 
        initial='zhao',
                               )
```

#### radioSelect

单radio值为字符串

```python
class LoginForm(forms.Form):
    username = forms.CharField(
        min_length=8,
        label="用户名",
        initial="张三",
        error_messages={
            "required": "不能为空",
            "invalid": "格式错误",
            "min_length": "用户名最短8位"
        }
    )
    pwd = forms.CharField(min_length=6, label="密码")
    #选择
    gender = forms.ChoiceField(
        choices=((1, "男"), (2, "女"), (3, "保密")),
        label="性别",
        initial=3,#默认是保密
        widget=forms.widgets.RadioSelect()
    )
```

#### 单选select

```python
class LoginForm(forms.Form):
    ...
    hobby = forms.ChoiceField(
        choices=((1, "篮球"), (2, "足球"), (3, "双色球"), ),
        label="爱好",
        initial=3,
        widget=forms.widgets.Select()
    )
```

#### 多选select

```python
class LoginForm(forms.Form):
    ...
    hobby = forms.MultipleChoiceField(
        choices=((1, "篮球"), (2, "足球"), (3, "双色球"), ),
        label="爱好",
        initial=[1, 3],
        widget=forms.widgets.SelectMultiple()
    )
```

#### 单选checkbox

```python
class LoginForm(forms.Form):
    ...
    keep = forms.ChoiceField(
        label="是否记住密码",
        initial="checked",
        widget=forms.widgets.CheckboxInput()
    )
```

#### 多选checkbox

```python
class LoginForm(forms.Form):
    ...
    hobby = forms.MultipleChoiceField(
        choices=((1, "篮球"), (2, "足球"), (3, "双色球"),),
        label="爱好",
        initial=[1, 3],
        widget=forms.widgets.CheckboxSelectMultiple()
    )
```

### 8、forms组件源码

```python
"""
切入点：
	form_obj.is_valid()
"""

 def is_valid(self):
        """Return True if the form has no errors, or False otherwise."""
        return self.is_bound and not self.errors
    #如果is_valid()要返回True的话，那么self.is_bound要位True，self.errors要为False
    
self.is_bound = data is not None or files is not None #只要传值了，肯定为True


@property
def errors(self):
    """Return an ErrorDict for the data provided for the form."""
    if self._errors is None:
        self.full_clean()
        return self._errors
    
    #forms组件所有的功能基本都出自该方法
    def full_clean(self):
        
        self._clean_fields()  #校验字段+局部钩子
        self._clean_form()	  #全局钩子
        self._post_clean()
```

## ==Cookie与Session==

HTTP被设计为”==⽆态==”，也就是俗称“脸盲”。 这⼀次请求和下⼀次请求 之间没有任何状态保持，我们⽆法根据请求的任何⽅⾯(IP地址，⽤户代理等)来识别来自同⼀ 个⼈的连续请求。

实现状态保持的⽅式：在客户端或服务器端存储与会话有关的数据 （客户端与服务器端的⼀次通信，就是⼀次会话）

* cookie
* session

不同的请求者之间不会共享这些数据，cookie和session与请求者⼀⼀对应

```python
"""
发展史:
	1.网站没有保存用户功能的需求，所有用户访问返回的结果都是一样的	eg:新闻，博客，文章
	
	2.出现了一些需要保存用户信息的网站
		eg:淘宝，支付宝，京东.....
		
	以登录功能为例子，如果不保存用户登陆状态，也就意味着用户每次访问网站都需要重复的输入用户名和密码（这样的网站你还想用吗？）
	当用户第一次登录成功之后，将用户的密码返回给用户浏览器，让用户浏览器保存在本地，之后访问网站的时候浏览器会自动将保存在浏览器上的用户名和密码发送给服务端，服务端获取之后，自动校验。
	早期这种方式具有很大的安全隐患，
	
	
	优化:
		当用户登录成功之后，服务端产生一个随机字符串（在服务端保存数据,用Kv键值对的形式），交由客户端浏览器保存，
		
		随机字符串1:用户1的相关信息
		随机字符串2:用户2的相关信息
		随机字符串3:用户3的相关信息
		
		之后访问服务端的时候，都带着该随机字符串，服务端去数据库中比对是否有对应的随机字符串，从而获取到对应的用户信息
		
但是如果拿到了随机字符串，那么就可以冒充当前用户，其实还存在安全隐患


在web领域，没有绝对的安全，也没有绝对的不安全
		
			
"""
cookie 	
	服务端保存在客户端浏览器上的信息都可以称之为cookie
    它的表现形式一般都是k:v键值对(可以有多个)
session
	数据是保存在服务端的，并且它的表现形式一般都是k:v键值对(可以有多个)
token
	session虽然数据保存在服务端，但是禁不住数据量大
    服务端不再保存数据，而是在登录成功之后，将一段信息进行加密处理，(加密算法只有自己知道)，将加密后的结果拼接在信息后面，整体返回给浏览器保存，浏览器下次再访问的时候带着该信息，服务端自动切取前面一段信息，再次使用自己的加密算法跟浏览器尾部的密文进行比对

jwt认证
	
 
"""总结"""
	1.cookie是保存在客户端浏览器上的信息
    2.session就是保存在服务端上的信息
    3.session是基于cookie工作的（大部分的保存用户状态的操作都需要使用到cookie）
```

### 1、Django操作cookie

```python
#虽然cookie是服务端告诉客户端需要保存的内容
#但是客户端可以选择拒绝保存，禁止了之后只要是需要记录用户状态的网站登录功能都无法使用
```

![image-20221108194110940](E:/MarkDown/markdown/imgs/image-20221108194110940.png)

```python
# 视图函数的返回值
return HttpResponse()
return render()
return redirects()

#如果想要操作cookie，就必须利用obj对象，不能再像之前那样写视图函数的返回值了
obj1=HttsResponse()
return obj1
obj2=render()
return obj2
obj3=redirect()
return obj3


"""设置cookie"""
	obj.set_cookie(key,value='')
"""获取cookie"""
	request.COOKIES.get(key)
    request.get_signed_cookie(key, default=RAISE_ERROR, salt='盐', max_age=None)
"""设置超时时间"""
	在设置cookie的时候，可以添加超时时间
    obj.set_cookie('username', 'zs666', max_age=5, expires=3)
    
    max_age
    expires
    	两者都是设置超时时间，都是以秒为单位，
        需要注意的是针对IE浏览器需要使用expires
"""删除cookie"""
obj.delete_cookie('username')

"""加盐"""
obj.set_sifned_cookie(key,value,salt='盐')
```

#### 简单实现用户登录

```python
def login(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')
        if username == 'zhao' and password == '123':
            # 保存用户登录状态
            obj = redirect('home')
            # 让浏览器记录cookie数据
            obj.set_cookie('username', 'zs666')
            """
            浏览器不单单会帮你存，而且每次访问的时候都会带着它
            """
            # 跳转到一个需要用户登录才能看到的页面
            return obj
    return render(request, 'login.html')


def home(request):
    # 获取cookie信息,进行判断
    if request.COOKIES.get('username') == 'zs666':
        return HttpResponse('我是Home页面，只有登录的用户才可进来')
    # 没有登录应该跳转到登录页面
    return redirect('login')
```

#### 加入装饰器

```python
"""
用户如果再没有登录的情况下，想访问一个需要登录的页面，那么先跳转到登录页面，
当用户输入正确的用户名和密码之后，
应该跳转到用户之前想到访问的页面，而不是直接写死
"""


# 校验用户是否登录的装饰器
def login_auth(func):
    def inner(request, *args, **kwargs):
        target_url = request.get_full_path()
        if request.COOKIES.get('username'):
            res = func(request, *args, **kwargs)
            return res
        else:
            return redirect('/login/?next=%s' % target_url)

    return inner


def login(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')
        if username == 'zhao' and password == '123':
            # 获取用户上一次想要访问的url
            target_url = request.GET.get('next')
            if target_url:
                obj = redirect(target_url)
            else:
                # 保存用户登录状态
                obj = redirect('home')
                # 让浏览器记录cookie数据
            obj.set_cookie('username', 'zs666')
            """
            浏览器不单单会帮你存，而且每次访问的时候都会带着它
            """
            # 跳转到一个需要用户登录才能看到的页面
            return obj
    return render(request, 'login.html')


@login_auth
def home(request):
    # 获取cookie信息,进行判断
    # if request.COOKIES.get('username') == 'zs666':
    #     return HttpResponse('我是Home页面，只有登录的用户才可进来')
    # 没有登录应该跳转到登录页面
    # return redirect('login')
    return HttpResponse('我是Home页面，只有登录的用户才可进来')


@login_auth
def index(request):
    # return redirect('login')
    return HttpResponse('我是index页面，只有登录的用户才可进来')


@login_auth
def func(request):
    return HttpResponse('我是func页面，只有登录的用户才可进来')


# 注销，删除cookie
@login_auth
def logout(request):
    obj = redirect('login')
    obj.delete_cookie('username')
    return obj

```

### 2、Django操作session

```python
session数据保存在服务端，给客户端返回的是一个随机字符串
	sessionid:随机字符串	
1.默认情况下操作session的时候需要Django默认的一张django_session表
	数据库迁移命令
    	django会自动的创建多张表，django_session就是其中的一张
"""设置session"""
request.session['key'] = value
"""获取session"""
request.session.get('key')
"""设置过期时间"""
request.session.set_expiry()
	括号内可以放四种类型的参数
    	1.整数   				多少秒
        2.日期对象   			到指定日期就失效
        3. 0 				  一旦当前浏览器窗口关闭立刻失效
        4. 不写				失效时间取决于django内部全局session默认的的失效时间（14天）
"""清除session"""
	request.session.delete() #只删服务端的session,客户端的不删
	request.session.flush() #浏览器和服务端都清空
 


2.session是保存在服务端的，但是session的保存位置可以有多种选择
	1.MySQL
    2.文件
    3.redis
    4.memcache
    ......
	
    
django_session表中的数据条数，是取决于浏览器的
	同一个计算机上(IP地址)，同一个浏览器只会同时有一条数据生效
    （当session过期的时候，可能会出现多条数据对应一个浏览器，但是该现象不会持续很久，内部会自动识别过期的数据清除，也可以手动通过代码清除）
    主要是为了节省服务端数据库资源
    
```

**先执行数据库迁移的两条命令，查看django_session表结构**

如果没有先执行数据库迁移命令，那么会报(`no such table:django_session`)错误

![image-20221108205236315](E:/MarkDown/markdown/imgs/image-20221108205236315.png)

#### 设置session

```python
def set_session(request):
    # 设置session
    request.session['hobby'] = 'JayChou'
    """
    设置session内部发生了那些事？
        1.django内部会自动生成一个随机字符串
        2.django内部自动将随机字符串和后面对应的数据存储到django_session表中（这一步不是直接生效的，）
            2.1先在内存中产生操作数据的缓存
            2.2在响应结果django中间件的时候才真正的操作数据
        3.将产生的随机字符串返回个客户端浏览器保存
    """

    return HttpResponse('稻香')
```

然后在浏览器输入路由，回车后，`django_session`表中会自动生成数据,

`expire_data`字段是过期时间

![image-20221108205735075](E:/MarkDown/markdown/imgs/image-20221108205735075.png)



---

session给客户端返回的是一个随机字符串,**随机字符串**指的就是`django_session表中的session_key`

==django默认的session过期时间是**14**天==，但是也可以认为的修改

![image-20221108211345212](E:/MarkDown/markdown/imgs/image-20221108211345212.png)

#### 获取session

```python
def get_session(request):
    # 获取session
    print(request.session.get('hobby'))
    """
    获取session发生了那些事:
        1.自动从浏览器请求中获取sessionid对应的随机字符串
        2.拿着该随机字符串去django_session表中查找对应的数据，
        3.如果比对上了，则将对应的数据取出并以字典的形式封装到request.session中
            比对不上，则request.session.get()返回None
    """
    return HttpResponse('haha')
```

![image-20221108211941911](E:/MarkDown/markdown/imgs/image-20221108211941911.png)

---



#### 过期时间

```python
request.session.set_expiry()
	括号内可以放四种类型的参数
    	1.整数   				多少秒
        2.日期对象   			到指定日期就失效
        3. 0 				  一旦当前浏览器窗口关闭立刻失效
        4. 不写				失效时间取决于django内部全局
```



```python
def set_session(request):
    # 设置session
    request.session['hobby'] = 'JayChou'
    # 设置超时时间
    request.session.set_expiry(0) # 浏览器关闭就失效
    return HttpResponse('稻香')

def get_session(request):
    # 获取session
    # print(request.session.get('hobby'))
    if request.session.get('hobby'):
        print(request.session.get('hobby'))
        return HttpResponse('haha')
    return HttpResponse('浏览器窗口关闭过了，得不到session数据')
```

#### 清除session

```python
def del_session(request):
    request.session.delete() ##只删服务端的session,客户端的不删
    return HttpResponse("删除")



推荐使用request.session.flush() #浏览器和服务端都清空
```



![image-20221108220200106](E:/MarkDown/markdown/imgs/image-20221108220200106.png)



## CBV添加装饰器

* 装饰器

```python
# 校验用户是否登录的装饰器
def login_auth(func):
    def inner(request, *args, **kwargs):
        target_url = request.get_full_path()
        if request.COOKIES.get('username'):
            res = func(request, *args, **kwargs)
            return res
        else:
            return redirect('/login/?next=%s' % target_url)

    return inner
```

* 添加装饰器 方式一

```python
# views.py
from django.views import View
from django.utils.decorators import method_decorator

class MyLogin(View):
    @method_decorator(login_auth)#方式一
    def get(self, requset):
        return HttpResponse('get')

    def post(self, reqeust):
        return HttpResponse('post')

    
# urls.py
    path('mylogin/',views.MyLogin.as_view())  
```

* 添加装饰器 方式二

```python
# views.py
from django.views import View
from django.utils.decorators import method_decorator

@method_decorator(login_auth, name='get')  # 方式二，可以添加多个，针对不同的方法加不同的装饰器
@method_decorator(login_auth, name='post')
class MyLogin(View):
    def get(self, requset):
        return HttpResponse('get')

    def post(self, reqeust):
        return HttpResponse('post')
    
    
# urls.py
    path('mylogin/',views.MyLogin.as_view())
```

* 添加装饰器 方式三

```python
# views.py
from django.views import View
from django.utils.decorators import method_decorator


class MyLogin(View):
    @method_decorator(login_auth)  # 方式三，会直接作用于当前类里的所有方法
    def dispatch(self, request, *args, **kwargs): # 
        pass

    def get(self, requset):
        return HttpResponse('get')

    def post(self, reqeust):
        return HttpResponse('post')
    
    
# urls.py
    path('mylogin/',views.MyLogin.as_view())   
```

## Django中间件

只要是涉及到全局相关的功能都可以使用中间件方便的完成

* 全局用户身份校验
* 全局用户权限校验
* 全局访问频率校验

```python
"""
django中间件是django的门户
	1.请求来的时候需要经过中间件才能达到真正的django后端
	2.响应走的时候最后也需要经过中间件才能发送出去
"""
```

![image-20221109184718273](E:/MarkDown/markdown/imgs/image-20221109184718273.png)

### 默认中间件源码分析

```python

class SecurityMiddleware(MiddlewareMixin):
        def process_request(self, request):
            path = request.path.lstrip("/")
            if (
                self.redirect
                and not request.is_secure()
                and not any(pattern.search(path) for pattern in self.redirect_exempt)
            ):
                host = self.redirect_host or request.get_host()
                return HttpResponsePermanentRedirect(
                    "https://%s%s" % (host, request.get_full_path())
                )

    def process_response(self, request, response):
        return response


class AuthenticationMiddleware(MiddlewareMixin):
    def process_request(self, request):
        request.user = SimpleLazyObject(lambda: get_user(request))


class CsrfViewMiddleware(MiddlewareMixin):
     def process_request(self, request):
        try:
            csrf_secret = self._get_secret(request)
        except InvalidTokenFormat:
            _add_new_csrf_cookie(request)
        else:
            if csrf_secret is not None:
                request.META["CSRF_COOKIE"] = csrf_secret

    def process_view(self, request, callback, callback_args, callback_kwargs):
        return self._accept(request)
   
    def process_response(self, request, response):
        return response
"""

django支持程序员自定义中间件，并且暴露给程序员五个可以自定义的方法
	1.必须了解
	process_request
	
	process_response
	2.了解即可
	process_view
	
	process_template_response
	
	process_exception
"""    
```

### 自定义中间件

```python
"""
1.在项目名或者应用下创建一个任意名称的文件夹，
2.在该文件夹下创建任意任意名字的py文件
3.在该py文件需要书写类，必须继承MiddlewareMixin
	然后就可以自定义五个方法
	(这五个方法用几个写几个，并不需要全都写)
4.需要将类的路径以字符串的形式注册到配置文件中才能生效
"""

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',  #
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    '自己写的中间件路径',
    '自己写的中间件路径',
    '自己写的中间价路径',
]
```

#### process_request

![image-20221109191456363](E:/MarkDown/markdown/imgs/image-20221109191456363.png)

**启动项目，查看中间件是否生效**

![image-20221109191548177](E:/MarkDown/markdown/imgs/image-20221109191548177.png)



---

添加一个路由，写一个视图函数，查看执行顺序

```python
from django.contrib import admin
from django.urls import path
from App import  views
urlpatterns = [
    path('admin/', admin.site.urls),
    path('index/',views.index),
]
```

```pytohn
# Create your views here.
def index(request):
    print('我是视图函数index')
    return HttpResponse('index')
```

启动项目，浏览器输入路由

![image-20221109192047022](E:/MarkDown/markdown/imgs/image-20221109192047022.png)



---

再写一个中间件，并颠倒注册顺序

![image-20221109192636506](E:/MarkDown/markdown/imgs/image-20221109192636506.png)



---

添加返回值

![image-20221109194306420](E:/MarkDown/markdown/imgs/image-20221109194306420.png)

---

**总结:**

1. 请求来的时候是要经过每一个中间件里的process_request方法，结果的顺序按照配置文件中注册的中间件从上往下的顺序依次执行

2. 如果中间件里面没有定义process_request方法，直接跳过，执行下一个
3. 如果该方法返回了HTTP对象，那么请求将不再继续往后执行，而是直接原路返回（校验失败，不允许访问）
   1. 所以process_request方法就是用来做全局相关的所有限制功能

#### process_response

```python
from django.utils.deprecation import MiddlewareMixin
from django.shortcuts import HttpResponse


class MyMiddleWare(MiddlewareMixin):
    def process_request(self, request):
        print('我是第一个自定义中间件里面的process_request方法')

    def process_response(self, request, response):
        print('我是第一个自定义中间件里面的process_response方法')

        """

        :param request:
        :param response: django后端返回给浏览器的内容
        :return:
        """
        return response


class MyMiddleWare2(MiddlewareMixin):
    def process_request(self, request):
        print('我是第二个自定义中间件里面的process_request方法')

    def process_response(self, request, response):
        print('我是第二个自定义中间件里面的process_response方法')
        return response
```

浏览器输入路由，回车

![image-20221109195429437](E:/MarkDown/markdown/imgs/image-20221109195429437.png)

返回自己的HttpResponse对象

原本后端是要返回`index`的，走到中间件里面，先经过`MyMiddleWare`，返回`index`,但是经过`MyMiddleWare2`的时候来了一个偷天换日，把原本想要返回的响应换成自己的`hello!`，之后所有的中间件拿到的全是自己写的返回响应的内容

![image-20221109195712980](E:/MarkDown/markdown/imgs/image-20221109195712980.png)

**总结：**

1. 响应走的时候需要经过每一个中间件里的process_response方法,该方法有两个额外的参数(request,response)
2. 该方法必须返回一个HttpResponse对象
   1. 默认返回的就是形参response
   2. 也可以自己返回自己的
3. 顺序是按照配置文件中注册了的中间件从下往上依次经过，如果没有定义，直接执行上一个

---

如果在先注册了的中间件中的process_request方法就已经返回了HttpResponse对象，那么响应走的时候经过所有的中间件里面的process_response还是有其他情况？



![image-20221109201241624](E:/MarkDown/markdown/imgs/image-20221109201241624.png)

==观察上图得出以下结论==：

​	首先请求来的时候，会依次经过每一个注册了的中间件里的`process_reques`t方法，一旦`process_request`方法返回了一个`HttpResponse`对象，那么会直接不再往下走，而是直接经过**同级别的`process_response`往外走。**

![image-20221109201546542](E:/MarkDown/markdown/imgs/image-20221109201546542.png)

![image-20221109201702210](E:/MarkDown/markdown/imgs/image-20221109201702210.png)

#### process_view(了解)

```python
def process_view(self, request, view_name, *args, **kwargs):
    print(view_name, args, kwargs)
    print('我是第一个自定义中间件里的process_view')
```

特点：

路由匹配成功之后，执行视图函数之前，会自动执行中间件里面的process_view方法,顺序按照配置文件中注册的中间件从上往下的顺序依次执行

![image-20221109203106289](E:/MarkDown/markdown/imgs/image-20221109203106289.png)

#### process_template_response(了解)

```python
def process_response(self, request, response):
    print('我是第二个自定义中间件里面的process_response方法')
    return response
```

```python
# views.py

def index(request):
    print('我是视图函数index')
    obj = HttpResponse('index')

    def render():
        print('内部的render')
        return HttpResponse('98K')

    obj.render = render
    return obj
```



返回的HttpResponse对象有render属性的时候，才会触发，顺序是

按照配置文件中注册的中间件从下往上依次经过

![image-20221109204530848](E:/MarkDown/markdown/imgs/image-20221109204530848.png)

#### process_execption(了解)

```python
def process_exception(self, request, exception):
    print(exception)
    print('我是第二个自定义中间件里的process_exception')
```

当视图函数中出现异常的情况下，触发，

顺序是按照配置文件中注册的中间件从下往上依次经过

![image-20221109205002531](E:/MarkDown/markdown/imgs/image-20221109205002531.png)

**全部代码:**

```python
# -*- coding: UTF-8 -*- 
# @Date ：2022/11/9 19:03
from django.utils.deprecation import MiddlewareMixin
from django.shortcuts import HttpResponse


class MyMiddleWare(MiddlewareMixin):
    def process_request(self, request):
        print('我是第一个自定义中间件里面的process_request方法')

    def process_response(self, request, response):
        print('我是第一个自定义中间件里面的process_response方法')

        """

        :param request:
        :param response: django后端返回给浏览器的内容
        :return:
        """
        return response

    def process_view(self, request, view_name, *args, **kwargs):
        print(view_name, args, kwargs)
        print('我是第一个自定义中间件里的process_view')

    def process_template_response(self, resquest, response):
        print('我是第一个自定义中间件里的process_template_response')
        return response

    def process_exception(self, request, exception):
        print(exception)
        print('我是第一个自定义中间件里的process_exception')


class MyMiddleWare2(MiddlewareMixin):
    def process_request(self, request):
        print('我是第二个自定义中间件里面的process_request方法')
        # return HttpResponse('JayChou')

    def process_response(self, request, response):
        print('我是第二个自定义中间件里面的process_response方法')
        return response
        # return HttpResponse('hello !')

    def process_view(self, request, view_name, *args, **kwargs):
        print(view_name, args, kwargs)
        print('我是第二个自定义中间件里的process_view')

    def process_template_response(self, resquest, response):
        print('我是第二个自定义中间件里的process_template_response')
        return response

    def process_exception(self, request, exception):
        print(exception)
        print('我是第二个自定义中间件里的process_exception')
```

## csrf跨站请求伪造

## auth模块



