### Node.js引入

**==Node.js==是一个基于Chrome V8引擎的==JavaScript运行环境==**

**浏览器是Javascript的前端运行 环境**

**Node.js是Javascript的后端运行环境**

**Node.js中无法调用DOM和BOM等浏览器内置API**

浏览器中Javascript学习路径

```python
Javascript基础语法 + 浏览器内置API（BOM + DOM） + 第三方库（jQuery、art-template等）
```

Node.js的学习路径

```python
JavaScript基础语法  + Node.js内置API模块（fs、path、http） + 第三方API模块（express、mysql）
```

### 环境安装以及测试

安装包可以从[Node.js官网](https://nodejs.org/zh-cn/)首页直接下载，点击绿色按钮，选择长期维护版下载，双击直接安装即可

![image-20221219135146992](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219135148.png)

**查看已安装Node.js版本号**

```markdown
cmd打开终端，终端中输入node -v后，即可查看已安装的Node.js的版本号
```

**Node.js环境中执行JavaScript代码**

```python
打开终端
输入  node 要执行的js文件路径
```

![image-20221219140010442](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219140013.png)

### fs文件系统模块

fs模块是Node.js官方提供，用来操作文件的模块，它提供了一系列的方法和属性，用来满足用户对文件的操作需求.

**在JavaScript代码中，使用fs模块来操作文件，需要先导入**

```js
const fs=require('fs')
```

* `fs.readFile()`方法：用来**读取**指定文件的内容

* `fs.writeFile()`方法：用来向指定的文件中**写入**内容

#### 读取文件

**fs.readFile()方法，语法格式：**

```js
fs.readFile(path[,option],callback)
```

* path:必选参数，字符串，表示文件的路径
* option:可选参数，表示以什么编码格式来读取文件
* callback:必选参数，文件读取完成后，通过回调函数拿到读取的结果

```js
//1.导入fs模块
const fs=require('fs')
//2.调用fs.readFile()读取文件
//参数1，读取文件的存放路径
//参数2，读取文件的编码格式，一般默认utf-8
//参数3,回到回调函数，拿到读取失败和成功的结果， err data
fs.readFile('./1.txt','utf-8',function(err,data){
    console.log(err)
    console.log('------------------')
    console.log(data);
})

//如果读取成功，err的值为null
//如果读取失败，err的值为错误对象，data的值为undefined
```



![image-20221219141750837](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219141754.png)

**判断文件是否读取成功**

```js
const fs=require('fs')

fs.readFile('./1.txt','utf-8',function(err,data){
   if(err){
     return console.log('读取失败'+ err.message)
   }else{
    console.log('读取文件成功!'+ data);
   }
})
```



![image-20221219142322678](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219142324.png)

#### 写入内容

**fs.writeFile()方法，语法格式：**

```js
fs.writeFile(file,dada[,option],callback)
```

* file: 必选参数，需要指定一个文件路径的字符串，表示文件的存放路径
* data:必选参数，表示写入的内容
* option:可选参数，表示以什么编码格式来读取文件，默认utf8
* callback:必选参数，文件写入完成后的回调结果

```js
//1.导入fs文件系统模块
const fs =require('fs')
 //2.调用 fs.wwrite()，写入文件内容
 //参数1，表示文件的存放路径
 //参数2，表示写入内容
 //参数3.回调函数
 fs.writeFile('./2.txt','12343225432',function(err){
    //如果文件写入成功，err的值为null
    //如果文件写入失败，err的值等于一个错误对象
        console.log(err);
 })
```

![image-20221219143258612](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219143300.png)

**判断文件是否写入成功**

```js
const fs =require('fs')

 fs.writeFile('./2.txt','12343225432',function(err){
    if(err){
        return console.log('文件写入失败'+err.message)
    }
    console.log('文件写入成功！')
 })
```



![image-20221219143447688](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219143451.png)

**案例分析**

使用fs文件系统模块，将当前目录下的成绩.txt文件中成绩数据，整理到成绩-ok.txt文件中

整理之前的数据格式如下：

![image-20221219143803763](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219143805.png)

整理之后，希望得到的成绩-ok.txt文件数据格式如下：

![image-20221219143907661](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219143911.png)

**实现步骤：**

1. 导入fs文件系统模块
2. 使用fs.readFile()方法,读取成绩.txt文件
3. 判断是否读取失败
4. 文件读取成功后，处理成绩数据
5. 调用fs.writeFile()方法，写入新文件成绩-ok.txt文件中

```js
const fs=require('fs')

fs.readFile('./成绩.txt','utf8',function(err,data){
    if(err){
        return console.log('文件读取失败' + err.message);
    }
    // console.log('文件读取成功',data);
    //先把成绩的数据按照空格分割
    const arrOld=data.split(' ')
    //循环分割后的数组，对每一项数据，进行字符串的替换操作
    const arrNew=[]
    arrOld.forEach(item =>{
        arrNew.push(item.replace('=',':'))
    })
  
    //把新数组中的每一项，进行合并，得到一个新的字符串
    const newStr=arrNew.join('\r\n')
    // console.log(newStr);

    //调用fs.wreiteFile()方法，把处理完毕的数据，存到新文件中
    fs.writeFile('./成绩-ok.txt',newStr,function(err){
        if(err){
            return console.log('文件写入失败'+ err.message);
        }
        console.log('文件写入成功！');
    })

})
```

![image-20221219145906911](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219145908.png)

### path路径模块

path模块是Node.js官方提供的，用来处理路径的模块，它提供了一系列的方法和属性，用来满足用户对路径的处理需求。

如果在JavaScript代码中，使用path 模块来处理路径，需要先导入：

```js
const path=require('path')
```

* `path.join()`方法：用来将多个路径片段拼接成一个完成的路径字符串
* `path.basename()`方法：用来从路径字符串中，将文件名解析出来

**path.join()方法**

```js
const path=require('path')

const pathStr = path.join('/a','/b/c','../','./d','e')
console.log(pathStr)

const pathStr2=path.join(__dirname,'/05.txt')
console.log(pathStr2)
```

![image-20221219163057169](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219163100.png)

**path.basename()方法**

可以获取路径中的最后一部分，经常通过这个方法获取路径中的文件名，语法格式如下：

```js
path.basename(path[,ext])
```

* path 必选参数，表示一个路径的字符串
* ext 可选参数，表示文件扩展名
* 返回：表示路径中的最后一部分

```js
const path=require('path')

//定义文件的存放路径
const fpath='/a/b/c/index.html'
const fullName=path.basename(fpath)
console.log(fullName) //输入index.html

const nameResult=path.basename(fpath,'.html')
console.log(nameResult)   //输出index

```

![image-20221219163830026](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219163833.png)

**path.extname()方法**

可以获取到路径中的扩展名部分，语法格式如下：

```js
path.extname(path)
```

* path：必选参数，表示一个路径的字符串
* 返回值：返回得到的扩展名字符串

```js
const path=require('path')

const fpath='/a/b/c/index.html'

const fext=path.extname(fpath)
console.log(fext  //输出.html
```

![image-20221219164120728](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219164124.png)

### http模块

**Node.js提供的http模块，通过几行简单的代码，就能轻松的手写一个服务器软件，从而对外提供web服务。**

**1、导入http模块**

```js
const http =require('http')
```

**2、创建web服务器实例**

**调用`http.createServer()`方法，即可快速创建一个web服务器实例**

```js
const server=http.createServer()
```

**3、为服务器实例绑定request事件**

为服务器实例绑定request事件，即可监听客户端发送过来的网络请求

```js
//使用服务器实例的.on()方法，为服务器绑定一个request事件
server.on('request',function(req,res) {
    //只要有客户端请求我们自己的服务器，就会触发request事件，从而调用这个事件处理函数。
    console.log('Someone visit our web server.')
})
```

**4、启动服务器**

调用服务器实例的.listen()方法，即可启动当前的web服务器实例；

```js
//调用 server.listen(端口号， cb回调)方法，即可启动web服务器
server.listen(80,() =>{
    console.log('http server running at http://127.0.0.1')
    
})
```

![image-20221219204701997](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219204706.png)

**req请求对象**

**只要服务器接收到了客户端的请求，就会调用通过server.on()为服务器绑定的request事件处理函数**

如果想在事件处理函数中，访问与客户端相关的**数据**或**属性**，可以使用以下方式：

```js
server.on('request',(req)=>{
    //req是请求对象，它包含了与客户端相关的数据和属性，例如：
    //req.url是客户端请求的URL地址
    //req.method是客户端的 method请求类型
        const str =`Your request url is ${url},and request method is ${method}`

    console.log(str)
})
```

![image-20221219212310258](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219212312.png)

**res响应对象**

在服务器的request事件处理函数中，如果想访问与服务器相关的数据或属性，可以使用以下的方式：

```js
const http=require('http')

const server =http.createServer()

server.on('request',(req,res)=>{
    const url=req.url
    const method=req.method
    const str =`Your request url is ${url},and request method is ${method}`
   
    //调用res.end()方法，向客户端响应一些内容
    res.end(str)
})  
server.listen(8888,()=>{
    console.log('server running at http://127.0.0.1:8888');
})
```

![image-20221219212944321](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219212946.png)

![image-20221219213014887](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219213017.png)

**解决中文乱码的问题**

当调用res.end()方法，向客户端发送中文内容的时候，会出现乱码问题，此时需要手动设置内容的编码格式：

```js
const http=require('http')

const server =http.createServer()

server.on('request',(req,res)=>{
    const url=req.url
    const method=req.method
    const str =`Your request url is ${url},and request method is ${method}`
   
    res.setHeader('Content-Type','text/html; charset=utf-8')
    //调用res.end()方法，向客户端响应一些内容
    res.end(str)
})  
server.listen(8888,()=>{
    console.log('server running at http://127.0.0.1:8888');
})
```

**根据不同的url响应不同的html内容**

```js
const http =require('http')
const server =http.createServer()
server.on('request',(req,res)=>{
    //获取请求的Url地址
    const url=req.url
    //设置默认的响应内容为404 not found
    let content='404 not found'
    //判断用户请i去是否为 / 或 /index.html首页
    //首页用户请求的是否为 /about.html关于页面
    if(url == '/' || url=='/index.html'){
        content ='<h1>首页</h1>'
    }else if(url=='/about.html'){
        content ='<h1>关于页面</h1>'
    }

    //设置Content-Type 响应头防止中文乱码
    res.setHeader('Content-Type','text/html; charset=utf-8')
    //使用res.end(),把内容响应给客户端
    res.end(content)

})

server.listen(888,()=>{
    console.log('server running at http://127.0.0.1:888')
})
```

![image-20221219214704138](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219214705.png)

![image-20221219214635913](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219214637.png)

![image-20221219214647183](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221219214648.png)

### 模块化