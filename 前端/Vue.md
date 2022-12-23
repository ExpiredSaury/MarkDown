# 1、vue基础

**Vue是一套用户==构建用户界面==的==渐进式==JavaScript框架**

![image-20221222105838026](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221222105842.png)

作者：尤雨溪

vue的特点：

1. 采用==组件化==模式，提高代码复用率，且让代码更好维护。
2. ==声明式==编码，让编码人员无需直接操作DOM，提高开发效率。
3. 使用==虚拟DOM==+优秀的==Diff算法==，尽量复用DOM节点。

### 1.1、安装

先用`<script>`引入,点击下载开发版本

![image-20221222113629845](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221222113631.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>初识Vue</title>
    <script src="../js/vue.js"></script>
</head>
<body>
    
</body>
</html>
```

运行代码，查看控制台，建议我们使用开发者工具

![image-20221222114627034](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221222114628.png)

可以去[扩展迷](https://www.extfans.com/)中搜索vue选择Vue.js devtools,点击下载，添加到谷歌浏览器的扩展程序中

![image-20221222115222414](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221222115223.png)

再次刷新刚才打开的初始vue.html网页，发现终端没有了提示安装开发者工具了，只是还是有提示只需要做一个全局配置就可以

![image-20221222115319547](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221222115321.png)

在Vue官方 [API](https://v2.cn.vuejs.org/v2/api/)中有一个`productionTip`默认是`true`

![image-20221222115700823](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221222115702.png)

```html
<body>
    <script type="text/javascript">
        Vue.config.productionTip = false;  //阻止 vue 在启动时生成生产提示。
    </script>
</body>
```

在刷新初识Vue.html打开控制台，就没有提示了

![image-20221222120256750](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221222120258.png)



### 1.2、Hello Vue

```html
<head>
    <title>初识Vue</title>
    <script type="text/javascript" src="../js/vue.js"></script>
</head>
<body>

    <!-- 准备一个容器 -->
    <div id="root">
        <h1>Hello {{name}} !</h1>
    </div>

    <script type="text/javascript">
        Vue.config.productionTip = false;;  //阻止 vue 在启动时生成生产提示。
        
        //创建Vue实例,
        new Vue({
            el:'#root' ,  //el用户指定当前Vue实例为哪个容器服务，值通常为css选择器字符串，
            // el:document.getElementById('root')
            data:{    //data中用于存储数据，数据供el所指定的容器使用, 值暂时先写成一个对象
                name:'Vue',
            }
        })


    </script>
</body>
```

**总结：**

1. 想让Vue工作，就必须创建一个Vue实例，且要传入一个配置对象；
2. root容器里的代码依然符合html规范，只不过混入了一些特殊的Vue语法;
3. root容器里的代码被成为【Vue模板】
4. Vue实例和容器是一一对应的
5. 真实开发中只有一个Vue实例，并且会配合着组件一起使用
6. {{xxx}}中的xxx要写js表达式，且xxx可以自动读取到data中的所有属性
7. 一旦data中的数据发生改变，那么页面中用到该数据的地方也会自动更新。

**注意区分：js表达式和js代码(语句)：**

```js
1. js表达式：一个表达式会生成一个值，可以放在任何一个需要 值的地方；
* a 
* a+b
* x === y ? 'a'  : 'b'

2. js代码（语句）
 * if(){}
 * for(){}
```

### 1.3、模板语法

```html
<body>

    <!-- 准备一个容器 -->
    <div id="root">
        <h1>插值语法</h1>
        <h3>你好  {{ name }}</h3>
        <hr>
        <h1>指令语法</h1>
        <a v-bind:href="url">点我去百度</a>
        <a v-bind:href="url.toUpperCase()">点我去百度</a>
        <a :href="Date.now()">点我去百度</a>

    </div>

    <script type="text/javascript">
        Vue.config.productionTip = false;;  //阻止 vue 在启动时生成生产提示。
        
        //创建Vue实例,
        new Vue({
            el:'#root' ,  //el用户指定当前Vue实例为哪个容器服务，值通常为css选择器字符串，
            // el:document.getElementById('root')
            data:{    //data中用于存储数据，数据供el所指定的容器使用, 值暂时先写成一个对象
                name:'张三',
                url:'http://www.baidu.com'
            }
        })


    </script>
</body>
```

```html
<body>

    <!-- 准备一个容器 -->
    <div id="root">
        <h1>插值语法</h1>
        <h3>你好  {{ name }}</h3>
        <hr>
        <h1>指令语法</h1>
        <a v-bind:href="school.url">点我去{{school.name}}</a>
        <a :href="Date.now()">点我去{{school.name}}</a>

    </div>

    <script type="text/javascript">
        Vue.config.productionTip = false;;  //阻止 vue 在启动时生成生产提示。
        
        //创建Vue实例,
        new Vue({
            el:'#root' ,  //el用户指定当前Vue实例为哪个容器服务，值通常为css选择器字符串，
            // el:document.getElementById('root')
            data:{    //data中用于存储数据，数据供el所指定的容器使用, 值暂时先写成一个对象
                name:'张三',
                school:{
                    url:'http://www.jingdong.com',
                    name:'京东'
                }
            }
        })


    </script>
</body>
```





**总结：**

Vue模板语法有2大类：

1. 插值语法：
   * 功能：用于解析标签体内容太
   * 写法：{{xxx}} xxx是js表达式，且可以直接读取到data中的所有属性
2. 指令语法：
   * 功能：用户解析标签（包括：标签属性，标签体内容，绑定事件...）
   * 举例：` v-bind:href="xxx"`  或    简写为 `:href="xxx"`  ,xxx同样要写js表达式，且可以直接读取到data中的所有属性
   * 注意：Vue中有很多指令，且形式都是 ： `v- ???:`此处只是拿`v-bind`举了个例子。



### 1.4、数据绑定

```html
<body>
    <!-- 
        Vue中有2种数据绑定的方式：
        1.单向绑定（v-bind):数据只能从data流向页面
        2.双向绑定（v-model):数据不仅仅能从data流向页面，还可以从页面流向data
        备注：
            1.双向绑定一般都应用在表单类元素上（如：input,select)
            2.v-model:value 可以简写为v-model，因为v-model默认收集的就是value值

    
    -->

    <!-- 准备一个容器 -->
    <div id="root">
       <!-- 普通写法 -->
       <!-- 单向数据绑定 ：<input type="text" v-bind:value="name">
       双向数据绑定 ：<input type="text" v-model:value="name"> -->
       
       <!-- 简写 -->
       单向数据绑定 ：<input type="text" :value="name">
       双向数据绑定 ：<input type="text" v-model="name">
       
       <!-- 如下代码是错误，因为v-model只能用在表单类元素（输入类元素）上 -->
       <!-- <h2 v-model:x="name">你好</h2> -->
    </div>

    <script type="text/javascript">
        Vue.config.productionTip = false;;  //阻止 vue 在启动时生成生产提示。
        new Vue({
            el:'#root',
            data:{
                name:'测试'
            }
        })

    </script>
