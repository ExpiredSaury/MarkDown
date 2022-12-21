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

**模块化**是指解决一个复杂的问题时，自顶向下逐层**==把系统划分成若干个模块的过程==**，对于整个系统来说**，模块化是可组合、分解和更换的单元。**

编程领域的模块化，就是**遵循固定的规则**，把一个**大文件拆成独立并互相依赖**的**多个小模块**

**把代码进行模块化的好处：**

1. 提高了代码的复用性
2. 提高了代码的可维护性
3. 可以实现按需加载

**Node.js中模块的分类**

Node.js中根据模块来源的不同，将模块分为了3大类分别是：

* ==内置模块==（内置模块有官方提供，例如fs,path,http等）
* ==自定义模块==（用户创建的每个js文件，都是自定义模块）
* ==第三方模块==（有第三方开发出来的模块，并非官方提供的内置模块，也不是用户创建的自定义模块，使用前需要先下载）

**加载模块**

使用强大的`require()`方法，可以加载需要的内置模块，自定义模块，第三方模块，例如：

```js
//加载内置模块
const fs=require('fs')
//加载自定义模块
const custom=require('./custom.js')
//加载第三方模块
const moment=require('moment')
```

==注意==：**使用require()方法的时候加载其他模块时，会执行被加载模块中的代码**

**模块作用域**

和函数作用域类似，在自定义模块中定义的变量、方法等成员，只能在当前模块内被访问，这种模块级别的访问限制，叫做模块作用域。

**module对象**

在每个.js自定义模块中都有一个module对象，它里面存储了和当前模块相关的信息，打印如下：

```js
console.log(module);
```

![image-20221220110747158](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220110748.png)

**module.exports对象**

在自定义模块中，可以使用module.exports对象，将模块内的成员共享出去，供外界使用。

外界使用	`require()`	方法导入自定义模块时，得到的就是`module.exports`所指向的对象。

![image-20221220133232816](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220133234.png)

![image-20221220133647253](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220133650.png)

**使用`require()`方法导入模块时，导入的结果，永远以`module.exports`指向的对象为标准。**

![image-20221220134141581](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220134143.png)

**exports对象**

由于`module.exports` 单词写起来比较复杂，为了简单化向外共享成员的的代码，Node提供了`exports`对象，==默认情况下，exports和module.exports指向同一个对象==，最终共享的结果，还是以`module.exports`指向的对象为准。

![image-20221220134602415](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220134604.png)

![image-20221220135043042](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220135046.png)

**exports和module.exports的使用误区：**

时刻谨记，`reuqire()`模块时，得到的永远是==`module.exports`==指向的对象；

**Node.js中模块化规范**

Node.js遵循了CommonJS模块化规范，CommonJS规定了==模块的特征和各模块之间如何相互转换。==

CommonJS规定：

1. 每个模块内部，==module变量==代表当前模块
2. module变量是一个对象，它的exports属性（即==module.exports）是对外的接口。==
3. 加载某个模块，其实是加载该模块的module.exports属性==，require()方法用于加载模块。==

### npm与包

#### 包

**Node.js中的第三方模块又叫做包**

就像**电脑和计算机**指的是相同的东西，**第三方模块和包**指的是同一个概念，只不过叫法不同。

包不同于Node.js中的内置模块与自定义模块，**包是由第三方个人或团队开发出来的**，免费供所有人使用。

==**注意**==：Node.js中 的包都是免费且开源的，不需要付费即可免费下载使用。

**包是基于内置模块封装出来的，**提供了跟高级，更方便的API，**极大的提高了开发的效率。**

#### **包的下载**

搜索所需要的包：https://www.npmpjs.com/

下载需要的包：https://registry.npmjs.org/

==npm,Inc.公司==提供了一个包管理工具，我们可以使用包管理工具，从https://registry,npmjs.org/服务器把需要的包下载到本地使用

这个包管理工具的名字叫做**Node Package Manager**(简称==npm包管理工具==)，这个包管理工具随着Node.js的安装包一个被安装到了用户的电脑上。

可以在终端输入npm -v命令，来查看自己电脑上的npm包管理工具的版本号

```js
npm -v
```

#### **npm初体验**

**1.格式化时间的传统做法**

```js
//12定义格式化时间.js
function dataFormat(dataStr){
    const data=new Date(dataStr)


    const y = padZero(data.getFullYear())
    const m=padZero(data.getMonth() + 1)
    const d=padZero(data.getDate())
    const hh =padZero(data.getHours())
    const mm = padZero(data.getMinutes())
    const ss=padZero(data.getSeconds())


    return `${y}-${m}-${d} ${hh}:${mm}:${ss}`
}

function padZero(n){
    return n>9?n: '0'+n
}

module.exports={
    dataFormat
}

```

