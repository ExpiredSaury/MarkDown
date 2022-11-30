## Web应用模式

**在Web开发中，有两种模式:**

1. **前后端分离**

---



![5a9c76bb3b243ebb771fe3e606ebc9f7](E:\MarkDown\markdown\imgs\5a9c76bb3b243ebb771fe3e606ebc9f7.jpg)

```python
#前后端分离:  只专注于后端，返回json格式数据
```



2. **前后端不分离**

---

![b2ab8ef6477b920668424a8071a5b51e](E:\MarkDown\markdown\imgs\b2ab8ef6477b920668424a8071a5b51e.jpg)

```python
# 前后端混合开发(前后端不分离)  返回的是HTML的内容,需要写模板

# 动态页面(查数据库)，静态页面（一个静止的html）

# 页面静态化
```

## API接口

通过网络，规定了前后端信息交互的url连接，也就是前后端信息交互的媒介

## Restful规范

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

## drf安装和简单使用

### 1、安装

```python 
#安装
pip install djangorestframework   
```

### 2、使用

```python
1.settings.py中
    INSTALLED_APPS = [
        'rest_framework'
    ]
2.在models.py中写表模型
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

4.视图中写视图类（CBV）
	from rest_framework.viewsets import ModelViewSet
    from .models import Book
    from .ser import BookModelSerializer     #ser指的是第三步中创建的py文件

    class BooksViewSet(ModelViewSet):
        queryset = Book.objects.all()
        serializer_class = BookModelSerializer

5.写路由关系
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
7.启动项目，测试
```

**启动项目程序**

![image-20221129203315903](http://zsong.oss-cn-hangzhou.aliyuncs.com/img/image-20221129203315903.png)

![image-20221129203332838](http://zsong.oss-cn-hangzhou.aliyuncs.com/img/image-20221129203332838.png)

![image-20221129203401800](http://zsong.oss-cn-hangzhou.aliyuncs.com/img/image-20221129203401800.png)

![image-20221129203421305](http://zsong.oss-cn-hangzhou.aliyuncs.com/img/image-20221129203421305.png)

### 3、postman测试

在`postman`中测试，`postman`中最后要加`/`，浏览器会自动重定向，但`postman`不会，所以在`postman`中最后要加`/`

* 查数据

![image-20221129203625840](http://zsong.oss-cn-hangzhou.aliyuncs.com/img/image-20221129203625840.png)

![image-20221129203740404](http://zsong.oss-cn-hangzhou.aliyuncs.com/img/image-20221129203740404.png)

* 删数据

![image-20221129204051596](http://zsong.oss-cn-hangzhou.aliyuncs.com/img/image-20221129204051596.png)

将`1`删除后，就找不到数据

![image-20221129204125671](http://zsong.oss-cn-hangzhou.aliyuncs.com/img/image-20221129204125671.png)

删除`1`数据后，再查看所有数据

![image-20221129204151343](http://zsong.oss-cn-hangzhou.aliyuncs.com/img/image-20221129204151343.png)

* 修改数据

![image-20221129205214042](http://zsong.oss-cn-hangzhou.aliyuncs.com/img/image-20221129205214042.png)

修改完后，再次查询所有

![image-20221129205232800](http://zsong.oss-cn-hangzhou.aliyuncs.com/img/image-20221129205232800.png)

* 增加数据

![image-20221129205459219](http://zsong.oss-cn-hangzhou.aliyuncs.com/img/image-20221129205459219.png)

增加后再次查看所有数据

![image-20221129205608684](http://zsong.oss-cn-hangzhou.aliyuncs.com/img/image-20221129205608684.png)

### 4、cbv源码分析

```python
ModelViewSet继承与View (djanog原生View)
```