</body>
```

### 1.5、el与data的两种写法

**el两种写法：**

```html
<body>
    <!-- 准备一个容器 -->
    <div id="root">
      <h1>你好，{{name}} </h1>
    </div>

    <script type="text/javascript">
        Vue.config.productionTip = false;;  //阻止 vue 在启动时生成生产提示。
        
        el的两种写法
        const v=new Vue({
            // el:'#root',    //第一种写法
            data:{
                name:'测试'
            }
        })
        // console.log(v)
        //setTimeout(()=>{
         //   v.$mount('#root')
        //},1000);
        
        v.$mount('#root')   //第二种写法
            
    </script>
</body>
```

**data的两种写法：**

```html
<body>
    <!-- 准备一个容器 -->
    <div id="root">
      <h1>你好，{{name}} </h1>
      
    </div>

    <script type="text/javascript">
        Vue.config.productionTip = false;;  //阻止 vue 在启动时生成生产提示。
          
        new Vue({
            el:'#root',
            // data:{      //第一种写法，对象式
            //     name:'测试'
            // }

            //第一种写法，函数式
            data:function(){  
                console.log('@@@',this); //此处的this指的是Vue实例对象
                return {
                    name:'Vue'
                }
            }
        })
    </script>
</body>
```

**总结：**

```python
 data与el的2种写法：
        1.el有两种写法：
            （1）new Vue时候配置el属性
            （2）先创建Vue实例，随后再通过vm.$mount('#root')指定el的值
        2.data有两种写法：
            （1）对象式
            （2）函数式
            如何选择：目前那种写法都可以，以后学习到组件的时候，data必须使用函数式，否则会报错
        3.一个重要原则：
            由Vue管理的函数，一定不要写成箭头函数，一旦写了箭头函数，this就不再是Vue实例了
