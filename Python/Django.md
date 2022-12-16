[toc]



## （一）纯手撸web框架

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

## （二）基于wsgiref模块（web服务网关接口）

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

![image-20221020093951059](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130121815.png)

![image-20221020094002008](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130122009.png)

![image-20221020094021483](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130121944.png)

## （三）动静态网页

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

![image-20221028155919554](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130122312.png)

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

## （四）模板语法之jinja2模块

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

![image-20221020101009731](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130122329.png)

![image-20221020104139128](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213439.png)

## （五）自定义简易版本web框架请求流程图

![image-20221020104944880](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213443.png)

```python
"""
wsgiref模块
	1.请求来的时候解析http格式的数据，封装成大字典
	2.响应走的时候给数据打包成符合http格式，再返回给浏览器
"""
```

## （六）Python三大主流web框架

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

## （七）注意事项

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

## （八）django基本操作

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

![image-20221105205022192](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213453.png)

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

![image-20221021095003878](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213457.png)

![image-20221021094805831](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213500.png)

---

##### 1. 3.==创建应用==

⼀个django项⽬中可以包含多个应⽤,可以使⽤以下命令建⽴应⽤：

```python
python manage.py startapp app01
```

![image-20221021095924761](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213503.png)

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

![image-20221021101334625](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213506.png)

![image-20221021101652876](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213509.png)

##### 2. 2.==启动django项目==



```python
# 2. 启动
	（1）用命令启动   python manage.py runserver
    （2）点击绿色小箭头
```



![image-20221021102142570](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213513.png)

**如下图所示**：**启动成功**

![image-20221021102225879](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213516.png)

---

##### 2. 3.==创建应用==



```python
# 3. 创建应用
	（1）pycharm提供的终端直接输入命令 python manage.py startapp app01
     (2)tools----run manage.py task提示
```

---

![image-20221021102406290](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213518.png)

- 有自动提示功能

![image-20221021102504743](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213521.png)

**如下图所示：应用创建成功**

![image-20221021102553348](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213524.png)

---

##### 2. 4.==还可以修改端口号==



![image-20221021102801948](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213526.png)

![image-20221021102835863](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213528.png)

**如下图所示：端口修改成功！**

![image-20221021102901754](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213531.png)

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

![image-20221028172420141](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213534.png)

![image-20221021104644697](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213536.png)

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
USE_TZ=False # 可以保证数据库时间和现实shi'jian同步，否则相差8个小时
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



## （九）django小白必会三板斧

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

##   （十）静态文件配置

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

![image-20221021173936220](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213546.png)

![image-20221021174520951](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213548.png)

* 引入了bootstrap的js文件和css文件，但是浏览器访问的时候并没有显示该有的样式，需**要开设该资源的接口**

![image-20221021174448402](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213557.png)

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

![image-20221021181046077](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213600.png)

![](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213603.png)

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



## （十一）pycharm链接数据库（MySQL）

pycharm可以充当很多数据库的客户端

![image-20221022100125505](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213606.png)



点击MySQL后,如果是**第一次使用pycharm中的MySQL**，那么需要**点击download下载对应驱动**



![image-20221022100404882](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213613.png)



==如果提示下载失败的话，可以点击Driver，选择**MySQL for 5.1,**然后重新下载，**重新测试连接，成功**，测试链接成功后，点击apply，点击OK。！==



![image-20221022102748371](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213616.png)

![image-20221022103600831](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213619.png)

​      

 **可以先通过==Navicat==添加数据**



![image-20221022103653311](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213621.png)



**在pycharm中点击刷新，即可看到创建的userinfo 表以及数据**



![image-20221022103754208](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213624.png)

![image-20221022103805763](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213627.png)

**通过pycharm修改数据，然后submit提交同步到数据库**

![image-20221022104045078](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213630.png)

**回到Navicat，刷新一下，数据就有了**

![image-20221022104129877](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213633.png)

## （十二）Django链接MySQL

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

![image-20221022105927612](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213637.png)

## ==（十三）Django ORM==

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



![image-20221022111412105](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213640.png)

应用文件夹下的==migrations文件夹==下多了一个==0001._initial.py==日志文件

![image-20221022111604561](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213643.png)

```python
 python manage.py migrate  #⽣成数据库表 
#将操作真正同步到数据库中
```

![image-20221022112226431](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213649.png)

**数据库迁移的两条命令输入完成后，此时数据库中会出现很多张表**



![image-20221022115814598](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213653.png)

**一个Djanog项目可以有多个应用，那么多个应用之间可能会出现表名冲突的情况，那么加上前缀就可以完全避免冲突**    

![image-20221022120128278](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213656.png)

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

![image-20221022124444920](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213659.png)

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

![image-20221022125639587](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213703.png)

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



## （十四）Django请求周期流程图（重要）

![image-20221023130145467](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213717.png)



```python
# 扩展
"""
缓存数据库
	提前将数据准备好，来了直接拿就可以，提高效率和响应时间
	
"""
```

## ==（十五）路由层==

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

![image-20221105214456492](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213721.png)

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

![image-20221105210631000](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213724.png)



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

![image-20221105211639543](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213727.png)

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



![image-20221105212419964](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213734.png)

---

Django检查url模式之前会移除模式前的`/`，所以url模式前⾯的`/`可以不写，但如果 在地址栏⾥请求的时候不带尾斜杠，则会引起重定向，重定向到带尾斜杠的地址， 所以请求的时候要带尾斜杠。

![image-20221023134245558](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213737.png)

**django路由匹配 的时候其实可以匹配两次，第一次如果url后面没有加斜杠，django会让浏览器加斜杠在发送一次请求**

```python
# settings.py

APPEND_SLASH = False# 取消自动加斜杠，默认是True
"""取消了自动加斜杠后，再次访问浏览器就会报错"""
```

![image-20221023134109323](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213743.png)

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

## （十六）虚拟环境

**安装**

```python
不同的项目依赖不同的模块版本，不能共用一套环境，需要使用虚拟环境
在系统的python环境中安装
    pip install virtualenv
    pip install virtualenvwrapper-win
   
```

 **配置虚拟环境管理器工作目录**

```python
# 配置环境变量：
# 控制面板 => 系统和安全 => 系统 => 高级系统设置 => 环境变量 => 系统变量 => 点击新建 => 填入变量名与值
变量名：WORKON_HOME  变量值：自定义存放虚拟环境的绝对路径
eg: WORKON_HOME: E:\Python\Virtualenvs

# 同步配置信息：
# 去向Python3的安装目录 => Scripts文件夹 => virtualenvwrapper.bat => 双击

```

**cmd中使用命令**

```python
workon                      查看已有的虚拟环境
workon 虚拟环境名称           使用某个虚拟环境
python | exit()				进入|退出 该虚拟环境的Python环境
pip或pip3 install 模块名		为虚拟环境安装模块
deactivate					退出当前虚拟环境

#创建
mkvirtualenv 虚拟环境名称	 					默认Python环境创建虚拟环境
mkvirtualenv -p python3.6 虚拟环境名称		基于某Python环境创建虚拟环境		

遇到虚拟环境没创建到 E:\Python\Virtualenvs，使用 mkvirtualenv E:\Python\Virtualenvs

# 删除
rmvirtualenv 虚拟环境名称




```



**requirements.txt文件**

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

**参数介绍**

```python
--encoding=utf8 ：为使用utf8编码

--force ：强制执行，当 生成目录下的requirements.txt存在时覆盖 

. /: 在哪个文件生成requirements.txt 文件
```

## （十七）Django版本区别

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

## ==（十八）视图层==

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
r
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
![在这里插入图片描述](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213803.png)

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



![image-20221107224043800](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213809.png)

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

## ==（十九）模版层==

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
    	1. 必须要在应用下创建一个名字"必须"叫templatetags文件夹
    	2.在该文件夹内创建"任意"名称的py文件
    	3.在该py文件内"必须"先书写两句话
            from django import template

            register = template.Library()
    """

    


#自定义过滤器(参数最多两个)
@register.filter(name='tag')
def my_sum(v1, v2):
    return v1 + v2

#页面使用
{% load mytag %}   #先加载自定义的py文件
<p>{{ n|tag:666 }}</p>
```

* 自定义标签

```python
标签多个参数彼此之间空格隔开
{% load mytag %}
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
{% load mytag %}
{% left 10 %}


#当html页面某一个地方的页面需要传参数能够动态的渲染出来，并且在多个页面上都需要使用该局部，那么就考虑该局部页面做成inclusion_tag形式
(bbs中会使用到)
```

![image-20221024224003718](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213828.png)

![image-20221024224030160](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213831.png)

![image-20221024224053678](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213835.png)

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

![image-20221025153951035](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213838.png)

```python
#然后子页面可以声明想要修改哪块划定了的区域
{% block 名字 %}
	子页面内容
    
    子版页面除了可以写自己的代码之外，还可以继续使用模板的内容
    {{ block.super }} 
{% endblock %}
```



![image-20221025152808944](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213841.png)

```python
        
#一般情况下模板页面上应该至少有三块可以被修改的区域
 1. css区域
 2. html区域
 3. js区域
    
    
每一个子页面都可以有自己独有的html代码，css代码，js代码
```

![image-20221025153413108](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213843.png)

![image-20221025153715030](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213846.png)

* **一般情况下，模板页面上划定的区域越多，那么该模板的扩展性就越高，但是如果太多，就还不如自己写**

### 6、模板的导入



```python
"""
将页面的某一个局部当作模块形式
哪个地方需要就可以直接导入使用
"""

{% include 'demo.html' %}"
```

![image-20221025155047637](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213851.png)

![image-20221025155207250](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213859.png)

## ==（二十）模型层==



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
    register_time = models.DateField(auto_now_add=True)  # 年月日
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



![image-20221026180126520](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213918.png)

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
    publish_date = models.DateField(auto_now_add=True)
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
    # book_obj.author.add(1)  # 书籍id为2的书籍绑定一个主键为1的作者
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
   #res1=models.Author.objects.filter(book__id=1).values('author_detail__phone')
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

![image-20221029164727597](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213935.png)



---

![image-20221029164841977](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213939.png)

#### **defer**

```python
#tests.py

res=models.Book.objects.defer('title')
    for i in res:
        print(i.price)
        """defer与only刚好相反
                defer括号内放的字段不在查询出来的对象里面，查询该字段需要重新走数据
                而如果查询的是非括号内的字段，则不需要走数据库
            """
```



![image-20221029164310790](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213950.png)

---

![image-20221029164520153](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130213954.png)

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

## （二十一）图书管理系统搭建

## （二十二）choices参数（数据库字段设计常见）

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
    该gender字段存的还是数字，但是如果存的数字在上面元组列举的范围之内，
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

## （二十三）MTV与MVC模型

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



## （二十四）多对多关系的三种创建方式

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
    book=models.ForeignKey(to='Book',on_delete=models.CASCADE)
    author=models.ForeignKey(to='Author',on_delete=models.CASCADE)
    
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

## ==（二十五）基于jQuery的Ajax实现（重点）==

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
            datatyp: 'JSON', //会自动反序列化
            success: function (args) {
                $('#d3').val(args)//通过DOM操作，动态渲染到第三个Input框
                #console.log(typeof (args))
                
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

![image-20221102200546457](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214008.png)

* **数据格式**

![image-20221102201124294](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214011.png)

![image-20221102201322322](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214014.png)



* **django后端针对符合urlencoded编码格式的数据会自动的帮你解析封装到request.POST中**



![image-20221102202334553](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214017.png)

![image-20221102203015897](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214020.png)



* **如果把编码格式改成formdata，那么针对普通的键值对还是解析到request.POST中,而将文件解析到request.FILES中**



![image-20221102202407309](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214022.png)

![image-20221102202618109](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214028.png)

![image-20221102202655337](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214025.png)

#### 3.2、ajax

```python
默认编码格式也是urlencoded
数据格式也是:username=tony&age=19
django后端针对符合urlencoded编码格式的数据会自动的帮你解析封装到request.POST中
	username=zhao&password=123  >>> request.POST
```

![image-20221102204855809](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214043.png)



* **编码格式**

  

![image-20221102203718139](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214047.png)



* **数据格式**



![image-20221102203825877](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214050.png)



* **django后端针对符合urlencoded编码格式的数据会自动的帮你解析封装到request.POST中**



![image-20221102204038587](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214052.png)

### 4、ajax发送json格式数据

==前后端传输数据的时候一定要确保**编码格式**跟**数据格式**是**一致**的==

```python
"""
前后端传输数据的时候一定要确保编码格式跟数据真正的格式是一致的


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

![image-20221102205741523](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214109.png)

![image-20221102205908090](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214125.png)

* 但是此时要发送的数据并不是json格式,**需要转成json格式**
* 前端使用`JSON.stringify()`将想要发送的数据转成json格式的数据

```python
data: JSON.stringify({'username': 'zhao', 'age': 19}),
```



![image-20221102210141395](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214120.png)

* 查看json格式数据

```python
{"username":"zhao","age":19}
```

![image-20221102210354092](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214116.png)

* 在`request.POST`中找不到数据

![image-20221102210714716](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214141.png)

* 去`request.body`中获取数据，转成`json`格式

![image-20221103171457707](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214145.png)

```python
json.loads() 括号内如果传入了一个二进制数据，那么内部可以 自动解码再反序列化
```

![image-20221102213336984](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214149.png)

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

![image-20221102221448232](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214154.png)

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

![image-20221104224139637](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214201.png)

## （二十六）Django自带的序列化组件(为drf做铺垫)

(drf：django rest framework)

```python
#在前端获取到,后端用户表里所有的数据，并且是列表套字典的格式
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



**前端显示结果：**

![image-20221103175900052](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214206.png)

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

![image-20221104205336198](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214210.png)

## （二十七）批量插入    bulk_create()

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

## （二十八）自定义分页器

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



![image-20221104223106449](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214222.png)

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



![image-20221104223029555](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214236.png)

## ==（二十九）校验性组件：froms组件==

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

![image-20221106210111924](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214242.png)

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
form_obj.is_valid()  #少传了一个字段，返回False
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

![image-20221106211541541](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214249.png)

```html
<form action="" method="post">
    <p>第一种渲染方式:代码书写极少，封装成度太高不便于后续拓展，一般只在本地测试使用</p>
    {{ form_obj.as_ul }}
</form>
```

![image-20221106212046568](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433.png)

```html
<form action="" method="post">
    <p>第一种渲染方式:代码书写极少，封装成度太高不便于后续拓展，一般只在本地测试使用</p>
    {{ form_obj.as_table }}
</form>
```

![image-20221106212158865](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214253.png)

#### 4.3、第二种渲染方式

```python
<p>{{ form_obj.username }}</p>
# form_obj是类对象，而username是类里的属性，
```

![image-20221106213211520](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214256.png)

```html
 <p>{{ form_obj.username.label }}{{ form_obj.username }}</p>
```

![image-20221106212855862](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214311.png)

```python
#可以在类里面修改代码，展示input框前面的注释信息，
username = forms.CharField(min_length=3, max_length=8,label='用户名')
```

![image-20221106212959362](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214314.png)

```html
<form action="" method="post">
    <p>第二种渲染方式：可扩展性非常强，但是书写代码太多</p>
    <p>{{ form_obj.username.label }}:{{ form_obj.username }}</p>
    <p>{{ form_obj.password.label }}:{{ form_obj.password }}</p>
    <p>{{ form_obj.email.label }}:{{ form_obj.email }}</p>
</form>
```

![image-20221106213434732](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214318.png)

#### 4.4、第三种渲染方式

```html
<form action="" method="post">
    <p>第三种渲染方式：</p>
    {% for form in form_obj %}
        <p>{{ form }}</p>
    {% endfor %}
</form>


```

![image-20221106213615001](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214322.png)

上图所示,`{{form}}`等价于第二种渲染方式的`{{form_obj.username}}`,那么直接`.label`就可以得到`input`框前面的**注释**,

```html
<form action="" method="post">

    <p>第三种渲染方式：代码精简，扩展性也高</p>
    {% for form in form_obj %}
        <p>{{ form.label }}:{{ form }}</p>

    {% endfor %}

</form>
```

![image-20221106214118737](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214325.png)

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

![image-20221107190747441](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214329.png)

---

![image-20221107191036994](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214331.png)

---

![image-20221107191023281](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214335.png)

* **如何让浏览器不自动校验**

```html
<form action="" method="post" novalidate>
```

![image-20221107191659967](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214338.png)

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

![image-20221107191923509](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214341.png)

前端渲染效果展示：就不会再自动生成`ul套li`的格式，而是生成一个`普通文本`

![image-20221107191810415](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214343.png)

但是又因为再后端做了限制，最大只能8位，所以会出现当输入到8位的时候就出现不能再输入的效果，可以通过打开检查，修改前端代码来达到目的。再次显示出前端校验的若不惊风！！

![image-20221107192646078](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214346.png)

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

![image-20221107194237578](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214353.png)

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

![image-20221107200041886](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214403.png)

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

Widget负责渲染网页上HTML表单的输入元素和提取提交的原始数据。widget是字段的一个内在属性，用于定义字段在浏览器的页面里以何种HTML元素展现。

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

默认是True，必填项，改成False，就可以不填

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

## ==（三十）Cookie与Session==

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

![image-20221108194110940](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214414.png)

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

![image-20221108205236315](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214427.png)

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
        3.将产生的随机字符串返回给客户端浏览器保存
    """

    return HttpResponse('稻香')
```

然后在浏览器输入路由，回车后，`django_session`表中会自动生成数据,

`expire_data`字段是过期时间

![image-20221108205735075](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-1.png)



---

session给客户端返回的是一个随机字符串,**随机字符串**指的就是`django_session表中的session_key`

==django默认的session过期时间是**14**天==，但是也可以修改

![image-20221108211345212](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-2.png)

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

![image-20221108211941911](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-3.png)

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



![image-20221108220200106](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-4.png)



## ==（三十一）CBV添加装饰器==

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

## ==（三十二）Django中间件==

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

![image-20221109184718273](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-5.png)

### 1、默认中间件源码分析

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

### 2、自定义中间件

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
    'django.contrib.sessions.middleware.SessionMiddleware',  
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

#### process_request（掌握）

<img src="https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-6.png" alt="image-20221109191456363" style="zoom: 80%;" />

**启动项目，查看中间件是否生效**

<img src="https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-7.png" alt="image-20221109191548177" style="zoom:80%;" />



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

```python
# Create your views here.
def index(request):
    print('我是视图函数index')
    return HttpResponse('index')
```

启动项目，浏览器输入路由

<img src="https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-8.png" alt="image-20221109192047022" style="zoom:80%;" />



---

再写一个中间件，并颠倒注册顺序

<img src="https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-9.png" alt="image-20221109192636506" style="zoom:80%;" />



---

添加返回值

<img src="https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-10.png" alt="image-20221109194306420" style="zoom:80%;" />

---

#### ==总结==:

1. **请求来的时候是要经过每一个中间件里的`process_request`方法，结果的顺序按照配置文件中注册的中间件从上往下的顺序依次执行**

2. **如果中间件里面没有定义`process_request`方法，直接跳过，执行下一个**
3. **如果该方法返回了`HttpResponse`对象，那么请求将不再继续往后执行，而是直接原路返回（校验失败，不允许访问）**
   1. **所以process_request方法就是用来做全局相关的所有限制功能**

#### process_response（掌握）

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

![image-20221109195429437](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-11.png)

返回自己的HttpResponse对象

原本后端是要返回`index`的，走到中间件里面，先经过`MyMiddleWare`，返回`index`,但是经过`MyMiddleWare2`的时候来了一个偷天换日，把原本想要返回的响应换成自己的`hello!`，之后所有的中间件拿到的全是自己写的返回响应的内容

![image-20221109195712980](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-12.png)

#### 总结：

1. **响应走的时候需要经过每一个中间件里的process_response方法,该方法有两个额外的参数(request,response)**
2. **该方法必须返回一个HttpResponse对象**
   1. **默认返回的就是形参response,**
   2. **也可以自己返回自己的HttpResponse对象**

3. **顺序是按照配置文件中注册了的中间件从下往上依次经过，如果没有定义，直接执行上一个**

---

如果在先注册了的中间件中的process_request方法就已经返回了HttpResponse对象，那么响应走的时候经过所有的中间件里面的process_response是否有其他情况？



![image-20221109201241624](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-13.png)

==观察上图得出以下结论==：

​	首先请求来的时候，会依次经过每一个注册了的中间件里的`process_reques`t方法，一旦`process_request`方法返回了一个`HttpResponse`对象，那么会直接不再往下走，而是直接经过**同级别的`process_response`往外走。**

![image-20221109201546542](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-14.png)

---

![image-20221109201702210](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-15.png)

#### process_view(了解)

```python
def process_view(self, request, view_name, *args, **kwargs):
    print(view_name, args, kwargs)
    print('我是第一个自定义中间件里的process_view')
```

特点：

路由匹配成功之后，执行视图函数之前，会自动执行中间件里面的process_view方法,顺序按照配置文件中注册的中间件从上往下的顺序依次执行

![image-20221109203106289](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-16.png)

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

![image-20221109204530848](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-17.png)

#### process_execption(了解)

```python
def process_exception(self, request, exception):
    print(exception)
    print('我是第二个自定义中间件里的process_exception')
```

当视图函数中出现异常的情况下，触发。

顺序是按照配置文件中注册的中间件从下往上依次经过

![image-20221109205002531](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-18.png)

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

## ==（三十三）csrf跨站请求伪造==

### 1、前戏

```python
"""
钓鱼网站
	搭建一个跟正规网站一摸一样的界面（中国银行）
	用户进入到我们的网站，用户给某人打钱，
	用户打钱操作确确实实是提交给了中国银行的系统，用户的钱也确确实实减少了，但是唯一不同是打钱的账户不是用户想要打的账户，变成了另一个账户

内部本质:
	在钓鱼网站的页面，针对对方账户，只给用户提供一个没有name属性的input框，然后我们再内部隐藏一个已经写好name和value的input框，
		
"""
```

* 真正的网站端口:8000

`http://127.0.0.1:8000`

```python
# 注释掉csrf
 'django.middleware.csrf.CsrfViewMiddleware',
```

```html

<body>
<h1>中国银行</h1>
<form action="" method="post">
    <p>username:<input type="text" name="username"></p>
    <p>target_user:<input type="text" name="target_user"></p>
    <p>money:<input type="text" name="money"></p>
    <input type="submit">
</form>
</body>
```

```python
# views.py
def transfer(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        target_user = request.POST.get('target_user')
        money = request.POST.get('money')
        print('%s给%s转了%s元' % (username, target_user, money))

    return render(request, 'transfer.html')

# urls.py
path('transfer/',views.transfer)
```

* 钓鱼网站模拟端口:8001

```python
#不用注释csfr
```

```html
<body>
<h1>phishing site</h1>
<form action="http://127.0.0.1:8000/transfer/" method="post">
    <p>username:<input type="text" name="username"></p>
    <p>target_user:<input type="text"></p>
    <input type="text" name="target_user" value="zhao" style="display: none">
    <p>money:<input type="text" name="money"></p>
    <input type="submit">
</form>
</body>
```

```python
#views.py
def transfer(request):
    return render(request,'transfer.html')

# urls.py
path('transfer/',views.transfer)
```

### 2、csrf校验

如何规避上述问题:
    csrf跨站请求伪造
    	网站再给用户返回一个提交数据功能的页面的时候会给这个页面加一个唯一标识
        当这个页面朝后端发post请求的时候，后端会先校验唯一标识，如果唯一标识不对，直接拒绝（403 forbiden),如果成功则正常执行
       

#### 2.1、from表单如何符合校验



```python
#开启配置文件里的csrf中间件
```

```html
<form action="" method="post">
    {% csrf_token %}
    <p>username:<input type="text" name="username"></p>
    <p>target_user:<input type="text" name="target_user"></p>
    <p>money:<input type="text" name="money"></p>
    <input type="submit">
</form>
```

![image-20221111105106883](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-19.png)

再次发送请求

![image-20221111105127536](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-20.png)

钓鱼网站再次发送请求

![image-20221111105520008](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-21.png)

#### 2.2、ajax如何符合校验

不论是ajax还是谁，只要是向我Django提交post请求的数据，都必须校验csrf_token来防伪跨站请求

* 方式一

通过获取隐藏的input标签中的`csrfmiddlewaretoken`值，放置在data中发送

```html
</head>
<body>
<h1>中国银行</h1>
<form action="" method="post">
    {% csrf_token %}
    <p>username:<input type="text" name="username"></p>
    <p>target_user:<input type="text" name="target_user"></p>
    <p>money:<input type="text" name="money"></p>
    <input type="submit">
</form>
<button id="d1">ajax请求</button>
</body>


<script>
    $('#d1').on('click', function () {
        $.ajax({
            url: '',
            type: 'post',
            //第一种:利用标签查找获取页面上的随机字符串
            data: {'username': 'zhao', 'csrfmiddlewaretoken': $('[name=csrfmiddlewaretoken]').val()},
            success() {

            }

        })

    })
</script>
```

* 方式二

```html
</head>
<body>
<h1>中国银行</h1>
<form action="" method="post">
    {% csrf_token %}
    <p>username:<input type="text" name="username"></p>
    <p>target_user:<input type="text" name="target_user"></p>
    <p>money:<input type="text" name="money"></p>
    <input type="submit">
</form>
<button id="d1">ajax请求</button>
</body>

<script>
    $('#d1').on('click', function () {
        $.ajax({
            url: '',
            type: 'post',
            //第二种方式:利用模板语法快捷书写
            data: {'username': 'zhao', 'csrfmiddlewaretoken': '{{csrf_token}}'},
            }
        })
    })
</script>
```

* 方式三

先拷贝js文件:

```js
function getCookie(name) {
    var cookieValue = null;
    if (document.cookie && document.cookie !== '') {
        var cookies = document.cookie.split(';');
        for (var i = 0; i < cookies.length; i++) {
            var cookie = jQuery.trim(cookies[i]);
            // Does this cookie string begin with the name we want?
            if (cookie.substring(0, name.length + 1) === (name + '=')) {
                cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                break;
            }
        }
    }
    return cookieValue;
}

var csrftoken = getCookie('csrftoken');


function csrfSafeMethod(method) {
    // these HTTP methods do not require CSRF protection
    return (/^(GET|HEAD|OPTIONS|TRACE)$/.test(method));
}

$.ajaxSetup({
    beforeSend: function (xhr, settings) {
        if (!csrfSafeMethod(settings.type) && !this.crossDomain) {
            xhr.setRequestHeader("X-CSRFToken", csrftoken);
        }
    }
});
```

将文件配置到静态文件中，在html页面上通过导入该文件即可自动帮我们解决ajax提交post数据时校验csrf_token的问题

![image-20221111112430616](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-22.png)

```html
</head>
<body>
<h1>中国银行</h1>
<form action="" method="post">
    {% csrf_token %}
    <p>username:<input type="text" name="username"></p>
    <p>target_user:<input type="text" name="target_user"></p>
    <p>money:<input type="text" name="money"></p>
    <input type="submit">
</form>
<button id="d1">ajax请求</button>
</body>


{% load static %}
<script src="{% static  'js/setup.js' %}"></script>
<script>
    $('#d1').on('click', function () {
        $.ajax({
            url: '',
            type: 'post',
            //第三种方式:引入js文件
            data: {'username': 'zhao'},
            success() {

            }

        })

    })
</script>
```

### 3、csrf相关装饰器

#### 3.1、FBV

* 网站整体都校验csrf，就单单几个视图函数不校验

```python
#配置文件中开始csrf中间件，
from django.views.decorators.csrf import csrf_protect, csrf_exempt

"""
csrf_protect,需要校验
csrf_exempt  忽视校验
"""

@csrf_exempt 
def transfer(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        target_user = request.POST.get('target_user')
        money = request.POST.get('money')
        print('%s给%s转了%s元' % (username, target_user, money))

    return render(request, 'transfer.html')
```

* 网站整体都不校验csrf，就单单几个视图函数需要校验

```python
# 注释掉form表单中 {% csrf_token %}#}
#关闭配置文件中csrf中间件

@csrf_protect
def transfer(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        target_user = request.POST.get('target_user')
        money = request.POST.get('money')
        print('%s给%s转了%s元' % (username, target_user, money))

    return render(request, 'transfer.html')
```

![image-20221111141146200](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-23.png)

#### 3.2、CBV

```pytohn
csrf_protect,需要校验
​	针对csrf_protect符合CBV装饰器的三种写法

csrf_exempt  忽视校验
​	针对csrf_protect只能给dispatch方法加才有效
```



* 网站整体都不校验csrf，就单单几个视图函数需要校验

```python
# views.py
from django.utils.decorators import method_decorator
from django.views import View


# @method_decorator(csrf_protect,name='post') #第二种
class MycsrfToken(View):
    @method_decorator(csrf_protect)
    def dispatch(self, request, *args, **kwargs):
        return super(MycsrfToken, self).dispatch(request, *args, **kwargs)

    def get(self, request):
        return HttpResponse('get')

    # @method_decorator(csrf_protect) #第一种方式可以
    def post(self, request):
        return HttpResponse('post')
    
    
# urls.py
 path('csrf/', views.MycsrfToken.as_view()),
```