```js
//test.js

//导入自定义的格式化时间的模块
const TIME=require('./12定义格式化时间')  


const dt=new Date()

const newDT=TIME.dataFormat(dt)
console.log(newDT)
```

![image-20221220142355533](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220142356.png)

**2.格式化时间的高级做法**

1. **使用npm包管理工具，在项目中安装格式化时间的包 moment**
2. **使用require()导入格式化时间的包**
3. **参考moment的官方API文档对时间进行格式化**

```js
const moment =require('moment')

const dt=moment().format('YYYY-MM-DD HH:mm:ss')
console.log(dt);
```

![image-20221220143357167](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221140323.png)

**3.在项目中安装包的命令**

```js
npm install 包的完整名称
```

上述的安装包的命令，可以简写成下面的格式：

```js
npm i 包的完整名称
```

**4.初次安装包后多了哪些文件**

初次安装包完成后，在项目文件夹下多了一个叫做`node_modules`的文件夹和`package-lock.json`的配置文件

其中：

`node_nodules `文件夹**用来存放所有已经安装到项目中 的包**， require()导入第三方包时，就是从这个目录中查找并加载包

`package-lock.json `配置文件用来**记录 node_modules目录下的每一个包的下载信息，**例如包的名字、版本号、下载地址等。

**5.安装指定版本的包**

默认情况下，使**用npm install 命令安装包的时候，会自动安装最新版本的包**，如果需要安装指定的版本，可以在包名之后，通过==@==符号指定具体发版本，例如：

```js
npm i moment@2.22.2
```

**6.切换npm的下包镜像源**

```js
//查看当前的下包镜像源
npm config get registry
//将下包的镜像源切换为淘宝镜像源
npm config set registry=https://registry.npm.taobao.org/

```

**7.nrm**

为了更加方便的切换下包的镜像源，可以安装nrm小工具，利用nrm提供的终端命令,可以款苏查看和切换下包的镜像源

```js
//安装
npm i nrm -g
//查看所有可用 的镜像源
nrm ls
//将下包的镜像源切换为taobao 镜像
nrm use taobao
```

![image-20221220145312092](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220145315.png)

**8.包 的分类**

==1、项目包==：被安装到**项目**的node_modules目录中的包，都是项目包

项目包又分为两类:

* **开发依赖包**（被记录到devDependencies节点中的包，只在开发期间会用到）
* **核心依赖包**（被记录到dependencies节点中的包，在开发期间和项目上线之后都会用到）



```js
npm i 包名 -D  //开发依赖包（被记录到devDependencies节点中的包）
npm i 包名    //核心依赖包  （被记录到dependencies节点中的包）
```

==2、全局包==：在 执行npm install 命令时，如果提供了 ==-g==参数，则会把包安装为全局包

全局包会被安装到 `C:\Users\用户目录\AppData\Roaming\npm\node_modules`目录下

```js
npm i 包名 -g   //全局安装指定的包
npm uninstall 包名 -g //卸载全局安装的包
```

* 只有**工具性质的包**，才有全局安装的必要性，因为他们提供了好用的终端命令

==3、i5ting_toc==

i5ting_toc是一个可以把md文档转成Html页面的小工具，使用步骤如下：

```js
npm install -g i5ting_toc 
 // -o是自动用默认浏览器打开
i5ting_toc -f 要转换的md文件路径 -o
```

### 模块的加载机制

**1、优先从缓存中加载**

==模块在第一次加载后会被缓存==，意味着多次调用require()不会导致模块的代码被执行多次

注意：不论是内置模块，用户自定义模块，还是第三方模块，它们都会优先从缓存中加载，从而提高模块的加载效率。

**2、内置模块的加载机制**

内置模块是由Node.js官方提供的，==内置模块的加载优先级最高==

**3、自定义模块的加载机制**

使用require()加载自定义模块时，必须以` ./ `或者 `../`开头的==路径标识符==，在加载自定义模块时，如果没有指定 ./ 或者 ../这样的路径标识符，则node会把它当作`内置模块`或者`第三方模块`进行加载

**4、第三方模块的加载机制**

如果传递给require()的模块标识符不是一个内置模块，也没有以 "./"或者"../"开头，则Node.js会从当前模块的父目录开始，尝试从/node_modules文件夹中加载第三方模块。