```

### 1.6、理解MVVM模型

M：模型（Model）：对应data种的数据

V：视图（View）：模板

VM：视图模型（ViewModel）：Vue实例对象

![image-20221222145756753](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221222145758.png)

### 1.7、Object.defineProperty方法

```html
<body>
        <script>
            let number = 18
            let person={
                name:'zs',
                sex:'男',
                // age:18,
            }

            Object.defineProperty(person,'age',{
                // value:18,
                // enumerable:true,//控制属性是否可以枚举，默认是false
                // writable:true,// 控制属性是否可以被修改，默认值false
                // configurable:true, //控制属性是否可以被删除，默认值是false

                //当有人读取person的age属性，get函数(getter)就会被调用，且返回值就是age的值
                get:function(){
                    return number
                },

                //当有人修改person的age属性，set函数(setter)就会被调用，且会收到被修改的值
                set(value){
                    number=value
                }
            })

            console.log(person);
        </script>
</body>
```

### 1.8、数据代理

通过一个对象代理对另一个对象中属性的操作 （读/写）

```js
<script>
    let obj={x:100}
    let obj2={y:200}
    Object.defineProperty(obj2,'x',{
        get(){
            return obj.x
        },
        set(value){
            obj.x=value 
        }
    })
</script>
```

```html
<body>
    <!-- 准备一个容器 -->
    <div id="root">
        <h2>学校名称:{{name}}</h2>
        <h2>学校地址:{{address}}</h2>
        
    </div>
    <script>
        Vue.config.productionTip = false;
        const vm = new Vue({
            el:'#root',
            data:{
                name:'北大',
                address:'北京'
            }
        })
    </script>
</body>
```



![image-20221223112022497](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221223112026.png)

1. Vue中的数据代理：
   * 通过vm对象来代理data对象 中属性的操作（读/写）
2. Vue中数据代理的好处：
   * 更加方便的操作data中的数据
3. 基本原理：
   * 通过Object.definProperty()把data对象中所有的属性添加到vm上
   * 为每一个添加到vm上的属性，都指定了一个getter/setter
   * 在getter/setter内部去操作（读/写）data中对应的属性

### 1.9、事件处理

**事件基本使用：**

```python
1. 基本v-on:xxx  或 @xxx 绑定事件，其中xxx是事件名；
2. 使用的回调需要配置在methods对象中，最终会在vm上；
3. methods中配置的函数，不需要使用箭头函数，！！否则this指的就不是vm了
4. methods中配置的函数，都是被Vue所管理的函数，this的指向是 vm 或 组件实例对象；
5. @click="demo"  和 @click="demo($event)"效果一致，但后者可以传参
```



```html
<body>
    <div id="root">
        <h2>Hello {{name}} !!!</h2>
        <button v-on:click="showInfo1">Click Me1!</button>
        <button @click="showInfo2(66,$event)">Click Me2!</button>

    </div>

    <script>
        Vue.config.productionTip = false;
        new Vue({
            el:'#root',
            data:{
                name:'Vue'
            },
            methods:{
                showInfo1(event){
                    alert('Hello Vue1!!!')
                    // console.log(event.target.innerText);
                    // console.log(this); //此处是vm,
                },
                showInfo2(number,event){
                    alert('Hello Vue2!!!')
                    console.log(number,event);
                }
            }
        })
    </script>
</body>
```

### 2.0、事件修饰符

```python
prevent:阻止默认事件（常用）
stop:阻止事件冒泡(常用)
once:事件只触发一次（常用）
capture:使用事件的捕获模式
self:只有event.target是当前操作的元素时，才触发事件
passive:事件的默认行为立即执行，无需等待事件回调执行完毕
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="viewport" content="width=device-width, initial-scale=2.0">
    <meta name="viewport" content="width=device-width, initial-scale=3.0">
    <meta name="viewport" content="width=device-width, initial-scale=4.0">
    <title>Document</title>
    <script src="../js/vue.js"></script>
    <style>
        *{
            margin-top: 20px;
        }
        .demo1{
            height: 50px;
            background-color: aqua;
        }
        .box1{
            padding: 5px;
            background-color: orange;
        }
        .box2{
            padding: 5px;
            background-color: blueviolet;
        }
        .list{
            width: 200px;
            height: 200px;
            background-color: aquamarine;
            overflow: auto;

        }
        li{
            height: 100px;
        }
    </style>