```python
# html  from表单向csrf路由提交
<h1>中国银行</h1>
<form action="/csrf/" method="post">
{#    {% csrf_token %}#}
    <p>username:<input type="text" name="username"></p>
    <p>target_user:<input type="text" name="target_user"></p>
    <p>money:<input type="text" name="money"></p>
    <input type="submit">
</form>


    
# urls.py
path('transfer/', views.transfer),

# views.py
def transfer(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        target_user = request.POST.get('target_user')
        money = request.POST.get('money')
        print('%s给%s转了%s元' % (username, target_user, money))

    return render(request, 'transfer.html')
```

* 网站整体都校验csrf，就单单几个视图函数不校验

```python
from django.utils.decorators import method_decorator
from django.views import View



# @method_decorator(csrf_exempt,name='dispatch') 
class MycsrfToken(View):

    @method_decorator(csrf_exempt)
    def dispatch(self, request, *args, **kwargs):
        return super(MycsrfToken, self).dispatch(request, *args, **kwargs)

    def get(self, request):
        return HttpResponse('get')

 
    def post(self, request):
        return HttpResponse('post')
```

## ==（三十四）基于Django中间件引发的编程思想（重点）==

#### **importlib模块使用**

能够以字符串的形式导入模块，最小单位只能到模块名

```python
# 1. 创建一个py文件 aaa.py
# 2. 创建一个mypach文件夹,里面创建一个bbb.py文件，写上name='zhao'

然后再aaa.py中书写以下代码
import importlib

res = 'mypack.bbb'  # 最小单位只能到模块的名字，不能点模块里的变量名
ret = importlib.import_module(res)  # from mypack import bbb
print(ret)


# 结果如下
<module 'mypack.bbb' from 'E:\\Python\\python\\python进阶\\WEB开发\\day12\\mypack\\bbb.py'>
```

#### **编程思想**

```python
1. 先创建一个notify文件夹,里面创建py文件，有几个功能，创建几个py文件
2. 因为一个文件夹，里面有一个个的py文件，就是一个包，所以要再notify文件夹里创建__init__.py文件
3. settings #配置文件
4. start.py #启动文件

```

![image-20221111151627195](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-24.png)

email.py

```python
class Email(object):
    def __init__(self):
        pass  # 发送邮箱需要做的前期准备工作   接口什么的

    def send(self, content):
        print('Email通知:%s' % content)
```

qq.py

```pytohn
class Qq(object):
    def __init__(self):
        pass  # 发送QQ需要做的前期准备工作

    def send(self, content):
        print('QQ通知:%s' % content)
```

wechat.py

```python
class Wechat(object):
    def __init__(self):
        pass  # 发送微信需要做的前期准备工作

    def send(self, content):
        print('微信通知:%s' % content)
```

**settings.py**

```python
NOTIFY_LIST = [       
    'notify.email.Email',
    'notify.qq.Qq',
    'notify.wechat.Wechat',
]
```

==`__init__.py`== **（编程思想的灵魂）**

```python
import settings
import importlib


def send_all(content):
    for path_str in settings.NOTIFY_LIST:  # 'notify.email.Email',
        module_path, class_name = path_str.rsplit('.', maxsplit=1)
        # module_path='notify.email'
        # class_name='Email'
        # 1. 利用字符串导入模块
        module = importlib.import_module(module_path)  # from motify import email
        # 2. 利用反射获取类名
        cls = getattr(module, class_name)  # Email Qq Wechat
        # 3. 生成类的对象
        obj = cls()
        # 4. 利用鸭子类型直接调用send方法
        obj.send(content)
```

start.py

```pytohn
import notify

notify.send_all('国庆不放假')
```

## ==（三十五）auth模块方法使用==

### 1、创建超级用户(管理员)

```python
"""
在创建好一个django项目后，直接执行数据库迁移命令后会自动生成很多表，  django_session  .............     
其中就包括 auth_user表

django在启动之后就可以直接访问admin路由，需要输入用户名和密码，数据参考的就是auth_user表，并且还必须是管理员用户才能进入

"""
```

先执行数据库迁移命令生成表

![image-20221111182406068](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-25.png)



```python
#创建管理员用户
python manage.py createsuperuser
```

![image-20221111182908751](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-26.png)

超级用户创建好之后，auth_user表中发生变化

![image-20221111183118544](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-27.png)

路由中输入admin，登录管理员用户

![image-20221112154955296](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-28.png)

登录进入后的界面：

![image-20221112155006201](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-29.png)

**依赖于auth_user表完成用户以下相关的所有功能：**

### 2、获取表，检验密码

登录功能

```html
<form action="" method="post">
    {% csrf_token %}
    <p>username:<input type="text" name="username"></p>
    <p>password:<input type="text" name="password"></p>
    <input type="submit">
</form>
```

```python
# views.py

from django.shortcuts import render, redirect, HttpResponse
from django.contrib import auth

def login(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')
        # 去用户表中校验数据
        # 1.获取表
        # 2.密码比对
        user_obj = auth.authenticate(request, username=username, password=password)
        # print(user_obj)  # 用户对象  数据不符合返回None
        # print(user_obj.username)  # 用户名
        # print(user_obj.password)  # ,密码
        
        
        """
        1.自动查找auth_user表
        2.自动给密码加密比对
        该方法的注意事项:
            括号内必须同时传入用户名和密码
            不能只传用户名
        """
        
        if user_obj:
            # 保存用户状态
            auth.login(request, user=user_obj)  # 类似于request.session[key]=user_obj
            
            """
            只要执行了该方法，就可以在任何地方通过request.user获取到当前登录的用户对象
            """ 
             return redirect('/home/')
      

    return render(request, 'login.html')
```



![image-20221112163308754](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-30.png)

### 3、保存用户状态

```python
# 保存用户状态
 auth.login(request, user=user_obj)  # 类似于request.session['key']=user_obj
    
 """
只要执行了该方法，就可以在任何地方通过request.user获取到当前登录的用户对象
""" 
```

保存用户状态后，`django_session`表中就多了条数据

![image-20221112205630730](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-31.png)

![image-20221112205701141](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-32.png)

### 4、获取用户对象，校验用户是否登录

```python
def home(request):
    print(request.user)  # 拿到用户对象
    """
    自动去django_session表中查找对应的用户对象给你封装到request.user中
    """
    # 判断用户是否登录
    print(request.user.is_authenticated)
    return HttpResponse('OK!')
```

当删除`django_session`表中的数据，就表示用户没有登录过，再次查看`request.user`拿到什么数据

![image-20221112210105392](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-33.png)

登陆成功后，返回当前登录的用户对象，返回True

![image-20221112210247146](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-34.png)

### 5、验证用户是否登录

用户登录后才能访问(加装饰器)

```python
"""用户登录之后才能看home"""

#  局部配置:用户没有登录跳转到login_user后面指定的网址
from django.contrib.auth.decorators import login_required
@login_required(login_url='/login/')  
def home(request):
    print(request.user) 
    print(request.user.is_authenticated)
    return HttpResponse('OK!')


# 全局配置，没有登录跳转到指定页面
#配置文件settings.py
LOGIN_URL = '/login/'


from django.contrib.auth.decorators import login_required
@login_required
def home(request):
    print(request.user)  
    print(request.user.is_authenticated)
    return HttpResponse('OK!')


# 如果局部和全局都有，会跳转到局部配置，
# 局部配置的优先级大于全局配置
# 全局的好处在于无需重复写代码，但是跳转的页面很单一
# 局部的好处在于不同的视图函数在用户没有登录的情况下可以跳转到不同的页面
```

### 6、修改密码



```python
#urls.py

#修改密码
path('set_password/',views.set_password),
```

```python
#views.py

@login_required
def set_password(request):
    if request.method == 'POST':
        old_password = request.POST.get('old_password')
        new_password = request.POST.get('new_password')
        confirm_password = request.POST.get('confirm_password')
        # 先校验两次密码是否一致
        if new_password == confirm_password:
            # 校验旧密码是否正确
            is_right = request.user.check_password(old_password)  # 自动加密，比对密码，返回布尔值
            if is_right:
                # 修改密码
                request.user.set_password(new_password)  # 仅仅是修改对象的属性
                request.user.save()  # 这一步才是真正的操作数据库，
        return redirect('login')
    return render(request, 'set_password.html', locals())
```

```html
<form action="" method="post">
    {% csrf_token %}
    <p>username:<input type="text" name="username" disabled value="{{ request.user.username }}"></p>
    <p>old_password:<input type="text" name="old_password"></p>
    <p>new_password:<input type="text" name="new_password"></p>
    <p>confirm_password:<input type="text" name="confirm_password"></p>

    <input type="submit">
</form>
```



![image-20221112213746681](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-35.png)

### 7、注销

```python
#注销
path('login_out/',views.login_out)
```

```python
@login_required
def login_out(request):
    auth.logout(request)  # 类似于 request.session.flush()
    return redirect('/login/')
```

### 8、注册

```html
<form action="" method="post">
    {% csrf_token %}
    <h1>注册</h1>
    <p>username:<input type="text" name="username"></p>
    <p>password:<input type="text" name="password"></p>
    <input type="submit">
</form>
```

```python
# 注册功能
path('register/',views.register),

#views.py
from django.contrib.auth.models import User
def register(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')
        # 操作auth_user表写入数据
        User.objects.create(username=username, password=password)  # 创建数据，但是用create创建，密码没有加密处理

    return render(request, 'register.html')
```

![image-20221112220654952](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-36.png)

```python
# 创建普通用户
User.objects.create_user(username=username, password=password)
```

![image-20221112221049076](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-37.png)

```python
 # 创建超级用户（了解）
        User.objects.create_superuser(username=username,password=password)
```

![image-20221112221346397](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-38.png)

### 9、方法总结

```python
"""1. 比对用户名和密码是否正确"""
user_obj = auth.authenticate(request, username=username, password=password)
print(user_obj)  # 用户对象  数据不符合返回None
print(user_obj.username)  # 用户名
print(user_obj.password)  # ,密码，密文
"""2.保存用户状态"""
# 保存用户状态
 auth.login(request, user=user_obj)  # 类似于request.session['key']=user_obj
#只要执行了该方法，就可以在任何地方通过request.user获取到当前登录的用户对象
"""3. 判断当前用户是否登录"""
print(request.user.is_authenticated)
"""4. 获取当前登录用户"""
request.user
"""5. 校验用户是否登录（装饰器）"""
#  局部配置:用户没有登录跳转到login_user后面指定的网址
from django.contrib.auth.decorators import login_required
@login_required(login_url='/login/')  
#全局配置
#settings.py
LOGIN_URL='/login/'
# 如果局部和全局都有,会跳转到局部配置，
# 局部配置的优先级大于全局配置
# 全局的好处在于无需重复写代码，但是跳转的页面很单一
# 局部的好处在于不同的视图函数在用户没有登录的情况下可以跳转到不同的页面
"""6.比对原密码"""
request.user.check_password(old_password)  # 自动加密，比对密码，返回布尔值
"""7. 修改密码"""
 request.user.set_password(new_password)  # 仅仅是修改对象的属性
 request.user.save()  # 这一步才是真正的操作数据库，
"""8. 注销"""
auth.logout(request)  # 类似于 request.session.flush()
"""9. 注册"""
from django.contrib.auth.models import User
 # 操作auth_user表写入数据
User.objects.create(username=username, password=password)  # 创建数据，但是用create创建，密码没有加密处理
# 创建普通用户
User.objects.create_user(username=username, password=password)
 # 创建超级用户（了解）
User.objects.create_superuser(username=username,password=password)
```

### 10、==如何扩展auth_user表==

```python
from django.db import models
from django.contrib.auth.models import User, AbstractUser

# Create your models here.
"""扩展表的第一种方式：一对一关系   不推荐使用"""
#
# class UserDetail(models.Model):
#     phone = models.BigIntegerField()
#     user = models.OneToOneField(to='User', on_delete=models.CASCADE)

"""第二种方式：利用面向对象的继承"""

#models.py
class UserInfo(AbstractUser):
    phone = models.BigIntegerField()
    create_time = models.DateTimeField(auto_now_add=True)
    """
    如果继承了AbstractUser
    那么在执行数据库迁移命令的时候auth_user表就不会在创建出来了
    而UserInfo表中会出现auth_user表中所有的字段，外加自己扩展的字段

    这么做的好处就在于能够直接点击你自己创建的表，更加快速的完成操作及扩展

    前提:
        1.在继承之前没有执行过数据库迁移命令（auth_user没有被创建）
            如果auth_user已经被创建，那么就重新换一个库
        2.继承的类型里面不要覆盖AbstractUser里面的字段名
            表里面的字段都不要动，只扩展额外的字段即可
        3. 需要在配置文件中告诉django你要用User Info替代auth_user
            AUTH_USER_MODEL='app01.UserInfo'
                                '应用名.表名'
    """
 
```

![image-20221112224513172](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130214433-39.png)

**如果自己写表替代了`auth_user`，那么auth模块还照常使用，参考的表也由原来的`auth_user`变成了现在的`UserInfo`**

# ==BBS项目开发==

bbs是一个前后端不分离的全栈项目，前后端需要自己一步步完成

### 1、流程

```python
# 1. 需求分析
	架构师+产品经理+开发组组长
    在跟客户谈需求之前，会大致了解客户的需求，然后自己先设计出一套比较好写的方案，在个客户沟通交流中，引导客户往我们想好的方案上靠。
    形成一个初步方案，
# 2. 项目设计
	架构师干的活
    编程语言的选择
    框架选择
    数据库选择  主库:MySQL，缓存数据库:redis,mongondb
	功能划分
    	将整个项目划分成几个功能模块
    找组长开会，
        	给每个组分发任务
    项目报价
    	技术这块需要多少人力，多少天（一个程序员一天按照1500~2000大致）
        产品经理公司层面：再加点钱，（人力，物力）
        公司财务
        公司老板签字确认
    产品经理去跟客户沟通
    后续需要加功能，继续加钱
    
    
# 3.分组开发
	组长找组员开会，安排各自的功能模块，
    我们其实就是再架构师设计好的框架里填写代码而已(码畜)
    
    我们在写代码的时候，写完需要自己先测试是否有bug
    如果有一个些显而易见的bug，你没有避免而是直接交给测试部门测试出来，那么就可能需要扣绩效了（一定要跟测试搞好关系）
    薪资组成 15k (合理合规合法的避税)
    	底薪	10k
        绩效	3k
        岗位津贴 1k
        生活补贴 1k
# 4. 测试
	测试部门测试代码
    压力测试
    .....
# 5. 交付上线
	1.交给对放的运维人员
    2.直接上线到我们的服务器上，收取维护费
    3.其他.....
```

### 2、==表设计==

```python
"""
一个项目最终的不是业务逻辑的书写
而是前期的表设计，只要将表设计好了，后续的功能书写才能一帆风顺

表设计
	1.用户表
		继承AbstractUser
		额外扩展字段
			phone 电话
			avatar	用户头像
			create_time	创建时间
		外键字段
		一对一个人站点表 
        
        
	2.个人站点表
		site_name 个人站点名称
		site_title  站点标题
		site_theme  站点样式 css，js
		
	3.文章标签表
		name 标签名
		
		外键字段 
		一对多个人站点表
		
	4.文章分类表
		name 分类名
		
		外键字段 
		一对多个人站点表
		
	5.文章表
		title 文章标题 
		desc 文章简介
		content 文章内容
		create_time 发布时间
		
		数据库字段设计优化（虽然下述参数字段，可以通过Orm从其他表计算得出，但是效率太低，）
		up_num 点赞数
		down_num 点踩数
		comment_num 评论数
		
		外键字段
		一对多个人站点
		多对多文章标签表
		一对多文章分类
		
		
	6.点赞点踩表
		用来记录哪个用户给那篇文章点了赞还是点了踩
		user    ForenginKey(to='User')
		artice	ForenginKey(to='Article')
		is_up   BooleanField()
		
		1	1	1
		1	2	1	
		1	3	0	
		2	1	1
		
		
		
	7.文章评论表
		用来记录哪个用户给那篇文章写了哪些评论内容
		user 				ForenginKey(to='User')
		artice				ForenginKey(to='Article')
		content				CharFeild()
		comment_time		DateField()	
		#自关联
		parent				ForenginKey(to='Comment' null=True)
		#orm提供的自关联写法
		parent				ForenginKey(to='self',null=True)
		
		
		id		user_id		article_id		parent_id
		1			1			1						
		2			2			1				1
		
根评论子评论的概念
	根评论直接评论当前发布的内容的
	子评论是评论别人的评论
	
	一个根评论可以有多个子评论
	一个多个子评论只属于一个根评论
	
	根评论与子评论是一对多的关系
	
"""
```



![image-20221113112818038](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221212121805.png)

### 3、表创建及同步

```python
"""数据库选择Mysql，由于django自带的sqllite3对日期不敏感，所以换成Mysql"""
```

```python
# models.py
from django.db import models

# Create your models here.
from django.contrib.auth.models import AbstractUser


class UserInfo(AbstractUser):
    phone = models.BigIntegerField(null=True, verbose_name='手机号')
    # 头像
    avatar = models.FileField(upload_to='avatar/', default='avatar/default.jpg', verbose_name='用户头像')
    """给avatar字段传文件对象，该文件会自动存储到avatar文件下，然后avatar字段只保存文件路径avatar/default.jpg"""
    create_time = models.DateField(auto_now_add=True)

    """外键字段"""
    blog = models.OneToOneField(to='Blog', null=True, on_delete=models.CASCADE)


class Blog(models.Model):
    site_name = models.CharField(max_length=32, verbose_name='站点名称')
    site_title = models.CharField(max_length=32, verbose_name='站点标题')
    site_theme = models.CharField(max_length=64, verbose_name='站点样式')  # 存css/js文件路径


# 文章分类
class Category(models.Model):
    name = models.CharField(max_length=32, verbose_name='文章分类')
    """外键字段"""
    blog = models.ForeignKey(to='Blog', null=True, on_delete=models.CASCADE)


class Tag(models.Model):
    name = models.CharField(max_length=32, verbose_name='文章标签')
    """外键字段"""
    blog = models.ForeignKey(to='Blog', null=True, on_delete=models.CASCADE)


class Article(models.Model):
    title = models.CharField(max_length=64, verbose_name='文章标题')
    desc = models.CharField(max_length=255, verbose_name='文章简介')
    # 文章内容很多，一般都使用TextField
    content = models.TextField(verbose_name='文章内容')
    create_time = models.DateField(auto_now_add=True)
    # 数据库字段设计优化
    up_num = models.BigIntegerField(default=0, verbose_name='文章点赞数')
    down_num = models.BigIntegerField(default=0, verbose_name='文章点踩数')
    comment_num = models.BigIntegerField(default=0, verbose_name='文章评论数')
    """外键字段"""
    blog = models.ForeignKey(to='Blog', null=True, on_delete=models.CASCADE)
    category = models.ForeignKey(to='Category', null=True, on_delete=models.CASCADE)
    tags = models.ManyToManyField(to='Tag',
                                  through='ArticleToTag',
                                  through_fields=('article', 'tag'))


# 文章表和标签表的第三张关系表
class ArticleToTag(models.Model):
    article = models.ForeignKey(to='Article', on_delete=models.CASCADE)
    tag = models.ForeignKey(to='Tag', on_delete=models.CASCADE)


class UpAndDown(models.Model):
    user = models.ForeignKey(to='UserInfo', on_delete=models.CASCADE)
    article = models.ForeignKey(to='Article', on_delete=models.CASCADE)
    is_up = models.BooleanField()  # 布尔值，存0/1


class Comment(models.Model):
    user = models.ForeignKey(to='UserInfo', on_delete=models.CASCADE)
    article = models.ForeignKey(to='Article', on_delete=models.CASCADE)
    content = models.CharField(max_length=255, verbose_name='评论内容')
    comment_time = models.DateTimeField(auto_now_add=True, verbose_name='评论时间')
    # 自关联
    parent = models.ForeignKey(to='self', on_delete=models.CASCADE, null=True)  # 有些评论就是根评论
```

### 4、注册功能

，用户头像前端实时展示

ajax

```python
"""之前是在views.py中写froms组件代码

为了解耦合，应该将所有forms组件代码单独写一个地方

如果项目至始至终只用到一个forms组件，那么直接建一个py文件即可
但是如果你的项目需要使用多个forms组件，那么你可以创建一个文件夹，在文件夹内根据forms组件的功能的不同创建多个py文件
myforms文件夹
	regform.py
	loginform.py
	userform.py
	orderform.py

1.利用forms组件渲染前端标签
	1.不利用form表单提交而是用ajax提交
	2.但是需要用到form标签来包含我们所有的获取用户数据的html代码
		$('#myform').serializeArray()
		获取到fomr标签内所有普通键值对的数据
		[{},{},{}]

2.手动渲染获取用户头像标签
      <label for="myfile">头像
                        <img src="{% static 'imgs/default.png' %}" id="myimg" alt="" width='80'
                             style="margin-top: 20px;margin-left: 10px">
                    </label>
                    <input type="file" id="myfile" name="avatar" style="display: none">
                    只要在label里面的内容点击 就会跳转到for指定的标签上
3.如何实时展示 用户头像
	1.利用文件阅读器
	2.change事件
	3.onload等待加载完毕
5.一旦用户信息不合法，如何精确的渲染提示信息
    1.forms组件渲染的标签组件都有一个固定的特点
        id_字段名
        ps:如何获取id值 form.auto_id
              <label for="{{ form.auto_id }}">{{ form.label }}</label>
	2.根据后端返回的字段以及字段对应的报错信息
    	自己手动拼接对应字段的id值
    3.提示功能完善
    	1.jQuery链式操作
    	2.input获取焦点事件
"""
```

**forms组件**

```python
# 书写针对用户表的forms组件|代码
from django import forms
from App import models


class MyRegForm(forms.Form):
    username = forms.CharField(label='用户名',
                               max_length=8,
                               min_length=3,
                               error_messages={
                                   'min_length': '用户名最小3位',
                                   'max_length': '用户名最大8位',
                                   'required': '用户不能为空',
                               },
                               widget=forms.widgets.TextInput(attrs={'class': 'form_control'})
                               )
    password = forms.CharField(label='密码',
                               max_length=8,
                               min_length=3,
                               error_messages={
                                   'min_length': '密码最小3位',
                                   'max_length': '密码最大8位',
                                   'required': '密码不能为空',
                               },
                               widget=forms.widgets.PasswordInput(attrs={'class': 'form_control'})
                               )
    confirm_password = forms.CharField(label='确认密码',
                                       max_length=8,
                                       min_length=3,
                                       error_messages={
                                           'min_length': '确认密码最小3位',
                                           'max_length': '确认密码最大8位',
                                           'required': '确认密码不能为空',
                                       },
                                       widget=forms.widgets.PasswordInput(attrs={'class': 'form_control'})
                                       )
    email = forms.EmailField(label='邮箱',
                             error_messages={
                                 'required': '用户不能为空',
                                 'invalid': '邮箱格式不正确',
                             },
                             widget=forms.widgets.EmailInput(attrs={'class': 'from-control'})
                             )

    # 钩子函数
    # 局部钩子：校验用户是否已经存在
    def clean_username(self):
        username = self.cleaned_data.get('username')
        # 去数据库中校验
        is_exist = models.UserInfo.objects.filter(username=username)
        if is_exist:
            # 提示信息:
            self.add_error('username', '用户名已存在')
        return username

    # 全局钩子:校验两次密码是否一致
    def clean(self):
        password = self.cleaned_data.get('password')
        confirm_password = self.cleaned_data.get('confirm_password')
        if not password == confirm_password:
            self.add_error('confirm_password', '两次密码不一致')

        return self.cleaned_data
```

**前端**

```html
<!--register.html---->

<body>
<div class="container-fluid">
    <div class="row">
        <div class="col-md-8 col-md-offset-2">
            <h1 class="text-center">注册</h1>
            <form id="myform">  <!-----这里不用form表单提交数据，只是单纯的用已给form标签而已--->
                {% csrf_token %}
                {% for form in form_obj %}
                    <dvi class="form-group">
                        <label for="{{ form.auto_id }}">{{ form.label }}</label>
                        {{ form }}
                        <span style="color: red" class="pull-right">{{ form.errors.0 }}</span>
                    </dvi>
                {% endfor %}

                <div class="form-group">
                    <label for="myfile">头像
                        <img src="{% static 'imgs/default.png' %}" id="myimg" alt="" width='80'
                             style="margin-top: 20px;margin-left: 10px">
                    </label>
                    <input type="file" id="myfile" name="avatar" style="display: none">
                </div>

                <input type="button" class="btn btn-primary pull-right" value="注册" id="id_commit">

            </form>

        </div>

    </div>
</div>

<script>
    $('#myfile').change(function () {
        //文件阅读器对象
        //1.先生成一个文件对象
        let myFileReaderObj = new FileReader();
        //2. 获取用户上传的头像文件
        let fileObj = $(this)[0].files[0];
        //3. 将文件对象交给阅读器对象读取
        myFileReaderObj.readAsDataURL(fileObj)  //异步操作 IO操作
        //4.利用文件阅读器将文件展示到前端页面  修改src属性
        //等待文件阅读器加载完毕后再执行
        myFileReaderObj.onload = function () {
            $('#myimg').attr('src', myFileReaderObj.result)
        }

    })
    $('#id_commit').click(function () {
        //发送ajax，但是我们发送的数据中，既包含普通的简直，也包含文件
        let formDataObj = new FormData()
        //1.添加普通键值对
        {#console.log($('#myform').serializeArray()) #}
        $.each($('#myform').serializeArray(), function (index, obj) {
            formDataObj.append(obj.name, obj.value)
        })
        //2.添加文件对象
        formDataObj.append('avatar', $('#myfile')[0].files[0])
        //3.发送ajax请求
        $.ajax({
            url: '',
            type: 'post',
            data: formDataObj,
            //需要指定两个关键性参数
            contentType: false,
            processData: false,
            success: function (args) {
                if (args.code == 1000) {
                    //跳转登录页面
                    window.location.href = args.url
                } else {
                    //将对应的错误提示展示到对应的input框下面
                    $.each(args.msg, function (index, obj) {
                        let targetId = '#id_' + index;
                        $(targetId).next().text(obj[0]).parent().addClass('has-error')
                    })
                }
            }

        })
    })
    //给所有的input框绑定获取焦点事件
    $('input').focus(function (){
        //将input框下面的span标签和外面的div标签修改内容及属性
        $(this).next().text('').parent().removeClass('has-error')
    })
</script>

</body>
```

**后端**

```python
#urls.py
path('register/', views.register, name='register'),

#views.py
def register(request):
    # 生成一个空对象
    form_obj = MyRegForm()
    if request.method == 'POST':
        back_dic = {'code': 1000, 'msg': ''}
        # 校验数据是否合法
        form_obj = MyRegForm(request.POST)
        # 判断数据是否合法
        if form_obj.is_valid():

            # print(form_obj.cleaned_data) #4个键值，多传的键值不要
            clean_data = form_obj.cleaned_data  # 将校验通过的数据字典赋值给变量clean_data
            # 将字典里面的confirm_password键值对删除
            clean_data.pop('confirm_password')
            # 用户头像
            file_obj = request.FILES.get('avatar')
            """针对用户头像一定要判断是否传值，不能直接添加到字段里去"""
            if file_obj:
                clean_data['avatar'] = file_obj
            # 直接操作数据库保存数据
            models.UserInfo.objects.create_user(**clean_data)
            back_dic['url'] = '/login/'
        else:
            back_dic['code'] = 2000
            back_dic['msg'] = form_obj.errors
        return JsonResponse(back_dic)

    return render(request, 'register.html', locals())
```

### 5、登录功能

实现图片验证码

ajax

```python
"""
img标签的src属性
	1.图片路径
	2.url后缀（自动朝该url发送get请求获取数据）
	3.图片的二进制数据
		
我们的计算机上面之所以能输出各式各样的字体样式，内部其实对应的是一个个.ttf结尾的文件

1.借助于pillow模块
	Image ImageDraw ImageFont
2.需要借助于内存管理器io模块
	BytesIo StringIo
3.字体样式  .ttf文件
4.手动产生随机验证码  （搜狗笔试题）
	random模块
	chr内置方法
5.验证码校验
	
"""
```

**前端**