如果没有找到对应的第三方模块，则移动到再上一层父目录中，进行加载，知道文件系统的根目录。

**5、目录作为模块**

当把目录作为模块标识符，传递给require()进行加载的时候，由三种加载方式：

1. 在被加载的目录下查找一个叫做package.json的文件，并寻找main属性，作为require()加载的入口
2. 如果目录里没有package.json文件，或者main入口不存在或者无法解析，则Node.js将会试图加载目录下的index.js文件。
3. 如果以上两步都失败了，则Node.js会把终端打印错误消息，报告模块的确实：Error Cannot find module 'XXX'

### Express

#### 初识Express

Express是基于Node.js平台，快速、开放、极简的Web开发框架

通俗理解：Express的作用和Node.js内置的http模块类似，是**专门用来创建Web服务器的**

==Express的本质==：就是一个npm上的第三方包，提供了快速创建Web服务器的便捷方法。

Express的中文官网：https://www.expressjs.com.cn/

使用Express，可以快速创建==Web网站==的服务器或==API接口==的服务器。

##### Express的基本使用

**1.安装**

在项目所处的目录中，运行如下终端命令，即可将express安装到项目中使用

```js
npm i express@4.17.1
```

**2.创建基本的Web服务器**

```js
//导入
const express =require('express')

//创建web服务器
const app =express()
//调用app.listen端口号，启动成功后的回调函数，启动服务器
app.listen(888,()=>{
    console.log('express server running at http://127.0.0.1:888');
})
```

![image-20221220154751594](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220154752.png)

**3.监听GET请求**

通过app.get(),可以监听客户端的GET请求，

```js
//参数1，客户端请求的URL地址
//参数2，请求对应的处理函数
// req,请求对象，（包含了与请求相关的属性和方法
//res,响应对象，（包含了与响应相关的属性和方法

app.get('请求的URL',function(req,res){
    /*处理函数*/
})
```

**4.监听POST请求**

通过app.post(),可以监听客户端的POST请求，

```js
//参数1，客户端请求的URL地址
//参数2，请求对应的处理函数
// req,请求对象，（包含了与请求相关的属性和方法
//res,响应对象，（包含了与响应相关的属性和方法

app.post('请求的URL',function(req,res){
    /*处理函数*/
})
```

**5.把内容响应给客户端**

通过res.send()方法，可以把处理好的内容，发送给客户端

```js
app.get('/user',(req,res)=>{
    //向客户端发送JSON对象
    res.send({name:'zhang',age:18,gender:'男'})
})
app.post('/user',(req,res)=>{
    //向客户端发送文本内容
    res.send('请求成功')
})
```

![image-20221220164145123](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220164147.png)

![image-20221220164159195](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220164200.png)

**6.获取URL中携带的查询参数**

通过==req.query==对象，可以访问到客户端通过==查询字符串==的形式，发送到服务器的参数：

```js
app.get('/',(req,res)=>{
    //req.query默认是一个空对象，
    //客户端使用 ?name=zs&age=18 这种查询字符串形式，发送到服务器的参数
    //可以通过req.query 对象访问到 例如：
    //req.query.name  req.query.age
    console.log(req.query)
    res.send(req.query)
})
```

![image-20221220164744123](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220164747.png)

![image-20221220164818457](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220164822.png)

**7.获取URL的动态参数**

通过`req.params`对象，可以访问到URL中，通过`:`匹配到的`动态参数`；

```js
app.get('/user/:id',(req,res)=>{
   //req.params默认是一个空对象
    //里面存放着通过 : 动态匹配到的参数值
    console.log(req.params)
    res.send(req.params)
})
```

![image-20221220165214375](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220165218.png)



![image-20221220165244166](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220165245.png)

```js
app.get('/user/:ids/:name',(req,res)=>{
   //req.params默认是一个空对象
    //里面存放着通过 : 动态匹配到的参数值
    console.log(req.params)
    res.send(req.params)
})
```

![image-20221220165522076](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220165524.png)



![image-20221220165530653](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220165533.png)

##### 托管静态资源

**1.express.static()**

通过它，可以非常便捷的创建一个静态资源服务器，

例如：通过如下代码可以将public目录下的图片，css文件，JavaScript文件对外开放访问：

```js
app.use(express.static('public'))
```

```js
const express=require('express')
const app =express()

//调用express.static方法，对外提供静态资源
app.use(express.static('./clock'))
app.listen(888,()=>{
    console.log('express server running http://127.0.0.1:888');
})
```