</head>
<body>
    <div id="root">
        <h2>Hello {{name}} !!!</h2>
        <!-- 阻止默认事件 -->
        <a href="http://www.baidu.com" @click.prevent="showInfo">百度</a>
        <!-- 阻止冒泡 -->
        <div class="demo1" @click="showInfo">
            <button @click.stop="showInfo">点我 提示信息</button>
        </div>
        <!-- 事件只触发一次 -->
        <button @click.once="showInfo">点我 提示信息</button>
        <!-- 使用事件的捕获模式 -->
        <div class="box1" @click.capture="showMsg(1)">
            div1
            <div class="box2" @click="showMsg(2)">div2</div>
        </div>
        <!-- 只有event.target是当前操作的元素时，才触发事件 -->
        <div class="demo1" @click.self="showInfo">
            <button @click="showInfo">点我 提示信息</button>
        </div>

        <!-- 事件的默认行为立即执行，无需等待事件回调执行完毕 -->
        <!-- <ul @scroll="demo" class="list">    滚动条-->   
        <!-- <ul @wheel="demo" class="list"> 鼠标滚轮滚动 -->
        <ul @wheel.passive="demo" class="list"> 
            <li>1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
        </ul>
    </div>

    <script>
        Vue.config.productionTip = false;
        new Vue({
            el:'#root',
            data:{
                name:'Vue'
            },
            methods:{
                showInfo(e){
                    alert('Hello Vue1!!!')
                },   
                showMsg(msg){
                    console.log(msg);
                }, 
                demo(){
                    for (let i = 0; i < 10000; i++) {
                        console.log('#')
                    }
                    console.log('累坏了')
                } 
            }
        })
    </script>
</body>
</html>
```

### 2.1、键盘事件

```python
1.Vue中常用的按键别名
    回车 =》enter
    删除 =》delete (捕获 “删除” 和“退格”键)
    退出 =》 esc
    空格 =》 space
    换行 =》tab
    上 =》 up
    下 =》down
    左 =》 left
    右 =》 right
    
2.Vue未提供别名的按键，可以使用按键原始的key值去绑定，但注意要转为kebab-case（短横线命名）

3.系统修饰键（用户特殊）：crtl alt shift  meta
	(1)配置keyup使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才能触发
    (2)配合keydown使用：正常触发事件
    
4.也可以使用keyCode去指定具体的按键（不推荐）

5.Vue.config.keyCodes.自定义键名 = 键码 ，可以去定制按键别名
```



```html
<body>
    <div id="root">
        <h2>欢迎学习 {{name}}</h2>
        <input type="text" placeholder="按下回车提示输入" @keyup.enter="showInfo">
    </div>
</body>

<script>
    Vue.config.productionTip = false;
    new Vue({
        el:'#root',
        data:{
            name:'Vue',
        },
        methods:{
            showInfo(event){
                console.log(event.target.value);
            
            }
        }
    })
</script>
```

### 2.2、计算属性

插值语法：

```html
<body>
    <div id="root">
            姓：<input type="text" v-model="firstName"><br/><br/>
            名: <input type="text" v-model="lastName"><br/><br/>
            全名: <span>{{firstName}}-{{lastName}}</span>
    </div>
</body>
<script>
    Vue.config.productionTip = false;
    new Vue({
        el:'#root',
        data:{
            firstName:'张',
            lastName:'三',
        }
    })

</script>
```

methods实现：

```html
<body>
    <div id="root">
            姓：<input type="text" v-model="firstName"><br/><br/>
            名: <input type="text" v-model="lastName"><br/><br/>
            全名: <span>{{fullName()}}</span>
    </div>
</body>
<script>
    Vue.config.productionTip = false;
    new Vue({
        el:'#root',
        data:{
            firstName:'张',
            lastName:'三',
        },
        methods:{
            fullName(){
              return this.firstName  + '-' +this.lastName
            }
        }
    })

</script>
```

计算属性：

```python
1.定义：要用的 属性不存在，要通过已有的属性计算得来
2.原理：底层借助了Object.defineproperty方法提供的getter和setter
3.get函数什么时候执行？
	（1）初次读取时会执行一次
    （2）当依赖的数据发生变化时会再次调用
4.优势：与methods实现相比，内部有缓存机制（复用），效率更高，调式方便
5.备注：
	（1）计算属性最终会出现在vm上，直接读取使用即可
    （2）如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发生改变