```html
{% load static %}
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!---<script src="../jQuery-3.6.0-min.js"></script>--->
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
    
    <link rel="stylesheet" href="{% static 'bootstrap-3.4.1-dist/css/bootstrap.min.css' %}">
    <script src="{% static 'bootstrap-3.4.1-dist/js/bootstrap.min.js' %}"></script>
</head>
<body>
<div class="container-fluid">
    <div class="row">
        <div class="col-md-offset-2 col-md-8">
            <h1 class="text-center">登录</h1>
            <div class="form-group">
                <label for="username">用户名</label>
                <input type="text" name="username" id="username" class="form-control">
            </div>
            <div class="form-group">
                <label for="password">密码</label>
                <input type="password" name="password" class="form-control" id="password">
            </div>
            <div class="form-group">
                <label for="">验证码</label>
                <div class="row">
                    <div class="col-md-6">
                        <input type="text" name="code" id="id_code" class="form-control">
                    </div>
                    <div class="col-md-6">
                        <img src="{% url 'code' %}" alt="" width="450" height="35" id="id_img">
                    </div>
                </div>

            </div>
            <input type="button" class="btn btn-success" value="登录" id="id_commit">
            <span style="color: red" id="error"></span>
        </div>
    </div>
</div>

<script>
    $('#id_img').click(function () {
        //1.获取标签之前的src
        let oldVal = $(this).attr('src');
        $(this).attr('src', oldVal += '?')
    })
    //点击按钮，发送ajax请求
    $('#id_commit').on('click', function () {
        $.ajax({
            url: '',
            type: 'post',
            data: {
                'username': $('#username').val(),
                'password': $('#password').val(),
                'code': $('#id_code').val(),
                'csrfmiddlewaretoken':'{{ csrf_token }}',
            },
            success: function (args) {
                if (args.code == 1000) {
                    //跳转到首页
                    window.location.href = args.url
                } else {
                    //渲染错误信息
                    $('#error').text(args.msg)

                }
            }

        })
    })
</script>
</body>
</html>
```

**后端**

```python
#urls.py
path('login/', views.login, name='login'),
# 图片验证码相关
path('get_code/', views.get_code, name='code'),

#views.py
def login(request):
    if request.method == 'POST':
        back_dic = {'code': 1000, 'msg': ''}
        username = request.POST.get('username')
        password = request.POST.get('password')
        code = request.POST.get('code')
        # 1. 验证验证码是否正确   忽略大小写(通过转成大写或者小写进行比较)
        if request.session.get('code').upper() == code.upper():
            # 2. 校验用户名和密码是否正确
            user_obj = auth.authenticate(request, username=username, password=password)
            if user_obj:
                # 保存用户状态
                auth.login(request, user_obj)
                back_dic['url'] = '/home/'
            else:
                back_dic['code'] = 2000
                back_dic['msg'] = '用户名或者密码错误'
        else:
            back_dic['code'] = 3000
            back_dic['msg'] = '验证码错误'
        return JsonResponse(back_dic)
    return render(request, 'login.html')


"""
图片相关模块
pip install pillow
"""
from PIL import Image, ImageDraw, ImageFont

"""
 Image：生成图片
 ImageDraw：能够在图片上乱涂乱画
 ImageFont：控制字体样式
"""
import random
from io import BytesIO, StringIO

"""
内存管理模块
BytesIO,:临时帮你存储数据，返回的时候数据是二进制格式
StringIO：临时帮你存储数据，返回的时候数据是字符串格式
"""


def get_random():
    return random.randint(0, 255), random.randint(0, 255), random.randint(0, 255)


def get_code(request):
    """推导数据一：直接获取后端现成的图片二进制数据发送给前端"""

    # with open(r'static/imgs/title.png', 'rb') as f:
    #     data = f.read()
    # return HttpResponse(data)

    """推导步骤2：利用pillow模块，动态产生图片"""
    # # img_obj = Image.new(mode='RGB', size=(430, 35), color='green')
    # # img_obj = Image.new(mode='RGB', size=(430, 35), color=(123, 23, 12))
    # img_obj = Image.new(mode='RGB', size=(430, 35), color=get_random())
    # # 先将图片对象保存起来
    # with open('xxx.png', 'wb') as f:
    #     img_obj.save(f, 'png')
    # # 再将图片对象读取出来
    # with open('xxx.png', 'rb') as f:
    #     data = f.read()
    # return HttpResponse(data)
    """推导步骤3:步骤2文件存储繁琐，IO效率低，借助于内存管理器模块"""
    # img_obj = Image.new(mode='RGB', size=(430, 35), color=get_random())
    # io_obj = BytesIO()  # 生成一个内存管理器对象，可以看成是一个文件句柄
    # img_obj.save(io_obj, 'png')
    # return HttpResponse(io_obj.getvalue())  # 从内存管理器中读取二进制数据返回给前端

    """最终步骤4：写图片验证码"""
    img_obj = Image.new(mode='RGB', size=(430, 35), color=get_random())
    img_draw = ImageDraw.Draw(img_obj)  # 产生一个画笔对象
    img_font = ImageFont.truetype('static/font/22.ttf', 30)  # 字体样式和大小

    # 随机验证码 五位数的随机验证码（数字，小写字母，大写字母）
    code = ''
    for i in range(5):
        random_upper = chr(random.randint(65, 90))  # chr 把数字转成对应的ascii对应的字符
        random_lower = chr(random.randint(97, 120))
        random_int = str(random.randint(0, 9))
        # 从上面三个里随机选一个
        tmp = random.choice([random_lower, random_int, random_upper])
        # 将产生的随机字符串写入到图片上
        """为什么一个个写而不是生成好了之后再写
        因为一个个写能控制每个字体之间的间隙，而生成好的没法控制间隙
        """
        img_draw.text((i * 50 + 90, -1), tmp, get_random(), img_font)
        # 拼接随机i字符串
        code += tmp
    print(code)
    # 随机验证码在登录的视图函数里要用到，要比对，所以要存起来，其他视图函数也能拿到
    request.session['code'] = code
    io_obj = BytesIO()
    img_obj.save(io_obj, 'png')
    return HttpResponse(io_obj.getvalue())
```

### 6、搭建BBS首页

**动态展示用户名称**

导航条根据用户是否登录展示不同的内容

```python
当用户登录之后显示用户名和更多操作
当用户没有登录显示注册和登录
```



```html
{% if request.user.is_authenticated %}
    <li><a href="#">{{ request.user.username }}</a></li>
    <li class="dropdown">
        <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
           aria-expanded="false">更多 <span class="caret"></span></a>
        <ul class="dropdown-menu">
            <li><a href="#">修改密码</a></li>
            <li><a href="#">修改头像</a></li>
            <li><a href="#">后台管理</a></li>
            <li role="separator" class="divider"></li>
            <li><a href="#">退出登录</a></li>
        </ul>
    </li>
{% else %}
    <li><a href="{% url 'register' %}">注册</a></li>
    <li><a href="{% url 'login' %}">登录</a></li>
{% endif %}
```

##### **修改密码**

```html
 <li><a href="#" data-toggle="modal" data-target=".bs-example-modal-lg">修改密码</a></li>

{#修改密码弹出框#}
<div class="modal fade bs-example-modal-lg" tabindex="-1" role="dialog"
     aria-labelledby="myLargeModalLabel">
    <div class="modal-dialog modal-lg" role="document">
        <div class="modal-content">
            <h1 class="text-center">修改密码</h1>
            <div class="row">
                <div class="col-md-offset-2 col-md-8">
                    <div class="form-group">
                        <label for="">用户名</label>
                        <input type="text" disabled value="{{ request.user.username }}"
                               class="form-control">
                    </div>
                    <div class="form-group">
                        <label for="">原密码</label>
                        <input type="password" id="id_old_pwd" class="form-control">
                    </div>
                    <div class="form-group">
                        <label for="">新密码</label>
                        <input type="password" id="id_new_pwd" class="form-control">
                    </div>
                    <div class="form-group">
                        <label for="">确认密码</label>
                        <input type="password" id="id_confirm_pwd" class="form-control">
                    </div>
                    <div class="modal-footer">
                        <button type="button" id="id_edit" class="btn btn-default"
                                data-dismiss="modal">
                            取消
                        </button>
                        <button type="button" class="btn btn-primary">保存</button>
                        <span style="color: red" id="pwd_error"></span>
                    </div>
                    <br>
                    <br>
                </div>
            </div>
        </div>
    </div>
</div>



  $('#id_edit').on('click', function () {
        $.ajax({
            url: '/set_pwd/',
            type: 'post',
            data: {
                'old_pwd': $('#id_old_pwd').val(),
                'new_pwd': $('#id_new_pwd').val(),
                'confirm_pwd': $('#id_confirm_pwd').val(),
                'csrfmiddlewaretoken': '{{ csrf_token }}'
            },
            success: function (args) {
                if (args.code == 1000) {
                    {#window.location.href='/login/'#}
                    window.location.reload() //修改成功后页面刷新
                } else {
                    $('#pwd_error').text(args.msg)

                }
            }
        })
    })
```

```python
from django.contrib.auth.decorators import login_required

@login_required
def set_pwd(request):
    back_dic = {'code': 1000, 'msg': ''}
    if request.is_ajax():
        if request.method == 'POST':
            old_pwd = request.POST.get('old_pwd')
            new_pwd = request.POST.get('new_pwd')
            confirm_pwd = request.POST.get('confirm_pwd')
            is_right = request.user.check_password(old_pwd)
            if is_right:
                if new_pwd == confirm_pwd:
                    request.user.set_password(new_pwd)
                    request.user.save()
                    back_dic['msg'] = '修改成功'
                else:
                    back_dic['code'] = 1001
                    back_dic['msg'] = '两次密码不一致'
            else:
                back_dic['code'] = 1002
                back_dic['msg'] = '原密码错误'
        return JsonResponse(back_dic)

    return HttpResponse('OK')
```

##### **退出登录**

```python
<li><a href="{% url 'logout' %}">退出登录</a></li>


#urls.py
path('logout/',views.logout,name='logout')
#views.py
@login_required
def logout(request):
    auth.logout(request)
    return redirect('home')
```

##### **admin后台管理**

```python
"""django提供了一个可视化的界面，方便对模型表进行增删改查操作

如果想要使用admin后台管理，需要先注册模型表
告诉admin需要操作哪些表
去应用下的admin.py种注册你的模型表
"""
```

```python
#注册模型表
from django.contrib import admin
from App import models

# Register your models here.
admin.site.register(models.UserInfo)
admin.site.register(models.Blog)
admin.site.register(models.Category)
admin.site.register(models.Tag)
admin.site.register(models.Article)
admin.site.register(models.ArticleToTag)
admin.site.register(models.UpAndDown)
admin.site.register(models.Comment)
```

```python
#admin会给每一个注册了的模型表自动生成增删改查四条url
拿用户表为例:
http://127.0.0.1:8000/admin/App/userinfo/   查
http://127.0.0.1:8000/admin/App/userinfo/add/     增
http://127.0.0.1:8000/admin/App/userinfo/1/change/    改
http://127.0.0.1:8000/admin/App/userinfo/1/delete/    删

个人站点表
http://127.0.0.1:8000/admin/App/blog/   查
http://127.0.0.1:8000/admin/App/blog/add/     增
http://127.0.0.1:8000/admin/App/blog/1/change/    改
http://127.0.0.1:8000/admin/App/blog/1/delete/    删

"""
关键点就在于urls.py中自带的第一条url
	path('admin/', admin.site.urls),

前期需要自己手动苦逼的录入数据
"""

#1.数据绑定尤其要注意用户和个人站点不要忘记绑定

#2.标签

#3.标签和文章
	千万不要把别人的文章绑定标签
```



![image-20221120210742677](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221210162428.png)

要想展示中文，可以在models.py中给每个表加上

```python
class Meta:
    verbose_name_plural = '自定义名称'  # 用来修改admin后台管理默认的表名
```

![image-20221120211249472](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221212121805-1.png)



![image-20221120211305579](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221212121805-2.png)

**admin路由分发的本质**

```python
# 路由分发的本质 include   可以无限制的嵌套n多层 path('index/',([],None,None))
 #列表里可以无限制的嵌套   
    path('index/', ([
        path('index_1/', ([
            path('index_1_1/', index),
            path('index_1_2/', index)
        ], None, None)),
        path('index_2/', index)
    ], None, None)),

#views,py
from django.shortcuts import HttpResponse

def index(request):
    return HttpResponse('index')    
    
```

![image-20221121144353582](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221212121805-3.png)

##### **media配置及用户头像展示**

```python
"""
1.网址所使用的静态文件默认放在static文件夹下
2.用户上传的静态文件也应该单独放在某个文件夹下	
"""
```

**media配置**：该设置可以让用户上传的所有文件都固定存放在某一个指定的文件夹下

```python
网站所使用 的静态资源默认都放在static文件夹下

#settings.py中
# 配置用户上传的文件存储位置
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')#文件名随意
会自动创建多级目录
```

![image-20221121092212547](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221212121759.png)

**如何开设后端指定文件夹资源**

```python
# 暴露后端指定文件夹资源
# 在暴露资源的时候，一定要明确该资源是否可以暴露
from django.views.static import serve
from BBS import settings  #项目下的settings.py文件
#固定代码
re_path(r'media/(?P<path>.*)', serve, {'document_root': settings.MEDIA_ROOT})
```

**头像展示**

```html
<img class="media-object" src="/media/{{ article_obj.blog.userinfo.avatar }}" alt="..."
                                     width="60">
```

**图片防盗链**

```python
#如何避免别的网站直接通过本网站的url访问本网站的资源
#简单的防盗链
 可以做到请求来的时候先看看当前请求是从哪个网站过来的
 如果是本网站那么正常访问
如果是其他网站直接拒绝
	请求头里有一个专门记录请求来自于哪个网址的参数
    	Referer
#如何避免
	1.直接修改请求头referer
    2.直接写爬虫把对方网站的所有资源下载到我们自己的服务器上
```

##### 首页

```python
<div class="col-md-8">
    <ul class="media-list">
        {% for article_obj in aritcle_queryset %}
            <li class="media">
                <h4 class="media-heading"><a href="#">{{ article_obj.title }}</a></h4>
                <div class="media-left">
                    <a href="#">
                        <img class="media-object" src="/media/{{ article_obj.blog.userinfo.avatar }}" alt="..."
                             width="60">
                    </a>
                </div>
                <div class="media-body">
                    {{ article_obj.desc }}
                </div>
                <br>
                <div>
                    <span><a href="/{{ article_obj.blog.userinfo.username }}/">{{ article_obj.blog.userinfo.username }}&nbsp;&nbsp;</a></span>
                    <span>发布于&nbsp;&nbsp;</span>
                    <span>{{ article_obj.create_time|date:'Y-m-d' }}&nbsp;&nbsp;</span>
                    <span><span
                            class="glyphicon glyphicon-comment"></span>评论({{ article_obj.comment_num }})&nbsp;&nbsp;</span>
                    <span><span
                            class="glyphicon glyphicon-thumbs-up"></span>点赞({{ article_obj.up_num }})&nbsp;&nbsp;</span>
                </div>
            </li>
            <hr>
        {% endfor %}

    </ul>
</div>
```

```python
# 首页
path('home/', views.home, name='home'),

def home(request):
    # 查询本网站所有的文章数据展示的前端页面，这里可以使用分页器做分页，
    aritcle_queryset = models.Article.objects.all()
    return render(request, 'home.html', locals())
```

##### 个人站点

```python
#诠释每个用户都可以有自己的站点样式
 <link rel="stylesheet" href="/media/css/{{ blog.site_theme }}">
  内部给每个人开设自定义的css样式和js文件接口，并且用户自定义后会将用户的文件保存下来，之后再打开用户界面的时候会自动加载用户自己写的css和js文件从而实现每个用户界面不一样的情况
"""
django官网提供的一个orm语法

date_list = models.Article.objects.filter(blog=blog).annotate(month=TruncMonth('create_time')).values(
        'month').annotate(count_number=Count('pk')).values_list('month', 'count_number')
"""

#如果出现日期报错的情况，去settings.py中修改以下代码
TIME_ZONE = 'Asia/Shanghai'
USE_TZ = False


侧边栏筛选功能
	1.多个url公用一个视图函数
    2.当多个url公用一个视图函数的时候，应该思考能否优化
    #re_path(r'^(?P<username>\w+)/category/(\d+)/', views.site),
    # re_path(r'^(?P<username>\w+)/tag/(\d+)/', views.site),
    # re_path(r'^(?P<username>\w+)/archive/(\w+)/', views.site),
    #  侧边栏筛选功能 上面的三条url合并成一条
    re_path(r'^(?P<username>\w+)/(?P<condition>category|tag|archive)/(?P<param>.*)/', views.site)
```

**侧边栏筛选功能**

```html
<div class="container-fluid">
    <div class="row">
        <div class="col-md-3">
            <div class="panel panel-primary">
                <div class="panel-heading">
                    <h3 class="panel-title">文章分类</h3>
                </div>
                <div class="panel-body">
                    {% for category in category_list %}
                        <p><a href="/{{ username }}/category/{{ category.2 }}">{{ category.0 }}({{ category.1 }})</a>
                        </p>

                    {% endfor %}

                </div>
            </div>
            <div class="panel panel-danger">
                <div class="panel-heading">
                    <h3 class="panel-title">文章标签</h3>
                </div>
                <div class="panel-body">
                    {% for tag in tag_list %}
                        <p><a href="/{{ username }}/tag/{{ tag.2 }}">{{ tag.0 }}({{ tag.1 }})</a></p>
                    {% endfor %}
                </div>
            </div>
            <div class="panel panel-info">
                <div class="panel-heading">
                    <h3 class="panel-title">日期归档</h3>
                </div>
                <div class="panel-body">
                    {% for date in date_list %}
                        <p>
                            <a href="/{{ username }}/archive/{{ date.0|date:'Y-m' }}">{{ date.0|date:'Y年m月' }}({{ date.1 }})</a>
                        </p>
                    {% endfor %}
                </div>
            </div>
        </div>
        <div class="col-md-9">
            <ul class="media-list">
                {% for article_obj in article_lists %}
                    <li class="media">
                        <h4 class="media-heading"><a href="#">{{ article_obj.title }}</a></h4>
                        <div class="media-left">
                            <a href="#">
                                <img class="media-object" src="/media/{{ article_obj.blog.userinfo.avatar }}" alt="..."
                                     width="60">
                            </a>
                        </div>
                        <div class="media-body">
                            {{ article_obj.desc }}
                        </div>

                        <div class="pull-right">
                            <span>posted&nbsp;</span>
                            <span>&nbsp;&nbsp;</span>
                            <span>{{ article_obj.create_time|date:'Y-m-d' }}&nbsp;&nbsp;</span>
                            <span>{{ article_obj.blog.userinfo.username }}&nbsp;&nbsp;</span>
                            <span><span
                                    class="glyphicon glyphicon-comment"></span>评论({{ article_obj.comment_num }})&nbsp;&nbsp;</span>
                            <span><span
                                    class="glyphicon glyphicon-thumbs-up"></span>点赞({{ article_obj.up_num }})&nbsp;&nbsp;</span>
                            <span><a href="">编辑</a></span>
                        </div>
                    </li>
                    <hr>
                {% endfor %}

            </ul>
        </div>
    </div>
</div>
```

```python
re_path(r'^(?P<username>\w+)/(?P<condition>category|tag|archive)/(?P<param>.*)/', views.site)
```

```python
from django.db.models import Count
from django.db.models.functions import TruncMonth

def site(request, username, **kwargs):
    """

    :param request:
    :param username:
    :param kwargs: 如果该参数 有值，就需要对article_list做额外的筛选工作
    :return:
    """
    # 先校验当前用户名对应的个人站点是否存在
    user_obj = models.UserInfo.objects.filter(username=username).first()
    # 用户不存在返回一个404页面
    if not user_obj:
        return render(request, 'error.html')
    blog = user_obj.blog
    # 查询当前站点下是所有文章
    article_lists = models.Article.objects.filter(blog=blog)
    if kwargs:
        condition = kwargs.get('condition')
        param = kwargs.get('param')
        # 判断用户想要按照哪个条件筛选
        if condition == 'category':
            article_lists = article_lists.filter(category_id=param)
        elif condition == 'tag':
            article_lists = article_lists.filter(tags__id=param)
        else:
            year, month = param.split('-')

            article_lists = article_lists.filter(create_time__year=year, create_time__month=month)

    # 1.查询当前用户所有的分类及分类下的文章数
    category_list = models.Category.objects.filter(blog=blog).annotate(count_number=Count('article__pk')).values_list(
        'name', 'count_number','pk')
    # 2.查询当前所有用户的标签及标签下的标签数
    tag_list = models.Tag.objects.filter(blog=blog).annotate(count_number=Count('article__pk')).values_list('name',
                                                                                                            'count_number','pk')
    # 3.按照年月统计所有的文章
    date_list = models.Article.objects.filter(blog=blog).annotate(month=TruncMonth('create_time')).values(
        'month').annotate(count_number=Count('pk')).values_list('month', 'count_number')
    # print(data_list)
    return render(request, 'site.html', locals())

```

#### 7、文章详情页

```python
#url设计
/username/article/1

#先验证url是否会被顶替

#文章详情页和个人站点基本一致

#侧边栏的渲染需要传入数据，才能渲染，并且该侧边栏再很多个页面都需要使用
	1.那个地方用，就考呗代码
    2.将侧边栏制作成inclusion_tag
    """
    三步骤:
    	1.应用下名字必须为templatetags文件夹
    	2.该文件夹下创建任意名字的py文件
    	3.再该py文件内先固定书写两行代码
    		from django import template
    		register=template.Labrary()
    		#自定义过滤器
    		#定义标签
    		#定义inclusion_tag
    """
    
```

**inclusion_tag**

```python
#mytag.py
from django import template
from App import models
from django.db.models import Count
from django.db.models.functions import TruncMonth

register = template.Library()


# 自定义inclusion_tag
@register.inclusion_tag('left_menu.html')
def left_menu(username):
    user_obj = models.UserInfo.objects.filter(username=username).first()
    blog = user_obj.blog

    # 构造侧边栏需要的数据
    # 1.查询当前用户所有的分类及分类下的文章数
    category_list = models.Category.objects.filter(blog=blog).annotate(count_number=Count('article__pk')).values_list(
        'name', 'count_number', 'pk')
    # 2.查询当前所有用户的标签及标签下的标签数
    tag_list = models.Tag.objects.filter(blog=blog).annotate(count_number=Count('article__pk')).values_list('name',
                                                                                                            'count_number',
                                                                                                            'pk')
    # 3.按照年月统计所有的文章
    date_list = models.Article.objects.filter(blog=blog).annotate(month=TruncMonth('create_time')).values(
        'month').annotate(count_number=Count('pk')).values_list('month', 'count_number')
    # print(data_list)
    return locals()
```

```html
{% load mytag %}
 {% left_menu username %}
```

**后端**

```python
def article_detail(request, username, article_id):
    user_obj = models.UserInfo.objects.filter(username=username).first()
    blog = user_obj.blog
    # 先获取文章对象，
    article_obj = models.Article.objects.filter(pk=article_id, blog__userinfo__username=username).first()

    if not article_obj:
        return render(request, 'error.html')
    # 获取当前文章所有的内容
    comment_list = models.Comment.objects.filter(article=article_obj)

    return render(request, 'article_detail.html', locals())
```

#### 8、文章点赞点踩

```python
"""
浏览器上看到的华丽花哨的内容，内部都是HTML(前端)代码

如何拷贝文章
	copy outerhtml
拷贝点赞点踩
	1.拷贝前端点赞点踩图标，html代码
	2.css样式也得拷
		由于有图片防盗链，所以将图片直接下载到本地
如何区分用户是点赞了还是点踩了
	1.给标签各自绑定一个事件
		两个标签对应的 代码基本一致，仅仅是 是否点赞点踩这一个参数不一致
	2.二合一
		给两个标签绑定一个事件
		 //给所有action类绑定事件
		   $('.action').click(function () {
                alert($(this).hasClass('diggit'))
            })
            
            
由于点赞点踩内部有一定的业务逻辑，所以后端单独开设视图函数处理
"""



# 前端页面
	1.拷贝博客园点赞点踩前端样式
        html代码 + css代码
    2.如何区分用户是点赞了还是点踩了
    	恰巧页面上有两个图标，所以给两个图标添加一个公共样式类
        然后给这个样式类绑定点击事件
        再利用this指代的就是当前被操作对象，利用hasClass判断是否有某个特定的类属性，从而判断出到底是两个图标中的哪一个
    
    3.书写ajax代码朝后端提交数据
    
    4.后端逻辑书写完毕后，前端针对点赞点踩动作实现需要动态展示提示信息
    	1.前端点赞点踩数字自增1，需要注意数据类型的问题
        	Number(old_num)+1
        2.用户没有登录，需要展示登录提示，并且登录可以条状 
        html()
        |safe()
        mark_safe()
#后端逻辑
	1.先判断用户是否登录
  	  request.user.authenticated()
    2.再判断当前文字是都是当前用户自己写的
    	通过文章主键值获取文章对象
        之后利用orm查询获取文章对象对应的用户对象与request.user比对
    3.判断当前用户是否已经给当前文章点了
    	利用article_obj文章对象和request.user用户对象去点赞点菜表中筛选数据，如果有数据则点过，没有则没点
    4.操作数据库，同时操作两张表
     	#前端发过来的是否点赞是一个字符串，需要自己转成布尔值活着利用字符串判断
       is_up=json.loads('is_up')
   		F查询
 """
 总结:再书写较为复杂的业务逻辑的时候，可以先按照一条线书写下去，之后再去弥补其他线路情况
 	类似于先走树的主干，之后再走分支
 
 """       
        
        
    
   
```



**前端**

```html
{#    点赞点踩图标样式开始#}
<div class="clearfix">
    <div id="div_digg">
        <div class="diggit action">
            <span class="diggnum " id="digg_count">{{ article_obj.up_num }}</span>
        </div>
        <div class="buryit action">
            <span class="burynum" id="bury_count">{{ article_obj.down_num }}</span>
        </div>
        <div class="clear"></div>
        <div class="diggword" id="digg_tips" style="color: red">
        </div>
    </div>
</div>
{#    点赞点踩图标样式结束#}
```

```js
//给所有action类绑定事件
$('.action').click(function () {
    let isUp = $(this).hasClass('diggit');
    let $div = $(this);
    //朝后端发送ajax
    $.ajax({
        url: '/up_or_down/',
        type: 'post',
        data: {
            'article_id': '{{ article_obj.pk }}',
            'is_up': isUp,
            'csrfmiddlewaretoken': '{{ csrf_token }}'
        },
        success: function (args) {
            if (args.code == 1000) {
                $('#digg_tips').text(args.msg)

                //将前端的数字加1
                let oldNum = $div.children().text();//文本是字符类型
                {#$btn.children().text(oldNum + 1 ) //字符串拼接#}
                $div.children().text(Number(oldNum) + 1) //字符串拼接

            } else {
                $('#digg_tips').html(args.msg)

            }
        }
    })
})
```

**后端**

```python
def up_or_down(request):
    """
    1.校验用户是否登录
    2.判断当前是否是当前用户自己写的，自己不能给自己 点赞
    3.当前用户是否已经给当前用户点过了
    4.操作数据库
    :param request:
    :return:
    """
    if request.is_ajax():
        back_dic = {'code': 1000, 'msg': ''}
        # 1.判断当前用户是否登录
        if request.user.is_authenticated:
            article_id = request.POST.get('article_id')
            is_up = request.POST.get('is_up')
            # print(is_up,type(is_up)) true <class 'str'>
            is_up = json.loads(is_up)  # 把json格式的字符串true,转成python的布尔值true
            # 2.判断当前文章是否是当前用户自己写的  根据文章id查询文章对象，根据文章对象查作者，跟request.user比对
            article_obj = models.Article.objects.filter(pk=article_id).first()
            if not article_obj.blog.userinfo == request.user:
                # 3.校验当前用户是否已经点赞过了
                is_click = models.UpAndDown.objects.filter(user=request.user, article=article_obj)
                if not is_click:
                    # 4.操作数据库   同步操作普通字段
                    # 判断当前用户点了赞还是点了踩，从而决定给哪个字段+1
                    if is_up:
                        # 给点赞数加1
                        models.Article.objects.filter(pk=article_id).update(up_num=F('up_num') + 1)
                        back_dic['msg'] = '点赞成功!!'
                    else:
                        # 给点踩数加1
                        models.Article.objects.filter(pk=article_id).update(down_num=F('down_num') + 1)
                        back_dic['msg'] = '点踩成功!!'
                    # 操作点赞点踩表
                    models.UpAndDown.objects.create(user=request.user, article=article_obj, is_up=is_up)
                else:
                    back_dic['code'] = 1001
                    back_dic['msg'] = '你已经点过了，不能再点了！'
            else:
                back_dic['code'] = 1002
                back_dic['msg'] = '自己不能给自己点'
        else:
            back_dic['code'] = 1003
            back_dic['msg'] = '请先<a href="/login/">登录</a>'

        return JsonResponse(back_dic)
```

#### 9、文章评论