![image-20221220170546220](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220170547.png)



![image-20221220170520717](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220170522.png)

![image-20221220170531220](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220170532.png)

**2.托管多个静态资源目录**

只需要多次调用express.static()函数

```js
app.use(express.static('public'))
app.use(express.static('files'))
```

访问静态资源文件时，express.static()，函数 会根据目录的添加顺序查找所需要的文件。

**3.挂载路径前缀**

如果希望在托管的静态资源访问路径之前，挂载路径前缀，则可以使用如下的方式：

```js
app.use('/public',express.static('public'))
```

![image-20221220171048459](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220171050.png)

##### nodemon

在编写Node.js项目时，如果修改了项目的代码，则需要频繁的手动的close掉，然后再重新重启，非常繁琐

**安装为全局可用的工具**

```js
npm i -g nodemon
```

**使用nodemon**

当基于Node.js编写了一个网站应用的时候，传统的方式，是运行`node app.js`命令，来启动项目，这样做的坏处是：代码被修改之后，需要手动的重启项目。

现在，可以将node命令替换为nodemon命令，使用` nodemon app.js`来启动项目，这样做的好处是：代码被修改之后，会被nodemon监听到，从而实现自动重启项目的效果。

![image-20221220171653872](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220171655.png)

修改了代码后，会自动重启，不用再手动关闭服务器再开启服务器了。

#### Express路由

路由指的是**客户端的请求**与**服务器处理函数**之间的**映射关系。**

在Express中的路由分为3部分组成，分别是**请求的类型**，**请求的URL地址**，**处理函数**，格式如下：

```js
app.METHOD(PATH,HANDLER)
```

```js
//匹配GET请求，且请求URL为 /
app.get('/',function(req,res){
    res.send('Hello World!')
})

//匹配POST请求，且请求URL 为 /
app.post('/',function(req,res){
    res.send('Got a POST request')
})

```

**1.路由匹配过程**

每当一个请求到达服务器之后，**需要经过路由的匹配**，只有匹配成功之后，才会调用对应的处理函数。

在匹配时，会按照路由的顺序进行匹配，如果**请求的类型**和**请求的URL**同时匹配成功，则Express会将这次请求，转交给对应的function函数进行处理。

##### 路由使用

**1.简单使用**

```js
const express =require('express')
const app=express()


//挂载路由
app.get('/',(req,res)=>{
    res.send('hello world!')
})

app.post('/',(req,res)=>{
    res.send('Post Request')
})
app.listen(888,()=>{
    console.log('http://127.0.0.1:888');
})
```

![image-20221220201600294](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220201603.png)



![image-20221220201614861](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220201617.png)

**2.模块化路由**

为了方便对路由进行模块化的管理，Express不建议将路由挂载到app上，而是**推荐将路由抽离为单独的模块。**

将路由抽离为单独模块的步骤如下：

1. 创建路由模块对应的js文件
2. 调用`express.Router()`函数创建路由对象
3. 向路由对象上挂载具体的路由
4. 使用`module.exports`向外共享路由对象
5. 使用`app.user()`函数注册路由模块。

**3.创建路由模块**

```js
//18创建路由模块.js
var express =require('express')   //1.导入express
var router=express.Router()   //2.创建路由对象

router.get('/user/list',function(req,res){  //3.挂载获取用户列表的路由
    res.send('Get user list!')
})
router.post('/user/add',function(req,res){  //4.挂载添加用户的路由
    res.send('Add new user')
})
module.exports=router      //5.向外导出路由对象
```

**4.注册路由模块**

```js
const express =require('express')
const app=express()

//1.导入路由模块
const router=require('./18创建路由模块')
//2.注册路由模块
app.use(router)


//app.user()函数的作用，就是来注册全局中间件

app.listen(888,()=>{
    console.log('http://127.0.0.1:888');
})
```

![image-20221220203108216](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220203109.png)



![image-20221220203123384](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220203124.png)

**5.为路由模块添加前缀**

类似于托管静态资源时，为静态资源统一访问前缀一样，路由模块添加前缀的方式也非常简单：

```js
//1.导入路由模块
const userRouter=require('./18创建路由模块')

//2.使用 app.use() 注册路由模块，并添加统一的访问前缀  /api
app.use('/api',userRouter)
```

#### Express中间件

中间件特指**业务流程**的**中间处理环节**

**1.Express中间件的调用流程**

当一个请求到达Express的服务器之后，可以连续调用多个中间件，从而对这次请求进行**预处理**