```



```html
<body>
    <div id="root">
            姓：<input type="text" v-model="firstName"><br/><br/>
            名: <input type="text" v-model="lastName"><br/><br/>
            全名: <span>{{fullName}}</span>
    </div>
</body>
<script>
    Vue.config.productionTip = false;
    new Vue({
        el:'#root',
        data:{
            firstName:'张',
            lastName:'三',
        },
        computed:{
            fullName:{
                //当有人读取fullName时，get就会调用，且返回值就作为fullName的值
                get(){
                    return this.firstName + '-' + this.lastName
                },
                //当fullName被修改时，set就会调用
                set(value){
                    const arr=value.split('-')
                    this.firstName=arr[0]
                    this.lastName=arr[1]


                }
            }
        }
    })

</script>
```

简写

```html
<body>
    <div id="root">
            姓：<input type="text" v-model="firstName"><br/><br/>
            名: <input type="text" v-model="lastName"><br/><br/>
            全名: <span>{{fullName}}</span>
    </div>
</body>
<script>
    Vue.config.productionTip = false;
    new Vue({
        el:'#root',
        data:{
            firstName:'张',
            lastName:'三',
        },
        computed:{
            fullName:function(){
                return this.firstName + '-' + this.lastName
            }
        }
    })

</script>
```

### 2.3、监视属性

天气案例

```html
<body>
    <div id="root">
        <h2>天气很{{info}}</h2>
        <button @click="changeWeather">切换天气</button>
        <!-- <button @click="isHot = !isHot">切换天气</button> -->

    </div>
</body>
<script>
    Vue.config.productionTip = false;
    new Vue({
        el:'#root',
        data:{
            isHot:true
        },
        computed:{
            info(){
                return this.isHot ? '炎热' : '凉爽'
            }
        },
        methods: {
            changeWeather(){
                this.isHot = !this.isHot
            }
        },
    
    })
</script>
```

监视属性

```python
1.当被监视的属性变化时，回调函数自动调用，进行相关操作
2.监视的属性必须存在，才能进行监视
3监视的两种写法：
	（1）new Vue时传入watch配置
   	（2）通过vm.$watch监视
```



```html
<body>
    <div id="root">
        <h2>天气很{{info}}</h2>
        <button @click="changeWeather">切换天气</button>
        <!-- <button @click="isHot = !isHot">切换天气</button> -->

    </div>
</body>
<script>
    Vue.config.productionTip = false;
    new Vue({
        el:'#root',
        data:{
            isHot:true
        },
        computed:{
            info(){
                return this.isHot ? '炎热' : '凉爽'
            }
        },
        methods: {

            changeWeather(){
                this.isHot = !this.isHot
            }
        },
        watch:{
            isHot:{
                immediate:true, //初始化时让handler调用以下
                //当isHot发生改变时，调用handler()
                handler(newValue,oldValue){
                    console.log('isHos被修改了',newValue,oldValue);

                }
            }
        }
    })
</script>
```

```html
</body>
<script>
    Vue.config.productionTip = false;
    const nw = new Vue({
        el:'#root',
        data:{
            isHot:true
        },
        computed:{
            info(){
                return this.isHot ? '炎热' : '凉爽'
            }
        },
        methods: {

            changeWeather(){
                this.isHot = !this.isHot
            }
        },
    })
    vm.$watch('isHot',{
        immediate:true,
        handler(newValue,oldValue){
            console.log('isHot被修改了 ',newValue,oldValue);
        }
    })
</script>
```

**深度监视**

```python
深度监视：
	（1）Vue中的watch默认不监视对象内部值的改变（一层）
    （2）配置deep:true可以监视对象内部值的改变（多层）
备注：
	（1）Vue自身可以配置对象内部值的改变，但Vue提供的watch默认不可以
    （2）使用watch时根据数据的具体结构，决定是否采用深度监视
```



```html
<body>
    <div id="root">
        <h2>天气很{{info}}</h2>
        <button @click="changeWeather">切换天气</button>
        <!-- <button @click="isHot = !isHot">切换天气</button> -->
        <hr>
        <h3>a的值是{{numbers.a}}</h3>
        <button @click="numbers.a++">点我让a+1</button>
        <h3>b的值是{{numbers.b}}</h3>
        <button @click="numbers.b++">点我让b+1</button>
    </div>