```python
#根评论
    先把整体的评论功能跑通，再去填补
    1.书写前端获取用户评论的标签
        可能点赞点踩有浮动带啦的影响
            clearfix
  	2.点击评论按钮发送Ajax请求
    
    3.后端对评论单独开设url，处理
    后端逻辑其实非常的简单非常少
    
    4.针对根评论设计到前端的两种渲染方式
    	1.DOM操作临时渲染评论楼
        	需要用到模板字符
             //临时渲染评论列表
                        let userName = '{{ request.user.username }}';
                        let temp = `
                        <li class="list-group-item">
                            <span>${username}</span>
                            <span><a href="" class="pull-right">回复</a></span>
                            <div>
                               ${conTent}
                            </div>
                        </li>`
         2.页面刷新永久(render)渲染
        	后端直接获取当前文章所有评论，传递给html页面即可
            前端利用for循环参考博客园样式渲染评论
            
       	 3.	评论框里面的内容需要清空
#子评论
从回复按钮入手
    点击回复按钮发生了那些事：
    1.评论框自动聚焦.focus()
    2.评论框里自动添加对应评论的评论人
    	@username\n
    思考：根评论和子评论点的是同一个按钮
    	根评论和子评论的区别:
            其实之前的ajax代码只需要添加一个父评论id即可
    点击恢复按钮之后，应该获取到根评论对应的用户名和主键值
    针对主键值，多个函数都需要用，所以利用全局的变量形式存储
    针对子评论内容，需要切割出不是用户写的 @username\n
    
          //获取用户评论内容
            let conTent = $('#id_comment').val()
            //判断当前评论是否是子评论，如果是，需要将
            if (parentId) {
                //先找到\n对应的索引值， 然后利用切片，但是切片顾头不顾尾，所以要索引+1
                let indexNum = conTent.indexOf('\n') + 1;
                conTent = conTent.slice(indexNum);//将indexNum之前的所有数据切除，只保留后面的部分

            }
      后端parent_id字段本来就就可以位空，传不传值都可以直接存储数据
    
   		前端针对子评论渲染评论区的时候需要额外的判断
         <div>
             {#判断当前评论是否是子评论，如果是，要渲染对应的评论人名#}
                 {% if comment.parent_id %}
                 <p>@{{ comment.parent.user.username }}</p>
                 {% endif %}
                 {{ comment.content }}
          </div>
                            
      前端parent_id字段每次提交之后需要手动清空
          //清空全局的parentId
           parentId = null;
                      
```

**前端**

```html
{#    {{ 评论楼渲染开始 }}#}
<div>
    <ul class="list-group">

        {% for comment in comment_list %}
            <li class="list-group-item">
                <span>#{{ forloop.counter }}楼</span>
                <span>{{ comment.comment_time|date:'Y-m-d h:i:s' }}</span>
                <span>{{ comment.user.username }}</span>
                <span><a class="pull-right replay" username="{{ comment.user.username }}"
                         comment_id="{{ comment.pk }}">回复</a></span>

                <div>
                    {#                        判断当前评论是否是子评论，如果是，要渲染对应的评论人名#}
                    {% if comment.parent_id %}
                        <p>@{{ comment.parent.user.username }}</p>
                    {% endif %}
                    {{ comment.content }}
                </div>
            </li>
        {% endfor %}


    </ul>

</div>
{#    {{ 评论楼渲染结束 }}#}



{#    文章评论开始#}
{% if   request.user.is_authenticated %}
    <div>
        <p><span class="glyphicon glyphicon-comment"></span>发表评论</p>
        <div>
            <textarea name="comment" id="id_comment" cols="60" rows="10"></textarea>
        </div>
        <button class="btn btn-primary" id="id_submit">提交评论</button>
        <span style="color: red" id="error"></span>
    </div>
{% else %}
    <li><a href="{% url 'register' %}">注册</a></li>
    <li><a href="{% url 'login' %}">登录</a></li>
{% endif %}
{#    文章评论结束#}
```

```js
        //设置一个全局的parent_id
        let parentId = null
//用户点击评论按钮，发送ajax请求，
$('#id_submit').on('click', function () {
    //获取用户评论内容
    let conTent = $('#id_comment').val()
    //判断当前评论是否是子评论，如果是，需要将
    if (parentId) {
        //先找到\n对应的索引值， 然后利用切片，但是切片顾头不顾尾，所以要索引+1
        let indexNum = conTent.indexOf('\n') + 1;
        conTent = conTent.slice(indexNum);//将indexNum之前的所有数据切除，只保留后面的部分

    }
    $.ajax({
        url: '/comment/',
        type: 'post',
        data: {
            'article_id': '{{ article_obj.pk }}',
            'content': conTent,
            'parent_id': parentId,
            //如果parentId没有值，就是null，后端存储null没有任何问题
            'csrfmiddlewaretoken': '{{ csrf_token }}'
        },
        success: function (args) {
            if (args.code == 1000) {
                $('#error').text(args.msg)
                //评论框里的内容清空
                $('#id_comment').val('')

                //临时渲染评论列表
                let userName = '{{ request.user.username }}';
                let temp = `
                <li class="list-group-item">
                    <span>${username}</span>
                    <span><a href="" class="pull-right">回复</a></span>
                    <div>
                       ${conTent}
                    </div>
                </li>`

                //将生成或的标签添加到url标签内
                $('.list-group').append(temp)
                //清空全局的parentId
                parentId = null;
            }
        }
    })
})




//给回复按钮绑定点击事件
$('.replay').click(function () {
    //需要评论对应的评论人姓名， 还需要评论人的主键值
    //获取用户名，
    let commentUsername = $(this).attr('username');
    //用户主键值 直接修改全局的变量名
    let parentId = $(this).attr('comment_id');
    //拼接信息塞给评论框
    $('#id_comment').val('@' + commentUsername + '\n').focus()
})
```

**后端**

```python
#评论功能
path('comment/',views.comment),
```

```python
def comment(request):
    if request.is_ajax():
        back_dic = {'code': 1000, 'msg': ''}
        if request.method == 'POST':
            if request.user.is_authenticated:
                article_id = request.POST.get('article_id')
                content = request.POST.get('content')
                parent_id=request.POST.get('parent_id')
                # 直接操作评论表存储数据 两张表
                with transaction.atomic():
                    models.Article.objects.filter(pk=article_id).update(comment_num=F('comment_num') + 1)
                    models.Comment.objects.create(user=request.user, article_id=article_id, content=content,parent_id=parent_id)
                back_dic['msg'] = '评论成功'
            else:
                back_dic['code'] = 1001
                back_dic['msg'] = '用户未登录'

        return JsonResponse(back_dic)

```

### 7、后台管理

**前端**



```html
<!---backend/base.html--->

<div class="container-fluid">
    <div class="row">
        <div class="col-md-2">

            <div class="panel-group" id="accordion" role="tablist" aria-multiselectable="true">
                <div class="panel panel-default">
                    <div class="panel-heading" role="tab" id="headingOne">
                        <h4 class="panel-title">
                            <a role="button" data-toggle="collapse" data-parent="#accordion" href="#collapseOne"
                               aria-expanded="true" aria-controls="collapseOne">
                                更多操作
                            </a>
                        </h4>
                    </div>
                    <div id="collapseOne" class="panel-collapse collapse in" role="tabpanel"
                         aria-labelledby="headingOne">
                        <div class="panel-body">
                            <a href="">添加文章</a>
                        </div>
                    </div>
                    <div id="collapseOne" class="panel-collapse collapse in" role="tabpanel"
                         aria-labelledby="headingOne">
                        <div class="panel-body">
                            <a href="">添加随笔</a>
                        </div>
                    </div>
                    <div id="collapseOne" class="panel-collapse collapse in" role="tabpanel"
                         aria-labelledby="headingOne">
                        <div class="panel-body">
                            <a href="">草稿箱</a>
                        </div>
                    </div>
                    <div id="collapseOne" class="panel-collapse collapse in" role="tabpanel"
                         aria-labelledby="headingOne">
                        <div class="panel-body">
                            <a href="">其他</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="col-md-10">

            <div>

                <!-- Nav tabs -->
                <ul class="nav nav-tabs" role="tablist">
                    <li role="presentation" class="active"><a href="#home" aria-controls="home" role="tab"
                                                              data-toggle="tab">文章</a></li>
                    <li role="presentation"><a href="#profile" aria-controls="profile" role="tab"
                                               data-toggle="tab">随笔</a>
                    </li>
                    <li role="presentation"><a href="#messages" aria-controls="messages" role="tab"
                                               data-toggle="tab">草稿</a>
                    <li role="presentation"><a href="#file" aria-controls="file" role="tab"
                                               data-toggle="tab">文件</a>

                    </li>
                    <li role="presentation"><a href="#settings" aria-controls="settings" role="tab"
                                               data-toggle="tab">设置</a>
                    </li>
                </ul>

                <!-- Tab panes -->
                <div class="tab-content">
                    <div role="tabpanel" class="tab-pane active" id="home">
                        {% block article %}

                        {% endblock %}
                    </div>
                    <div role="tabpanel" class="tab-pane" id="profile">随笔页面</div>
                    <div role="tabpanel" class="tab-pane" id="messages">草稿页面</div>
                    <div role="tabpanel" class="tab-pane" id="file">文件页面</div>
                    <div role="tabpanel" class="tab-pane" id="settings">设置页面</div>
                </div>

            </div>


        </div>
    </div>
</div>
</body>
</html>
```

```html
{% extends 'backend/base.html' %}

{% block article %}
    {#    接收当前用户 所有文章#}
    <table class="table-hover table-striped table">
        <thead>
        <tr>
            <th>标题</th>
            <th>评论数</th>
            <th>点赞数</th>
            <th>操作</th>
            <th>操作</th>
        </tr>
        </thead>
        <tbody>
        {% for article in page_queryset %}
            <tr>
                <td><a href="/{{ request.user.username }}/article/{{ article.pk }}">{{ article.title }}</a></td>
                <td>{{ article.comment_num }}</td>
                <td>{{ article.up_num }}</td>
                <td><a href="">编辑</a></td>
                <td><a href="">删除</a></td>
            </tr>
        {% endfor %}
        </tbody>
    </table>
    <div class="pull-right">    {{ page_obj.page_html|safe }}</div>
{% endblock %}
```

**后端**

```python
#后台管理
path('backend/',views.backend),
```

```python
#添加分页器
from utils.mypage import Pagination


@login_required
def backend(request):
    article_list = models.Article.objects.filter(blog=request.user.blog)
    page_obj = Pagination(current_page=request.GET.get('page', 1), all_count=article_list.count())
    page_queryset=article_list[page_obj.start:page_obj.end]
    return render(request, 'backend/backend.html', locals())
```

### 8、文章增查

**前端**

```html
{% extends 'backend/base.html' %}

{% block article %}
    <h3>添加文章</h3>
    {#    直接利用from表单提交数据#}
    <form action="" method="post">
        {% csrf_token %}
        <p>标题</p>
        <div>
            <input type="text" name="title" class="form-control">
        </div>
        <p>内容</p>
        <div>
            <textarea name="content" id="id_content" cols="30" rows="10"></textarea>
        </div>
        <p>分类</p>
        <div>
            {% for category in category_list %}
                <input type="radio" value="{{ category.pk }}" name="category">{{ category.name }}
            {% endfor %}
        </div>
        <p>标签</p>
        <div>
            {% for tag in tag_list %}
                <input type="checkbox" value="{{ tag.pk }}" name="tag">{{ tag.name }}
            {% endfor %}

        </div>
        <input type="submit" class="btn btn-danger">


    </form>
{% endblock %}
```

**前期后端代码**

```python
#添加文章
path('add/article/',views.add_article),
```

```python
def add_article(request):
    category_list = models.Category.objects.filter(blog=request.user.blog)
    tag_list = models.Tag.objects.filter(blog=request.user.blog)

    return render(request, 'backend/add_article.html',locals())
```

**后期后端代码**

```python
@login_required
def add_article(request):
    if request.method == 'POST':
        title = request.POST.get('title')
        content = request.POST.get('content')
        category_id = request.POST.get('category')
        tag_id_list = request.POST.getlist('tag')

        # 文章简介
        # 1.先简单暴力的直接切取content  150个字符
        desc = content[0:150]
        article_obj = models.Article.objects.create(
            title=title,
            content=content,
            desc=desc,
            category_id=category_id,
            blog=request.user.blog,
        )
        # 文章和标签的关系表是我们自己创建的，无法 使用add,set,remoce,clear方法
        # 自己去操作关系表  一次性可能需要创建多条数据， 批量插入数据 bulk_create()
        article_obj_list = []
        for i in tag_id_list:
            tag_article_obj = models.ArticleToTag(article=article_obj, tag_id=i)
            article_obj_list.append(tag_article_obj)
        # 批量插入数据
        models.ArticleToTag.objects.bulk_create(article_obj_list)
        # 跳转到后台管理文件展示页面
        return redirect('/backend/')
    category_list = models.Category.objects.filter(blog=request.user.blog)
    tag_list = models.Tag.objects.filter(blog=request.user.blog)

    return render(request, 'backend/add_article.html', locals())
```

#### 前端编辑器(kindeditor富文本编辑器)