**2.Express中间件的格式**

本质上就是一个**function处理器**

中间件函数的形参列表中，**必须包含next参数**，而路由处理函数中只包含req和res

**3.next函数的作用**

**next函数**是实现**多个中间件连续调用**的关键，它表示把流程关系**转交**给**下一个中间件或路由**

![image-20221220205012581](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221220205014.png)

```js
const express=require('express')
const app=express()



//常量 mw 所指向的，就是一个中间件函数
const mw =function(req,res,next){
    console.log('这是一个最简单的中间件函数');

    //注意：在当前中间件的业务处理完毕后，必须使用next()函数
    //表示 把流转关系转交给下一个中间件或路由
    next()
}


app.listen(888,()=>{
    console.log('http://127.0.0.1:888');
})

```

**4.全局生效的中间件**

客户端发起的**任何请求**，到达服务器之后，**都会触发的中间件**，叫做全局生效的中间件。

通过调用 app.use(中间件函数)，即可定义一个全局生效的中间件，实例代码如下：

```js
//常量 mw 所指向的，就是一个中间件函数
const mw =function(req,res,next){
    console.log('这是一个最简单的中间件函数');

    next()
}
//全局生效的中间件
app.use(mw)
```

**5.定义全局中间件的简化形式**

```js
app.use(function (req,res,next){
    console.log('这是一个简单的中间件函数')
    next()
})
```

```js
const express=require('express')
const app=express()

//常量 mw 所指向的，就是一个中间件函数
const mw =function(req,res,next){
    console.log('这是一个最简单的中间件函数');

    //注意：在当前中间件的业务处理完毕后，必须使用next()函数
    //表示 把流转关系转交给下一个中间件或路由
    next()
}
//将mw注册为全局生效的中间件
app.use(mw)

app.get('/',(req,res)=>{
    console.log('调用了  / 这个路由！')
    res.send('Home page')
})

app.post('/user', (req,res) =>{
    console.log('调用了  /user 这个路由！')
    res.send('User Page!')
})

app.listen(888,()=>{
    console.log('http://127.0.0.1:888');
})

```



![image-20221221092545453](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221092547.png)



![image-20221221092610799](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221092612.png)

**6.中间件的作用**

多个中间件之间，**共享同一份req和res，**基于这样的特性，我们可以在上游的中间件中，**统一**为req或res对象添加自定义的属性和方法，供下游的中间件或路由进行使用。

```js
const express=require('express')
const app=express()

//常量 mw 所指向的，就是一个中间件函数
const mw =function(req,res,next){
    console.log('这是一个最简单的中间件函数');
    //获取到请求到达服务器的时间
    const time=Date.now()
    // 为req对象，挂载自定义属性，从而把时间共享给后面的所有路由
    req.startTime = time
    
    next()
}

app.get('/',(req,res)=>{
    res.send('Home page' + req.startTime)
})

app.post('/user', (req,res) =>{
    res.send('User Page!'  + req.startTime)
})

app.listen(888,()=>{
    console.log('http://127.0.0.1:888');
})
```

**7.定义多个全局中间件**

可以使用 app.use()连续定义多个全局中间件，客户端请求到达服务器之后，会按照中间件定义的先后顺序依次进行调用，示例代码如下：

```js
const express =require('express')
const app =express()

//定义第一个全局中间件
app.use((req,res,next)=>{

    console.log('调用了第一个全局中间件')
    next()
})
//定义第二个全局中间件
app.use((req,res,next)=>{

    console.log('调用了第二个全局中间件')
    next()
})

//定义一个路由
app.get('/use',(req,res)=>{
    res.send('User page!')
})

app.listen(888,()=>{
    console.log('http://127.0.0.1:888');
})
```

![image-20221221093759028](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221093802.png)



**8.局部生效的中间件**

不使用 `app.use()`,定义的中间件，叫做**局部生效的中间件**，示例代码如下：

```js
const express =require('express')
const app =express()

//定义第一个中间件
const mw1=(req,res,next)=>{
    console.log('调用了局部生效的中间件')
}
//mw1这个中间件只在 当前路由中生效
app.get('/',mw1,(req,res)=>{
    res.send('Home page!')
})

app.get('/user',(req,res)=>{
    res.send('User page!')
})

app.listen(888,()=>{
    console.log('http://127.0.0.1:888');
})
```

**9.定义多个局部中间件**