</body>
<script>
    Vue.config.productionTip = false;
    const nw = new Vue({
        el:'#root',
        data:{
            isHot:true,
            numbers:{
                a:1,
                b:1
            }
        },
        computed:{
            info(){
                return this.isHot ? '炎热' : '凉爽'
            }
        },
        methods: {

            changeWeather(){
                this.isHot = !this.isHot
            }
        },
        watch:{
            //正常写法
            // isHot:{
            //     immediate:true, //初始化时让handler调用以下
            //     deep:true, //深度监视
            //     //当isHot发生改变时，调用handler()
            //     handler(newValue,oldValue){
            //         console.log('isHos被修改了',newValue,oldValue);

            //     }
            // },

            //简写
            isHot(newValue,oldValue){
                console.log('isHos被修改了',newValue,oldValue);
            }
        }
    })

</script>
```

**watch对比computed区别**

```python
computed与watch之间的区别
	1.computed能完成的功能，watch都可以完成
	2.watch能完成的功能，computed不一定能完成，例如:watch可以进行异步惭怍
两个重要的小原则：
	1.所被Vue管理的函数，最好写成普通函数这样this的指向才是vm或者组件实例对象
    2.所有不被Vue所管理的函数和（定时器的回调函数、Ajax的回调函数），最好写成箭头函数
    这样this的指向才是vm  或组件实例对象
```



watch

```html
<body>
    <div id="root">
        姓: <input type="text" v-model="firstName"><br><br>
        名:<input type="text" v-model="lastName"><br><br>
        全名: <span>{{ fullName }}</span>
    </div>
</body>
<script>
    Vue.config.productionTip = false;

    new Vue({
        el:'#root',
        data:{
            firstName:'张',
            lastName:'三',
            fullName:'张-三',
        },
        watch:{
            firstName(val){
                this.firstName=val + '-' + this.lastName
            },
            lastName(val){
                this.fullName= this.firstName + '-' + val
            }
        }
    })
</script>
```

computed

```html
<body>
    <div id="root">
            姓：<input type="text" v-model="firstName"><br/><br/>
            名: <input type="text" v-model="lastName"><br/><br/>
            全名: <span>{{fullName}}</span>
    </div>
</body>
<script>
    Vue.config.productionTip = false;
    new Vue({
        el:'#root',
        data:{
            firstName:'张',
            lastName:'三',
        },
        computed:{
            fullName:{
                //当有人读取fullName时，get就会调用，且返回值就作为fullName的值
                get(){
                    return this.firstName + '-' + this.lastName
                },
                //当fullName被修改时，set就会调用
                set(value){
                    const arr=value.split('-')
                    this.firstName=arr[0]
                    this.lastName=arr[1]


                }
            }
        }
    })

</script>
```

### 2.4、绑定样式

* 在应用界面中，某些（个）元素的样式时变化的
* class/style绑定就是专门用来实现动态样式效果的技术

```python
绑定样式：
    1. class样式
        写法:class="xxx" xxx可以是字符串、对象、数组。
        字符串写法适用于：类名不确定，要动态获取。
        对象写法适用于：要绑定多个样式，个数不确定，名字也不确定。
        数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用。
    2. style样式
        :style="{fontSize: xxx}"其中xxx是动态值。
        :style="[a,b]"其中a、b是样式对象。
	
```



class绑定：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="../js/vue.js"></script>
    <style>
        .basic{
            width: 400px;
            height: 100px;
            border: 1px solid black;
        }
        
        .happy{
            border: 4px solid red;;
            background-color: rgba(255, 255, 0, 0.644);
            background: linear-gradient(30deg,yellow,pink,orange,yellow);
        }
        .sad{
            border: 4px dashed rgb(2, 197, 2);
            background-color: gray;
        }
        .normal{
            background-color: skyblue;
        }

        .bj1{
            background-color: yellowgreen;
        }
        .bj2{
            font-size: 30px;
            text-shadow:2px 2px 10px red;
        }
        .bj3{
            border-radius: 20px;
        }
    </style>
</head>
<body>
    <div id="root">
        <!-- 绑定class样式--字符串写法，适用于：样式的类名不确定需要动态指定 -->
        <div class="basic " :class="mood" @click="changeMood">{{name}}</div>
        <br>
        <br>
        <!-- 绑定class样式--数组写法，适用于：要绑定的样式个数不确定，名字也不确定 -->
        <div class="basic " :class="classArr" >{{name}}</div>
        <br>
        <br>
        <!-- 绑定class样式--对象写法 ，适用于：要绑定的样式个数确定，名字确定 ，但是要动态决定用不用 -->
        <div class="basic " :class="classObj" >{{name}}</div>

    </div>
</body>
<script>
    Vue.config.productionTip = false;
    new Vue({
        el:'#root',
        data:{
            name:'北京',
            mood:'normal',
            classArr:['bj1','bj2','bj3'],
            classObj:{
                bj1:true,
                bj2:true,
            }
        },
        methods: {
            changeMood(){
                // this.mood  = 'happy'
                const arr=['happy','sad','normal']
                const index=Math.floor(Math.random()*3)
                this.mood =arr[index]
            }
        },
    })
</script>
</html>
```

