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