```js
app.get('/',mw1,mw2,(req,res)=>{res.send('Home page!')})
app.get('/',[mw1,mw2],(req,res)=>{res.send('Home page!')})
```

**10.中间的5个注意事项**

1. 一定要在路由之前注册中间件
2. 客户端发送过来的请求，可以连续调用多个中间件 进行处理
3. 执行完中间件的业务逻辑代码之后，不要忘记调用next()函数
4. 为了防止代码逻辑混乱，调用next()函数后不要再写额外的代码
5. 连续调用多个中间件时，多个中间件之间，共享req和res对象

#### 使用Express写接口

**1.创建基本的服务器**

```js
//导入express
const express =require('express')
//创建服务器实例
const app=express()

//启动服务器
app.listen(888,()=>{
    console.log('express server running at http://127.0.0.1');
})
```

**2.创建API路由模块**

```js
//导入express
const express =require('express')
//创建服务器实例
const app=express()


//挂载对应的路由
const router = require('./24路由模块')
//把路由模块注册到app上
app.use('/api',router)

//启动服务器
app.listen(888,()=>{
    console.log('express server running at http://127.0.0.1');
})
```

```js
//24路由模块.js

const express =require('express')

const router=express.Router()


//这里挂载对应的路由

module.exports =router



```

**3.编写GET接口**

```js
//24路由模块.js
const express =require('express')

const router=express.Router()


//这里挂载对应的路由
router.get('/get',(req,res)=>{
    //通过req.query获取客户端通过查询字符串，发送到服务器的数据
    const query=req.query
    //调用res.send()，向客户端响应处理的函数
    res.send({
        status:0,//0表示成功，1表示失败
        msg:'GET 请求成功', //状态的描述
        data:query   //需要相应给客户端的数据
    })

})


module.exports =router
```

![image-20221221100309427](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221100311.png)

**4.编写POST接口**

```js
//POST接口
router.post('/post',(req,res)=>{
    //通过req.query获取客户端通过查询字符串，发送到服务器的数据
    const body=req.body
    //调用res.send()，向客户端响应处理的函数
    res.send({
        status:0,//0表示成功，1表示失败
        msg:'POST 请求成功', //状态的描述
        data:body   //需要相应给客户端的数据
    })

})

```

**5.使用cors中间件解决跨域问题**



cors是Express的一个第三方中间件，通过安装和配置cors中间件，可以很方便的解决跨域问题。

使用步骤分为如下3步：

1. 运行` npm install cors`安装中间件
2. 使用` const cors = require('cors')`导入中间件
3. 在路由之前调用  `app.use(cors()) `配置中间件

#### 跨域资源共享

**cors（跨域资源共享）由一系列HTTP响应头组成，这些HTTP响应头决定浏览器是否阻止前端JS代码跨域获取资源**

**浏览器的同源安全策略默认会阻止网页“跨域”获取资源。但如果接口服务器配置了CORS相关的HTTP响应头，就可以解决浏览器端的跨域访问限制。**

![image-20221221113214936](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221113217.png)

**1.CORS的注意事项**

* CORS主要在服务器端进行配置，客户端浏览器无须做任何额外的配置，即可请求开启了CORS的接口。
* CORS在浏览器中有兼容性，只有支持XMLHttpRequest Level2的浏览器，才能正常访问开启了CORS的服务端接口

**2.CORS响应头部-Access-Control-Allow-Origin**

响应头部中可以携带一个Access-Control-Allow-Origin字段，其语法格式如下：

```js
Access-Control-Allow-Origin: <origin>  | *
```

origin的参数值指定了==允许访问该资源的外域URL。==

例如：下面的字段值将只允许来自http:demo.com的请求

```js
res.setHeader('Access-Control-Allow-Origin','http:demo.com')
```

如果指定了Access-Control-Allow-Origin字段的值为通配符 * ，表示允许来自任何域的请求，示例代码如下：

```js
res.setHeader('Access-Control-Allow-Origin','*')
```

**3.CORS响应头部-Access-Control-Allow-Headers**

默认情况下，CORS仅支持客户端向服务端发送如下 的9个==请求头==：

![image-20221221120500394](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221120502.png)

```js
res.setHeader('Access-Control-Allow-Headers','Content-Type,X-Custom-Header')
```

**4.CORS响应头部-Access-Control-Allow-Methods**

默认情况下，CORS仅支持客户端发起GET，POST，HEAD请求。

如果客户端希望通过==PUT、DELETE==等方式请求服务器的资源，则需要在服务器端，通过Access-Control-Allow-Methods来==指明实际请求所允许使用的HTTP方法==