style绑定:

```html
<body>
		
		<!-- 准备好一个容器-->
		<div id="root">
			<!-- 绑定style样式--对象写法 -->
			<div class="basic" :style="styleObj ">{{name}}</div> <br/><br/>
			<!-- 绑定style样式--数组写法 -->
            <div class="basic" :style="[styleObj,styleObj2]">{{name}}</div>
			<div class="basic" :style="styleArr">{{name}}</div>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false
		
		const vm = new Vue({
			el:'#root',
			data:{
				name:'北京',	
				styleObj:{
					fontSize: '40px',
					color:'red',
				},
				styleObj2:{
					backgroundColor:'orange'
				},
				styleArr:[
					{
						fontSize: '40px',
						color:'blue',
					},
					{
						backgroundColor:'gray'
					}
				]
			},
			
		})
	</script>
```

### 2.5、条件渲染

```python
条件渲染：
    1.v-if
        写法：
            (1).v-if="表达式" 
            (2).v-else-if="表达式"
            (3).v-else="表达式"
        适用于：切换频率较低的场景。
        特点：不展示的DOM元素直接被移除。
        注意：v-if可以和:v-else-if、v-else一起使用，但要求结构不能被“打断”。

    2.v-show
        写法：v-show="表达式"
        适用于：切换频率较高的场景。
        特点：不展示的DOM元素未被移除，仅仅是使用样式隐藏掉

    3.备注：使用v-if的时，元素可能无法获取到，而使用v-show一定可以获取到。
```

```html
	<body>
	
		<!-- 准备好一个容器-->
		<div id="root">
			<h2>当前的n值是:{{n}}</h2>
			<button @click="n++">点我n+1</button>
			<!-- 使用v-show做条件渲染 -->
			<!-- <h2 v-show="false">欢迎来到{{name}}</h2> -->
			<!-- <h2 v-show="1 === 1">欢迎来到{{name}}</h2> -->

			<!-- 使用v-if做条件渲染 -->
			<!-- <h2 v-if="false">欢迎来到{{name}}</h2> -->
			<!-- <h2 v-if="1 === 1">欢迎来到{{name}}</h2> -->

			<!-- v-else和v-else-if -->
			<!-- <div v-if="n === 1">Angular</div>
			<div v-else-if="n === 2">React</div>
			<div v-else-if="n === 3">Vue</div>
			<div v-else>哈哈</div> -->

			<!-- v-if与template的配合使用 -->
			<template v-if="n === 1">
				<h2>你好</h2>
				<h2>Vue</h2>
				<h2>北京</h2>
			</template>

		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false

		const vm = new Vue({
			el:'#root',
			data:{
				name:'Vue',
				n:0
			}
		})
	</script>
```

### 2.6、列表渲染

#### 基本列表

```python
v-for指令:
    1.用于展示列表数据
    2.语法：v-for="(item, index) in xxx" :key="yyy"
    3.可遍历：数组、对象、字符串（用的很少）、指定次数（用的很少）
```