[**KindEditor** 可视化 HTML 编辑器](https://www.oschina.net/p/kindeditor?hmsr=aladdin1e1)

[文档](http://kindeditor.net/doc.php)

```html
{% extends 'backend/base.html' %}

{% block article %}
    <h3>添加文章</h3>
    {#    直接利用from表单提交数据#}
    <form action="" method="post">
        {% csrf_token %}
        <p>标题</p>
        <div>
            <input type="text" name="title" class="form-control">
        </div>
        <p>内容</p>
        <div>
            <textarea name="" id="id_content" cols="30" rows="10"></textarea>
        </div>
        <p>分类</p>
        <div>
            {% for category in category_list %}
                <input type="radio" value="{{ category.pk }}" name="category">{{ category.name }}
            {% endfor %}
        </div>
        <p>标签</p>
        <div>
            {% for tag in tag_list %}
                <input type="checkbox" value="{{ tag.pk }}" name="tag">{{ tag.name }}
            {% endfor %}

        </div>
        <input type="submit" class="btn btn-danger">


    </form>

    {% block js %}
        {% load static %}
        <script charset="utf-8" src="{% static 'kindeditor-master/kindeditor-all-min.js' %}"></script>
        {#        <script charset="utf-8" src="/editor/lang/zh-CN.js"></script>#}
        <script>
            KindEditor.ready(function (K) {
                window.editor = K.create('#id_content', {
                    width: '100%',
                    height: '600px',
                    items: [
                        'source', '|', 'undo', 'redo', '|', 'preview', 'print', 'template', 'code', 'cut', 'copy', 'paste',
                        'plainpaste', 'wordpaste', '|', 'justifyleft', 'justifycenter', 'justifyright',
                        'justifyfull', 'insertorderedlist', 'insertunorderedlist', 'indent', 'outdent', 'subscript',
                        'superscript', 'clearhtml', 'quickformat', 'selectall', '|', 'fullscreen', '/',
                        'formatblock', 'fontname', 'fontsize', '|', 'forecolor', 'hilitecolor', 'bold',
                        'italic', 'underline', 'strikethrough', 'lineheight', 'removeformat', '|', 'image', 'multiimage',
                        'flash', 'media', 'insertfile', 'table', 'hr', 'emoticons', 'baidumap', 'pagebreak',
                        'anchor', 'link', 'unlink', '|', 'about'
                    ],
                    resizeType: 1,
                })
            });
        </script>
    {% endblock %}
{% endblock %}
```

### 10、处理XSS攻击以及文章摘要的处理

```pytohn
针对用户直接编写的html代码的网址
针对用户直接书写script标签，需要处理
	1.注释标签内部的内容
	2.直接将script删除
如何解决
	针对1 后端通过正则表达式筛选
	针对2 首相需要确定及获取script标签
	
	bs4模块，beautifulsoup模块
	专门用来处理html页面
	专门用于爬虫程序
	
```

```python
"""bs4模块使用"""
soup = BeautifulSoup(content, 'html.parser')
# 获取所有的标签
tags = soup.find_all()
for tag in tags:
    # print(tag.name)
    if tag.name == 'script':
        # 删除标签
        tag.decompose()
# 文章简介
# 1.先简单暴力的直接切取content  150个字符
# desc = content[0:150]
# 2.截取文本150个
desc = soup.text[0:150]
```

### 11、编辑器上传图片

```python
"""
别人写好了接口，但是接口不是你写的，
需要自己手动修改
"""
```

### 12、修改用户头像

### 13、总结

```python
#主要功能总结:
	表设计 开发流程
    注册功能
    	forms组件使用
        头像动态展示
        错误信息提示
    登录功能
    	图片验证码
        滑动验证码
    首页展示
    	media配置
        主动暴露任意资源接口
    个人站点展示
    	侧边栏展示
        侧边栏筛选
        侧边栏inclusion_tag
    文章详情页
    	点赞点踩
        评论
    后台管理
"""
针对BBS需要掌握每个功能的书写思路，内部逻辑
只够再去敲代码熟悉，找感觉
"""
```





# ==django-rest-framework==

## （1）Web应用模式

**在Web开发中，有两种模式:**

1. **前后端分离**

---



![5a9c76bb3b243ebb771fe3e606ebc9f7](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130185928.jpg)

```python
#前后端分离:  只专注于后端，返回json格式数据
```



2. **前后端不分离**

---

![b2ab8ef6477b920668424a8071a5b51e](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130185934.jpg)

```python
# 前后端混合开发(前后端不分离)  返回的是HTML的内容,需要写模板

# 动态页面(查数据库)，静态页面（一个静止的html）

# 页面静态化
```

## （2）API接口

通过网络，规定了前后端信息交互的url连接，也就是前后端信息交互的媒介

## （3）Restful规范

```python
REST全称是Representational State Transfer,中文意思是表述（编者注：通常译为表征性状态转移）。它首次出现在2000年Roy Fielding的博士论文中

RESTFUL是一种定义Web API接口的设计风格，尤其适合前后端分离的应用模式中。

这种风格的理念认为后端开发任务就是提供数据的，对外提供的是数据资源访问的接口，所以在定义接口时，客户端访问的URL路径就表示这种需要操作的数据资源

事实上，我们可以使用任何一种框架都可以实现符合RESTFUL规范的API接口
```

1.  数据的安全保障 

```python
url链接一般都采用https协议进行传输，注：采用Https协议，可以提高数据		交互过程中的安全性
```

2. 接口的特征表现

```python
     -用api关键字标识接口url:
            -[https://api.baidu.com](https://api.baidu.com/)
            -https://www.baidu.com/api
        注：看到api字段，就代表该请求url链接是完成前后端数据交互的
```

3. 多版本共存

```python
-在url链接中标识数据版本
-https://api.baidu.com/v1
-https://api.baidu.com/v2
注:url链接中的v1，v2,就是不同数据版本的提现，(只有在一种数据资源有多版本情况下)
```

4. ==**数据即资源**==

```python
接口作为前后端交互的媒介其交互的数据又被叫做资源，推荐使用名词或者名词的复数形式，对于某些特殊接口，我们可以使用动词（api/books api/login）
    	- 接口一般都是前后端数据的交互，交互的数据称之为资源
        	- https://api.baidu.com/users
            - https://api.baidu.com/books
```

5. ==**资源操作由请求方式决定 **==     (method)

```python
资源的操作我们直接通过提交的请求方式来决定(get、post、put/patch、delete)
 - 操作资源一般都涉及到增删改查，以下请求方式来标识增删改查动作
        https://api.baidu.com/books     -get请求：获取所有书
        https://api.baidu.com/books/1   -get请求：获取主键为1 的书
        https://api.baidu.com/books     -post请求: 新增一本书
        https://api.baidu.com/books/1   -put请求:整体修改主键为1的书
        https://api.baidu.com/books/1   -patch请求：局部修改主键为1 的书
        https://api.baidu.com/books/1   -delete请求:删除主键为1 的书
```

6. 过滤

```python
我们可以再url上传参的形式传递搜索条件
    https://api.example.com/v1/people?limit=10  :指定返回记录的数量
    https://api.example.com/v1/people?offset=10 :指定返回记录的开始位置
    https://api.example.com/v1/people?page=2&per_page=100 :指定第几页，以及每页的记录数
    https://api.example.com/v1/people?sortby=name&order=asc:指定返回结果按照哪个属性排序，以及排序顺序
    https://api.example.com/v1/people?animal_type_id=1 :指定筛选条件
```

7. 响应状态码

```python
# 正常响应
    200(正常请求)、
    201(创建成功)、
# 重定向响应
    301(永久重定向)、
    302(临时重定向)、
# 客户端异常
    403(请求无权限)、
    404(请求路径不存在)、
    405(请求方法不存在)、
# 服务端异常
	500(服务器异常)、

```

8. 错误处理

```python
应当返回错误信息   error当作key

{
    error:'无权限操作'
}
```

9. 返回结果

```python
根据不同的请求以及请求得到的数据， 服务端返回不同的结果
	GET /collection:返回资源对象的列表（数组）
    GET /collection/resource:返回单个资源对象
    POST /collection:返回新生成的资源对象
    PUT /collection/resource:返回完整的资源对象
    PATCH /collection/resource:返回完成的资源对象
    DELETE /collection/resource:返回一个空文档
    
```

10. 需要url请求的资源需要访问资源的请求链接

```python
# Hypermedia API ,RESTFUL API最好做到Hypermedia，即返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么
    {
        "status":0,
        "msg":"OK",
        "results":[
            {
                "name":"肯德基",
                "img":"https://image.baidu.com/ftc/001.png"
            }
            ........
        ]  
    }  
```

## （4）drf安装和简单使用

### 1、安装

```python 
#安装
pip install djangorestframework   
```

### ==2、使用==

```python
1. settings.py中
    INSTALLED_APPS = [
        'rest_framework'
    ]
2. 在models.py中写表模型
    class Book(models.Model):
        nid = models.AutoField(primary_key=True)
        name = models.CharField(max_length=32)
        price = models.DecimalField(max_digits=8, decimal_places=2)
        author = models.CharField(max_length=32)
        
3. 新建一个序列化类 (新建一个py文件名字随意)
	from rest_framework.serializers import ModelSerializer
    from App.models import Book


    class BookModelSerializer(ModelSerializer):
        class Meta:
            model = Book
            fields = '__all__'

4. 视图中写视图类（CBV）
	from rest_framework.viewsets import ModelViewSet
    from .models import Book
    from .ser import BookModelSerializer     #ser指的是第三步中创建的py文件

    class BooksViewSet(ModelViewSet):
        queryset = Book.objects.all()
        serializer_class = BookModelSerializer

5. 写路由关系
    from django.contrib import admin
    from django.urls import path
    from rest_framework.routers import DefaultRouter
    from App import views

    router = DefaultRouter()  # 可以处理视图的路由器
    router.register('book', views.BooksViewSet)  # 向路由中注册视图集

    # 将路由器中的所有路由信息追溯到django的路由列表中
    urlpatterns = [
        path('admin/', admin.site.urls),
    ]
    # 两个列表相加
    urlpatterns += router.urls  # router.urls是一个列表

6. 数据迁移
	python manage.py makemigrations
    python manage.py migrate
7. 启动项目，测试
```

**启动项目程序**

![image-20221130190040523](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130190042.png)

![image-20221130190128209](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130190129.png)



![image-20221130190231361](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130190233.png)



![image-20221130190310415](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130190312.png)

### 3、postman测试

在`postman`中测试，`postman`中最后要加`/`，浏览器会自动重定向，但`postman`不会，所以在`postman`中最后要加`/`

* 查数据

![image-20221130190547021](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130190548.png)

![image-20221130190741110](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130190742.png)

* 删数据

![image-20221130190817949](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130190819.png)

将`2`删除后，就找不到数据

![image-20221130190905553](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130190906.png)

删除`2`数据后，再查看所有数据

![image-20221130190931665](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130190933.png)

* 修改数据

![image-20221130191046558](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130192131.png)

修改完后，再次查询所有

![image-20221130191105815](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130191107.png)

* 增加数据

![image-20221130191218113](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130192133.png)

增加后再次查看所有数据

![image-20221130191237150](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130191238.png)

## （5）源码分析

### cbv

```python
ModelViewSet继承与View (djanog原生View)
```

![image-20221130191945881](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221130192132.png)

```python
path('books/',views.Books.as_view())#在这个地方应该写函数内存地址，views.Books.as_view()执行完，是个函数内存地址,as_view是一个类方法，类直接来调用，会把类自动传入
放入了一个view的内存地址，（View--》as_view--》内层函数）

class Books(View):
    # 只能接收get请求
    http_method_names = ['get']
    def get(self, request):
        print(self.request)
        return HttpResponse('ok')
    def post(self,request):
        print(request.POST)
- 视图类里边必须继承View，，
- 在类里写get方法，post方法，只要get请求来了，就走get方法， 跟之前fbv写法完全一样。
- 路由:views.Books.as_view()-------->放入了一个view的内存地址，
- 请求一旦来了，路由匹配上----》view(request)---》self.dispatch(request,*args, **kwargs)
-dispatch---》把请求方法转成小写，----》通过反射，去对象中找，没有get方法，有就加括号执行，并且把request传进去


"""源码分析"""

#请求来了，如果路径匹配，会执行，函数内存地址（request)
def view(request, *args, **kwargs):
    #request是当次请求的request
    self = cls(**initkwargs)
    self.setup(request, *args, **kwargs)
    if not hasattr(self, 'request'): #实例化得到一个对象 Book对象
        raise AttributeError(
            "%s instance has no 'request' attribute. Did you override "
            "setup() and forget to call super()?" % cls.__name__
        )
        return self.dispatch(request, *args, **kwargs)
    
    
    
    
def dispatch(self, request, *args, **kwargs):
    #request就是当次请求的request  selef是Book对象
    if request.method.lower() in self.http_method_names:
        #handle是自己写的Book类的get方法的内存地址
        handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
    else:
        handler = self.http_method_not_allowed
        return handler(request, *args, **kwargs) #执行get(request)   
```



### APIView源码分析(drf提供，扩展了View功能)

```python
- 视图类里边必须继承APIView，，
- 在类里写get方法，post方法，只要get请求来了，就走get方法， 跟之前fbv写法完全一样。
- 路由:views.Books.as_view()-------->放入了一个view的内存地址，处理了csrf，所有请求都没有csrf校验了
- 请求一旦来了，路由匹配上----》view(request)---》self.dispatch(request,*args, **kwargs)，现在这个dispatch不是View中的dispatch,而是APIView中的dispatch
-dispatch---》把请求方法转成小写，----》通过反射，去对象中找，没有get方法，有就加括号执行，并且把request传进去






#urls.py
path('booksapiview/', views.BooksAPIView.as_view()),在这个地方应该写函数内存地址

#views.py
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.request import Request

class BooksAPIView(APIView):
    def get(self, request):
        #request已经不是原生的django的request了，是drf自己定义的request对象
        print(self.request)
        return HttpResponse('ok')
    
"""源码分析"""

#APIView的as_view方法（类的绑定方法）
@classmethod
def as_view(cls, **initkwargs):
    view = super().as_view(**initkwargs)#调用父类（View）的as_view方法，
    view.cls = cls
    view.initkwargs = initkwargs
    #以后所有的请求都没有csrf认证了，只要继承了APIV i额外，就没有csrf认证了
    return csrf_exempt(view)

#请求来了，路由匹配上，会执行view(request),调用了self.dispatch(),会执行apiview的self.dispatch()

#APIView的dispatch方法
 def dispatch(self, request, *args, **kwargs):

        self.args = args
        self.kwargs = kwargs
        #请求模块（解析模块）
        # self.initialize_request(request, *args, **kwargs)   这里的request是当次请求的request
        #request= self.initialize_request  这里的request是一个新的request对象
        #重新包装成一个request对象，以后再用的request对象，就是新的request对象
        request = self.initialize_request(request, *args, **kwargs)
        self.request = request
        self.headers = self.default_response_headers  # deprecate?

        try:
            #三大认证模块
            self.initial(request, *args, **kwargs)

            # Get the appropriate handler method
            if request.method.lower() in self.http_method_names:
                handler = getattr(self, request.method.lower(),
                                  self.http_method_not_allowed)
            else:
                handler = self.http_method_not_allowed
			#响应模块
            response = handler(request, *args, **kwargs)

        except Exception as exc:
            #异常模块
            response = self.handle_exception(exc)
		#渲染模块
        self.response = self.finalize_response(request, response, *args, **kwargs)
        return self.response

    
#APIView的initial方法源码分析
    def initial(self, request, *args, **kwargs):
       	#认证组件:校验用户，游客，合法用户，非法用户
        #游客:代表校验通过，直接进入下一步校验（权限校验）
        #合法用户:代表校验通过，将用户存储在request.user中，再进入下一步校验（权限校验）
        #非法用户:代表校验失败，抛出异常，返回403权限异常结果
        self.perform_authentication(request)
        #权限组件:校验用户权限-必须登录，所有用户、登录读写游客只读，自定义用户角色
        #认证通过:可以进入下一步校验(频率认证)
        #认证失败:抛出异常，返回403权限异常结果
        self.check_permissions(request)
        #频率权限:限制视图接口被访问的频率次数-限制的条件(IP,id,唯一键)、频率周期时间(s、m、h)、频率次数(3/s)
        #没有达到限次:正常访问接口
        #达到限次:限制时间内不能访问，限制时间达到后，可以重新访问
        self.check_throttles(request)
```

### 补充：

**一切皆对象**

```python
def foo(a, b):
    return a + b

foo.name = 'zhao'

print(foo(1, 3))
print(foo.name)
```

局部禁用csrf

```python 
#视图函数中加装饰器@csrf_exempt

#csrf_exempt(view)和在视图函数上加装饰器是一摸一样的
```

```python
from django.views.decorators.csrf import csrf_exempt

urlpatterns = [
    path('test/',csrf_exempt(views.test))#也是禁用csrf认证
]
```

### drf的Request类

```python
from rest_framework.request import Request
#只要继承了APIView,视图中的request对象，都是新的，也就是上面这个request的对象，

#老的request在新的request._request里

#以后使用request对象，就像使用之前的request是一摸一样的（因为重写了__getattr__方法）

 def __getattr__(self, attr):
        try:
            return getattr(self._request, attr) #通过反射，取原生的request对象，取出属性和方法
        except AttributeError:
            return self.__getattribute__(attr)
        
        
# request.data 感觉是数据属性，其实是个方法，@property修饰了,,它是一个字段，前端以三种编码格式传入的数据，都在request.data中
# 请求对象.query_params 与django标准的request.GET相同，只是更换了更正确的名称而已



#get请求传过来的数据，从哪取?
	request.GET
 

"""源码分析"""
******************************************
    @property
    def query_params(self):
        """
        More semantically correct name for request.GET.
        """
        return self._request.GET
******************************************    
	
    #views.py
    class BooksAPIView(APIView):
    def get(self, request):
        print(self.request)
        print(request.query_params)#get请求,地址中的参数
		#原来获取get请求，用的是request.GET
        print(request.GET)

        return HttpResponse('ok')

#文件 FILES
"""源码分析"""
****************************************** 
    @property
    def FILES(self):
        # Leave this one alone for backwards compat with Django's request.FILES
        # Different from the other two cases, which are not valid property
        # names on the WSGIRequest class.
        if not _hasattr(self, '_files'):
            self._load_data_and_files()
        return self._files
****************************************** 
```

### drf的Response类

```python
#from rest_framework.response import Response

class Response(SimpleTemplateResponse):
    def __init__(self, data=None, status=None,
                 template_name=None, headers=None,
                 exception=False, content_type=None):

# data:要返回的数据，字典
#status:返回的状态码，默认是200
#template_name 渲染的模板的名字，（自定制的模板）
#headers:响应头，可以往响应头中放东西，就是一个字典
#content_type:响应的编码格式 'application/json'  和 'text/html'

```

```python
#urls.py
   path('test/',views.TestView.as_view()),

#views.py
class TestView(APIView):
    def get(self, request):
        print(request)
        return Response({'name': 'zhao'}, status=301, headers={'token': 'test'})
```

![image-20221203202936839](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221203202939.png)

**status状态码**

为了方便设置状态码，`rest framework`在`rest_framework.status`模块中提供了常用的状态码**常量**

1) **信息告知-1xx**

```python
HTTP_100_CONTINUE = 100
HTTP_101_SWITCHING_PROTOCOLS = 101
HTTP_102_PROCESSING = 102
HTTP_103_EARLY_HINTS = 103
```



2. **成功-2xx**

```python
HTTP_200_OK = 200
HTTP_201_CREATED = 201
HTTP_202_ACCEPTED = 202
HTTP_203_NON_AUTHORITATIVE_INFORMATION = 203
HTTP_204_NO_CONTENT = 204
HTTP_205_RESET_CONTENT = 205
HTTP_206_PARTIAL_CONTENT = 206
HTTP_207_MULTI_STATUS = 207
HTTP_208_ALREADY_REPORTED = 208
HTTP_226_IM_USED = 226
```



3. **重定向-3xx**

```python
HTTP_300_MULTIPLE_CHOICES = 300
HTTP_301_MOVED_PERMANENTLY = 301
HTTP_302_FOUND = 302
HTTP_303_SEE_OTHER = 303
HTTP_304_NOT_MODIFIED = 304
HTTP_305_USE_PROXY = 305
HTTP_306_RESERVED = 306
HTTP_307_TEMPORARY_REDIRECT = 307
HTTP_308_PERMANENT_REDIRECT = 308
```



4. **客户端报错-4xx**

```python



HTTP_400_BAD_REQUEST = 400
HTTP_401_UNAUTHORIZED = 401
HTTP_402_PAYMENT_REQUIRED = 402
HTTP_403_FORBIDDEN = 403
HTTP_404_NOT_FOUND = 404
HTTP_405_METHOD_NOT_ALLOWED = 405
HTTP_406_NOT_ACCEPTABLE = 406
HTTP_407_PROXY_AUTHENTICATION_REQUIRED = 407
HTTP_408_REQUEST_TIMEOUT = 408
HTTP_409_CONFLICT = 409
HTTP_410_GONE = 410
HTTP_411_LENGTH_REQUIRED = 411
HTTP_412_PRECONDITION_FAILED = 412
HTTP_413_REQUEST_ENTITY_TOO_LARGE = 413
HTTP_414_REQUEST_URI_TOO_LONG = 414
HTTP_415_UNSUPPORTED_MEDIA_TYPE = 415
HTTP_416_REQUESTED_RANGE_NOT_SATISFIABLE = 416
HTTP_417_EXPECTATION_FAILED = 417
HTTP_418_IM_A_TEAPOT = 418
HTTP_421_MISDIRECTED_REQUEST = 421
HTTP_422_UNPROCESSABLE_ENTITY = 422
HTTP_423_LOCKED = 423
HTTP_424_FAILED_DEPENDENCY = 424
HTTP_425_TOO_EARLY = 425
HTTP_426_UPGRADE_REQUIRED = 426
HTTP_428_PRECONDITION_REQUIRED = 428
HTTP_429_TOO_MANY_REQUESTS = 429
HTTP_431_REQUEST_HEADER_FIELDS_TOO_LARGE = 431
HTTP_451_UNAVAILABLE_FOR_LEGAL_REASONS = 451
```



5. **服务器错误-5xx**

```python

HTTP_500_INTERNAL_SERVER_ERROR = 500
HTTP_501_NOT_IMPLEMENTED = 501
HTTP_502_BAD_GATEWAY = 502
HTTP_503_SERVICE_UNAVAILABLE = 503
HTTP_504_GATEWAY_TIMEOUT = 504
HTTP_505_HTTP_VERSION_NOT_SUPPORTED = 505
HTTP_506_VARIANT_ALSO_NEGOTIATES = 506
HTTP_507_INSUFFICIENT_STORAGE = 507
HTTP_508_LOOP_DETECTED = 508
HTTP_509_BANDWIDTH_LIMIT_EXCEEDED = 509
HTTP_510_NOT_EXTENDED = 510
HTTP_511_NETWORK_AUTHENTICATION_REQUIRED = 511

```

![image-20221208100105613](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221208100108.png)

```python
from rest_framework import status

class TestView(APIView):
    def get(self, request):
        print(request)
        return Response({'name': 'zhao'}, status=status.HTTP_200_OK, headers={'token': 'test'})
```

## ==（6）序列化器-Serializer==

**作用**:

1. 序列化，序列化器会把模型对象转换成字典，经过response以后变成json字符串
2. 反序列化，把客户端发送过来的数据，经过request以后变成字典，序列化器可以把字典转成模型
3. 反序列化，完成数据校验功能

```python
#先在models.py中写创建表
from django.db import models

class Book(models.Model):
    id = models.AutoField(primary_key=True)
    name = models.CharField(max_length=32)
    price = models.DecimalField(max_digits=8, decimal_places=3)
    author = models.CharField(max_length=32)
    publish = models.CharField(max_length=32)
```

### 1、序列化器使用

```python
写一个序列化类，继承Serializer
在类中写要序列化的字段，想要序列化哪个字段，就在类中写哪个字段
在视图类中使用，导入自己写的序列化类（ser.py）---》实例化得到序列化类的对象，把要序列化的对象传入
序列化类的对象.data  是一个字典
把字典返回，如果不使用rest_framework提供的Resposne，就得使用JsonResponse
```



```python
# 自己创建的ser.py文件中写序列化类

from rest_framework import serializers

# 需要继承Serializer
class BookSeralizer(serializers.Serializer):
    #想要序列化哪个字段，就在类中写哪个字段
    id = serializers.CharField()
    name = serializers.CharField()
    price = serializers.CharField()
    author = serializers.CharField()
    publish = serializers.CharField()
```

```python
#urls.py

re_path('books/(?P<pk>\d+)', views.BookView.as_view())
```



```python
#views.py
from rest_framework.views import APIView
from App.models import Book
from App.ser import BookSeralizer
from rest_framework.response import Response  # drf提供的响应对象

class BookView(APIView):
    def get(self, request, pk):
        book_obj = Book.objects.filter(pk=pk).first()
        # 序列化谁，就把谁传过来
        book_ser = BookSeralizer(book_obj)  # 调用类的__init__方法
        # 序列化对象.data就是序列化后的字典
        return Response(book_ser.data)
```

![image-20221202161136295](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221202161137.png)

![image-20221202161300266](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221202161313.png)

如果使用`JsonResponse`返回数据，效果如下:

**如果使用`JsonResponse`返回数据，就不需要在`settings.py`中注册`rest_framework`了**

![image-20221202161048599](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221202161050.png)



**注意**：

如果碰到下面的报错，需要把`rest_framework`在settings.py中的app中注册

![image-20221202155024033](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221202155026.png)

![image-20221202155145795](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221202155147.png)

### 2、序列化类的字段类型

```python
有很多，
只需记住 CharField IntegerField，DateField......
```

**字段类型**:

![20210405180619734](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221202162323.png)

![20210405180655435](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221202162305.png)

**选项参数:**

![20210405180757635](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221202162432.png)

**通用参数**:

![20210405180910624](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221202162502.png)

### 3、序列化组件修改保存数据

```python
1.写一个序列化类，继承Serializer
2.在类中写要反序列化的字段，想要反序列化哪个字段，就在类中写哪个字段,字段的属性(max_length.....)
3.在视图类中使用，导入---》实例化得到序列化类的对象，把要修改的数据传入         
		book_ser = BookSeralizer(book_obj, request.data)
    	book_ser = BookSeralizer(instance=book_obj, data=request.data)
```



```python
#自己创建的ser.py文件

from rest_framework import serializers
from rest_framework.exceptions import ValidationError


# 需要继承Serializer
class BookSeralizer(serializers.Serializer):
    name = serializers.CharField(max_length=16, min_length=4)
    price = serializers.CharField()
    author = serializers.CharField()
    publish = serializers.CharField()

    def update(self, instance, validated_data):
        # instance是Book这个对象
        # validated_data是校验后的数据
        instance.name = validated_data.get('name')
        instance.price = validated_data.get('price')
        instance.author = validated_data.get("author")
        instance.publish = validated_data.get('publish')
        instance.save()  # book.save() 是django 的orm提供的
        return instance
```



```python
#urls.py
re_path('books/(?P<pk>\d+)', views.BookView.as_view())


#views.py
from rest_framework.views import APIView
from App.models import Book
from App.ser import BookSeralizer
from rest_framework.response import Response  # drf提供的响应对象

class BookView(APIView):
    def put(self, request, pk):
        response_msg = {'code': 100, 'msg': ''}
        # 找到要改的对象
        book_obj = Book.objects.filter(pk=pk).first()
        # 得到序列化类的对象
        # book_ser = BookSeralizer(book_obj, request.data)
        book_ser = BookSeralizer(instance=book_obj, data=request.data)
        # 要验证（和forms组件校验一样）
        if book_ser.is_valid():  # 表示验证通过
            book_ser.save()
            response_msg['msg'] = '数据校验成功'
            response_msg['data'] = book_ser.data

        else:
            response_msg['code'] = 101
            response_msg['msg'] = '数据校验失败'
            response_msg['data'] = book_ser.errors
        return Response(response_msg)
```



### 4、序列化组件校验数据

```python
1.写一个序列化类，继承Serializer
2.在类中写要反序列化的字段，想要反序列化哪个字段，就在类中写哪个字段,字段的属性(max_length.....)
3.在视图类中使用，导入---》实例化得到序列化类的对象，把要修改的数据传入         
		book_ser = BookSeralizer(book_obj, request.data)
    	book_ser = BookSeralizer(instance=book_obj, data=request.data)

4.数据校验 if book_ser.is_valid()
5.如果校验通过就保存，视图中调用 序列化对象 ook_ser.save() 序列化对象.save()
6.如果不通过，逻辑自己写
7.如果字段的校验规则不够，可以写钩子函数（局部和全局）
	# 局部钩子
	def validate_price(self, data):  # validate_字段名，接受一个参数
        # print(type(data))
        # print(data)
        if float(data) < 10:
            # 校验失败，抛异常
            raise ValidationError('价格太低')
        return data
 
     # 全局钩子
    def validate(self, validated_data): 
        author = validated_data.get("author")
        publish = validated_data.get("publish")
        if author == publish:
            raise ValidationError("作者跟出版社一样")
            return validated_data
8. 可以使用字段的validators来校验
		author = serializers.CharField(validators=[check_author])， 来校验
        -写一个函数
        def check_author(data):
            if data.startswith('sb'):
                raise ValidationError('作者名不能以sb开头')
        -配置:author =validators=[check_author]

```

```python
re_path('books/(?P<pk>\d+)', views.BookView.as_view())
```

**局部（全局）钩子校验:**

```python
#自己创建的ser.py文件

from rest_framework import serializers
from rest_framework.exceptions import ValidationError


# 需要继承Serializer
class BookSeralizer(serializers.Serializer):
    name = serializers.CharField(max_length=16, min_length=4)
    price = serializers.CharField()
    author = serializers.CharField()
    publish = serializers.CharField()

    # 局部钩子
    def validate_price(self, data):  # validate_字段名，接受一个参数
        # print(type(data))
        # print(data)
         # 如果价格小于10，校验不通过
        if float(data) < 10:
            # 校验失败，抛异常
            raise ValidationError('价格太低')
        return data
    
	 # 全局钩子
    def validate(self, validated_data): 
        author = validated_data.get("author")
        publish = validated_data.get("publish")
        if author == publish:
            raise ValidationError("作者跟出版社一样")
            return validated_data


    def update(self, instance, validated_data):
        # instance是Book这个对象
        # validated_data是校验后的数据
        instance.name = validated_data.get('name')
        instance.price = validated_data.get('price')
        instance.author = validated_data.get("author")
        instance.publish = validated_data.get('publish')
        instance.save()  # book.save() 是django 的orm提供的
        return instance
```

**自己逻辑上的校验:**

```python
#views.py

from rest_framework.views import APIView
from App.models import Book
from App.ser import BookSeralizer
from rest_framework.response import Response  # drf提供的响应对象

class BookView(APIView):
    def put(self, request, pk):
        response_msg = {'code': 100, 'msg': ''}
        # 找到要改的对象
        book_obj = Book.objects.filter(pk=pk).first()
        # 得到序列化类的对象
        # book_ser = BookSeralizer(book_obj, request.data)
        book_ser = BookSeralizer(instance=book_obj, data=request.data)
        # 要验证（和forms组件校验一样）
        if book_ser.is_valid():  # 表示验证通过
            book_ser.save()
            response_msg['msg'] = '数据校验成功'
            response_msg['data'] = book_ser.data

        else:
            response_msg['code'] = 101
            response_msg['msg'] = '数据校验失败'
            response_msg['data'] = book_ser.errors
        return Response(response_msg)
```

![image-20221202172556039](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221202172557.png)

**校验：**

```python
def check_author(data):
    if data.startswith('sb'):
        raise ValidationError('作者名不能以sb开头')


# 需要继承Serializer
class BookSeralizer(serializers.Serializer):
    name = serializers.CharField(max_length=16, min_length=4)
    price = serializers.CharField()
    author = serializers.CharField(validators=[check_author])  # validators=[]，列表中写函数内存地址
    publish = serializers.CharField()
```

![image-20221202182256623](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221202182258.png)

**全部代码：**

* 1 表模型类

```python
#models.py

from django.db import models

class Book(models.Model):
    id = models.AutoField(primary_key=True)
    name = models.CharField(max_length=32)
    price = models.DecimalField(max_digits=8, decimal_places=3)
    author = models.CharField(max_length=32)
    publish = models.CharField(max_length=32)
```

* 2  序列化类

```python
# -*- coding: UTF-8 -*- 
# @Date ：2022/12/2 15:29

#ser.py

from rest_framework import serializers
from rest_framework.exceptions import ValidationError


def check_author(data):
    if data.startswith('sb'):
        raise ValidationError('作者名不能以sb开头')


# 需要继承Serializer
class BookSeralizer(serializers.Serializer):
    name = serializers.CharField(max_length=16, min_length=4)
    price = serializers.CharField()
    author = serializers.CharField(validators=[check_author])  # validators=[]，列表中写函数内存地址
    publish = serializers.CharField()

    # 局部钩子
    def validate_price(self, data):  # validate_字段名，接受一个参数
        # print(type(data))
        # print(data)

        # 如果价格小于10，校验不通过
        if float(data) < 10:
            # 校验失败，抛异常
            raise ValidationError('价格太低')
        return data

    def validate(self, validated_data):  # 全局钩子
        author = validated_data.get("author")
        publish = validated_data.get("publish")
        if author == publish:
            raise ValidationError("作者跟出版社一样")
        return validated_data

    def update(self, instance, validated_data):
        # instance是Book这个对象
        # validated_data是校验后的数据
        instance.name = validated_data.get('name')
        instance.price = validated_data.get('price')
        instance.author = validated_data.get("author")
        instance.publish = validated_data.get('publish')
        instance.save()  # book.save() 是django 的orm提供的
        return instance
```

* 3 路由配置

```python
#urls.py

from django.contrib import admin
from django.urls import path, re_path
from App import views

urlpatterns = [
    path('admin/', admin.site.urls),
    re_path('books/(?P<pk>\d+)', views.BookView.as_view())
]
```

* 4 书写视图类

```python
#views.py

from django.shortcuts import render
from rest_framework.views import APIView
from App.models import Book
from App.ser import BookSeralizer
from rest_framework.response import Response  # drf提供的响应对象
from django.http import JsonResponse


class BookView(APIView):
    def get(self, request, pk):
        book_obj = Book.objects.filter(pk=pk).first()
        # 序列化谁，就把谁传过来
        book_ser = BookSeralizer(book_obj)  # 调用类的__init__方法
        # 序列化对象.data就是序列化后的字典
        return Response(book_ser.data)
        # return JsonResponse(book_ser.data)

    def put(self, request, pk):
        response_msg = {'code': 100, 'msg': ''}
        # 找到要改的对象
        book_obj = Book.objects.filter(pk=pk).first()
        # 得到序列化类的对象
        # book_ser = BookSeralizer(book_obj, request.data)
        book_ser = BookSeralizer(instance=book_obj, data=request.data)
        # 要验证（和forms组件校验一样）
        if book_ser.is_valid():  # 表示验证通过
            book_ser.save()
            response_msg['msg'] = '数据校验成功'
            response_msg['data'] = book_ser.data

        else:
            response_msg['code'] = 101
            response_msg['msg'] = '数据校验失败'
            response_msg['data'] = book_ser.errors
        return Response(response_msg)
```

### 4、reand_only和write_only

* read_only

```python
表明该字段仅仅用于序列化输出，默认False，
如果设置成True,postman中可以看到该字段，修改时不需要传该字段
```

* write_only

```python
表明该字段仅仅用户反序列化输入，默认False，
如果设置成了True，postman看不到该字段，但修改时，该字段必须传
```

![image-20221202194101218](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221202194102.png)

### 5、查看所有，删除，新增

1. 查询所有数据

```python

path('books/', views.BooksView.as_view())

#views.py
重新写一个视图类
class BooksView(APIView):
    def get(self, request):
        response_msg = {'code': 100, 'msg': ''}
        books_obj = Book.objects.all()
        book_ser = BookSeralizer(books_obj, many=True)  # 序列化多条,如果序列化一条，不需要写
        response_msg['data'] = book_ser.data
        response_msg['msg'] = '校验成功'
        return Response(response_msg)

```

2. 新增数据

```python
# urls.py

path('books/', views.BooksView.as_view())


#views.py
class BooksView(APIView):
    # 新增数据
    def post(self, request):
        response_msg = {'code': 100, 'msg': '校验成功'}
        # 修改才有instance,新增没有，只有data
        book_ser = BookSeralizer(data=request.data)
        # book_ser = BookSeralizer(request.data) #按位置传参数request.data，会给instance,就报错了

        # 校验数据
        if book_ser.is_valid():
            book_ser.save()
            response_msg['data'] = book_ser.data
        else:
            response_msg['code'] = 102
            response_msg['msg'] = '数据校验失败'
            response_msg['data'] = book_ser.errors
        return Response(response_msg)

```

```python
#ser.py

#在序列化类中重写create方法
    def create(self, validated_data):
        # Book.objects.create(name=validated_data.get('name'))
        instance = Book.objects.create(**validated_data)
        return instance
```

![image-20221202191930627](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221202191932.png)

![image-20221202191630502](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221202191632.png)

3. 删除数据

```python
#urls.py
re_path('books/(?P<pk>\d+)', views.BookView.as_view()),


#views.py
class BookView(APIView):
    def delete(self, request, pk):
        book_obj = Book.objects.filter(pk=pk).delete()
        response_msg = {'code': 100, 'msg': '删除成功'}
        return Response(response_msg)
```

![image-20221202192731507](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221202192733.png)

### 6、自定义Response对象

```python
# -*- coding: UTF-8 -*- 
# @Date ：2022/12/2 19:30

#创建一个utils.py文件 
class MyResponse():
    def __init__(self):
        self.code = 100
        self.msg = '成功'

    @property
    def get_dict(self):
        return self.__dict__
```

代码:

* models.py

```python
from django.db import models


# Create your models here.
class Book(models.Model):
    id = models.AutoField(primary_key=True)
    name = models.CharField(max_length=32)
    price = models.DecimalField(max_digits=8, decimal_places=3)
    author = models.CharField(max_length=32)
    publish = models.CharField(max_length=32)
```

* 序列化类 

```python
# -*- coding: UTF-8 -*- 
# @Date ：2022/12/2 19:53
from rest_framework import serializers
from rest_framework.exceptions import ValidationError
from App.models import Book


def check_author(data):
    if data.startswith('sb'):
        raise ValidationError('作者不能以sb开头')


class BookSerializer(serializers.Serializer):
    id = serializers.CharField(read_only=True)
    name = serializers.CharField(max_length=16, min_length=4)
    price = serializers.CharField(write_only=True)
    author = serializers.CharField(validators=[check_author])
    publish = serializers.CharField()

    def validate_price(self, data):
        if float(data) < 10:
            raise ValidationError('价格太低了')
        return data

    def validate(self, validate_data):
        author = validate_data.get('author')
        publish = validate_data.get('publish')
        if author == publish:
            raise ValidationError('作者和出版社一样')
        return validate_data

    def update(self, instance, validated_data):
        instance.name = validated_data.get('name')
        instance.price = validated_data.get('price')
        instance.author = validated_data.get('author')
        instance.publish = validated_data.get('publish')
        instance.save()
        return instance

    def create(self, validated_data):
        instance = Book.objects.create(**validated_data)
        return instance
```

* 自定义Response

```python 
# -*- coding: UTF-8 -*- 
# @Date ：2022/12/2 20:12
class MyResponse:
    def __init__(self):
        self.code = 100
        self.msg = '检验成功'

    @property
    def get_dict(self):
        return self.__dict__


if __name__ == '__main__':
    res = MyResponse()
    res.data = {'name': 'zhao'}
    print(res.get_dict)

```

* urls.py

```python
from django.contrib import admin
from django.urls import path, re_path
from App import views

urlpatterns = [
    path('admin/', admin.site.urls),
    re_path('book/(?P<pk>\d+)',views.BookView.as_view()),
    path('books/',views.BooksView.as_view())
]
```

* 视图类的书写

```python
from django.shortcuts import render
from rest_framework.views import APIView
from App.models import Book
from App.ser import BookSerializer
from rest_framework.response import Response
from App.utils import MyResponse


# Create your views here.
class BookView(APIView):
    # 获取单个数据
    def get(self, request, pk):
        book_obj = Book.objects.filter(pk=pk).first()
        book_ser = BookSerializer(book_obj)
        return Response(book_ser.data)

    # 修改数据
    def put(self, request, pk):
        response = MyResponse()
        book_obj = Book.objects.filter(pk=pk).first()
        book_er = BookSerializer(instance=book_obj, data=request.data)
        if book_er.is_valid():
            book_er.save()
            response.data = book_er.data
            response.msg = '校验成功'

        else:
            response.msg = '校验失败'
            response.code = 101
            response.data = book_er.errors
        return Response(response.get_dict)

    # 删除数据
    def delete(self, request, pk):
        response = MyResponse()
        Book.objects.filter(pk=pk).delete()
        return Response(response.get_dict)


class BooksView(APIView):
    # 获取所有数据
    def get(self, request):
        response = MyResponse()
        books_obj = Book.objects.all()
        book_er = BookSerializer(books_obj, many=True)
        response.data = book_er.data
        return Response(response.get_dict)

    # 新增数据
    def post(self, request):
        response = MyResponse()
        book_ser = BookSerializer(data=request.data)
        if book_ser.is_valid():
            book_ser.save()
            response.data = book_ser.data
        else:
            response.data = book_ser.errors
            response.msg = '校验失败'
            response.code = 101
        return Response(response.get_dict)
```

### 7、模型类的序列化器

```python
#ser.py 序列化类

class BookModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = Book  # 对应models.py中的模型
        fields = '__all__'  # 表示所有字段都序列化
        # fields = ('name', 'price')  # 序列化指定字段
        # exclude = ('name',)  # 除了name字段，其他都序列化
        
        # 给authors和publish加write_only属性
        # name加max_len属性
        extra_kwargs = {
            'name': {'max_length': 8},
            'publish': {'write_only': True},
            'authors': {'write_only': True},
        }

```

```python
#urls.py
path('books2/', views.BooksView2.as_view())
```

```python
#views.py
from App.ser import BookModelSerializer
class BooksView2(APIView):
    def get(self, request):
        response = MyResponse()
        book_obj = Book.objects.all()
        book_ser = BookModelSerializer(book_obj, many=True)
        response.data = book_ser.data
        return Response(response.get_dict)
```

### 8、关键字many源码分析

```python
# 序列化多条，需要传many=True
```

```python
path('many/',views.ManyView.as_view()),

class ManyView(APIView):
    def get(self, request):
        response = MyResponse()
        book_obj = Book.objects.filter().first()
        books_obj = Book.objects.all()
        book_ser = BookModelSerializer(book_obj)#序列化单条
        books_ser = BookModelSerializer(books_obj, many=True)#序列化多条
        print(type(book_ser))  # <class 'App.ser.BookModelSerializer'>
        print(type(books_ser))  
        # <class 'rest_framework.serializers.ListSerializer'>
        response.data = books_ser.data
        return Response(response.get_dict)
    
    
    #对象的生成---》先调用类的__new__方法，生成空对象
	#对象=类名(name=zhao),触发类的__init__()
    #类的__new__方法控制对象的生成    
```



```python
"""源码分析"""
class BaseSerializer(Field):
    def __new__(cls, *args, **kwargs):
        if kwargs.pop('many', False):
            return cls.many_init(*args, **kwargs)
        #没有传many=True,走下面，正常的对象实例化，
        return super().__new__(cls, *args, **kwargs)
    
    
    
     @classmethod
    def many_init(cls, *args, **kwargs):
        allow_empty = kwargs.pop('allow_empty', None)
        max_length = kwargs.pop('max_length', None)
        min_length = kwargs.pop('min_length', None)
        child_serializer = cls(*args, **kwargs)
        list_kwargs = {
            'child': child_serializer,
        }
        if allow_empty is not None:
            list_kwargs['allow_empty'] = allow_empty
        if max_length is not None:
            list_kwargs['max_length'] = max_length
        if min_length is not None:
            list_kwargs['min_length'] = min_length
        list_kwargs.update({
            key: value for key, value in kwargs.items()
            if key in LIST_SERIALIZER_KWARGS
        })
        meta = getattr(cls, 'Meta', None)
        list_serializer_class = getattr(meta, 'list_serializer_class', ListSerializer)
        return list_serializer_class(*args, **list_kwargs)
```

### 9、Serializer高级用法

新创建一个应用，**==记得一定要去注册!==**

```python
# 路由分发
from App2 import urls
 # path('App2/', include('App2.urls'))
 path('App2/', include(urls)),
```

```python
#App2.等models.py
from django.db import models

class Book(models.Model):
    title = models.CharField(max_length=32)
    price = models.IntegerField()
    pub_date = models.DateTimeField()
    publish = models.ForeignKey('Publish', on_delete=models.CASCADE, null=True)
    authors = models.ManyToManyField('Author')

    def __str__(self):
        return self.title


class Publish(models.Model):
    name = models.CharField(max_length=32)
    email = models.EmailField()

    def __str__(self):
        return self.name


class Author(models.Model):
    name = models.CharField(max_length=32)
    age = models.IntegerField()

    def __str__(self):
        return self.name

```

```python
#App2.views.py

from django.shortcuts import render
from App2.models import Book
from App2.ser import BookSerializer
from rest_framework.views import APIView
from rest_framework.response import Response


class App2BookView(APIView):
    def get(self, request, pk):
        book_obj = Book.objects.filter(pk=pk).first()
        book_ser = BookSerializer(book_obj)
        return Response(book_ser.data)



```

```python
# -*- coding: UTF-8 -*- 
# @Date ：2022/12/3 16:27

#App2.ser.py
from rest_framework import serializers


class BookSerializer(serializers.Serializer):
    title123 = serializers.CharField(source='title')
    price = serializers.CharField()
    authors = serializers.CharField()
    publish = serializers.CharField(source='publish.email')  # 相当于book.publish.email
    pub_date = serializers.CharField()
```

![image-20221203173233427](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221203173235.png)

```python
# -*- coding: UTF-8 -*- 
# @Date ：2022/12/3 16:27
from rest_framework import serializers


class BookSerializer(serializers.Serializer):
    title123 = serializers.CharField(source='title')
    price = serializers.CharField()
    # authors = serializers.CharField()
    authors = serializers.SerializerMethodField()  # 必须配一个方法，方法名叫get_字段名，返回值就是要显示的内容

    def get_authors(self, instance):
        # book对象
        authors = instance.authors.all()  # 取出所有作者
        l = []
        for author in authors:
            l.append({'name': author.name, 'age': author.age})
        return l

    publish = serializers.CharField(source='publish.email')  # 相当于book.publish.email
    pub_date = serializers.CharField()

```

![image-20221203174011856](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221203174013.png)

#### source

```python
# 可以改字段名字
title123 = serializers.CharField(source='title')
# 可以.跨表

publish = serializers.CharField(source='publish.email')  # 相当于book.publish.email

# 可以执行方法
pub_date = serializers.CharField(source='test')  # test是Book表模型中
```

<img src="https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221203174714.png" alt="image-20221203174712830" style="zoom:80%;" />

#### SerializerMethodField()

```python
authors = serializers.SerializerMethodField()  # 必须配一个方法，方法名叫get_字段名，返回值就是要显示的内容

    def get_authors(self, instance):
        # book对象
        authors = instance.authors.all()  # 取出所有作者
        l = []
        for author in authors:
            l.append({'name': author.name, 'age': author.age})
        return l
```

### 10、回顾

```python
#1.Serializer类，需要序列化什么，必须写一个继承类，想要序列化什么字段，就在里面写字段，（source）
#2 序列化queryset（列表）对象和真正的对象，many=True的作用，instance=要序列化的对象



#3 反序列化 instance=要序列化的对象，data=request.data
#4 字段验证，序列化类中，给字段加属性，局部和全局钩子函数，字段属性validators=[check_author]
#5 当在视图中调用， 序列化对象.is_valid()  booK_ser.is_valid(raise_exception=True) 只要验证不通过，直接抛出异常
#6 修改保存---》调用序列化列对象.save(),重写Serializer类 的update方法


		 def update(self, instance, validated_data):
            # instance是Book这个对象
            # validated_data是校验后的数据
            instance.name = validated_data.get('name')
            instance.price = validated_data.get('price')
            instance.author = validated_data.get("author")
            instance.publish = validated_data.get('publish')
            instance.save()  # book.save() 是django 的orm提供的
            return instance
#7 序列化得到的字段， 序列化对象.data
#8 自定义Response对象
    class MyResponse():
        def __init__(self):
            self.code = 100
            self.msg = '成功'

        @property
        def get_dict(self):
            return self.__dict__

    
 #9 反序列化的新增 序列化类(data=request.data),如果只传了data,当调用序列化对象.save(),会触发序列化类的create方法执行，当传了instance和data时，调用 序列化对象.save(),会触发序列化类的update方法执行

#10 重写create方法，（可以很复杂）
	    def create(self, validated_data):
        instance = Book.objects.create(**validated_data)
        return instance
    
    
#11 ModelSerializer跟Model做了对应
	class BookModelSerializer(serializers.ModelSerializer):
        class Meta:
            model = Book  # 对应models.py中的模型
            fields = '__all__'  # 表示所有字段都序列化
            # fields = ('name', 'price')  # 序列化指定字段
            # exclude = ('name',)  # 除了name字段，其他都序列化，不能跟fields连用，写谁，就排除谁

            # 给authors和publish加write_only属性
            # name加max_len属性
            extra_kwargs = {
                'name': {'max_length': 8},
                'publish': {'write_only': True},
                'authors': {'write_only': True,'max_lenth':6,'min_length':4},
            }
#12 如果在ModelSerializer中写一个局部钩子或全局钩子，跟之前一摸一样

#13 many=True能够序列化多条的原因-----》__new__是在__init__之前执行的，造出了一个空对象

#14 接口：为了统一子类的行为
```

## ==（7）局部和全局响应配置==

`rest framework`提供了一个响应类`Response`，使用该类构造响应对象时，响应的具体数据内容会被转换成（render渲染）成符合前端需求的类型

`rest framework`提供了`Renderer`渲染器，用来根据请求头中的`Accept（接收数据类型声明）`来自动转换响应数据到对应的格式，如果前端请求中未进行`Accept`声明，则会采用默认方式处理响应数据，我们可以通过配置来修改默认响应格式。

可以在`rest_framework.settings`查找所有的drf默认配置项

```python
DEFAULTS = {
    # Base API policies
    'DEFAULT_RENDERER_CLASSES': [          # 默认响应渲染类
        'rest_framework.renderers.JSONRenderer',# json渲染器
        'rest_framework.renderers.BrowsableAPIRenderer', # 浏览器API渲染器
    ],
    'DEFAULT_PARSER_CLASSES': [
        'rest_framework.parsers.JSONParser',
        'rest_framework.parsers.FormParser',
        'rest_framework.parsers.MultiPartParser'
    ],
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.BasicAuthentication'
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.AllowAny',
    ],
    'DEFAULT_THROTTLE_CLASSES': [],
    'DEFAULT_CONTENT_NEGOTIATION_CLASS': 'rest_framework.negotiation.DefaultContentNegotiation',
    'DEFAULT_METADATA_CLASS': 'rest_framework.metadata.SimpleMetadata',
    'DEFAULT_VERSIONING_CLASS': None,

    # Generic view behavior
    'DEFAULT_PAGINATION_CLASS': None,
    'DEFAULT_FILTER_BACKENDS': [],

    # Schema
    'DEFAULT_SCHEMA_CLASS': 'rest_framework.schemas.openapi.AutoSchema',

    # Throttling
    'DEFAULT_THROTTLE_RATES': {
        'user': None,
        'anon': None,
    },
    'NUM_PROXIES': None,

    # Pagination
    'PAGE_SIZE': None,

    # Filtering
    'SEARCH_PARAM': 'search',
    'ORDERING_PARAM': 'ordering',

    # Versioning
    'DEFAULT_VERSION': None,
    'ALLOWED_VERSIONS': None,
    'VERSION_PARAM': 'version',

    # Authentication
    'UNAUTHENTICATED_USER': 'django.contrib.auth.models.AnonymousUser',
    'UNAUTHENTICATED_TOKEN': None,

    # View configuration
    'VIEW_NAME_FUNCTION': 'rest_framework.views.get_view_name',
    'VIEW_DESCRIPTION_FUNCTION': 'rest_framework.views.get_view_description',

    # Exception handling
    'EXCEPTION_HANDLER': 'rest_framework.views.exception_handler',
    'NON_FIELD_ERRORS_KEY': 'non_field_errors',

    # Testing
    'TEST_REQUEST_RENDERER_CLASSES': [
        'rest_framework.renderers.MultiPartRenderer',
        'rest_framework.renderers.JSONRenderer'
    ],
    'TEST_REQUEST_DEFAULT_FORMAT': 'multipart',

    # Hyperlink settings
    'URL_FORMAT_OVERRIDE': 'format',
    'FORMAT_SUFFIX_KWARG': 'format',
    'URL_FIELD_NAME': 'url',

    # Input and output formats
    'DATE_FORMAT': ISO_8601,
    'DATE_INPUT_FORMATS': [ISO_8601],

    'DATETIME_FORMAT': ISO_8601,
    'DATETIME_INPUT_FORMATS': [ISO_8601],

    'TIME_FORMAT': ISO_8601,
    'TIME_INPUT_FORMATS': [ISO_8601],

    # Encoding
    'UNICODE_JSON': True,
    'COMPACT_JSON': True,
    'STRICT_JSON': True,
    'COERCE_DECIMAL_TO_STRING': True,
    'UPLOADED_FILES_USE_URL': True,

    # Browseable API
    'HTML_SELECT_CUTOFF': 1000,
    'HTML_SELECT_CUTOFF_TEXT': "More than {count} items...",

    # Schemas
    'SCHEMA_COERCE_PATH_PK': True,
    'SCHEMA_COERCE_METHOD_NAMES': {
        'retrieve': 'read',
        'destroy': 'delete'
    },
}
```

```python
#浏览器响应成浏览器的格式，postman响应成json格式，通过配置实现的。（默认配置）
	
```

#### 局部配置

* 对某个视图类有效

==**drf的配置信息----》先从自己的类中找，-----》项目的settings.py中找，找不到再采用默认的**==

* 在视图类中写如下代码

```python

from rest_framework.renderers import JSONRenderer
class TestView(APIView):
    renderer_classes = [JSONRenderer]
    def get(self, request):
        print(request)
        return Response({'name': 'zhao'}, status=200, headers={'token': 'test'})
```



#### 全局配置

* 全局的视图类，所有请求，都有效

==**drf有默认的配置文件-----》先从项目的`settings.py`中找，找不到，采用默认的**==

* 在settings.py中添加如下代码

```python
# 这个变量REST_FRAMEWORK，里面都是drf的配置信息
REST_FRAMEWORK = {
    'DEFAULT_RENDERER_CLASSES': [             # 默认响应渲染类
        'rest_framework.renderers.JSONRenderer', # json渲染器
        'rest_framework.renderers.BrowsableAPIRenderer', # 浏览器API渲染器
    ]
}


#如果设置了上述代码，其实是没有变化的，浏览器还显示浏览器的样式，postman还显示josn格式数据
```

## ==（8）视图==

```python
#两个视图基类
APIView
GenericAPIView(继承APIView)，
涉及到数据库和序列化类的操作，尽量用GenericAPIView
```

**先写模型类和序列化类，然后配置路由**

* models.py

```python
from django.db import models


# Create your models here.
class Book(models.Model):
    name = models.CharField(max_length=32)
    price = models.DecimalField(max_digits=8, decimal_places=3)
    publish = models.CharField(max_length=32)
```

* ser.py

```python
# -*- coding: UTF-8 -*- 
# @Date ：2022/12/8 11:18
from rest_framework import serializers
from App3.models import Book


class BookSerializer(serializers.ModelSerializer):
    class Meta:
        model = Book
        fields = '__all__'
```

* urls.py

```python
from django.urls import path, re_path
from App3 import views

urlpatterns = [
    
]
```

#### 1、基于APIView写5个接口

```python
from rest_framework.views import APIView
from App3.ser import BookSerializer
from App3.models import Book
from rest_framework.response import Response


class BookView(APIView):
    # 获取所有
    def get(self, request):
        book_list_obj = Book.objects.all()
        book_ser = BookSerializer(book_list_obj, many=True)
        return Response(book_ser.data)

    def post(self, request):
        book_ser = BookSerializer(data=request.data)
        if book_ser.is_valid():
            book_ser.save()
            return Response(book_ser.data)
        else:
            return Response({'status': 101, 'msg': '校验失败'})


class BookDetailView(APIView):
    # 获取单条
    def get(self, request, pk):
        book_obj = Book.objects.filter(pk=pk).first()
        book_ser = BookSerializer(book_obj)
        return Response(book_ser.data)

    def put(self, request, pk):
        book_obj = Book.objects.filter(pk=pk).first()
        book_ser = BookSerializer(instance=book_obj, data=request.data)
        if book_ser.is_valid():
            book_ser.save()
            return Response(book_ser.data)
        else:
            return Response({'status': 101, 'msg': '校验失败'})

    def delete(self, request, pk):
        book_obj = Book.objects.filter(pk=pk).delete()
        return Response({'status': 100, 'msg': '删除成功'})
```

```python
"""基于APIView的路由配置"""
path('books/', views.BookView.as_view()),
re_path('book/(?P<pk>\d+)', views.BookDetailView.as_view()),
```

#### 2、基于GenericAPIView写5个接口

```python
"""基于GenericAPIView写的5个接口"""


class BookGenericView(GenericAPIView):
    # queryset要传QuerySet对象
    # serializer_class要传使用哪个序列化类来序列化数据
    queryset = Book.objects.all()
    serializer_class = BookSerializer

    # 获取所有
    def get(self, request):
        book_list_obj = self.get_queryset()
        # book_ser=self.get_serializer_class()(book_list_obj,many=True)
        book_ser = self.get_serializer(book_list_obj,many=True)
        return Response(book_ser.data)
	
    def post(self, request):
        # book_ser = self.get_serializer_class()(data=request.data)
        book_ser = self.get_serializer(data=request.data)
        if book_ser.is_valid():
            book_ser.save()
            return Response(book_ser.data)
        else:
            return Response({'status': 101, 'msg': '校验失败'})


class BookGenericDetailView(GenericAPIView):
    queryset = Book.objects
    serializer_class = BookSerializer

    # 获取单条
    def get(self, request, pk):
        book_obj = self.get_object()
        # book_ser=self.get_serializer_class()(book_obj)
        book_ser = self.get_serializer(book_obj)
        return Response(book_ser.data)

    def put(self, request, pk):
        book_obj = self.get_object()
        # book_ser = self.get_serializer_class()(instance=book_obj,data=request.data)
        book_ser = self.get_serializer(instance=book_obj,data=request.data)
        if book_ser.is_valid():
            book_ser.save()
            return Response(book_ser.data)
        else:
            return Response({'status': 101, 'msg': '校验失败'})

    def delete(self, request, pk):
        book_obj = self.get_object().delete()
        return Response({'status': 100, 'msg': '删除成功'})
```

```python
"""基于GenericAPIView的路由配置"""
path('books2/', views.BookGenericView.as_view()),
re_path('book2/(?P<pk>\d+)', views.BookGenericDetailView.as_view()),
```

#### 3、基于GenericAPIView和5个视图扩展类写的接口

```python
父类都是object
ListModelMixin, 
CreateModelMixin,
UpdateModelMixin, 
DestroyModelMixin, 
RetrieveModelMixin
```



```python
from rest_framework.mixins import ListModelMixin, CreateModelMixin, UpdateModelMixin, DestroyModelMixin, RetrieveModelMixin


# views.py
class Book3View(GenericAPIView, ListModelMixin, CreateModelMixin):
    queryset = Book.objects
    serializer_class = BookSerializer

    def get(self, request):
        return self.list(request)

    def post(self, request):
        return self.create(request)


class Book3DetailView(GenericAPIView, RetrieveModelMixin, DestroyModelMixin, UpdateModelMixin):
    queryset = Book.objects
    serializer_class = BookSerializer

    def get(self, request, pk):
        return self.retrieve(request, pk)

    def put(self, request, pk):
        return self.update(request, pk)

    def delete(self, request, pk):
        return self.destroy(request, pk)

 
#urls.py
path('books3/', views.Book3View.as_view()),
re_path('books3/(?P<pk>\d+)', views.Book3DetailView.as_view()),
        
```

#### 4、基于GenericAPIView写的9个视图子类

```python
# 继承了GenericAPIView+一个或两者或三个试图扩展类
CreateAPIView
ListAPIView
RetrieveAPIView
DestroyAPIView
UpdateAPIView
ListCreateAPIView
RetrieveUpdateAPIView
RetrieveDestroyAPIView
RetrieveUpdateDestroyAPIView
```



```python
#views.py
from rest_framework.generics import CreateAPIView, ListAPIView, UpdateAPIView, RetrieveAPIView, DestroyAPIView,ListCreateAPIView,RetrieveUpdateDestroyAPIView,RetrieveUpdateAPIView,RetrieveDestroyAPIView


class Book4View(ListAPIView, CreateAPIView):
    queryset = Book.objects
    serializer_class = BookSerializer


class Book4DetailView(UpdateAPIView,RetrieveAPIView,DestroyAPIView):
    queryset = Book.objects
    serializer_class = BookSerializer

     
#urls.py
path('books4/', views.Book4View.as_view()),
re_path('books4/(?P<pk>\d+)', views.Book4DetailView.as_view()),

```

#### 5、使用ModelViewSet编写五个接口

```python
#views.py
from rest_framework.viewsets import ModelViewSet

class Book5View(ModelViewSet):
    queryset = Book.objects
    serializer_class = BookSerializer
    
#urls.py
path('books5/', views.Book5View.as_view(actions={'get': 'list', 'post': 'create'})),
# 当路径匹配，又是get请求，会执行Book5View的list方法

re_path('books5/(?P<pk>\d+)',views.Book5View.as_view(actions={'get': 'retrieve', 'put': 'update', 'delete': 'destroy'})),
```

#### 6、ViewSetMixin源码分析

```python
#重写了as_view()

# 路由中只要配置了对应关系，比如：{'get':'list'},当get请求来，就会执行list方法
def view(request, *args, **kwargs):
    self = cls(**initkwargs)

    if 'get' in actions and 'head' not in actions:
        actions['head'] = actions['get']

    
    self.action_map = actions

   
    for method, action in actions.items():
        #method:get
        #action:list,
        handler = getattr(self, action)
        #执行完上一句，handler就变成了list的内存地址
        setattr(self, method, handler)
        #执行完上一句，对象.get=list
        #for循环完毕，对象,get:list      对象.post:create

    self.request = request
    self.args = args
    self.kwargs = kwargs

   
    return self.dispatch(request, *args, **kwargs)
```

#### 7、继承ViewSetMixin的视图类

```python
#继承ViewSetMixin的视图类，路由可以改写,视图类里的方法名字随意

#views.py
from rest_framework.viewsets import ViewSetMixin

class Book6View(ViewSetMixin, APIView):  # ViewSetMinxin一定要放在APIView前，
    """继承的查找顺序：自己里面找，如果没有，先去第一个父类里找，再找第二个父类，"""
    def get_all_book(self, request):
        book_obj = Book.objects.all()
        book_ser = BookSerializer(book_obj, many=True)
        return Response(book_ser.data)

    
#urls.py
path('books6/', views.Book6View.as_view(actions={'get': 'get_all_book'})),
```

## ==（9）路由Routers==

对于视图集ViewSet，除了可以自己手动指明请求方式与动作action之间，还可以使用Routers来快速实现路由信息

**`rest framework`提供了两个router**

* **SimpleRouter**
* **DefaultRouter**

```python
#1.在urls.py中配置
path('books/', views.BookView.as_view()),
re_path('book/(?P<pk>\d+)', views.BookDetailView.as_view()),

#2.一旦视图类，继承了ViewSetMixin，路由
  path('books5/', views.Book5View.as_view(actions={'get': 'list', 'post': 'create'})),
re_path('books5/(?P<pk>\d+)',views.Book5View.as_view(actions={'get': 'retrieve', 'put': 'update', 'delete': 'destroy'})),

#3.继承自视图类，ModelViewSet的路由写法（自动生成路由）
```

#### 1、自动生成路由

```python
# urls.py

# 第一步，导入routers模块
	from rest_framework import routers
# 第二部，有两个类,实例化得到对象
    # routers.DefaultRouter:生成的路由更多，
    # routers.SimpleRouter
	router = routers.SimpleRouter()
# 第三步，注册
    # router.register(prefix='前缀', viewset=继承自ModelViewSet视图类,basename='别名')
    router.register(prefix='books', viewset=views.BookViewSet)
# 第四步，自动生成路由
	# print(router.urls)
    urlpatterns += router.urls
```

```python
#models.py
from django.db import models
class Book(models.Model):
    name = models.CharField(max_length=32)
    price = models.DecimalField(max_digits=8, decimal_places=3)
    publish = models.CharField(max_length=32)

#ser.py
from rest_framework import serializers
from app01.models import Book
class BookSerializer(serializers.ModelSerializer):
    class Meta:
        model = Book
        fields = '__all__'
               
#views.py
from rest_framework.viewsets import ModelViewSet
from app01.models import Book
from app01.serializer import BookSerializer
class BookViewSet(ModelViewSet):
    queryset = Book.objects
    serializer_class = BookSerializer
```

#### 2、action的使用

```python
为了给继承自ModelsViewSet的视图类中自定义的函数也添加路由
```

```python
from rest_framework.viewsets import ModelViewSet
from rest_framework.response import Response
from rest_framework.decorators import action  # 装饰器
from app01.models import Book
from app01.serializer import BookSerializer

class BookViewSet(ModelViewSet):
    queryset = Book.objects.all()
    serializer_class = BookSerializer

    # methods传一个列表，列表中放请求方式，
    # detail，布尔类型,
    #
    @action(methods=['get'], detail=False)
    # ^books/get_1/$ [name='book-get-1'] # 朝这个地址发送get请求，会执行下面的函数
    def get_1(self, request):
        book_obj = self.get_queryset()[:2]  # 从0开始截取一条
        ser = self.get_serializer(book_obj, many=True)
        return Response(ser.data)

    @action(methods=['get'], detail=True)
    # 生成  ^books/(?P<pk>[^/.]+)/get_1/$ [name='book-get-1']
    def get_2(self, request, pk):
        book_obj = self.get_queryset()[:2] 
        ser = self.get_serializer(book_obj, many=True)
        return Response(ser.data)
    
#装饰器放在被装饰的函数中，methods:请求方式，detail:是否带pk
```

![image-20221208183641841](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221208183644.png)

![image-20221208183956318](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221208183958.png)

![image-20221208183826571](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221208183829.png)

## ==（10）认证Authentication==

#### 1、认证的写法

```python
#认证的实现
 -1.写一个类，继承BaseAuthentication，重写authenticate,认证的逻辑写在里面,返回两个值，一个值最终给了Request对象的user，,如果认证失败就抛异常:AuthenticationFailed
 -2.全局使用，局部使用
```

#### 2、认证的源码分析

```python
# 1. APIView-----》dispatch方法-----》self.initial(request, *args, **kwargs)----》有认证，权限，频率
# 2.  只读认证源码 initial-----》 self.perform_authentication(request)

    def perform_authentication(self, request):
     	#去Resquest类中查找，方法属性user的get方法
        request.user
        
# 3. self.perform_authentication(request)就一句话:   request.user ,需要去drf的Request对象中找user属性（方法）
    class Request:
        @property
        def user(self):
            if not hasattr(self, '_user'):
                with wrap_attributeerrors():
                    
                    self._authenticate()
                    
            return self._user

# 4. Request类中的user方法，刚开始，没有_user,所以走self._authenticate()
	    @property
    def user(self):
        if not hasattr(self, '_user'):
            with wrap_attributeerrors():
                self._authenticate()
        return self._user
    
# 5. 核心 就是 Request类的_authenticate(self)
    def _authenticate(self):
        
        #遍历拿到一个个认证器，进行认证
        # self.authenticators配置的一堆认证类产生的认证类对象组成的list
        #self.authenticators在视图类中配置的一个个认证类: authentication_classes=[认证类1,认证类2],对象的列表
        #每次循环，拿到一个认证类的对象
        for authenticator in self.authenticators:
            try:
                #认证器（对象）调用认证方法authenticate(认证类对象self,request请求对象)
                #返回值：登录的用户与认证的信息组成的tuple
                #该方法被try包裹，代表该方法会抛异常，抛异常就代表认证失败
                user_auth_tuple = authenticator.authenticate(self)
            except exceptions.APIException:
                self._not_authenticated()
                raise

            if user_auth_tuple is not None:
                self._authenticator = authenticator
                #如何有返回值，就将登录用户 与 登录认证 分别保存到 request.user,request.auth
                self.user, self.auth = user_auth_tuple
                return
        #如果返回值user_auth_tuple为空，代表认证通过，但是没有 登录用户 与 登录认证信息，代表游客
        self._not_authenticated()

```

#### 3、认证组件的使用

```python
# 写一个认证类,app_auth.py，名字随意

from rest_framework.authentication import BaseAuthentication
from rest_framework.exceptions import AuthenticationFailed
from app01.models import UserToken

class MyAuthentication(BaseAuthentication):
    def authenticate(self, request):
        # 认证逻辑，如果认证通过，返回两个值
        # 如果认证失败，抛出AuthenticationFailed
        token = request.GET.get('token')
        if token:
            user_token = UserToken.objects.filter(token=token).first()
            # 认证通过
            if user_token:
                return user_token.user,token
            else:
                raise AuthenticationFailed('认证失败')
        else:
            raise AuthenticationFailed('请求地址中需要携带token')


```

##### 全局配置

```python
#可以有多个认证，从左到右依次执行
#全局使用,在settings.py中配置
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        "app01.app_auth.MyAuthentication"
    ]
}

```

##### 局部使用

```python
#局部使用，在视图类上写
authentication_classes = [MyAuthentication]
```

```python
#urls.py
from django.urls import path
from app01 import views
from rest_framework import routers

router = routers.SimpleRouter()
router.register(prefix='books', viewset=views.BookViewSet)
urlpatterns = [
    path('login/', views.LoginView.as_view()),
]
urlpatterns += router.urls


#views.py
from rest_framework.viewsets import ModelViewSet
from rest_framework.decorators import action  # 装饰器
from app01.models import Book
from app01.serializer import BookSerializer
from app01.app_auth import MyAuthentication

class BookViewSet(ModelViewSet):
    
    authentication_classes = [MyAuthentication]
    
    queryset = Book.objects.all()
    serializer_class = BookSerializer

    @action(methods=['get'], detail=False)
    def get_1(self, request):
        book_obj = self.get_queryset()[:2]  # 从0开始截取一条
        ser = self.get_serializer(book_obj, many=True)
        return Response(ser.data)

 
from rest_framework.views import APIView
from rest_framework.response import Response
from app01 import models
import uuid

class LoginView(APIView):
    def post(self, request):
        username = request.data.get('username')
        password = request.data.get('password')
        user = models.User.objects.filter(username=username, password=password).first()
        if user:
            # 生成随机字符串
            res = uuid.uuid4()
            #存到usertoken表中
            # models.UserToken.objects.create(token=res,user=user)#用它每次登录都会存一条数据，不好，
            models.UserToken.objects.update_or_create(defaults={'token':res},user=user)
            return Response({'status': 100, 'msg': '登录成功', 'token':res})

        return Response({'status':101,'msg':'用户名或密码错误'})
```



##### 局部禁用

```python
#局部禁用
配置了全局后可以局部禁用
authentication_classes = []
```

```python
class BookViewSet(ModelViewSet):
    authentication_classes = [MyAuthentication]
    queryset = Book.objects.all()
    serializer_class = BookSerializer

    @action(methods=['get','post'], detail=False)
    def get_1(self, request):
        book_obj = self.get_queryset()[:2]
        ser = self.get_serializer(book_obj, many=True)
        return Response(ser.data)


class LoginView(APIView):
    authentication_classes = []

    def post(self, request):
        username = request.data.get('username')
        password = request.data.get('password')
        user = User.objects.filter(username=username, password=password).first()
        if user:
            token = uuid.uuid4()
            UserToken.objects.update_or_create(defaults={'token': token}, user=user)
            return Response({'status': 100, 'msg': '登录成功', 'token': token})
        else:
            return Response({'status': 101, 'msg': '用户名或密码错误'})
```

![image-20221209205742421](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221209205744.png)

![image-20221209205643076](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221209205647.png)

## ==（11）权限Permissions==

权限控制可以限制用户对于视图的访问和对于具体数据对象的访问。

* 在执行视图的dispatch方法前，会先进行视图访问权限的判断
* 在通过get_object()获取具体对象时，会进行模型对象访问权限的判断

**区分不同的用户访问不同 的接口**

#### 1、源码分析

```python
#APIView----》dispatch-----》initial-----》self.check_permissions(reqeust)  (APIView的对象方法)
	    def check_permissions(self, request):
        #遍历权限对象列表得到一个权限对象 （权限器），进行权限认证
        for permission in self.get_permissions():#权限类的对象，放到列表中
            #权限类一定有一个has_permission权限方法，用来做权限认证的
            #参数:权限对象self、请求对象request、视图类对象
            #返回值:有权限返回True，无权限返回False,
            if not permission.has_permission(request, self):
                self.permission_denied(
                    request,
                    message=getattr(permission, 'message', None),
                    code=getattr(permission, 'code', None)
                )
```

![image-20221209215104778](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221209215108.png)

#### 2、自定义权限

```python
#写一个类，继承BasePermission，重写has_permission，如果权限通过，就返回True,不通过返回False
from rest_framework.permissions import BasePermission


class UserPermission(BasePermission):
    def has_permission(self, request, view):
        # 不是超级用户不能访问，
        # 由于已经认证过了，所有request里有user对象，(当前登录的用户)
        user = request.user  # 当前登录用户
        print(user.get_user_type_display())
        # 该字段用来choices参数，通过get_字段名_display()就能取出choices后面的中文
        if user.user_type == 1:
            return True
        else:
            return False
```

##### 局部使用

```python
path('test/', views.TestView.as_view()),
path('test2/', views.Test2View.as_view()),
    
    
# 只有超级用户可以访问
from app01.app_auth import UserPermission
class TestView(APIView):
    authentication_classes = [MyAuthentication]
    permission_classes = [UserPermission]

    def get(self, request, *args, **kwargs):
        return Response('我是测试数据1111')
        # return Response(True)


# 只要登录用户就可以访问
class Test2View(APIView):
    authentication_classes = [MyAuthentication]

    def get(self, request, *args, **kwargs):
        return Response('我是测试数据2222')

```

****

只有登录后，才能认证，才能有权限

![image-20221209215635389](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221209215921.png)

sb用户不是管理员用户，所有不能访问test路由

![image-20221209215331678](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221209215335.png)

但是sb用户 可以访问test2路由，只要登录了就能访问

![image-20221209215906414](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221209215910.png)

##### 全局使用

```python
#settings.py

REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        "app01.app_auth.MyAuthentication"
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'app01.app_auth.UserPermission',
    ],
}

```

加了全局配置后，test2就不能访问了，也需要超级管理员才能访问，这首就需要**局部禁用**,加个空列表就可可以了

##### 局部禁用

```python
class Test2View(APIView):
    authentication_classes = [MyAuthentication]
    permission_classes = []
    def get(self, request, *args, **kwargs):
        return Response('我是测试数据2222')

```





#### 3、内置权限

##### IsAdminUser使用

```PYTHON
先创建超级用户 python manage.py createsuperuser
并登录。
```

![image-20221209222802457](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221209222806.png)

```python
#views.py
from rest_framework.permissions import IsAdminUser
from rest_framework.authentication import BasicAuthentication,SessionAuthentication
# 超级管理员可以查看
class Test3View(APIView):
    authentication_classes = [SessionAuthentication]
    permission_classes = [IsAdminUser]

    def get(self, request, *args, **kwargs):
        return Response('我是测试数据3333')
    
    
#urls.py
 path('test3/', views.Test3View.as_view()),
    
```

先登录到admin,再访问test3路由，就有quan'xia

![image-20221209222849719](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221209222853.png)

## ==（12）频率限制==

```python
可以对接口访问的频次进行限制，以减轻服务器压力
一般用于付费购买次数，投票等场景使用
```

### 1、内置的频率限制

#### 未登录用户访问频次

**全局使用:限制未登录用户1分钟访问五次**

```python
#settings.py
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_CLASSES': ['rest_framework.throttling.AnonRateThrottle'],
    'DEFAULT_THROTTLE_RATES': {
       
        'anon': '5/m',
    },
}

```

```python
# 演示全局未登录用户访问频率
class Test4View(APIView):
    authentication_classes = []
    permission_classes = []

    def get(self, request, *args, **kwargs):
        return Response('我是未登录用户')
```



访问5次后就会出现下图所示:

![image-20221210085900164](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221210085901.png)

**内置使用:局部未登录用户的访问频次**

```python
from rest_framework.throttling import AnonRateThrottle


# 演示局部未登录用户的访问频次
class Test5View(APIView):
    authentication_classes = []
    permission_classes = []
    throttle_classes = [AnonRateThrottle]

    def get(self, request, *args, **kwargs):
        return Response('我是未登录用户')
```

**内置使用要注释掉全局配置中的下面这句代码**

```python
REST_FRAMEWORK = {
   # 'DEFAULT_THROTTLE_CLASSES': ['rest_framework.throttling.AnonRateThrottle'],
   
}

```



#### 登录用户访问频次

**全局使用**

```python
#settings.py

REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_CLASSES': [    
    'rest_framework.throttling.UserRateThrottle'
    ],
    'DEFAULT_THROTTLE_RATES': {
        'user': '10/m',
    },
}

```

**未登录用户一分钟访问5次，登录用户一分钟访问10次**

```python
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_CLASSES': [
        'rest_framework.throttling.AnonRateThrottle',
        'rest_framework.throttling.UserRateThrottle'
    ],
    'DEFAULT_THROTTLE_RATES': {
        'user': '10/m',
        'anon': '5/m',
    },
}

局部配置，
	再视图类中配一个就行，禁用的话加中括号就行
```

```python
# 未登录用户一分钟访问5次，登录用户一分钟访问10次
from rest_framework.authentication import SessionAuthentication

class Test6View(APIView):
    authentication_classes = [SessionAuthentication]
    # permission_classes = []
    # throttle_classes = [AnonRateThrottle]

    def get(self, request, *args, **kwargs):
        return Response('我是未登录用户')
```

#### 根据IP进行频率限制

```python
#全局配置
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_RATES': {
        'zhao': '5/m',
    },

}
```

```python
#utils.throttling.py

#写一个类，继承SimpleRateThrottle，只需要重写get_cache_key方法
from rest_framework.throttling import SimpleRateThrottle


class Mythrottling(SimpleRateThrottle):
    scope = 'zhao'

    def get_cache_key(self, request, view):
        print(request.META.get('REMOTE_ADDR'))
        return request.META.get('REMOTE_ADDR')#返回什么就根据什么来限制

# python manage.py runserver 0.0.0.0:8080 局域网可以相互访问

#settings.py
ALLOWED_HOSTS = [*]
```

```python
path('books3/', views.BookView.as_view()),
#views.py
from utils.throttling import Mythrottling
class BookView(APIView):
    throttle_classes = [Mythrottling]
    def get(self, request, *args, **kwargs):
        book_list = Book.objects.all()
        # 实例化得到分页器对象
        page_cursor = MyPageNumberPagination()
        book_list = page_cursor.paginate_queryset(book_list, request, view=self)
        next_url = page_cursor.get_next_link()  # 下一页
        pre_url = page_cursor.get_previous_link()  # 上一页
        print(next_url)
        print(pre_url)
        book_ser = BookModelSerializer(book_list, many=True)
        return Response(data=book_ser.data)
```

![image-20221210205915047](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221210205917.png)







### 2、自定义频率限制

```python
#子定制频率类需要写两个方法
	-判断是否限次，没有限次可以请求True，限次了不可以请求False 
        def allow_request(self, request, view):
   -限次后调用，显示还需要等待多长时间才能再次访问，返回等待的时间seconds
	   def wait(self):
```

```python
#utils.throttling.py中
# 自定义逻辑
# （1）取出访问者IP
# (2)判断当前IP在不在访问字典里，不再就添加进去，并且直接返回True，表示第一次访问，在字典里就继续往下走
# （3）循环判断当前IP的列表，有值，并且当前时间减去列表的最后一个时间大于60s，把这种数据pop掉，这样列表中只有60s以内的访问
# （4）判断，当列表小于3，说明一分钟以内访问不足三次，把当前时间插入到列表第一个位置，返回True，顺利通过
# （5)当大于等于3，说明一分钟内访问超过三次 ，返回False,验证失败
class IPThrottle():
    VISIT_DIC = {}  # 定义成类属性，所有的对用用的都是这一个

    def __init__(self):
        self.history_list = []

    def allow_request(self, request, view):
        ctime = time.time()
        ip = request.META.get("REMOTE_ADDR")
        if ip not in self.VISIT_DIC:
            self.VISIT_DIC[ip] = [ctime, ]
            return True
        self.history_list = self.VISIT_DIC[ip]  # 当前访问者的时间列表
        while True:
            if ctime - self.history_list[-1] > 60:
                self.history_list.pop()  # 把最后一个移除
            else:
                break
        if len(self.history_list) < 3:
            self.history_list.insert(0, ctime)
            return True
        else:
            return False

    def wait(self):
        # 当前时间减去列表中最后一个时间
        ctime = time.time()

        return 60 - (ctime - self.history_list[-1])
```

全局配置

```python
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_CLASSES':(
        'utils.throttling.IPThrottle',
    ),
}
```

## （13）过滤组件

对于列表数据可能需要根据字段进行**过滤**，我们可以通过添加django-filter扩展来增强支持

```python
pip install django-filter
```

在配置文件中添加过滤后端的设置

```python 
INSTALLED_APPS=[
    'django_filter',#需要注册应用
]

#全局配置，也可以局部配置
REST_FRAMEWORK = {
	'DEFAULT_FILTER_BACKENDS': ['django_filters.rest_framework.DjangoFilterBackend'],
}


```

**在视图类中添加 filterset_fields类属性，指定可以过滤的字段**

```python
from rest_framework.generics import ListAPIView
from app01.models import Book
from django_filters.rest_framework import DjangoFilterBackend

class BookView(ListAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
    filter_backends = [DjangoFilterBackend]  # 在视图中加上该类，就可以
    filterset_fields = ('name','price')   #配置按照哪个字段来过滤
    
    
#http://127.0.0.1:8000/books/?price=40
#http://127.0.0.1:8000/books/?name=活着
```

## （14）排序

在类视图中设置**`filter_backends`** 属性，使用**`rest_framework.filters.OrderingFilter`**过滤器，DRF会在**请求的查询字符串参数**中检查**是否包含了ordering参数**，如果包含了ordering参数，则**按照ordering参数指明的排序字段**对数据集进行排序后展示。

```python
from rest_framework.generics import ListAPIView
from rest_framework.filters import OrderingFilter
from app01.models import Book
from app01.serializer import BookSerializer


# # 排序组件使用
class Book2View(ListAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
    filter_backends = [OrderingFilter]  
    ordering_fields = ('id' ,'price',) # # 指明按照'id'和'price'字段的值的大小对数据进行排序后展示
    
    
#urls.py
path('books2/', views.Book2View.as_view()),

#必须是ordering = 某个值
#http://127.0.0.1:8000/books2/?ordering=price
# 127.0.0.1:8000/books2/?ordering=-id
# -id 表示针对id字段进行倒序排序
# id  表示针对id字段进行升序排序
```

![image-20221210105958743](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221210110000.png)

**根据`id`降序排序**

![image-20221210110027126](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221210110028.png)

**如果需要过滤后在排序，则选哟过滤和排序两者结合使用**

```python
from rest_framework.generics import ListAPIView
from rest_framework.filters import OrderingFilter
from django_filters.rest_framework import DjangoFilterBackend
from app01.models import Book
from app01.serializer import BookSerializer

#views.py
class Book3View(ListAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
    # 因为'filter_backends'是局部过滤配置，局部配置会覆盖'settinigs.py'文件中的全局配置,所以需要再次声明过滤组件核心类'DjangoFilterBackend',否则过滤功能会失效
    filter_backends = [DjangoFilterBackend, OrderingFilter]  # 在视图中加上该类，就可以
    filterset_fields = ('name', 'price')
    ordering_fields = ('id', 'price',)
    
    
#urls.py
path('books3/', views.Book3View.as_view()),


#http://127.0.0.1:8000/books3/?price=66&ordering=-id
#先过滤出价格为66的，再根据id字段降序排序
```

![image-20221210111252689](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221210111254.png)

## （15）异常处理，统一接口

```python
#全局配置
REST_FRAMEWORK = {
    'EXCEPTION_HANDLER': 'app01.app_auth.exception_handler',
}
#统一接口的返回
```

```python
#app_auth.py
# 自定义异常处理的方法
from rest_framework.views import exception_handler
from rest_framework.response import Response
from rest_framework import status


def my_exception_handler(exc, context):
    response = exception_handler(exc, context)
    # 两种情况，一种是None,def没有处理
    # 还有一种是response对象，django处理，但不太符合咱自己的要求
    if not response:
        return Response(data={'status': 999, 'msg': str(exc)}, status=status.HTTP_400_BAD_REQUEST)
    else:
        # return response
        return Response(data={'status': 888, 'msg': response.data.get('detail')}, status=status.HTTP_400_BAD_REQUEST)
```

## （16）封装Response对象

```python
#utils.py
#自定制响应
from rest_framework.response import Response

class CommonResponse(Response):
    def __init__(self, code=10, msg='成功', data=None, **kwargs):
        dic = {'code': code, 'msg': msg, 'data': data}
        if data:
            dic = {'code': code, 'msg': msg, 'data': data}
        dic.update(kwargs)
        super().__init__(data=dic, status=None, headers=None, )
```

```python
#urls.py
path('test7/', views.Test7View.as_view()),

#views.py
from app01.utils import CommonResponse
class Test7View(APIView):
    def get(self, request, *args, **kwargs):
        return CommonResponse(data={'name':'zhao'},demo='kfdha',token='fldksjafld')
```

![image-20221210124805607](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221210124808.png)

```python
#views.py
from app01.utils import CommonResponse
class Test7View(APIView):
    def get(self, request, *args, **kwargs):
       
        return CommonResponse(token='fldksjafld')
```

![image-20221210124855617](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221210124858.png)

```python
class Test7View(APIView):
    def get(self, request, *args, **kwargs):
   
        return CommonResponse(code=101,msg='错误',data={'name':'lisi'},aa='falkda')

```

![image-20221210125027179](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221210125030.png)

## （17）drf数据的增删改查

#### 模型类

```python
from django.db import models


class BaseModel(models.Model):
    is_delete = models.BooleanField(default=False)
    # auto_now_add创建的时候，不需要手动插入时间，自动插入当前时间
    create_time = models.DateTimeField(auto_now_add=True)
    # auto_now,只要更新，就会把当前时间插入
    last_update_time = models.DateTimeField(auto_now=True)

    # import datetime
    # create_time = models.DateTimeField(default=datetime.datetime.now)

    class Meta:
        # 单个字段有索引，有唯一  db_index  unique
        # 联合起来，多个字段，有联合索引，联合唯一,,,index_together,unique_together

        abstract = True  # 抽象表，不再数据库中建立表


# Create your models here.
class Book(BaseModel):
    id = models.AutoField(primary_key=True)
    name = models.CharField(max_length=32, verbose_name='书名', help_text='这里填写书名')
    price = models.DecimalField(max_digits=8, decimal_places=2, verbose_name='价格', help_text='这里填写书的价格')
    # to_field 默认不写，关联到Publish主键
    # db_constraint=False,逻辑上的关联，实质上并没有外键联系，增删不会受外键影响，但是orm查询不影响
    publish = models.ForeignKey(to='Publish', on_delete=models.DO_NOTHING, db_constraint=False)

    # 社么时候用自动，什么时候用手动？
    # 第三张表只有关联字段，用自动，,第三张表有扩展字段，需要手动写，
    authors = models.ManyToManyField(to='Author', db_constraint=False)

    class Meta:
        verbose_name_plural = '书表'  # admin中表名的显示

    def __str__(self):
        return self.name

    @property
    def publish_name(self):
        return self.publish.name

    def author_list(self):
        authors_list=self.authors.all()
        # ll=[]
        # for author in authors_list:
        #     ll.append({'name': author.name,'sex':author.get_sex_display()})
        # # return ll
        return [{'name': author.name,'sex':author.get_sex_display()} for author in authors_list]




class Publish(models.Model):
    name = models.CharField(max_length=32)
    addr = models.CharField(max_length=64)

    class Meta:
        verbose_name_plural = '出版社表'

    def __str__(self):
        return self.name


class Author(BaseModel):
    name = models.CharField(max_length=32)
    sex = models.IntegerField(choices=((1, '男'), (2, '女')))
    author_detail = models.OneToOneField(to='AuthorDetail', db_constraint=False, on_delete=models.CASCADE)

    class Meta:
        verbose_name_plural = '作者表'


class AuthorDetail(BaseModel):
    mobile = models.CharField(max_length=11)

    class Meta:
        verbose_name_plural = '作者详情表'

    def __str__(self):
        return self.mobile

# models.CASCADE
# models.SET_DEFAULT
# models.SET_NULL
# models.DO_NOTHING
# 表断关联
# 1.表之间没有外键关联，但是有哦外键逻辑关联，（有充当外键的字段）
# 2.断关联后不会影响数据库查询效率，但是会极大提高数据库增删改效率，（不影响增删改查操作）
# 3.断关联一定要通过逻辑保证表之间数据的安全，不要出现脏数据，代码控制
# 4.断关联
# 5.级联关系：
# 作者没了，详情也没了，on_delete=models.CASCADE
# 出版社没了，书还是那个出版社出版 :on_delete=models.DO_NOTHING
# 部门没了，员工没有部门（空不能）：null=True,on_delete=models.SER_NULL
# 部门没了，员工进入默认部门（默认值）：default=0,on_delete=models.SET_DEFAULT
```

**写完后在admin.py中注册**

```python
from app01 import models

admin.site.register(models.Book)
admin.site.register(models.Author)
admin.site.register(models.AuthorDetail)
admin.site.register(models.Publish)
```

#### 序列化类

```python
# -*- coding: UTF-8 -*- 
# @Date ：2022/12/10 16:42
from rest_framework import serializers
from app01.models import Book


class BookListSerializer(serializers.ListSerializer):
    # def create(self, validated_data):
    #     print(validated_data)
    # super().__init__(validated_data)

    def update(self, instance, validated_data):
        print(validated_data)
        # 报错数据
        return [
            self.child.update(instance[i], attrs) for i, attrs in enumerate(validated_data)
        ]


class BookModelSerializer(serializers.ModelSerializer):
    # 如果序列化的是数据库的表，尽量使用ModelsSerializer
    # 第一种方案（序列化可以，反序列化有问题
    # publish = serializers.CharField(source='publish.name')

    # 第二中方案 ,models中写方法
    class Meta:
        list_serializer_class = BookListSerializer
        model = Book
        # fields = '__all__'
        # depth = 1
        fields = ('id','name', 'price', 'authors', 'publish', 'publish_name', 'author_list')
        extra_kwargs = {
            'publish': {'write_only': True},
            'publish_name': {'read_only': True},
            'authors': {'write_only': True},
            'author_list': {'read_only': True},
        }
```

#### 路由配置

```python
#urls.py
path('books/', views.BookAPIView.as_view()),
re_path('books/(?P<pk>\d+)', views.BookAPIView.as_view()),
```

#### 视图类

```python
#views.py

from rest_framework.views import APIView
from app01.models import Book
from app01.serialize import BookModelSerializer
from rest_framework.response import Response


class BookAPIView(APIView):
    def get(self, request, *args, **kwargs):
        # 查单个和查所有合并到一起
        # 查单条数据
        if kwargs:
            pk = kwargs.get('pk')
            book_obj = Book.objects.filter(pk=pk).first()
            book_ser = BookModelSerializer(book_obj)
            return Response(data=book_ser.data)
        # 查所有数据
        book_list = Book.objects.all().filter(is_delete=False)
        book_list_ser = BookModelSerializer(book_list, many=True)
        return Response(data=book_list_ser.data)

    def post(self, request, *args, **kwargs):
        # 具备增单条，和增多条的功能
        if isinstance(request.data, dict):
            book_ser = BookModelSerializer(data=request.data)
            book_ser.is_valid(raise_exception=True)
            book_ser.save()
            return Response(data=book_ser.data)
        elif isinstance(request.data, list):
            # 此时的book_ser是Listserializer对象
            book_ser = BookModelSerializer(data=request.data, many=True)  # 增多条
            book_ser.is_valid(raise_exception=True)
            book_ser.save()  # 调用的是Listserializer的save方法
            # 新增，会掉create方法，Listserializer的create方法
            return Response(data=book_ser.data)

    def put(self, request, *args, **kwargs):
        # 改一个
        if kwargs.get('pk', None):
            book_obj = Book.objects.filter(pk=kwargs).first()
            book_ser = BookModelSerializer(instance=book_obj, data=request.data, partial=True)
            book_ser.is_valid(raise_exception=True)
            book_ser.save()
            return Response(data=book_ser.data)
        else:
            book_list = []
            modify_data = []

            for item in request.data:
                pk = item.pop('id')
                book_obj = Book.objects.get(pk=pk)
                book_list.append(book_obj)
                modify_data.append(item)

            # for i,is_data in enumerate(modify_data):
            #     book_ser = BookModelSerializer(instance=book_list[i], data=is_data, many=True)
            #     book_ser.save()
            # return Response(book_ser.data)

            book_ser = BookModelSerializer(instance=book_list, data=modify_data, many=True)
            book_ser.is_valid(raise_exception=True)
            book_ser.save()
            return Response(book_ser.data)

    def delete(self, request, *args, **kwargs):
        # 单个删除，
        # 批量删除
        pk = kwargs.get('pk')
        pks = []
        if pk:
            pks.append(pk)
            # 不管单条删除还是多条删除都用多条删除
        else:
            pks = request.data.get('pks')
        # 把is_delete设置成True
        # ret返回受影响的行数
        ret = Book.objects.filter(pk__in=pks, is_delete=False).update(is_delete=True)
        if ret:
            return Response(data={'msg': '删除成功'})
        else:
            return Response(data={'msg': '没有要删除的数据'})
```

## （18）分页器

**三种分页方式:**

* PageNumberPagination
* LimitOffsetPagination
* CursorPagination

```python
#settings.py全局配置
REST_FRAMEWORK = {
    'PAGE_SIZE': 2,
}
```

#### PageNumberPagination

```python
# urls.py
path('books2/', views.BookListView.as_view()),#继承了ListAPIView


#views.py
from rest_framework.generics import ListAPIView
from rest_framework.pagination import PageNumberPagination, LimitOffsetPagination, CursorPagination

#重写
class MyPageNumberPagination(PageNumberPagination):
    page_size = 3  # 每页显示的条数
    # page_query_param = 'num'  # 前端发送的页数关键字，默认为'page'
    max_page_size = 5  # 每页最大显示条数
    page_size_query_param = 'size'#每一页显示的条数


class BookListView(ListAPIView):
    queryset = Book.objects.all()
    serializer_class = BookModelSerializer
    # 配置分页
    pagination_class = MyPageNumberPagination
    
    
#http://127.0.0.1:8000/api/books2/?num=2
#http://127.0.0.1:8000/api/books2/?num=2&size=6   #一页最多显示5数据
```

![image-20221210192345931](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221210192350.png)

![image-20221210192630387](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221210192632.png)

#### LimitOffsetPagination

```python
# urls.py
path('books2/', views.BookListView.as_view()),#继承了ListAPIView


#views.py
class MyLimitOffsetPagination(LimitOffsetPagination):
    default_limit = 3     #每页条数
    limit_query_param = 'limit'   #往后拿几条
    offset_query_param = 'offset'   #从第几个开始
    max_limit = 5

class BookListView(ListAPIView):
    queryset = Book.objects.all()
    serializer_class = BookModelSerializer
    # 配置分页

    pagination_class = MyLimitOffsetPagination
    
#http://127.0.0.1:8000/api/books2/?offset=6&limit=2       
```

![image-20221210192918656](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221210192920.png)

#### CursorPagination

**写出来只有上一页，下一页，不能跳**

```python
# urls.py
path('books2/', views.BookListView.as_view()),#继承了ListAPIView


#views.py
class MyCursorPagination(CursorPagination):
    page_size = 2  # 每一页显示的条数
    cursor_query_param = 'cursor'  # 每一页查询的key
    ordering = '-id'  # 排序


class BookListView(ListAPIView):
    queryset = Book.objects.all()
    serializer_class = BookModelSerializer
    # 配置分页

    pagination_class = MyCursorPagination

```

![image-20221210193535346](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221210193538.png)

![image-20221210193740135](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221210193743.png)

#### 使用APIView写分页

```python
#urls.py
path('books3/', views.BookView.as_view()),

#views.py
from rest_framework.pagination import PageNumberPagination, LimitOffsetPagination, CursorPagination
from rest_framework.views import APIView
from rest_framework.response import Response
from app01.models import Book
from app01.serialize import BookModelSerializer

#重写
class MyPageNumberPagination(PageNumberPagination):
    page_size = 3  # 每页显示的条数
    page_query_param = 'num'  # 前端发送的页数关键字，默认为'page'
    max_page_size = 5  # 每页最大显示条数
    page_size_query_param = 'size'  # 每一页显示的条数
    
class BookView(APIView):
    def get(self, request, *args, **kwargs):
        book_list = Book.objects.all()
        # 实例化得到分页器对象
        page_cursor = MyPageNumberPagination()
        book_list = page_cursor.paginate_queryset(book_list, request, view=self)
        next_url = page_cursor.get_next_link()  # 下一页
        pre_url = page_cursor.get_previous_link()  # 上一页
        print(next_url)#打印下一页的url
        print(pre_url)#打印下一页的url
        book_ser = BookModelSerializer(book_list, many=True)
        return Response(data=book_ser.data)
        
```



![image-20221210195511467](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221210195513.png)

![image-20221210195526353](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221210195528.png)

## （19）自动生成接口文档

rest framework可以自动帮助生成接口文档

接口文档以网页的方式呈现

自动接口文档能生成的是继承自`APIView`及其`子类`的视图

#### 1、安装依赖

`rest framework`生成接口文档需要`coreapi`库的支持

```python
pip install coreapi
```

#### 2、设置接口文档访问路径

**在总路由中添加接口文档路径**

文档路由对应的视图配置为`rest_framework.documentation. include_docs_urls`

```python
from rest_framework.documentation import include_docs_urls
urlpatterns = [
    ...
    path('docs/',include_docs_urls(title='站点页面标题'))
]

```

#### 3、文档描述说明的定义位置

1) 单一方法的视图，可以i直接使用视图类的文档字符串，如：