```js
//只允许POST、GET、DELETE、HEAD请求方法
res.setHEader('Access-Control-Allow-Methods','POST,GET,DELETE,HEAD')
//允许所有的HTTP请求方法
res.setHEader('Access-Control-Allow-Methods','*')
```

**5.CORS请求的分类**

客户端在请求CORS接口时，根据请求方式和请求头的不同，可以将CORS的请求分为两大类，分别是：

1. 简单请求
2. 预检请求

**简单请求：**

![image-20221221121204554](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221121208.png)

**预检请求**

![image-20221221121314232](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221121316.png)

**简单请求和预检请求的区别：**

**简单请求特点**：客户端与服务器之间只会发生一次请求。

**预检请求特点**：客户端与服务器之间会发生两次请求，OPTION预检请求成功之后，才会发起真正的请求。

#### 在Express中操作数据库

##### 1.安装Mysql模块

mysql模块时托关于npm上的第三方模块，它提供了在Node.js项目中连接和操作MySQL数据库的能力。

想要在项目中使用，必须使用如下命令，将Mysql安装为项目的依赖包。

```js
npm i mysql
```

##### 2.配置mysql模块

```js
//导入
const mysql =require('mysql')
//建立与MySQL数据库的连接
const db = mysql.createPool({
    host:'127.0.0.1',
    user:'root',
    password:'123456',
    database:'express_db'
})
```

##### 3.测试mysql模块是否能正常工作

调用db.query()函数，指定要执行的SQL语句，通过回调函数拿到执行的结果。

```js
//测试mysql模块能否正常工作
db.query('select 1',(err,results)=>{
    if(err){
        return console.log(err.message)
    }
    //只要能打印出[ RowDataPacket {'1':1} ] 的结果，证明连接正常
    console.log(results)
})
```

![image-20221221132042406](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221132046.png)

##### 4.查询数据

```js
//查询数据
db.query('select * from user',(err,results)=>{
    if(err){
        return console.log(err.message)
    }
    console.log(results)
})
```

![image-20221221132415229](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221132419.png)

##### 5.插入数据

```js
//插入数据
const user={name:'tank',gender:'男'}
//待执行的sql语句，其中英文的?表示占位符
const sqlStr = 'insert into user (name,gender) values(?,?)'
//使用数据的形式，依次为？占位符指定具体的值
db.query(sqlStr,[user.name,user.gender],(err,results)=>{
    if(err){
        return console.log(err.message) //失败
    }
    if(results.affectedRows ===1 ){
        console.log('插入数据成功！')  //成功
    }
})
```

![image-20221221133113589](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221133117.png)

##### 6.便捷插入数据

```js
const user={name:'shasha',gender:'女'}
// 待执行的sql语句，其中英文的?表示占位符
const sqlStr = 'insert into user set ?'
// 直接将数据对象当作占位符的值
db.query(sqlStr,user,(err,results)=>{
    if(err){
        return console.log(err.message) //失败
    }
    if(results.affectedRows ===1 ){
        console.log('插入数据成功！')  //成功
    }
})
```



##### 7.更新数据

```js
const user={'id':2,name:'aaa',gender:'男'}
//待执行的sql语句，其中英文的?表示占位符
const sqlStr = 'update user set name=?,gender=? where id=?'
//
db.query(sqlStr,[user.name,user.gender,user.id],(err,results)=>{
    if(err){
        return console.log(err.message) //失败
    }
    if(results.affectedRows ===1 ){
        console.log('更新数据成功！')  //成功
    }
})
```

##### 8.更新数据的便捷方式

```js

const user={'id':2,name:'aaa',gender:'男'}
//待执行的sql语句，其中英文的?表示占位符
const sqlStr = 'update user set ? where id=?'
//
db.query(sqlStr,[user,user.id],(err,results)=>{
    if(err){
        return console.log(err.message) //失败
    }
    if(results.affectedRows ===1 ){
        console.log('更新数据成功！')  //成功
    }
})
```

##### 9.删除数据

在删除数据时，尽量根据id这样的唯一标识，来删除对应的数据，

```js

const sqlStr = 'delete from user where id=?'

db.query(sqlStr,1,(err,results)=>{
    if(err){
        return console.log(err.message) //失败
    }
    if(results.affectedRows ===1 ){
        console.log('删除数据成功！')  //成功
    }
})
```

##### 10.标记删除

使用delete语句，会把真正的数据从表中删除，为了保险起见，推荐使用标记删除的形式，来模拟删除的操作