```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
	<script src="../js/vue.js"></script>
</head>
<body>
	<div id="root">
		<!-- 遍历数组 -->
		<h2>人员列表</h2>
		<ul>
			<li v-for="(p,index) in persons" :key="p.id">
				{{p.name}}-{{p.age}}
			</li>
		</ul>

		<!-- 遍历对象 -->
		<h2>汽车信息</h2>
		<ul>
			<li v-for="(value,k) of car" :key="k">
				{{k}}-{{value}}

			</li>
		</ul>

		<!-- 遍历字符串 -->
		<h3>遍历字符串</h3>
		<ul>
			<li v-for="(char,index) of str" ::key="index">
				{{char}}-{{index}}
			</li>
		</ul>
		
		<!-- 遍历指定次数 -->
		<h3>遍历指定次数</h3>
		<ul>
			<li v-for="(number,index) of 5" :key="index">
				{{index}}-{{number}}
			</li>
		</ul>
	</div>
</body>
<script>
	Vue.config.productionTip = false;
	new Vue({
		el:'#root',
		data:{
			persons:[
				{id:'001',name:'张三',age:18},
				{id:'002',name:'李四',age:19},
				{id:'003',name:'王五',age:20}
			],
			car:{
				name:'奥迪A8',
				price:'78万',
				color:'黑色',
			},
			str:'helloVue'
		}

	})
</script>
</html>
```

#### key的工作原理

```python
面试题：react、vue中的key有什么作用？（key的内部原理）

1. 虚拟DOM中key的作用：
    key是虚拟DOM对象的标识，当数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】, 
    随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较，比较规则如下：

2.对比规则：
    (1).旧虚拟DOM中找到了与新虚拟DOM相同的key：
    ①.若虚拟DOM中内容没变, 直接使用之前的真实DOM！
    ②.若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM。

    (2).旧虚拟DOM中未找到与新虚拟DOM相同的key
    创建新的真实DOM，随后渲染到到页面。

3. 用index作为key可能会引发的问题：
    1. 若对数据进行：逆序添加、逆序删除等破坏顺序操作:
        会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。

    2. 如果结构中还包含输入类的DOM：
    会产生错误DOM更新 ==> 界面有问题。

4. 开发中如何选择key?:
    1.最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值。
    2.如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，
    使用index作为key是没有问题的。
```

![image-20221223202028986](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221223202032.png)

![image-20221223202057512](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221223202100.png)

```html
<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<!-- 遍历数组 -->
			<h2>人员列表（遍历数组）</h2>
			<button @click.once="add">添加一个老刘</button>
			<ul>
				<li v-for="(p,index) of persons" :key="index">
					{{p.name}}-{{p.age}}
					<input type="text">
				</li>
			</ul>
		</div>

		<script type="text/javascript">
			Vue.config.productionTip = false
			
			new Vue({
				el:'#root',
				data:{
					persons:[
						{id:'001',name:'张三',age:18},
						{id:'002',name:'李四',age:19},
						{id:'003',name:'王五',age:20}
					]
				},
				methods: {
					add(){
						const p = {id:'004',name:'老刘',age:40}
						this.persons.unshift(p)
					}
				},
			})
		</script>
```

#### 列表过滤

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>列表过滤</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<h2>人员列表</h2>
			<input type="text" placeholder="请输入名字" v-model="keyWord">
			<ul>
				<li v-for="(p,index) of filPerons" :key="index">
					{{p.name}}-{{p.age}}-{{p.sex}}
				</li>
			</ul>
		</div>

		<script type="text/javascript">
			Vue.config.productionTip = false
			
			//用watch实现
			//#region 
			/* new Vue({
				el:'#root',
				data:{
					keyWord:'',
					persons:[
						{id:'001',name:'马冬梅',age:19,sex:'女'},
						{id:'002',name:'周冬雨',age:20,sex:'女'},
						{id:'003',name:'周杰伦',age:21,sex:'男'},
						{id:'004',name:'温兆伦',age:22,sex:'男'}
					],
					filPerons:[]
				},
				watch:{
					keyWord:{
						immediate:true,
						handler(val){
							this.filPerons = this.persons.filter((p)=>{
								return p.name.indexOf(val) !== -1
							})
						}
					}
				}
			}) */
			//#endregion
			
			//用computed实现
			new Vue({
				el:'#root',
				data:{
					keyWord:'',
					persons:[
						{id:'001',name:'马冬梅',age:19,sex:'女'},
						{id:'002',name:'周冬雨',age:20,sex:'女'},
						{id:'003',name:'周杰伦',age:21,sex:'男'},
						{id:'004',name:'温兆伦',age:22,sex:'男'}
					]
				},
				computed:{
					filPerons(){
						return this.persons.filter((p)=>{
							return p.name.indexOf(this.keyWord) !== -1
						})
					}
				}
			}) 
		</script>
</html>
```



# 2、vue-cli

# 3、vue-router

# 4、vuex

# 5、element-ui

# 6、vue3