```python
class BookListView(ListAPIView):
    """
    返回所有图书信息
    """
```

2. 包含多个方法的视图，在类视图的文档字符串中，分开方法定义，如:

```python
class BookListAPIView(ListAPIView):
    """
    get:
    返回所有图书信息

    post:
    新建图书
    """
```

3. 对于视图集`ViewSet`,仍在类视图的文档字符串中分开定义，但是应使用`action`名称区分，如:

```python
from rest_framework import mixins
from rest_framework.viewsets import GenericViewSet

class BookInfoViewSet(mixins.ListModelMixin, mixins.RetrieveModelMixin, GenericViewSet):
   """
   list:
   返回图书列表数据

   retrieve:
   返回图书的详情数据

   latest:
   返回最新的图书数据

   read:
   修改图书的阅读量
   """
```

最后浏览器中输入，`http://127.0.0.1:8000/docs/`，报错

![image-20221211121826453](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221211121828.png)

需要在配置文件`settings.py`中设置如下:

```python
REST_FRAMEWORK = {
    'DEFAULT_SCHEMA_CLASS': 'rest_framework.schemas.coreapi.AutoSchema',
    #默认用的是:  'rest_framework.schemas.openapi.AutoSchema',
}
```

![image-20221211122824978](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221211122826.png)