所谓的标记删除，就是在表中设置类似于status这样的状态字段，来标记当前这条数据是否被删除

当用户执行了删除的动作时，我们并没有执行delete语句把数据删除，而是执行了update语句，将这条数据对应的status字段标记为删除即可。

```js
const sqlStr = 'update user set status=1 where id=?'

db.query(sqlStr,2,(err,results)=>{
    if(err){
        return console.log(err.message) //失败
    }
    if(results.affectedRows ===1 ){
        console.log('删除数据成功！')  //成功
    }
})
```

### 前后端身份认证

#### 1.web开发模式

* 基于**服务器渲染**的传统Web开发模式
* 基于**前后端分离**的新型Web开发模式

服务器渲染的优缺点：

![image-20221221140010509](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221140013.png)

**前后端分离的概念：前后端分离的开发模式，==依赖于Ajax技术的广泛应用==，简而言之，前后端分离的Web开发模式，就是==后端只负责提供API接口，前端使用Ajax调用接口==的开发模式。**

前后端分离的优缺点：

![image-20221221135946312](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221135949.png)

#### 2.身份认证

==身份认证==（Authentication)又称为‘身份验证’，‘鉴权’，是指==通过一定的手段，完成对用户身份的确认。==

身份认证的目的：是为了确认当前所声称为某种身份的用户，确实是所声称的用户。

对于服务器渲染和前后端分离这两种开发模式来说，分别有着不同的身份认证方案：

* 服务器渲染推荐使用Session认证机制
* 前后端分离推荐使用JWT认证机制

##### **Cookie**

![image-20221221153707229](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221153709.png)

![image-20221221154020342](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221154036.png)

由于Cookie是存储在浏览器中，而且**浏览器也提供了读写Cookie的API**，因此**COokie很容易被伪造**，不具有安全性，因此不建议服务器将重要的隐私数据，通过Cookie的形式发送给浏览器。

##### **Session的工作原理**

![image-20221221154515237](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221154518.png)

**在Express中使用Session认证**

**1.安装 express-session中间件**

```js
npm i express-session
```

**2.配置express-session中间价**

```js
const session =require('express-session')
app.use(session({
  secret:'itheima',
  resave:false,
  saveUninitialized:true,
})
)

```

**3.向session中存数据**

当`express-session`中间件配置成功之后，即可通过` req.session`来访问和使用 `session`对象，从而存储用户的关键信息；

```js
app.post('/api/login', (req, res) => {
  // 判断用户提交的登录信息是否正确
  if (req.body.username !== 'admin' || req.body.password !== '000000') {
    return res.send({ status: 1, msg: '登录失败' })
  }

  // TODO_02：请将登录成功后的用户信息，保存到 Session 中
//只有成功配置了express-session中间件后，才能够通过req点session这个属性
  req.session.user=req.body  //用户的信息
  req.session.islogin=true  //用户的登录状态
  res.send({ status: 0, msg: '登录成功' })
})
```

**4.从session中取数据**

可以直接从`req.session`对象上获取之前存储的数据，示例代码如下：

```js
app.get('/api/username', (req, res) => {
 
  if(req.session.islogin){
    return res.send({status:1,msg:'fail'})
  }
  res.send({
    status:0,
    msg:'success',
    username:req.session.user.username
  })
})

```

**5.清空session**

```js
app.post('/api/logout', (req, res) => {
  // 清空 Session 信息
  req.session.destroy()
  res.send({
    status:0,
    msg:'退出登录成功'
  })
})

```

##### JWT认证机制

![image-20221221160220146](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221160221.png)

**JWT工作原理**

![image-20221221160424700](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221160426.png)

**用户的信息通过Token字符串的形式，保存在客户端浏览器中，服务器通过还原Token字符串的形式来认证用户的身份。**

**JWT的组成**

JWT通常由三部分组成，分别是Header（头部），Payload（有效荷载），Signature（签名），三者之间由英文的  “ .  ”分隔



安装JWT相关包

```js
npm install jsonwebtoken express-jwt
```

* `jsonwebtoken`用于**生成JWT字符串**
* `express-jwt`用户**将JWT字符串解析还原成JSON对象**

```js
const jwt = require('jsonwebtoken')
const expressJWT =require('express-jwt')
```

![image-20221221161649868](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221161652.png)

![image-20221221162249899](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221162253.png)

![image-20221221162606397](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221162609.png)

![image-20221221163253848](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221163256.png)

![image-20221221163429394](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221221163432.png)