![image-20221211122649995](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221211122651.png)

**注意：**

1. 视图集ViewSet中的retrieve名称在接口文档中叫做read
2. 参数的Description需要在模型类或者序列化器类的字段中以help_text选项定义，如下:

```python
class Book(BaseModel):
    id = models.AutoField(primary_key=True)
    name = models.CharField(max_length=32, verbose_name='书名', help_text='这里填写书名')
    price = models.DecimalField(max_digits=8, decimal_places=2, verbose_name='价格', help_text='这里填写书的价格')
```

或者

```python
class BookModelSerializer(serializers.ModelSerializer):
    class Meta:
        list_serializer_class = BookListSerializer
        model = Book
        fields = '__all__'
        extra_kwargs = {
            'publish': {
                'required':True,
                'write_only': True,
                'help_text':'出版社'
            }   
        }

```



## （20）JWT认证

在用户注册信息登陆后，想记录用户的登录状态，或者为用户创建身份认证的凭证，不再使用Session认证机制，而使用`Json Web Token`(本质就是**token**)认证机制

#### 1、构成

JWT就是一段字符串，由三段信息文本用`.`链接一起就构成了JWT字符串，

```python
aaaa.bbbb.cccc
```

第一个部分成为头部（header），第二部分成为载荷（payload），第三部分是签名（signature）

#### 2、原理

1.  jwt分三段式：头.体.签名 （head.payload.sgin）
2. 头和体是可逆加密，让服务器可以反解出user对象；签名是不可逆加密，保证整个token的安全性
3. 头体签名三部分，都是采用json格式的字符串，进行加密，可逆加密一般采用base64算法，不可逆加密一般采用hash(md5)算法
4. 头中的内容是基本信息：公司信息、项目组信息、token采用的加密方式信息

```python
{
  "company": "公司信息",
  ...
}
```

5. 体中的内容是关键信息：用户主键、用户名、签发时客户端信息(设备号、地址)、过期时间

```python
{
  "user_id": 1,
  "username":"zhangsan",
  ...
}
```

6. 签名中的内容时安全信息：头的加密结果 + 体的加密结果 + 服务器不对外公开的安全码 进行md5加密

```python
{
  "head": "头的加密字符串",
  "payload": "体的加密字符串",
  "secret_key": "安全码"
}
```

#### 3、校验

1. 将token按` . `拆分为三段字符串，第一段 头加密字符串 一般不需要做任何处理
2. 第二段 体加密字符串，要反解出用户主键，通过主键从User表中就能得到登录用户，过期时间和设备信息都是安全信息，确保token没过期，且是同一设备来的
3. 再用 第一段 + 第二段 + 服务器安全码 不可逆md5加密，与第三段 签名字符串 进行碰撞校验，通过后才能代表第二段校验得到的user对象就是合法的登录用户

#### 3、drf项目的jwt认证开发流程

```python
1）用账号密码访问登录接口，登录接口逻辑中调用 签发token 算法，得到token，返回给客户端，客户端自己存到cookies中
2）校验token的算法应该写在认证类中(在认证类中调用)，全局配置给认证组件，所有视图类请求，都会进行认证校验，所以请求带了token，就会反解出user对象，在视图类中用request.user就能访问登录的用户

注：登录接口需要做 认证 + 权限 两个局部禁用

#第三方写好的 django-rest-framework-jwt
```

#### 4、drf简单使用jwt

##### 安装

```python 
 pip install djangorestframework-jwt 
```

##### 继承AbstractUser表

```python
#新建项目，继承AbstractUser表，做数据库迁移，配置文件中配置AUTH_USER_MODEL = 'app01.User'   # '应用名.表名'

from django.contrib.auth.models import AbstractUser
class User(AbstractUser):
    phone = models.CharField(max_length=32)
    icon = models.ImageField(upload_to='icon')  # ImageField依赖于pillow模块
```

##### 创建超级用户

```python
python manage.py  createsuperuser 
```

##### 简单使用

```python
from django.contrib import admin
from django.urls import path
from rest_framework_jwt.views import obtain_jwt_token,ObtainJSONWebToken, VerifyJSONWebToken, RefreshJSONWebToken

# 基类：JSONWebTokenAPIView，继承了APIView
# ObtainJSONWebToken, VerifyJSONWebToken, RefreshJSONWebToken都继承了JSONWebTokenAPIView
"""
obtain_jwt_token = ObtainJSONWebToken.as_view()
refresh_jwt_token = RefreshJSONWebToken.as_view()
verify_jwt_token = VerifyJSONWebToken.as_view()
"""

urlpatterns = [
    path('admin/', admin.site.urls),
    path('login/',obtain_jwt_token)#用下面那个也可以
    # path('login/', ObtainJSONWebToken.as_view())
]

```

发送get请求会报错

 ![image-20221211134557687](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221211134559.png)

必须发post请求

![image-20221211134954891](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221211134956.png)

##### 简单认证使用

```python
#views.py

from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework_jwt.authentication import JSONWebTokenAuthentication

class TestView(APIView):
    authentication_classes = [JSONWebTokenAuthentication]
    def get(self, request):
        return Response('ok')
    
    
#urls.py
path('test/', views.TestView.as_view()),


#settings.py中要注册
INSTALLED_APPS = [
    'rest_framework',
    'rest_framework_jwt'
]

```

![image-20221211141310776](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221211141312.png)

**过期时间到了后会显示签名过期**

![image-20221211140933111](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221211140934.png)

#### 5、使用jwt自定制认证类

##### **基于BaseJSONWebTokenAuthentication写的**

```python
#auth.py，自定义认证类

from rest_framework_jwt.authentication import jwt_decode_handler
from rest_framework import exceptions
from rest_framework_jwt.authentication import BaseJSONWebTokenAuthentication


class MyToken(BaseJSONWebTokenAuthentication):
    def authenticate(self, request):
        jwt_value = str(request.META.get("HTTP_AUTHORIZATION"))  # 获取到token字符串
        # 认证
        try:
            payload = jwt_decode_handler(jwt_value)#把三段式解析出来，看认证是否篡改，是否过期
        except Exception:
            raise exceptions.AuthenticationFailed('认证失败')
        user = self.authenticate_credentials(payload)  # user是当前登录用户，
        return user, payload
```

```python
#views.py

from rest_framework.views import APIView
from rest_framework.response import Response
from app01.auth import MyToken

class TestView(APIView):
    authentication_classes = [MyToken]

    def get(self, request):
        print(request.user)
        return Response('ok')
  
#urls.py
path('test/', views.TestView.as_view()),    
```

##### **基于BaseAuthentication写的**

```python
from rest_framework.authentication import BaseAuthentication
from rest_framework_jwt.authentication import jwt_decode_handler
from rest_framework.exceptions import AuthenticationFailed
from rest_framework_jwt.utils import jwt_decode_handler  # 上面按个那个用哪个都一样
import jwt
from api.models import User


class MyJwtAuthenticationnnnn(BaseAuthentication):
    def authenticate(self, request):
        jwt_value = request.META.get("HTTP_AUTHORIZATION")
        if jwt_value:
            try:
                # jwt提供了通过三段token,取出payload的方法，并且还有校验功能
                payload = jwt_decode_handler(jwt_value)
            except jwt.ExpiredSignature:
                raise AuthenticationFailed('签名过期')
            except jwt.InvalidTokenError:
                raise AuthenticationFailed('非法用户')
            except Exception as e:
                raise AuthenticationFailed(str(e))
            # 因为payload就是用户信息 的字典，
            print(payload)  # {'user_id': 1, 'username': 'zhao', 'exp': 1670811589, 'email': '13644@qq.com'}
            # return payload, jwt_value
            """需要得到user对象，第一种方式，取数据库查"""
            # user = User.objects.filter(pk=payload.get('user_id'))
            """第二种方式"""
            user = User(id=payload.get('user_id'), username=payload.get('username'))
            return user, jwt_value

        # 没有值直接抛异常
        raise AuthenticationFailed('没有携带认证信息')
```

```python
#urls.py
path('goods/',views.GoodsInfoAPIView.as_view()),

#views.py
from app02.utils import MyJwtAuthenticationnnnn


class GoodsInfoAPIView(APIView):
    authentication_classes = [MyJwtAuthenticationnnnn]

    def get(self, request, *args, **kwargs):
        print(request.user)
        return Response('商品信息')

```

![image-20221212102326109](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221212102327.png)

![image-20221212102810487](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221212102812.png)

#### 6、jwt控制访问权限

**控制用户登录后才能访问，和不登陆就能访问**

```python
#1.控制用户登录后才能访问，和不登陆就能访问

from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework_jwt.authentication import JSONWebTokenAuthentication
from rest_framework.permissions import IsAuthenticated

"""
可以通过认证类JSONWebTokenAuthentication和权限类IsAuthenticated，来控制用户登陆以后才能访问某些接口
如果用户不登陆就能访问，只需要把权限类IsAuthenticated去掉
"""

class OrderAPIView(APIView):   #登录才能访问
    authentication_classes = [JSONWebTokenAuthentication]
    # 权限控制
    permission_classes = [IsAuthenticated]
    def get(self, request, *args, **kwargs):
        return Response('这是订单信息')


class UserInfoAPIView(APIView):    #不登陆就可以访问
    authentication_classes = [JSONWebTokenAuthentication]
    def get(self, request, *args, **kwargs):
        return Response('UserInfoAPIView')
    
    
#urls.py
#子路由
from rest_framework_jwt.views import obtain_jwt_token
urlpatterns = [
  path('login/',obtain_jwt_token),
  path('order/',views.OrderAPIView.as_view()),
  path('userinfo/',views.UserInfoAPIView.as_view())
]
#总路由
 path('app02/', include('app02.urls')),

```

登陆后才可访问

![image-20221211203722091](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221211203723.png)

不登陆就可访问

![image-20221211203516209](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221211203517.png)

#### 7、jwt控制返回自定义数据格式

**控制登录接口返回的数据格式**

```python
#2.控制登录接口返回的数据格式
 - 第一种方式，自己写登录接口
 - 第二种方式，用内置的，控制登录接口返回的数据格式
	-jwt的配置文件种有个这么个属性
    	  'JWT_RESPONSE_PAYLOAD_HANDLER': 'rest_framework_jwt.utils.jwt_response_payload_handler',
    -重写jwt_response_payload_handler，配置成自己的返回格式
```

```python
#在app02下创建utils.py

def myjwt_response_payload_handler(token, user=None, request=None):#返回什么，前端就能看到什么

    return {
        'token': token,
        'msg':'登录成功',
        'status':100,
        'username':user.username
    }
```

```python
#settings.py中配置自己重写的方法
JWT_AUTH={
    'JWT_RESPONSE_PAYLOAD_HANDLER':'app02.utils.myjwt_response_payload_handler'
}
```

再次登录

![image-20221211205139618](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221211205141.png)

#### 8、多方式登录,自动签发token

```python
#使用用户名，手机号，邮箱都可以登录
#前端需要传的数据格式
{
    "username":"zhao/13145786651/123@qq.com",
    "password":"123456"
}
```

模型类

```python
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    mobile = models.CharField(max_length=32, unique=True)  # 唯一
    
#settings.py中配置
AUTH_USER_MODEL = 'api.User'    '应用.表名'
```

序列化类

```python
from rest_framework import serializers
from api import models
import re
from rest_framework.exceptions import ValidationError
from rest_framework_jwt.utils import jwt_encode_handler, jwt_payload_handler


class LoginSerializer(serializers.ModelSerializer):
    username = serializers.CharField()

    class Meta:
        model = models.User
        fields = ['username', 'password']

    def validate(self, attrs):
        # 写逻辑校验
        username = attrs.get('username')  # 用户名有三种方式
        password = attrs.get('password')
        # 通过判断username数据不同，查询字段不一样
        # 通过正则匹配，如果是手机号
        if re.match('^1[3-9][0-9]{9}$', username):  # 手机号
            user = models.User.objects.filter(models=username).first()
        elif re.match('.+@.+$', username):  # 邮箱
            user = models.User.objects.filter(email=username).first()
        else:#用户名
            user = models.User.objects.filter(username=username).first()
        if user:  # 用户存在
            # 校验密码
            if user.check_password(password):
                # 签发token
                payload = jwt_payload_handler(user)
                token = jwt_encode_handler(payload)
                # self.aa = token
                self.context['token'] = token
                self.context['username'] = user.username
                return attrs
            else:
                raise ValidationError('密码错误')
        else:
            raise ValidationError('用户不存在')
```

视图类

```python
from rest_framework.views import APIView
from rest_framework.viewsets import ViewSetMixin, ViewSet
from app02 import serializer


# class LoginView(ViewSetMixin, APIView):
class LoginViewSet(ViewSet):  # 跟上面完全一样
    # def post(self, request):#不写post,直接写login
    #     #这是登录接口
    #     pass
    def login(self, request, *args, **kwargs):
        # 1.需要有一个序列化的类
        # 2.生成序列化类对象
        login_serializer = serializer.LoginSerializer(data=request.data,context={'request':request})
        # 3.调用序列化对象的is_validad
        login_serializer.is_valid(raise_exception=True)
        token = login_serializer.context.get('token')
        user = login_serializer.context.get('username')
        # 4. return
        return Response({'status': 100, 'msg': '登录成功', 'token': token,'username':user})
```

路由配置

```python
path('login2/', views.LoginViewSet.as_view({'post': 'login'})),
```

![image-20221212115825810](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221212115827.png)

![image-20221212115858977](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221212115900.png)

#### 9、配置过期时间

```python
#settings.py种手动配置

import datetime
JWT_AUTH={
    'JWT_EXPIRATION_DELTA': datetime.timedelta(days=7),#过期时间手动配置7天

}
```

## （21）基于角色的权限控制（django内置的auth体系)

```python
#RBAC（Role-Based Access Control：基于角色的访问控制）

#django的auth就是内置了一套RBAC的权限系统


#django中
	#后台权限控制（公司内部系统，crm客户关系管理,erp,协同平台）
	user表
    group表
    permission表
    user_group表是user和group的中间表
    group_permission表是group和permission的中间表
    user_permission表是user和permission的中间表
    #前台（主站），需要用三大认证，
```

## （22）Django缓存

[相关博客](https://www.cnblogs.com/liuqingzheng/articles/9803351.html)

在动态网站中,用户所有的请求,服务器都会去数据库中进行相应的增,删,查,改,渲染模板,执行业务逻辑,最后生成用户看到的页面.

当一个网站的用户访问量很大的时候,每一次的的后台操作,都会消耗很多的服务端资源,所以必须使用缓存来减轻后端服务器的压力.

缓存是将一些常用的数据保存内存或者memcache中,在一定的时间内有人来访问这些数据时,则不再去执行数据库及渲染等操作,而是直接从内存或memcache的缓存中去取得数据,然后返回给用户.



**缓存位置通过配置文件来操作（以文件缓存为例子**）

```python
# 前后端混合开发缓存的使用
#settings.py中配置

CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.filebased.FileBasedCache', #指定缓存使用的引擎
        'LOCATION': 'E:\cache测试',        #指定缓存的路径
        'TIMEOUT':300,              #缓存超时时间(默认为300秒,None表示永不过期)
        'OPTIONS':{
            'MAX_ENTRIES': 300,            # 最大缓存记录的数量（默认300）
            'CULL_FREQUENCY': 3,           # 缓存到达最大个数之后，剔除缓存个数的比例，即：1/CULL_FREQUENCY（默认3）
        }
    }
}

# 前后端分离缓存的使用
```

   **缓存的粒度：**
   ```python
   -全站缓存
   -单页面缓存
   	在视图函数上加装饰器
   -页面局部缓存
   ```

##### **单页面缓存**

```python
#views.py 
@cache_page(5)  # 表示缓存5秒中
def test_cache(request):
    import time
    ctime = time.time()
    return render(request, 'index.html', context={'ctime': ctime})
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
{{ ctime }}
</body>
</html>
```

![动画](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221212145245.gif)

##### **页面局部缓存**

刷新页面时,整个网页有一部分实现缓存

```python
def test_cache(request):
    import time
    ctime = time.time()
    return render(request, 'index.html', context={'ctime': ctime})
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!---<script src="../jQuery-3.6.0-min.js"></script>--->

</head>
<body>
{{ ctime }}

<hr>
页面局部缓存
{% load cache %}
{% cache 5 'name' %}   #5表示5秒钟，name是唯一key值
    {{ ctime }}
{% endcache %}
</body>
</html>
```

![动画](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221212145018.gif)

##### **全站缓存**

既然是全站缓存,当然要使用Django中的中间件.

用户的请求通过中间件,经过一系列的认证等操作,如果请求的内容在缓存中存在,则使用FetchFromCacheMiddleware获取内容并返回给用户

当返回给用户之前,判断缓存中是否已经存在,如果不存在,则UpdateCacheMiddleware会将缓存保存至Django的缓存之中,以实现全站缓存

```markdown
缓存整个站点，是最简单的缓存方法
在 MIDDLEWARE_CLASSES 中加入 “update” 和 “fetch” 中间件

MIDDLEWARE_CLASSES = (
    'django.middleware.cache.UpdateCacheMiddleware', #第一
     .....
     ......
    'django.middleware.cache.FetchFromCacheMiddleware', #最后
)

“update” 必须配置在第一个
“fetch” 必须配置在最后一个
```

```python
#settings.py
MIDDLEWARE_CLASSES = (
    'django.middleware.cache.UpdateCacheMiddleware',   #响应HttpResponse中设置几个headers
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.auth.middleware.SessionAuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'django.middleware.security.SecurityMiddleware',
    'django.middleware.cache.FetchFromCacheMiddleware',   #用来缓存通过GET和HEAD方法获取的状态码为200的响应

)


CACHE_MIDDLEWARE_SECONDS=10   #缓存时间10秒钟
```

视图

```python
from django.core.cache import cache
cache.set('key',value可以是任意数据类型)
cache.get('key')
```

```python
#views.py
from django.core.cache import cache

class Person:
    def __init__(self,name,age):
        self.name=name
        self.age=age

def test_cache(request):
    p=Person('zhao','18')
    cache.set('name', p)
    return HttpResponse('数据放进去')


def test_cache2(request):
    p = cache.get('name')
    print(type(p))
    print(p.name)
    return HttpResponse('数据拿出来')


#urls.py
path('test/',views.test_cache),
path('test2/',views.test_cache2)
```

![动画](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221212174114.gif)

## （23）项目

#### 后台创建，配置修改，目录变更

```python
#在控制台直接指向项目 python manage.py runserver----》manage.py中的内容修改
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'luffyapi.settings.dev')
```

![image-20221213114424340](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221213114427.png)

```python
#项目上线走的不是manage.py，而是wsgi.py中的
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'luffyapi.settings.dev')
```

![image-20221213114609506](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221213114611.png)

```python
#国际化，改成中文
    LANGUAGE_CODE = 'zh-hans'

    TIME_ZONE = 'Asia/shanghai'

    USE_TZ = False
```

```python
#创建app,
    命令startapp在哪里执行，就把app创建在哪里
    cd luffyapi
    cd apps
    python ../../manage.py startapp user

#注册app
	#配置环境变量
        import sys
        sys.path.insert(0, BASE_DIR)
        sys.path.insert(1, os.path.join(BASE_DIR, 'apps'))
    #注册
    	INSTALLED_APPS = [
       'user'  # 因为apps目录已经被加到环境变量了，所以直接能找到
]

```

```python
#导模块标红
```

![image-20221213120538321](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221213120541.png)

#### 数据库配置

```python
#项目依赖数据库  create database luffyapi;
#创建数据库用户,并且授予luffyapi库的权限
GRANT ALL PRIVILEGES on luffyapi.* to 'zhao'@'%' IDENTIFIED BY "luffyapi123?";
GRANT ALL PRIVILEGES on luffyapi.* to 'zhao'@'localhost' IDENTIFIED BY "luffyapi123?";
flush privileges;


#项目连接
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql',
            'NAME': 'luffyapi',
            'PASSWORD': 'luffyapi123?',
            'USER': 'zhao',
            'PORT': 3306,
            'CHARSET': 'utf8',
            'HOST':'127.0.0.1',
        }
    }
    
#pymysql连接    
    import pymysql
	pymysql.install_as_MySQLdb()



```

#### 配置user表

```python 
用户要基于auth的user表，数据库迁移命令之前操作好，否则报错
    -把app下的迁移文件全部删除
    -admin，auth,app下的迁移文件删除
    -删库（删库之前一定要导出）
```

```python
#user表
from django.contrib.auth.models import AbstractUser
class User(AbstractUser):
    telephone=models.CharField(max_length=11)
    icon=models.ImageField(upload_to='icon',default='icon/default.png')
```

```python
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
AUTH_USER_MODEL = 'user.user'
```



```python
#暴露资源接口
from django.contrib import admin
from django.urls import path,re_path
from django.views.static import serve
from django.conf import settings
urlpatterns = [
    path('admin/', admin.site.urls),
    re_path('media/(?P<path>.*)',serve,{'document_root':settings.MEDIA_ROOT})
]
```

#### 前台搭建

```python
1.安装node ,
2. 安装模块
npm install -g cnpm --registry=https://registry.npm.taobao.org

3.创建vue工程（需要一个脚手架）
npm install -g @vue/cli

4#命令行敲vue,就会有提示

5创建vue项目 
vue create 项目名
```

![image-20221213151202084](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221213151204.png)

![image-20221213151316277](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221213151318.png)

![image-20221213151409631](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221213151411.png)

![image-20221213151619358](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221213151622.png)

```python
6.pycharm中安装vue.js插件
7.pycharm打卡，terminal下敲:npm run serve,启动项目
8.配置pycharm

```

![image-20221213152416074](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221213152418.png)

![image-20221213152605190](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221213152607.png)

```python
目录介绍
public
    - favicon.ico
    - index.html   #整个项目的单页面
    
-src
	- assets        #静态文件，css,js，img
    - components    #小组件  头部组件，尾部组件
    - roter			#路由相关，（跳转）
    - store			#vuex相关，状态管理器，临时存储数据的地方
    - views			#页面组件  
	- main.js       #配置文件，（根django的settings一样）
	- App.vue		#根组件‘
    
    
    
#任何一个组件都有三部分， template,script,style
<template>

</template>

<script>

</script>


<style scoped>

</style>

```

#### 响应和异常处理

```python
REST_FRAMEWORK = {
    'EXCEPTION_HANDLER': 'luffyapi.utils.expextion.common_exception_handler',
}
```

异常

```python
# -*- coding: UTF-8 -*- 
# @Date ：2022/12/13 16:46
from rest_framework.views import exception_handler
from .response import APIResponse


def common_exception_handler(exc, context):
    response = exception_handler(exc, context)
    if isinstance(exc,KeyError):
        return APIResponse(code=0, msg='key error')
    if not response:  # drf内置处理不了，丢该django
        return APIResponse(code=0, msg='error', result=str(exc))
    else:
        return APIResponse(code=0, msg='error', result=response.data)
```

响应

```python
# -*- coding: UTF-8 -*- 
# @Date ：2022/12/13 16:33
from rest_framework.response import Response


class APIResponse(Response):
    def __init__(self, code=100, msg='成功', result=None, status=None,
                         headers=None,
                         content_type=None,**kwargs):
        dic={
            'code':code,
             'msg':msg,
        }
        if result:
            dic['result']=result
        dic.update(kwargs)
        super().__init__(data=dic,status=status,headers=headers,content_type=content_type)
```

#### 配置日志

- debug等级：终端查看、在线调试
- info等级：报告程序进度和状态星系
- waring等级：警告信息
- error等级：状态错误
- critical等级：致命的错误

```python
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'formatters': {
        'verbose': {
            'format': '%(levelname)s %(asctime)s %(module)s %(lineno)d %(message)s'
        },
        'simple': {
            'format': '%(levelname)s %(module)s %(lineno)d %(message)s'
        },
    },
    'filters': {
        'require_debug_true': {
            '()': 'django.utils.log.RequireDebugTrue',
        },
    },
    'handlers': {
        'console': {
            # 实际开发建议使用WARNING
            'level': 'DEBUG',
            'filters': ['require_debug_true'],
            'class': 'logging.StreamHandler',
            'formatter': 'simple'
        },
        'file': {
            # 实际开发建议使用ERROR
            'level': 'INFO',
            'class': 'logging.handlers.RotatingFileHandler',
            # 日志位置,日志文件名,日志保存目录必须手动创建，注：这里的文件路径要注意BASE_DIR代表的是小luffyapi
            'filename': os.path.join(os.path.dirname(BASE_DIR), "logs", "luffy.log"),
            # 日志文件的最大值,这里我们设置300M
            'maxBytes': 300 * 1024 * 1024,
            # 日志文件的数量,设置最大日志数量为10
            'backupCount': 10,
            # 日志格式:详细格式
            'formatter': 'verbose',
            # 文件内容编码
            'encoding': 'utf-8'
        },
    },
    # 日志对象
    'loggers': {
        'django': {
            'handlers': ['console', 'file'],
            'propagate': True, # 是否让日志信息继续冒泡给其他的日志处理系统
        },
    }
}

```

**utils.logger.py**

```python
#封装logger对象
import logging

# logger=logging.getLogger('名字')  #名字跟配置文件中loggers日志对象下的名字对应
logger = logging.getLogger('django')  # 'django'

```

![image-20221213171050833](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221213171054.png)

**将日志配置到错误处理中， utils.exeception.py**

```python
# -*- coding: UTF-8 -*- 
# @Date ：2022/12/13 16:46
from rest_framework.views import exception_handler
from .response import APIResponse
from .logger import logger


def common_exception_handler(exc, context):
    # 日志信息
    logger.error('view的是%s 错误是: %s' % (context['view'], str(exc)))
    response = exception_handler(exc, context)

    if isinstance(exc, KeyError):
        return APIResponse(code=0, msg='key error')
    if not response:  # drf内置处理不了，丢该django
        return APIResponse(code=0, msg='error', result=str(exc))
    else:
        return APIResponse(code=0, msg='error', result=response.data)

```

![image-20221213175133119](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221213175136.png)

![image-20221213175324861](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221213175326.png)

```python
#取出类名，上面的返回的是对象，因该取出类名
logger.error('view是 %s  错误是: %s' % (context['view'].__class__.__name__, str(exc)))
```

**utils.execption.py终极代码**

```python
# -*- coding: UTF-8 -*- 
# @Date ：2022/12/13 16:46
from rest_framework.views import exception_handler
from .response import APIResponse
from .logger import logger


def common_exception_handler(exc, context):
    # 日志信息
    # logger.error('view的是%s 错误是: %s' % (context['view'], str(exc)))
    logger.error('view是 %s  错误是: %s' % (context['view'].__class__.__name__, str(exc)))
    response = exception_handler(exc, context)

    if isinstance(exc, KeyError):
        return APIResponse(code=0, msg='key error')
    if not response:  # drf内置处理不了，丢该django
        return APIResponse(code=0, msg='error', result=str(exc))
    else:
        return APIResponse(code=0, msg='error', result=response.data)

```

![image-20221213180215321](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221213180218.png)

#### 跨域问题

##### cors

```python
#xss跨站脚本攻击，csrf跨站请求伪造。cors跨域资源共享
1 同源策略：浏览器的安全策略，不允许去另一个域加载数据
2 域：ip或者端口都必须一致
3 前后端分离项目会出现跨域

4 CORS：后端技术，跨域资源共享，服务端只要做配置，（本身浏览器已经支持了），就支持跨域
	Access-Control-Allow-Origin: *  # 所有的域都可以向我发送请求，浏览器不会禁止
    允许不同的域名来我的服务器拿数据

6 浏览器将CORS请求分成两类：
	-简单请求（simple request）
	-非简单请求（not-so-simple request）
7 满足以下两大条件就是简单请求
    (1) 请求方法是以下三种方法之一：
        HEAD
        GET
        POST
    (2)HTTP的头信息不超出以下几种字段：
        Accept
        Accept-Language
        Content-Language
        Last-Event-ID
        Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain
        
8 简单请求，只发送一次，如果后端写了CORS，浏览器就不禁止了
9 非简单请求，发两次，第一次是OPTIONS（预检请求），看后端是否允许，如果允许再发真正的请求

```



##### 引入

重新创建一个项目，修改端口号8008，书写简单的视图函数，配上路由，向luffyapi项目发送ajax请求

```python
def test(request):
   return render(request,'index.html')

    path('test/',views.test)
```

```html
<body>
<button onclick="test()">点我</button>
</body>
<script>
    function test() {
        $.ajax({
            url: 'http://127.0.0.1:8000/home/home/',
            type: 'get',
            success: function (data) {
                console.log(data)
            }
        })
    }
</script>
```

![image-20221213220141174](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221213220147.png)

在luffyapi项目上设置允许谁访问;`Access-Control-Allow-Origin':'*'`,意思就是允许所有域来访问。

```python
class TestView(APIView):
    def get(self, request, *args, **kwargs):
        dic= {'name':'zhao' }
        print(123)
        return APIResponse(headers={'Access-Control-Allow-Origin':'*'})
```

![image-20221213221745221](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221213221747.png)



##### 后端自定义中间件处理cors

**utils.middle.py**

```python
from django.utils.deprecation import MiddlewareMixin


class MyMiddle(MiddlewareMixin):
    def process_response(self, request, response):
        response['Access-Control-Allow-Origin'] = '*'
        if request.method == 'OPTION':
            response['Access-Control-Allow-Origin'] = 'Content-Type'
            return response
```

```python
#配置到中间件中
MIDDLEWARE = [
  	....
    'luffyapi.utils.middle.MyMiddle',
]
```

##### django使用django-cores-headers解决跨域问题

**1、安装**

```python
pip install django-cors-headers
```

**2、添加到settings的app中**

```python
INSTALLED_APPS={
    ....
    'corsheaders'
    ....
}
```

**3、添加中间件**

```python
MIDDLEWARE = [
    ...
    'corsheaders.middleware.CorsMiddleware',
    ...
]
```

**4、在settings.py文件末尾添加如下配置**



```python
# 跨域增加忽略
CORS_ALLOW_CREDENTIALS = True
CORS_ORIGIN_ALLOW_ALL = True
CORS_ORIGIN_WHITELIST = (
    # 这里添加你允许的跨域来源
    'http://127.0.0.1',
)

# 添加运行的请求方法
CORS_ALLOW_METHODS = (
    'DELETE',
    'GET',
    'OPTIONS',
    'PATCH',
    'POST',
    'PUT',
    'VIEW',
)

# 添加允许的请求头
CORS_ALLOW_HEADERS = (
    'XMLHttpRequest',
    'X_FILENAME',
    'accept-encoding',
    'authorization',
    'content-type',
    'dnt',
    'origin',
    'user-agent',
    'x-csrftoken',
    'x-requested-with',
    'Pragma',
)
```

#### 前后端打通

```python
#前台可以发送ajax请求，axios
#安装:  npm install axios
#配置:
	- main.js中
    	import axios from "axios" //导入安装的axios
		Vue.prototype.$axios = axios //相当于把axios这个对象放到vue对象中，以后用vue对象
        
    -  views  里面的 HomeView.vue中书写js代码
            <script>
                export default {
                  name: 'HomeView',
                  create() {
                   //向某个地址发送get请求 this.$axios.get('http://127.0.0.1:8000/home/home/').then(function (response) {
                      console.log(response)  //如果请求成功，返回的数据再response
                    }).catch(function (error) {
                      console.log(error)
                    })
                  },
                  components: {}
                }
            </script>
            
 #es6xie'f           
  create() {
   
    this.$axios.get('http://127.0.0.1:8000/home/home/').then(response => {
      console.log(response.data)
    }).catch(errors => {
      console.log(errors)
    })
  },
```

#### xadmin后台管理

##### 安装

```python
pip install https://codeload.github.com/sshwsfc/xadmin/zip/django2
```

##### app注册

```python
#app中注册
INSTALLED_APPS = [
	#xadmin主体模块
    'xadmin',
    #渲染表格模块
    'crispy_forms',
    #为模型通过版本控制，可以回滚数据
    'reversion',
]
```

##### 路由配置

```python
#路由配置
import xadmin
xadmin.autodiscover()
# xversion模块自动注册需要版本控制的Model
from xadmin.plugins import xversion
xversion.register_models()

urlpatterns = [
    # path('admin/', admin.site.urls),
    path('xadmin/', xadmin.site.urls),
]

```

##### 安装报错

```python
将xadmin-->plugins-->importexport.py中

把 48行复制一行然后注释掉，在49行里 去掉 SKIP_ADMIN_LOG, TMP_STORAGE_CLASS，换成 ImportMixin
# from import_export.admin import DEFAULT_FORMATS, SKIP_ADMIN_LOG, TMP_STORAGE_CLASS
from import_export.admin import DEFAULT_FORMATS, ImportMixin
```

##### 数据迁移

```python
python manage.py makemigrations
python managepy migrate
```

