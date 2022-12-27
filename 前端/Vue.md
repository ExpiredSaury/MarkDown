# 1、vue核心基础

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

1. <span style="color:red">插值语法</span>：
   * 功能：用于解析标签体内容太
   * 写法：{{xxx}} xxx是js表达式，且可以直接读取到data中的所有属性
2. <span style="color:red">指令语法</span>：
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

#### 列表排序

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
			<button @click="sortType = 2">年龄升序</button>
			<button @click="sortType = 1">年龄降序</button>
			<button @click="sortType = 0">原顺序</button>
			<ul>
				<li v-for="(p,index) of filPerons" :key="p.id">
					{{p.name}}-{{p.age}}-{{p.sex}}
				</li>
			</ul>
		</div>

		<script type="text/javascript">
			Vue.config.productionTip = false
			
			//用computed实现
			new Vue({
				el:'#root',
				data:{
					keyWord:'',
					sortType:0,  //0代表原顺序，1降序，2升序
					persons:[
						{id:'001',name:'马冬梅',age:19,sex:'女'},
						{id:'002',name:'周冬雨',age:20,sex:'女'},
						{id:'003',name:'周杰伦',age:21,sex:'男'},
						{id:'004',name:'温兆伦',age:22,sex:'男'}
					],
					
				},
				computed:{
					filPerons(){
						const arr =  this.persons.filter((p)=>{
							return p.name.indexOf(this.keyWord) !== -1
						})
						//判断以下是否需要排序
						if(this.sortType){
							arr.sort((p1,p2)=>{
								return this.sortType === 1 ? p2.age-p1.age : p1.age-p2.age
							})

						}
						return arr
					}
				}
			}) 
		</script>
</html>
```

#### 列表更新

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>更新时的一个问题</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<h2>人员列表</h2>
			<button @click="updateMei">更新马冬梅的信息</button>
			<ul>
				<li v-for="(p,index) of persons" :key="p.id">
					{{p.name}}-{{p.age}}-{{p.sex}}
				</li>
			</ul>
		</div>

		<script type="text/javascript">
			Vue.config.productionTip = false
			
			const vm = new Vue({
				el:'#root',
				data:{
					persons:[
						{id:'001',name:'马冬梅',age:30,sex:'女'},
						{id:'002',name:'周冬雨',age:31,sex:'女'},
						{id:'003',name:'周杰伦',age:18,sex:'男'},
						{id:'004',name:'温兆伦',age:19,sex:'男'}
					]
				},
				methods: {
					updateMei(){
						// this.persons[0].name = '马老师' //奏效
						// this.persons[0].age = 50 //奏效
						// this.persons[0].sex = '男' //奏效
						// this.persons[0] = {id:'001',name:'马老师',age:50,sex:'男'} //不奏效
						this.persons.splice(0,1,{id:'001',name:'马老师',age:50,sex:'男'})
					}
				}
			}) 

		</script>
</html>
```

#### Vue监测数据改变的原理_对象

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>Vue监测数据改变的原理</title>
		<!-- 引入Vue -->
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<h2>学校名称：{{name}}</h2>
			<h2>学校地址：{{address}}</h2>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		const vm = new Vue({
			el:'#root',
			data:{
				name:'北大',
				address:'北京',
				student:{
					name:'tom',
					age:{
						rAge:40,
						sAge:29,
					},
					friends:[
						{name:'jerry',age:35}
					]
				}
			}
		})
	</script>
</html>
```



#### Vue.set使用

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>Vue.set的使用</title>
		<!-- 引入Vue -->
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<h1>学校信息</h1>
			<h2>学校名称：{{school.name}}</h2>
			<h2>学校地址：{{school.address}}</h2>
			<h2>校长是：{{school.leader}}</h2>
			<hr/>
			<h1>学生信息</h1>
			<button @click="addSex">添加一个性别属性，默认值是男</button>
			<h2>姓名：{{student.name}}</h2>
			<h2 v-if="student.sex">性别：{{student.sex}}</h2>
			<h2>年龄：真实{{student.age.rAge}}，对外{{student.age.sAge}}</h2>
			<h2>朋友们</h2>
			<ul>
				<li v-for="(f,index) in student.friends" :key="index">
					{{f.name}}--{{f.age}}
				</li>
			</ul>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		const vm = new Vue({
			el:'#root',
			data:{
				school:{
					name:'清华',
					address:'北京',
				},
				student:{
					name:'tom',
					age:{
						rAge:40,
						sAge:29,
					},
					friends:[
						{name:'jerry',age:35},
						{name:'tony',age:36}
					]
				}
			},
			methods: {
				addSex(){
					// Vue.set(this.student,'sex','男')
					this.$set(this.student,'sex','男')
				}
			}
		})
	</script>
</html>
```

#### Vue监测数据改变的原理_数组

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>Vue监测数据改变的原理_数组</title>
		<!-- 引入Vue -->
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<h1>学校信息</h1>
			<h2>学校名称：{{school.name}}</h2>
			<h2>学校地址：{{school.address}}</h2>
			<h2>校长是：{{school.leader}}</h2>
			<hr/>
			<h1>学生信息</h1>
			<button @click="addSex">添加一个性别属性，默认值是男</button>
			<h2>姓名：{{student.name}}</h2>
			<h2 v-if="student.sex">性别：{{student.sex}}</h2>
			<h2>年龄：真实{{student.age.rAge}}，对外{{student.age.sAge}}</h2>
			<h2>爱好</h2>
			<ul>
				<li v-for="(h,index) in student.hobby" :key="index">
					{{h}}
				</li>
			</ul>
			<h2>朋友们</h2>
			<ul>
				<li v-for="(f,index) in student.friends" :key="index">
					{{f.name}}--{{f.age}}
				</li>
			</ul>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		const vm = new Vue({
			el:'#root',
			data:{
				school:{
					name:'尚硅谷',
					address:'北京',
				},
				student:{
					name:'tom',
					age:{
						rAge:40,
						sAge:29,
					},
					hobby:['抽烟','喝酒','烫头'],
					friends:[
						{name:'jerry',age:35},
						{name:'tony',age:36}
					]
				}
			},
			methods: {
				addSex(){
					// Vue.set(this.student,'sex','男')
					this.$set(this.student,'sex','男')
				}
			}
		})
	</script>
</html>
```

![image-20221224123403537](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221224123405.png)

#### 总结

```python
Vue监视数据的原理：
    1. vue会监视data中所有层次的数据。

    2. 如何监测对象中的数据？
        通过setter实现监视，且要在new Vue时就传入要监测的数据。
        (1).对象中后追加的属性，Vue默认不做响应式处理
        (2).如需给后添加的属性做响应式，请使用如下API：
        Vue.set(target，propertyName/index，value) 或 
        vm.$set(target，propertyName/index，value)

    3. 如何监测数组中的数据？
    	通过包裹数组更新元素的方法实现，本质就是做了两件事：
        (1).调用原生对应的方法对数组进行更新。
        (2).重新解析模板，进而更新页面。

    4.在Vue修改数组中的某个元素一定要用如下方法：
        1.使用这些API:push()、pop()、shift()、unshift()、splice()、sort()、reverse()
        2.Vue.set() 或 vm.$set()

    特别注意：Vue.set() 和 vm.$set() 不能给vm 或 vm的根数据对象 添加属性！！！

```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>总结数据监视</title>
		<style>
			button{
				margin-top: 10px;
			}
		</style>
		<!-- 引入Vue -->
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		
		<!-- 准备好一个容器-->
		<div id="root">
			<h1>学生信息</h1>
			<button @click="student.age++">年龄+1</button><br>
			<button @click="addSex">添加性别属性，默认值：男</button><br>
			<button @click="student.sex= '未知'">修改性别属性</button><br>
			<button @click="addFriend">在列表首位添加一个朋友</button><br>
			<button @click="updateFirstFriendName">修改第一个朋友的名字为张三</button><br>
			<button @click="addHobby">添加一个爱好</button><br>
			<button @click="updateFirstHobby">修改第一个 爱好为 开车</button><br>
			<button @click="removeSmoke">过滤掉爱好中的 抽烟</button><br>
			<h3>姓名：{{student.name}}</h3>
			<h3>年龄：{{student.age}}</h3>
			<h3 v-if="student.sex">性别：{{student.sex}}</h3>
			<h3>爱好：</h3>
			<ul>
				<li v-for="(h,index) in student.hobby" :key="index">
					{{h}}
				</li>
			</ul>
			<h3>朋友们：</h3>
			<ul>
				<li v-for="(f,index) in student.friends" :key="index">
					{{f.name}}--{{f.age}}
				</li>
			</ul>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		const vm = new Vue({
			el:'#root',
			data:{
				student:{
					name:'tom',
					age:18,
					hobby:['抽烟','喝酒','烫头'],
					friends:[
						{name:'jerry',age:35},
						{name:'tony',age:36}
					]
				}
			},
			methods: {
				addSex(){
					// Vue.set(this.student,'sex','男')
					vm.$set(this.student,'sex','男')
				},
				addFriend(){
					this.student.friends.unshift({
						name:'莎莎',
						age:70,
					})
				},
				updateFirstFriendName(){
					this.student.friends[0].name='张三'
					// this.student.friends[0].age=18
				},
				addHobby(){
					this.student.hobby.push('学习')
				},
				updateFirstHobby(){
					// this.student.hobby.splice(0,1,'开车')
						// Vue.set(this.student.hobby,0,'开车')
						this.$set(this.student.hobby,0,'开车')
				},
				removeSmoke(){
					this.student.hobby = this.student.hobby.filter((h)=>{
						return h!== '抽烟'
					})
				}


			},
			
		})
	</script>
</html>
```

### 2.7、收集表单数据

```python

收集表单数据：
    若：<input type="text"/>，则v-model收集的是value值，用户输入的就是value值。
    若：<input type="radio"/>，则v-model收集的是value值，且要给标签配置value值。
    若：<input type="checkbox"/>
        1.没有配置input的value属性，那么收集的就是checked（勾选 or 未勾选，是布尔值）
        2.配置input的value属性:
            (1)v-model的初始值是非数组，那么收集的就是checked（勾选 or 未勾选，是布尔值）
            (2)v-model的初始值是数组，那么收集的的就是value组成的数组
    备注：v-model的三个修饰符：
        lazy：失去焦点再收集数据
        number：输入字符串转为有效的数字
        trim：输入首尾空格过滤

```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>收集表单数据</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		
		<!-- 准备好一个容器-->
		<div id="root">
			<form @submit.prevent="demo">
				账号： <input type="text" v-model.trim="userInfo.account"><br><br>
				密码： <input type="password" v-model="userInfo.password"><br><br>
				年龄 <input type="number" v-model.number="userInfo.age"><br><br>
				性别： 
				男：<input type="radio" name="sex" value="male" v-model="userInfo.sex">
				女：<input type="radio" name="sex" value="female" v-model="userInfo.sex"><br><br>
				爱好:
				学习：<input type="checkbox" value="study" v-model="userInfo.hobby">
				打游戏<input type="checkbox" value="game" v-model="userInfo.hobby">
				吃饭：<input type="checkbox" value="eat" v-model="userInfo.hobby"> 
				<br><br>
				所属校区：
				<select v-model="userInfo.city">
					<option value="">请选择校区</option>
					<option value="beijing">北京</option>
					<option value="shanghai">上海</option>
					<option value="guangdong">广东</option>
					<option value="wuhan">武汉</option>
				</select>
				<br><br>
				其他信息：
				<textarea v-model.lazy="userInfo.other"></textarea>
				<br><br>
				<input type="checkbox" v-model="userInfo.agree"> 阅读并接受<a href="http://www.baidu.com">《用户协议》</a>
				<br><br>
				<button>提交</button>
			</form>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false
		new Vue({
			el:'#root',
			data:{
				userInfo:{
					account:'',
					password:'',
					sex:'female',
					hobby:[],
					city:'beijing',
					other:'',
					agree:'',
				}
			},
			methods: {
				demo(){
				
				}
			},
		})
		
	</script>
</html>
```

### 2.8、过滤器

```python
过滤器：
    定义：
    	对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）。
    语法：
        1.注册过滤器：Vue.filter(name,callback) 或 new Vue{filters:{}}
        2.使用过滤器：{{ xxx | 过滤器名}}  或  v-bind:属性 = "xxx | 过滤器名"
    备注：
    	1.过滤器也可以接收额外参数、多个过滤器也可以串联

```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>过滤器</title>
		<script type="text/javascript" src="../js/vue.js"></script>
		<script type="text/javascript" src="../js/dayjs.min.js"></script>
	</head>
	<body>
		
		<!-- 准备好一个容器-->
		<div id="root">
			<h2>显示格式化后的时间</h2>
			<!-- 计算属性实现 -->
			<h3>现在是：{{fmtTime}}</h3>
			<!-- methods实现 -->
			<h3>现在是：{{getFmtTime()}}</h3>
			<!-- 过滤器实现 -->
			<h3>现在是：{{time | timeFormater1 }}</h3>
			<!-- 过滤器实现（传参） -->
			<h3>现在是：{{time | timeFormater2('YYYY_MM_DD')}}</h3>
			<h3>现在是：{{time | timeFormater3('YYYY_MM_DD') | mySlice}}</h3>
			
			
		</div>

		<div id="root2">
			<h3>{{msg | mySlice}}</h3>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false
        //全局过滤器

		Vue.filter('mySlice',function(value){
			return value.slice(0,4)
		})
		
		new Vue({
			el:'#root',
			data:{
				time:Date.now()
			},
			computed:{
				fmtTime(){
					return dayjs(this.time).format('YYYY-MM-DD HH:mm:ss')
				}
			},
			methods: {
				getFmtTime(){
					return dayjs(this.time).format('YYYY-MM-DD HH:mm:ss')
				}
			},
			filters:{
				timeFormater1(value){
					return dayjs(value).format('YYYY-MM-DD HH:mm:ss')
				},
				timeFormater2(value,str){
					return dayjs(value).format(str)
				},
				timeFormater3(value,str='YYY年MM月DD日 HH:mm:ss'){
					return dayjs(value).format(str)
				},
				mySlice(value){
					return value.slice(0,4)
				}
			},
			
		})
		new Vue({
			el:'#root2',
			data:{
				msg:'Hello Vue'
			}
		})
	
	</script>
</html>
```

### 2.9、内置指令

#### v-text

```python

我们学过的指令：
    v-bind	: 单向绑定解析表达式, 可简写为 :xxx
    v-model	: 双向数据绑定
    v-for  	: 遍历数组/对象/字符串
    v-on   	: 绑定事件监听, 可简写为@
    v-if 	 	: 条件渲染（动态控制节点是否存存在）
    v-else 	: 条件渲染（动态控制节点是否存存在）
    v-show 	: 条件渲染 (动态控制节点是否展示)
v-text指令：
    1.作用：向其所在的节点中渲染文本内容。
    2.与插值语法的区别：v-text会替换掉节点中的内容，{{xx}}则不会。

```



```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>v-text指令</title>
		<!-- 引入Vue -->
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<div>你好，{{name}}</div>
			<div v-text="name"></div>
			<div v-text="str"></div>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
		
		new Vue({
			el:'#root',
			data:{
				name:'Vue',
				str:'<h3>你好啊！</h3>'
			}
		})
	</script>
</html>
```

#### v-html

```python
v-html指令：
    1.作用：向指定节点中渲染包含html结构的内容。
    2.与插值语法的区别：
        (1).v-html会替换掉节点中所有的内容，{{xx}}则不会。
        (2).v-html可以识别html结构。
    3.严重注意：v-html有安全性问题！！！！
        (1).在网站上动态渲染任意HTML是非常危险的，容易导致XSS攻击。
        (2).一定要在可信的内容上使用v-html，永不要用在用户提交的内容上！
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>v-html指令</title>
		<!-- 引入Vue -->
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<div>你好，{{name}}</div>
			<div v-html="str"></div>
			<div v-html="str2"></div>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		new Vue({
			el:'#root',
			data:{
				name:'尚硅谷',
				str:'<h3>你好啊！</h3>',
				str2:'<a href=javascript:location.href="http://www.baidu.com?"+document.cookie>兄弟我找到你想要的资源了，快来！</a>',
			}
		})
	</script>
</html>
```

#### v-cloak

```python
v-cloak指令（没有值）：
    1.本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性。
    2.使用css配合v-cloak可以解决网速慢时页面展示出{{xxx}}的问题。
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>v-cloak指令</title>
		<style>
			[v-cloak]{
				display:none;
			}
		</style>
		<!-- 引入Vue -->
	</head>
	<body>
		
		<!-- 准备好一个容器-->
		<div id="root">
			<h2 v-cloak>{{name}}</h2>
		</div>
		<script type="text/javascript" src="http://localhost:8080/resource/5s/vue.js"></script>
	</body>
	
	<script type="text/javascript">
		console.log(1)
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
		
		new Vue({
			el:'#root',
			data:{
				name:'尚硅谷'
			}
		})
	</script>
</html>
```



#### v-once

```python
v-once指令：
    1.v-once所在节点在初次动态渲染后，就视为静态内容了。
    2.以后数据的改变不会引起v-once所在结构的更新，可以用于优化性能。
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>v-once指令</title>
		<!-- 引入Vue -->
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		
		<!-- 准备好一个容器-->
		<div id="root">
			<h2 v-once>初始化的n值是:{{n}}</h2>
			<h2>当前的n值是:{{n}}</h2>
			<button @click="n++">点我n+1</button>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
		
		new Vue({
			el:'#root',
			data:{
				n:1
			}
		})
	</script>
</html>
```



#### v-pre

```python
v-pre指令：
    1.跳过其所在节点的编译过程。
    2.可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译。
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>v-pre指令</title>
		<!-- 引入Vue -->
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		
		<!-- 准备好一个容器-->
		<div id="root">
			<h2 v-pre>Vue其实很简单</h2>
			<h2 >当前的n值是:{{n}}</h2>
			<button @click="n++">点我n+1</button>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		new Vue({
			el:'#root',
			data:{
				n:1
			}
		})
	</script>
</html>
```



### 3.0、自定义指令

需求1：定义一个v-big指令，和v-text功能类似，但会把绑定的数值放大10倍。
需求2：定义一个v-fbind指令，和v-bind功能类似，但可以让其所绑定的input元素默认获取焦点。

```python
自定义指令总结：
    一、定义语法：
        (1).局部指令：
        new Vue({															new Vue({
            directives:{指令名:配置对象}   或   		directives{指令名:回调函数}
        }) 																		})
        (2).全局指令：
        Vue.directive(指令名,配置对象) 或   Vue.directive(指令名,回调函数)

    二、配置对象中常用的3个回调：
        (1).bind：指令与元素成功绑定时调用。
        (2).inserted：指令所在元素被插入页面时调用。
        (3).update：指令所在模板结构被重新解析时调用。

    三、备注：
        1.指令定义时不加v-，但使用时要加v-；
        2.指令名如果是多个单词，要使用kebab-case命名方式，不要用camelCase命名。

```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>自定义指令</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<h2>{{name}}</h2>
			<h2>当前的n值是：<span v-text="n"></span> </h2>
			<!-- <h2>放大10倍后的n值是：<span v-big-number="n"></span> </h2> -->
			<h2>放大10倍后的n值是：<span v-big="n"></span> </h2>
			<button @click="n++">点我n+1</button>
			<hr/>
			<input type="text" v-fbind:value="n">
		</div>
	</body>
	
	<script type="text/javascript">
		Vue.config.productionTip = false

		//定义全局指令
		/* Vue.directive('fbind',{
			//指令与元素成功绑定时（一上来）
			bind(element,binding){
				element.value = binding.value
			},
			//指令所在元素被插入页面时
			inserted(element,binding){
				element.focus()
			},
			//指令所在的模板被重新解析时
			update(element,binding){
				element.value = binding.value
			}
		}) */

		new Vue({
			el:'#root',
			data:{
				name:'北大',
				n:1
			},
			directives:{
				//big函数何时会被调用？1.指令与元素成功绑定时（一上来）。2.指令所在的模板被重新解析时。
				/* 'big-number'(element,binding){
					// console.log('big')
					element.innerText = binding.value * 10
				}, */
				big(element,binding){
					console.log('big',this) //注意此处的this是window
					// console.log('big')
					element.innerText = binding.value * 10
				},
				fbind:{
					//指令与元素成功绑定时（一上来）
					bind(element,binding){
						element.value = binding.value
					},
					//指令所在元素被插入页面时
					inserted(element,binding){
						element.focus()
					},
					//指令所在的模板被重新解析时
					update(element,binding){
						element.value = binding.value
					}
				}
			}
		})
		
	</script>
</html>
```





### 3.1、生命周期

```python
生命周期：
    1.又名：生命周期回调函数、生命周期函数、生命周期钩子。
    2.是什么：Vue在关键时刻帮我们调用的一些特殊名称的函数。
    3.生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的。
    4.生命周期函数中的this指向是vm 或 组件实例对象。		
```



```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>引出生命周期</title>
		<!-- 引入Vue -->
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<h2 v-if="a">你好啊</h2>
			<h2 :style="{opacity}">欢迎学习Vue</h2>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
		
		 new Vue({
			el:'#root',
			data:{
				a:false,
				opacity:1
			},
			methods: {
				
			},
			//Vue完成模板的解析并把初始的真实DOM元素放入页面后（挂载完毕）调用mounted
			mounted(){
				console.log('mounted',this)
				setInterval(() => {
					this.opacity -= 0.01
					if(this.opacity <= 0) this.opacity = 1
				},16)
			},
		})
 
		//通过外部的定时器实现（不推荐）
		/* setInterval(() => {
			vm.opacity -= 0.01
			if(vm.opacity <= 0) vm.opacity = 1
		},16) */
	</script>
</html>
```

![生命周期](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221224182232.png)



```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>分析生命周期</title>
		<!-- 引入Vue -->
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<!-- 准备好一个容器-->
		<div id="root" :x="n">
			<h2 v-text="n"></h2>
			<h2>当前的n值是：{{n}}</h2>
			<button @click="add">点我n+1</button>
			<button @click="bye">点我销毁vm</button>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		new Vue({
			el:'#root',
			// template:`
			// 	<div>
			// 		<h2>当前的n值是：{{n}}</h2>
			// 		<button @click="add">点我n+1</button>
			// 	</div>
			// `,
			data:{
				n:1
			},
			methods: {
				add(){
					console.log('add')
					this.n++
				},
				bye(){
					console.log('bye')
					this.$destroy()
				}
			},
			watch:{
				n(){
					console.log('n变了')
				}
			},
			beforeCreate() {
				console.log('beforeCreate')
			},
			created() {
				console.log('created')
			},
			beforeMount() {
				console.log('beforeMount')
			},
			mounted() {
				console.log('mounted')
			},
			beforeUpdate() {
				console.log('beforeUpdate')
			},
			updated() {
				console.log('updated')
			},
			beforeDestroy() {
				console.log('beforeDestroy')
			},
			destroyed() {
				console.log('destroyed')
			},
		})
	</script>
</html>
```

**总结：**

```python
常用的生命周期钩子：
    1.mounted: 发送ajax请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】。
    2.beforeDestroy: 清除定时器、解绑自定义事件、取消订阅消息等【收尾工作】。

关于销毁Vue实例
    1.销毁后借助Vue开发者工具看不到任何信息。
    2.销毁后自定义事件会失效，但原生DOM事件依然有效。
    3.一般不会在beforeDestroy操作数据，因为即便操作数据，也不会再触发更新流程了。
```



```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>引出生命周期</title>
		<!-- 引入Vue -->
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		
		<!-- 准备好一个容器-->
		<div id="root">
			<h2 :style="{opacity}">欢迎学习Vue</h2>
			<button @click="opacity = 1">透明度设置为1</button>
			<button @click="stop">点我停止变换</button>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

		 new Vue({
			el:'#root',
			data:{
				opacity:1
			},
			methods: {
				stop(){
					this.$destroy()
				}
			},
			//Vue完成模板的解析并把初始的真实DOM元素放入页面后（挂载完毕）调用mounted
			mounted(){
				console.log('mounted',this)
				this.timer = setInterval(() => {
					console.log('setInterval')
					this.opacity -= 0.01
					if(this.opacity <= 0) this.opacity = 1
				},16)
			},
			beforeDestroy() {
				clearInterval(this.timer)
				console.log('vm即将驾鹤西游了')
			},
		})

	</script>
</html>
```



# 2、组件化编程

### 1.1、模块与组件、模块化与组件化

==模块的定义：向外提供特定功能的 js 程序, 一般就是一个 js 文件===

==组件的定义：实现应用中**局部**功能**代码**和**资源**的**集合**==

==模块化的定义：当应用中的 js 都以模块来编写的, 那这个应用就是一个模块化的应用。==

==组件化的定义：当应用中的功能都是多组件的方式来编写的, 那这个应用就是一个组件化的应用,。==

### 1.2、非单文件组件

一个文件中包含有n个组件

#### 基本使用

```python
Vue中使用组件的三大步骤：
    一、定义组件(创建组件)
    二、注册组件
    三、使用组件(写组件标签)

一、如何定义一个组件？
    使用Vue.extend(options)创建，其中options和new Vue(options)时传入的那个options几乎一样，但也有点区别；
    区别如下：
        1.el不要写，为什么？ ——— 最终所有的组件都要经过一个vm的管理，由vm中的el决定服务哪个容器。
        2.data必须写成函数，为什么？ ———— 避免组件被复用时，数据存在引用关系。
    备注：使用template可以配置组件结构。

二、如何注册组件？
    1.局部注册：靠new Vue的时候传入components选项
    2.全局注册：靠Vue.component('组件名',组件)

三、编写组件标签：
    <school></school>

```



```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>基本使用</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		
		<!-- 准备好一个容器-->
		<div id="root">
			<h2>{{msg}}</h2>
			<hr>
			<hello></hello>   <!---全局使用组件--->
			<hr>
			<!-- 第三步，编写组件标签 -->
			<xuexiao></xuexiao>
			<hr>
			<!-- 第三步，编写组件标签 -->
			<xuesheng></xuesheng>
	
		</div>

		<div id="root2">
			<hello></hello>	
		</div>

		
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false

		//第一步 创建school组件
		const school =Vue.extend({
			template:`
			<div>
				<h2>学校名称:{{schoolName}}</h2>
			    <h2>学校地址:{{address}}</h2>	
				<button @click="showName">点我提示学校</button>
			</div>
			`,
			data(){
				return {
					schoolName:'清华',
					address:'北京',
				
				}
			},
			methods: {
				showName(){
					alert(this.schoolName)
				}
			},
		})
		//第一步 创建student组件
		const student =Vue.extend({
			template:`
			<div>
				<h2>学生名称：{{studentName}}</h2>
			    <h2>学校年龄:{{age}}</h2>
			</div>
			`,
			data(){
				return {
					studentName:'张三',
					age:18,
				}
			}
		})
		//第一步 创建hello 组件
		const hello = Vue.extend({
			template:`
			<div>
				<h2>你好  {{name}}</h2>	
			</div>	
			`,
			data(){
				return {
					name:'秋刀鱼'
				}

			}
		})
		
		//第二部全局注册组件
		Vue.component('hello',hello)

		// 创建vm
		new Vue({
			el:'#root',
			data:{
				msg:'你好啊'
			},
			// 第二部 注册组件(局部注册)
			components:{
				xuexiao:school,
				xuesheng:student,
			}
		})
		new Vue({
			el:'#root2',
		})
	</script>
</html>
```

#### 几个注意点

```python
几个注意点：
    1.关于组件名:
        一个单词组成：
            第一种写法(首字母小写)：school
            第二种写法(首字母大写)：School
        多个单词组成：
            第一种写法(kebab-case命名)：my-school
            第二种写法(CamelCase命名)：MySchool (需要Vue脚手架支持)
        备注：
            (1).组件名尽可能回避HTML中已有的元素名称，例如：h2、H2都不行。
            (2).可以使用name配置项指定组件在开发者工具中呈现的名字。

    2.关于组件标签:
        第一种写法：<school></school>
        第二种写法：<school/>
        备注：不用使用脚手架时，<school/>会导致后续组件不能渲染。

    3.一个简写方式：
        const school = Vue.extend(options) 可简写为：const school = options

```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>几个注意点</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<h1>{{msg}}</h1>
			<!-- 使用组件 -->
			<school></school>
			<!-- <school/> -->
			
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false
		//定义组件
		const school =Vue.extend({
            name:'School'
			template:`
			<div>
				<h2>学校名称:{{name}}</h2>
			    <h2>学校地址:{{address}}</h2>	
			</div>
			`,
			data(){
				return {
					name:'清华',
					address:'北京'
				}
			}
		})
		

		new Vue({
			el:'#root',
			data:{
				msg:'欢迎学习Vue!'
			},
			//注册组件
			components:{
				// school:school
				school   //简单写法
			}
			
		})
	</script>
</html>
```

#### 组件的嵌套

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>组件的嵌套</title>
		<!-- 引入Vue -->
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<school></school>
			<hello></hello>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
		  
		//定义学生组件
		const student = Vue.extend({
			name:'Student',
			template:`
			<div>
				<h2>学生名称：{{name}}</h2>
				<h2>学生年龄：{{age}}</h2>
			</div>
			`,
			data(){
				return {
					name:'王五',
					age:18
				}
			}

		})
		
		//定义school组件
		const school = Vue.extend({
			name:'School',
			template:`
			<div>
				<h2>学校名称：{{name}}</h2>
				<h2>学校地址：{{address}}</h2>
				<student></student>
			</div>
			`,
			data(){
				return {
					name:'清华',
					address:'北京'
				}
			},
			//注册组件（局部）
			components:{
				student
			}
		})
       //定义hello组件
		const hello = Vue.extend({
			template:`
			<h2>{{msg}}</h2>
			`,
			data(){
				return {
					msg:'Hello Vue!!'
				}
			}
		})
	
		new Vue({
			el:'#root',
			//注册组件（局部）
			components:{school,hello}
		})
	</script>
</html>
```

![image-20221224211017353](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221224211021.png)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>组件的嵌套</title>
		<!-- 引入Vue -->
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<!-- <app></app> -->
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
		  
		//定义学生组件
		const student = Vue.extend({
			name:'Student',
			template:`
			<div>
				<h2>学生名称：{{name}}</h2>
				<h2>学生年龄：{{age}}</h2>
			</div>
			`,
			data(){
				return {
					name:'王五',
					age:18
				}
			}

		})
		
		//定义school组件
		const school = Vue.extend({
			name:'School',
			template:`
			<div>
				<h2>学校名称：{{name}}</h2>
				<h2>学校地址：{{address}}</h2>
				<student></student>
			</div>
			`,
			data(){
				return {
					name:'清华',
					address:'北京'
				}
			},
			//注册组件（局部）
			components:{
				student
			}
		})
       //定义hello组件
		const hello = Vue.extend({
			template:`
			<h2>{{msg}}</h2>
			`,
			data(){
				return {
					msg:'Hello Vue!!'
				}
			}
		})
	
		//定义app组件
		const app =Vue.extend({
			template:`
				<div>
					<hello></hello>
					<school></school>	
				</div>

			`,


			components:{
				school,
			hello
			}
		})
		new Vue({
			template:'<app></app>',
			el:'#root',
			//注册组件（局部）
			components:{app}
		})
	</script>
</html>
```

![image-20221224211529319](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221224211531.png)



#### VueComponent构造函数

```python
关于VueComponent：
    1.school组件本质是一个名为VueComponent的构造函数，且不是程序员定义的，是Vue.extend生成的。

    2.我们只需要写<school/>或<school></school>，Vue解析时会帮我们创建school组件的实例对象，
     即Vue帮我们执行的：new VueComponent(options)。

    3.特别注意：每次调用Vue.extend，返回的都是一个全新的VueComponent！！！！

    4.关于this指向：
    (1).组件配置中：
    	data函数、methods中的函数、watch中的函数、computed中的函数 它们的this均是【VueComponent实例对象】。
    (2).new Vue(options)配置中：
    	data函数、methods中的函数、watch中的函数、computed中的函数 它们的this均是【Vue实例对象】。

    5.VueComponent的实例对象，以后简称vc（也可称之为：组件实例对象）。
    Vue的实例对象，以后简称vm。

```

![image-20221224212319484](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221224212322.png)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>VueComponent</title>
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<!-- 准备好一个容器-->
		<div id="root">
			<school></school>
			<hello></hello>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false
		
		const school=Vue.extend({
			name:'School',
			template:`
			<div>
				<h2>学校名称:{{name}}</h2>
			    <h2>学校地址:{{address}}</h2>	
			</div>
			`,
			data(){
				return {
					name:'北大',
					address:'北京'
				}
			}

		})
		

		const test =Vue.extend({
			template:`
			<span>{{msg}}</span>
			`,
			data(){
				return {
					msg:'测试'
				}
			}
		})
		const hello =Vue.extend({
			template:`
			<div>
				<h2>{{msg}}</h2>
			    <test></test>	
				
			</div>
			`,
			data(){
				return {
					msg:'你好Vue'
				}
			},
			components:{
				test
			}
		})
		// console.log('@@@@@',school);

		const vm =new Vue({
			el:'#root',
			components:{
				school,hello
			}
		})
	
	</script>
</html>
```

![image-20221224214821806](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221224214823.png)

#### 内置关系

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>一个重要的内置关系</title>
		<!-- 引入Vue -->
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<!-- 
				1.一个重要的内置关系：VueComponent.prototype.__proto__ === Vue.prototype
				2.为什么要有这个关系：让组件实例对象（vc）可以访问到 Vue原型上的属性、方法。
		-->
		<!-- 准备好一个容器-->
		<div id="root">
			<school></school>
		</div>
	</body>

	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
		Vue.prototype.x = 99

		//定义school组件
		const school = Vue.extend({
			name:'school',
			template:`
				<div>
					<h2>学校名称：{{name}}</h2>	
					<h2>学校地址：{{address}}</h2>	
					<button @click="showX">点我输出x</button>
				</div>
			`,
			data(){
				return {
					name:'尚硅谷',
					address:'北京'
				}
			},
			methods: {
				showX(){
					console.log(this.x)
				}
			},
		})

		//创建一个vm
		const vm = new Vue({
			el:'#root',
			data:{
				msg:'你好'
			},
			components:{school}
		})

		
		//定义一个构造函数
		/* function Demo(){
			this.a = 1
			this.b = 2
		}
		//创建一个Demo的实例对象
		const d = new Demo()

		console.log(Demo.prototype) //显示原型属性

		console.log(d.__proto__) //隐式原型属性

		console.log(Demo.prototype === d.__proto__)

		//程序员通过显示原型属性操作原型对象，追加一个x属性，值为99
		Demo.prototype.x = 99

		console.log('@',d) */

	</script>
</html>
```

### 1.3、单文件组件

**安装插件：Vetur**

![image-20221225111700390](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221225111702.png)

安装插件后，快速生成组件模板
![动画](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221225113506.gif)

一个文件中只包含1个组件

**步骤一：创建一个Student.vue文件**

template里写结构，script中写脚本，脚本里包含给组件命名，配置数据，配置计算属性，等等。style中配置样式

```vue
<template>
  <div>
    <h2>姓名:{{ name }}</h2>
    <h2>年龄:{{ age }}</h2>
  </div>
</template>


<script>
export default {
  name: "Student",
  data() {
    return {
      name: "李四",
      age: 18,
    };
  },
};
</script>
```

**步骤二：创建一个School.vue文件**

```vue 
<template>
<!-- 组件的结构 -->
<div class="demo">
    <h2>学校名称：{{schoolName}}</h2>
    <h2>学校地址：{{address}}</h2>
    <button @click="showName">点我提示学校名称</button>
</div>

</template>


<script>
//组件交互相关的代码（数据、方法）
export default{
    name:'School',
    data(){
        return {
            schoolName:'北大',
            address:'北京',
        }
    },
    methods:{
        showName(){
            alert(this.schoolName)
        }
    }
}
</script>




<style>
/* 组件的样式 */
    .demo{
        background-color: aqua;
    }
</style>
```

**步骤三：创建一个App.vue文件，汇总所有组件**

```vue
<template>
  <div>
    <School></School>
    <Student></Student>
    
  </div>
</template>

<script>
    //引入组件
    import School from './School.vue'
    import Student from  './Student.vue'
    export default {
        name:'App',
        //注册
        components:{
            School,
            Student
        }
        

}
</script>

<style>

</style>
```

**步骤四：借助main.js创建Vue实例，指定为哪个容器服务**

```js
import App  from './App.vue '


new Vue({
    el:"#root",
    template:`<App></App>`,
    components:{App}
})
```

**步骤五：创建一个index.html页面**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>单文件语法使用</title>
</head>
<body>
    <div id="root">
        <!-- <App></App> -->
    </div>
    <script src="../js/vue.js"></script>
    <script src="./main.js"></script>
</body>
</html>
```

以上步骤不能运行，需要放到脚手架中

# 3、使用Vue脚手架

Vue CLI: Vue **command line interface**

Vue 脚手架是 Vue 官方提供的标准化开发工具（开发平台）

[文档](https://cli.vuejs.org/zh/)

### 1.1、具体步骤

1. 全局安装@vue/cli

```bash
npm install -g @vue/cli
```

2. **切换到要创建项目的目录**，然后使用命令创建项目

```bash
vue create xxxx
```

3. 启动项目

```bash
npm run serve
```

如果出现下载速度缓慢需要配置npm淘宝镜像：

```bash
npm config set registry https://registry.npm.taobao.org
```

![动画](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221225210648.gif)



### 1.2、分析脚手架结构

```python
├── node_modules
├── public
│ ├── favicon.ico: 页签图标
│ └── index.html: 主页面
├── src
│ ├── assets: 存放静态资源
│ │ └── logo.png
│ │── component: 存放组件
│ │ └── HelloWorld.vue
│ │── App.vue: 汇总所有组件
│ │── main.js: 入口文件
├── .gitignore: git 版本管制忽略的配置
├── babel.config.js: babel 的配置文件
├── package.json: 应用包配置文件
├── README.md: 应用描述文件
├── package-lock.json：包版本控制文件
```



```python
当执行了npm run serve 命令之后，直接找src里的main.js
```

```js
/*
该文件是整个项目的入口文件

*/

//引入Vue
import Vue from 'vue'
//引入App组件，它是所有组件的父组件
import App from './App.vue'
//关闭Vue的生产提示
Vue.config.productionTip = false

//创建Vue实例对 --vm

// new Vue({
//   //将App组件放入容器中
//   render: h => h(App),
// }).$mount('#app')

new Vue({
  el:"#app",
  //将App组件放入容器中
  render: h => h(App),
})

```

```python
找到src中的App.vue ，引入组件，进行组件注册
```

```vue
<template>
  <div>
    <img src="./assets/logo.png" alt="logo">
    <School></School>
    <Student></Student>
    
  </div>
</template>

<script>
    //引入组件
    import School from './components/School.vue'
    import Student from  './components/Student.vue'
    export default {
        name:'App',
        //注册
        components:{
            School,
            Student
        }
        

}
</script>

<style>

</style>
```

```python
最后找到容器public下的index.html
```

```html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
    <!-- 针对IE浏览器的特殊配置，让IE浏览器以最高的渲染级别渲染页面 -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!-- 开启移动端的理想视口 -->
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <!-- 配置页签图标 -->
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <!-- 配置网页标题 -->
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <!-- 当浏览器不支持js时，noscript中的元素就会被渲染 -->
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <!-- 容器 -->
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>

```

终端运行npm run serve 报错,可以在vue.config.js中设置如下代码：

![image-20221225130838549](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221225130839.png)

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  lintOnSave:false /*关闭语法检查*/
})

```

![image-20221225131304170](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221225131305.png)

### 1.3、render函数

```python
关于不同版本的Vue：
    1.vue.js与vue.runtime.xxx.js的区别：
        (1).vue.js是完整版的Vue，包含：核心功能+模板解析器。
        (2).vue.runtime.xxx.js是运行版的Vue，只包含：核心功能；没有模板解析器。

    2.因为vue.runtime.xxx.js没有模板解析器，所以不能使用template配置项，需要使用
```

### 1.4、vue.config.js配置文件



![image-20221225140627810](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221225140629.png)

Vue 脚手架隐藏了所有 webpack 相关的配置，若想查看具体的webpakc 配置，请在项目终端执行：

```vue
vue inspect > output.j以查看到Vue脚手架的默认配置
```

使用vue.config.js可以对脚手架进行个性化定制，详情见：https://cli.vuejs.org/zh

下面代码的路径都可以修改

```js
module.exports = {
  pages: {
    index: {
      // page 的入口
      entry: 'src/index/main.js',
      // 模板来源
      template: 'public/index.html',
      // 在 dist/index.html 的输出
      filename: 'index.html',
      // 当使用 title 选项时，
      // template 中的 title 标签需要是 <title><%= htmlWebpackPlugin.options.title %></title>
      title: 'Index Page',
      // 在这个页面中包含的块，默认情况下会包含
      // 提取出来的通用 chunk 和 vendor chunk。
      chunks: ['chunk-vendors', 'chunk-common', 'index']
    },
    // 当使用只有入口的字符串格式时，
    // 模板会被推导为 `public/subpage.html`
    // 并且如果找不到的话，就回退到 `public/index.html`。
    // 输出文件名会被推导为 `subpage.html`。
    subpage: 'src/subpage/main.js'
  }
}
```

vue.config.js文件中配置关闭语法检查

```js
module.exports = defineConfig({
  transpileDependencies: true,
  lintOnSave:false /*关闭语法检查*/
})

```

### 1.5、ref属性

1. 被用来给元素或子组件注册引用信息（id的替代者）
2. 应用在html标签上获取的是真实DOM元素，应用在组件标签上是组件实例对象（vc）
3. 使用方式：
   1. 打标识：```<h1 ref="xxx">.....</h1>``` 或 ```<School ref="xxx"></School>```
   2. 获取：```this.$refs.xxx```

* main.js

```js

//引入vue
import Vue from 'vue'
//引入App
import App from './App.vue'
//关闭Vue的身长提示
Vue.config.productionTip=false

//创建Vue实例---vm
new Vue({
    el:"#app",
    render:h =>h(App)
})
```

* School.vue

```vue
<template>
  <div class="school">
    <h2>学校名称：{{name}}</h2>
    <h2>学校地址：{{address}}</h2>
  </div>
</template>

<script>
export default {
    name:'School',
    data(){
        return {
            name:'北大',
            address:'北京'
        }
    }
}
</script>

<style>
    .school{
        background-color: bisque;
    }
</style>
```

* App.vue

```vue
<template>
  <div>
    <h1 v-text="msg" ref="title"></h1>
    <button ref="btn" @click="showDOM">点我输出上面的DOM元素</button>
    <School ref="sch"/>
  </div>
</template>

<script>
//引入School组件
import School from './components/School.vue'
export default {
    name:'App',
    data(){
        return {
            msg:'学习Vue中'
        }
    },
    components:{
        School
    },
    methods:{
        showDOM(){
            console.log(this.$refs.title);  //真实DOM元素
            console.log(this.$refs.btn);  //真实DOM元素
            console.log(this.$refs.sch);  //School组件的实例对象（vc)
        }
    }
}
</script>

```

![image-20221225143939876](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221225143941.png)

### 1.6、props配置

1. **功能：让组件接收外部传过来的数据**

2. 传递数据：```<Demo name="xxx"/>```

3. 接收数据：

   1. 第一种方式（只接收）：```props:['name'] ```

   2. 第二种方式（限制类型）：```props:{name:String}```

   3. 第三种方式（限制类型、限制必要性、指定默认值）：

      ```js
      props:{
      	name:{
      	type:String, //类型
      	required:true, //必要性
      	default:'老王' //默认值
      	}
      }
      ```

   > 备注：props是只读的，Vue底层会监测你对props的修改，如果进行了修改，就会发出警告，若业务需求确实需要修改，那么请复制props的内容到data中一份，然后去修改data中的数据。



* Student.vue

```vue
<template>
  <div>
    <h1>{{msg}}</h1>
    <h2>学生姓名：{{name}}</h2>
    <h2>学生性别：{{sex}}</h2>
    <h2>学生年龄：{{age+1}}</h2>
  </div>
</template>

<script>
export default {
    name:'School',
    data(){
        return {
            msg:'我是一名学生',
        }
    },
    // props:['name','sex','age']  //简单声明接收

    //接收的同时，对数据进行类型限制
    // props:{
    //     name:String,
    //     age:Number,
    //     sex:String
    // }

    //接收的同时对数据类型进行限制，默认值的指定 + 必要性的限制
    props:{
        name:{
            type:String,  //name的类型
            require:true  //name是必传的
        },
        age:{
            type:Number,
            default:99   //默认值
        },
        sex:{
            type:String,
            require:true,
        }
    }
}
</script>


```

* App.vue

```vue
<template>
  <div>
    <Student name="李四" sex="女" :age="18"/>
  </div>
</template>

<script>
//引入School组件
import Student  from './components/Student.vue'
export default {
    name:'App',
    components:{
        Student
    },
   
}
</script>

```

### 1.7、mixin混入

1. 功能：可以把多个组件共用的配置提取成一个混入对象

2. 使用方式：

   第一步定义混合：

   ```
   {
       data(){....},
       methods:{....}
       ....
   }
   ```

   第二步使用混入：

   ​	全局混入：```Vue.mixin(xxx)```
   ​	局部混入：```mixins:['xxx']	```

   ![image-20221225171151049](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221225171152.png)

局部混入

* Student.vue

```vue
<template>
  <div>

    <h2 @click="showName">学生姓名：{{ name }}</h2>
    <h2>学生性别：{{ sex }}</h2>
  </div>
</template>

<script>
//引入mixin
import {mixin,mixin2} from '../mixin'
export default {
  name: "School",
  data() {
    return {
      
      name: "张三",
      sex: "男",
    };
  },
  //使用
  mixins:[mixin,mixin2]
};
</script>


```



* School.vue

```vue
<template>
  <div>

    <h2 @click="showName">学校名称：{{ name }}</h2>
    <h2>学校地址：{{ address }}</h2>
  </div>
</template>

<script>
import {mixin,mixin2} from '../mixin'
export default {
  name: "School",
  data() {
    return {
  
      name: "北大",
      address: "北京",
    };
  },
  mixins:[mixin,mixin2]

};
</script>


```



* src/mixin.js

```js
export const mixin ={
    methods:{
        showName(){
            alert(this.name)
        }
      },
      mounted() {
        console.log('你好啊！！');
      },
}
export const mixin2={
    data(){
        return {
            x:100,
            y:200
        }
    }
}
```



全局混入

```js
//main.js
import {mixin,mixin2} from './mixin'
Vue.mixin(mixin)
Vue.mixin(mixin2)
```

### 1.8、插件

1. 功能：用于增强Vue

2. 本质：包含install方法的一个对象，install的第一个参数是Vue，第二个以后的参数是插件使用者传递的数据。

3. 定义插件：

   ```js
   对象.install = function (Vue, options) {
       // 1. 添加全局过滤器
       Vue.filter(....)
   
       // 2. 添加全局指令
       Vue.directive(....)
   
       // 3. 配置全局混入(合)
       Vue.mixin(....)
   
       // 4. 添加实例方法
       Vue.prototype.$myMethod = function () {...}
       Vue.prototype.$myProperty = xxxx
   }
   ```

4. 使用插件：```Vue.use()```

* Student.vue

```vue
<template>
  <div>

    <h2>学生姓名：{{ name | mySlice}}</h2>
    <h2>学生性别：{{ sex }}</h2>
    <button @click="test">点我测试hello方法</button>
  </div>
</template>

<script>
//引入mixin

export default {
  name: "School",
  data() {
    return {
      
      name: "张三12313123",
      sex: "男",
    };
  },
  methods:{
    test(){
      this.hello()
    }
  }

};
</script>


```

* src/plugins.js

```js
export default{
    install(Vue,x,y,z){
        console.log(x,y,z)
        //全局过滤器
		Vue.filter('mySlice',function(value){
			return value.slice(0,4)
		})
        //全局混入
        Vue.mixin({
            data(){
                return {
                    x:100,
                    y:200
                }
            }
        })
        //给Vue原型上添加上方法
        Vue.prototype.hello=()=>{alert('你好')}
		
    }
}

```

* main.js

```js
//引入插件
import  plugins from './plugins' 

//使用插件
//Vue.use(plugins)  //不带参数
Vue.use(plugins,1,2,3)  //带参数
```

### 1.9、scoped样式

1. 作用：让样式在局部生效，防止冲突。
2. 写法：```<style scoped>```

```vue
<style scoped>
.demo {
  background-color: yellow;
}
</style>
```

```vue
<template>
  <div class="demo">
    <h2 class="que">学生性别：{{ sex }}</h2>
  </div>
</template>

<style lang="less">
.demo {
  background-color: yellow;
  .que{
    font-size: 40px;
  }
}
```

### 2.0、Todo-list案例

1. 组件化编码流程：

   ​	(1).拆分静态组件：组件要按照功能点拆分，命名不要与html元素冲突。

   ​	(2).实现动态组件：考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用：

   ​			1).一个组件在用：放在组件自身即可。

   ​			2). 一些组件在用：放在他们共同的父组件上（<span style="color:red">状态提升</span>）。

   ​	(3).实现交互：从绑定事件开始。

2. props适用于：

   ​	(1).父组件 ==> 子组件 通信

   ​	(2).子组件 ==> 父组件 通信（要求父先给子一个函数）

3. 使用v-model时要切记：v-model绑定的值不能是props传过来的值，因为props是不可以修改的！

4. props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但不推荐这样做。



* Top.vue

```vue
<template>
  <div class="todo-header">
    <input type="text" placeholder="请输入你的任务名称，按回车键确认" v-model="title" @keyup.enter="add"/>
  </div>
</template>

<script>
import  {nanoid} from 'nanoid'
export default {
  name: "Header",
  props:["addTodo"],
  data(){
   return {
     title:''
   }
  },
  methods:{
    add(){
      //校验数据
      if(!this.title.trim) return alert('输入不能为空')
      //将用户的输入包装成一个todo对象
      const todoObj={id:nanoid(),title:this.title,done:false}
      //通知App组件，添加一个todo对象
      this.addTodo(todoObj)
      //清空输入
      this.title=''
    }
  },
  
}
</script>

<style scoped>
/*header*/
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}

.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}
</style>
```



* List.vue

```vue
<template>
  <ul class="todo-main">
    <Item v-for="todoObj in todos" 
    :key="todoObj.id"  
    :todo="todoObj" 
    :checkTodo="checkTodo"
    :deleteTodo="deleteTodo"/>
  </ul>
</template>

<script>
import Item from "./Item";
export default {
  name: "List",
  components: {
    Item,
  },
  props:['todos','checkTodo','deleteTodo']

};
</script>

<style scoped>
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
</style>
```



* Item.vue

```vue
<template>
  <li>
    <label>
      <input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)"/>
      <span>{{todo.title}}</span>
    </label>
    <button class="btn btn-danger" @click="handelDelete(todo.id)">删除</button>
  </li>
</template>

<script>
  export default {
    name: "Item",
    //声明接收todo对象
    props:['todo','checkTodo','deleteTodo'],
    methods:{
      // 勾选or取消勾选
      handleCheck(id){
        //通知App组件将对应的todo对象的done值取反
        this.checkTodo(id)
      },
      //删除
      handelDelete(id){
        if(confirm('确定删除吗？')){
          this.deleteTodo(id)
        }
      }
    }
  }
</script>

<style scoped>

li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover{
  background-color: #ddd;
}

li:hover button {
  display: block;
}
</style>
```



* Footer.vue

```vue
<template>
  <div class="todo-footer" v-show="total">
    <label>
      <input type="checkbox" :checked="isAll" @change="checkAll"/>
    </label>
    <span> <span>已完成{{doneTotal}}</span> / 全部{{total}}</span>
    <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
  </div>
</template>

<script>
export default {
  name: "Footer",
  props:['todos','checkAllTodo','clearAllTodo'],
  computed:{
    doneTotal(){
      return  this.todos.reduce((pre,todo)=> pre + (todo.done ? 1 :0),0)
    },
    total(){
      return this.todos.length
    },
    isAll(){
      return this.doneTotal == this.total  && this.total >0
    }
  },
  methods:{
    checkAll(e){
     
      this.checkAllTodo(e.target.checked)
    },
    clearAll(){
      this.clearAllTodo()
    }
  }
};
</script>

<style scoped>
/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>
```



* App.vue

```vue
<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <Top :addTodo="addTodo"/>
        <List :todos="todos" :checkTodo="checkTodo" :deleteTodo="deleteTodo"/>
        <Footer :todos="todos" :checkAllTodo="checkAllTodo" :clearAllTodo="clearAllTodo"/>
      </div>
    </div>
  </div>
</template>

<script>
//引入School组件
import Top from "./components/Top.vue";
import List from "./components/List.vue";
import Footer from "./components/Footer.vue";

export default {
  name: "App",
  components: {
    Top,
    List,
    Footer,
  },
  data() {
    return {
      todos: [
        { id: "001", title: "喝酒", done: true },
        { id: "002", title: "打游戏", done: false },
        { id: "003", title: "抽烟", done: true },
      ],
    };
  },
  methods:{
    //添加todo
    addTodo(todoObj){
      this.todos.unshift(todoObj)
    },
    //勾选or取消勾选一个todo
    checkTodo(id){
      this.todos.forEach((todo)=>{
        if(todo.id ===id) todo.done = !todo.done
      })
    },
    //删除一个todo
    deleteTodo(id){
      this.todos=this.todos.filter((todo)=>{
        return todo.id !== id
      })
    },
    //全选or全不选
    checkAllTodo(done){
      this.todos.forEach((todo)=>{
        todo.done =done
      })
    },
    //清除所有已经完成的todo
    clearAllTodo(){
      this.todos=this.todos.filter((todo)=>{
        return !todo.done
      })
    }

  }
}
</script>

<style scoped>
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>

```

* main.js

```js
//引入vue
import Vue from 'vue'
//引入App
import App from './App.vue'

//关闭Vue的身长提示
Vue.config.productionTip=false



//创建Vue实例---vm
new Vue({
    el:"#app",
    render:h =>h(App)
})
```

### 2.1、浏览器本地存储

#### localStorage

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>localStorage</title>
</head>
<body>
    <h2>localStorage</h2>
    <button onclick="saveData()">点我保存数据</button>
    <button onclick="readData()">点我读取数据</button>
    <button onclick="deleteData()">点我删除数据</button>
    <button onclick="clearData()">点我清空数据</button>
</body>
<script>
    let p={name:'张三',age:18}
    function saveData(){   
        window.localStorage.setItem('msg','hello')
        localStorage.setItem('msg2',666)
        localStorage.setItem('person',JSON.stringify(p))
    }
    function readData(){   
        console.log(localStorage.getItem('msg'));
        console.log(localStorage.getItem('msg2'));

        const result =localStorage.getItem('person')
        console.log(JSON.stringify(result))
    }
    function deleteData(){  
        // localStorage.removeItem('msg') 
        localStorage.removeItem('msg2') 
       
    }
    function clearData(){  
        // localStorage.removeItem('msg') 
        localStorage.clear() 
       
    }
</script>
</html>
```

![image-20221226151644910](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221226151647.png)

#### sessionStorage

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>sessionStorage</title>
</head>
<body>
    <h2>sessionStorage</h2>
    <button onclick="saveData()">点我保存数据</button>
    <button onclick="readData()">点我读取数据</button>
    <button onclick="deleteData()">点我删除数据</button>
    <button onclick="clearData()">点我清空数据</button>
</body>
<script>
    let p={name:'张三',age:18}
    function saveData(){   
        window.sessionStorage.setItem('msg','hello')
        sessionStorage.setItem('msg2',666)
        sessionStorage.setItem('person',JSON.stringify(p))
    }
    function readData(){   
        console.log(sessionStorage.getItem('msg'));
        console.log(sessionStorage.getItem('msg2'));

        const result =sessionStorage.getItem('person')
        console.log(JSON.stringify(result))
    }
    function deleteData(){  
        // sessionStorage.removeItem('msg') 
        sessionStorage.removeItem('msg2') 
       
    }
    function clearData(){  
        // sessionStorage.removeItem('msg') 
        sessionStorage.clear() 
       
    }
</script>
</html>
```

![image-20221226151945290](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221226151947.png)

### 2.2、组件的自定义事件

#### 绑定，解绑

* Schoole.vue

```vue
<template>
  <div class="school">
    <h2>学校名称：{{ name }}</h2>
    <h2>学校地址：{{ address }}</h2>
    <button @click="sendSchoolName">把学校名给App</button>
  </div>
</template>

<script>

export default {
  name: "School",
  data() {
    return {
      name: "北大",
      address: "北京",
    }
  },
  props:['getSchoolName'],
  methods:{
    sendSchoolName(){
      this.getSchoolName(this.name)
    }
  }


};
</script>

<style>
  .school{
    background-color: orange;
    padding:5px;
    margin-top:30px ;
  }
</style>


```



* Student.vue

```vue
<template>
  <div class="student">

    <h2>学生姓名：{{ name }}</h2>
    <h2>学生性别：{{ sex }}</h2>
    <h2>当前求和为：{{number}}</h2>
    <button @click="add">点我number++</button>
    <button @click="sendStudentName">把学生名给App</button>
    <button @click="unbind">解绑atguigu事件</button>
    <button @click="death">销毁当前Student组件的实例</button>

  </div>
</template>

<script>

export default {
  name: "School",
  data() {
    return {
      
      name: "张三",
      sex: "男",
      number:0
    };
  },
  methods:{
    add(){
      console.log('add被调用了');
      this.number++
    },
    sendStudentName(){
      //触发Student组件实例身上的atguigu事件
      this.$emit('atguigu',this.name,666,888,999)
      this.$emit('demo')
    },
    unbind(){
      // this.$off('atguigu');  //解绑一个自定义事件
      // this.$off(['atguigu','demo']);  //解绑多个自定义事件
      this.$off();  //解绑所有自定义事件
    },
    death(){
      this.$destroy() //销毁了当前Student组件的实例,销毁后所有Student实例的自定义事件全都不会奏效
    }
  }

};
</script>

<style>
  .student{
    background-color: skyblue;
     padding:5px;
  }
</style>

```

* App.vue

```vue
<template>
  <div class="app">
    <h2>{{ msg }}</h2>
    <!-- 通过父组件给子组件传递函数类型的Props实现：子给父传递数据 -->
    <School :getSchoolName="getSchoolName" />
    <!-- 通过父组件给子组件绑定一个自定义事件实现：子给父传递数据 -->
    <!-- <Student v-on:atguigu="getStudentName" /> -->
    <Student @atguigu="getStudentName" @demo="m1" />
    <!-- <Student ref="student" /> -->
  </div>
</template>

<script>
//引入School组件
import Student from "./components/Student.vue";
import School from "./components/School.vue";
export default {
  name: "App",
  components: {
    Student,
    School,
  },
  data() {
    return {
      msg: "你好Vue~~",
    };
  },
  methods: {
    getSchoolName(name) {
      console.log("App收到了学校名:", name);
    },
    getStudentName(name,...params){
      console.log('App收到了学生名:',name,params)
    },
    m1(){
      console.log('demo事件被触发')
    }
  },
  mounted(){
    setTimeout(()=>{
      this.$refs.student.$on('atguigu',this.getStudentName)
    },3000)
  }
}
</script>
<style>
.app {
  background-color: gray;
  padding: 5px;
}
</style>

```





### 2.3、全局事件总线

1. 一种组件间通信的方式，适用于<span style="color:red">任意组件间通信</span>。

2. 安装全局事件总线：

   ```js
   new Vue({
   	......
   	beforeCreate() {
   		Vue.prototype.$bus = this //安装全局事件总线，$bus就是当前应用的vm
   	},
       ......
   }) 
   ```

3. 使用事件总线：

   1. 接收数据：A组件想接收数据，则在A组件中给$bus绑定自定义事件，事件的<span style="color:red">回调留在A组件自身。</span>

      ```js
      methods(){
        demo(data){......}
      }
      ......
      mounted() {
        this.$bus.$on('xxxx',this.demo)
      }
      ```

   2. 提供数据：```this.$bus.$emit('xxxx',数据)```

4. 最好在beforeDestroy钩子中，用$off去解绑<span style="color:red">当前组件所用到的</span>事件。

* main.js

```js
//引入vue
import Vue from 'vue'
//引入App
import App from './App.vue'
//关闭Vue的身长提示
Vue.config.productionTip=false

// const Demo =Vue.extend({})
// const d = new Demo()



//创建Vue实例---vm
new Vue({
    el:"#app",
    render:h =>h(App),
    beforeCreate(){
        Vue.prototype.$bus= this  //安装全局事件总线
    }
})

```

* Student.vue

```vue
<template>
  <div class="student">
    <h2>学生姓名：{{ name }}</h2>
    <h2>学生性别：{{ sex }}</h2>
    <button @click="sendStudentName">把学生名给School组件</button>
  </div>
</template>

<script>
export default {
  name: "School",
  data() {
    return {
      name: "张三",
      sex: "男",
    };
  },
  methods: {
    sendStudentName() {
      this.$bus.$emit('hello', 666);
    },
  },
  mounted() {
    // console.log('Student',this.x)
  },
};
</script>

<style>
.student {
  background-color: skyblue;
  padding: 5px;
}
</style>

```

* School.vue

```vue
<template>
  <div class="school">
    <h2>学校名称：{{ name }}</h2>
    <h2>学校地址：{{ address }}</h2>
   
  </div>
</template>

<script>

export default {
  name: "School",
  data() {
    return {
      name: "北大",
      address: "北京",
    }
  },
  mounted(){
    // console.log('School',this)
    this.$bus.$on('hello',(data)=>{
      console.log('我是School组件,收到了数据 》',data)
    })
  },
  beforeDestroy(){
    this.$bus.$off('hello')
  }
};
</script>

<style>
  .school{
    background-color: orange;
    padding:5px;
    margin-top:30px ;
  }
</style>


```

### 2.4、消息订阅与发布pubsub-js

1. 一种组件间通信的方式，适用于<span style="color:red">任意组件间通信</span>。

2. 使用步骤：

   1. 安装pubsub：```npm i pubsub-js```

   2. 引入: ```import pubsub from 'pubsub-js'```

   3. 接收数据：A组件想接收数据，则在A组件中订阅消息，订阅的<span style="color:red">回调留在A组件自身。</span>

      ```js
      methods(){
        demo(data){......}
      }
      ......
      mounted() {
        this.pid = pubsub.subscribe('xxx',this.demo) //订阅消息
      }
      ```

   4. 提供数据：```pubsub.publish('xxx',数据)```

   5. 最好在beforeDestroy钩子中，用```PubSub.unsubscribe(pid)```去<span style="color:red">取消订阅。</span>

   * Student.vue

```vue
<template>
  <div class="student">
    <h2>学生姓名：{{ name }}</h2>
    <h2>学生性别：{{ sex }}</h2>
    <button @click="sendStudentName">把学生名给School组件</button>
  </div>
</template>

<script>
import pubsub from 'pubsub-js'
export default {
  name: "School",
  data() {
    return {
      name: "张三",
      sex: "男",
    };
  },
  methods: {
    sendStudentName() {
      // this.$bus.$emit('hello', 666);
      pubsub.publish('hello',666)
    },
  },
  mounted() {
    // console.log('Student',this.x)
  },
};
</script>

<style>
.student {
  background-color: skyblue;
  padding: 5px;
}
</style>

```

* School.vue

```vue
<template>
  <div class="school">
    <h2>学校名称：{{ name }}</h2>
    <h2>学校地址：{{ address }}</h2>
  </div>
</template>

<script>
import pubsub from 'pubsub-js'

export default {
  name: "School",
  data() {
    return {
      name: "北大",
      address: "北京",
    }
  },
  methods:{
    demo(message,data){
      console.log('有人发布了hello消息，hello消息的回调执行了',message,data)
    }
  },
  mounted(){
    // console.log('School',this)
    // this.$bus.$on('hello',(data)=>{
    //   console.log('我是School组件,收到了数据 》',data)
    // })
    this.pubId=pubsub.subscribe('hello',this.demo)
    },
  beforeDestroy(){
    // this.$bus.$off('hello')
    pubsub.unsubscribe(this.pubId)
  }
};
</script>

<style>
  .school{
    background-color: orange;
    padding:5px;
    margin-top:30px ;
  }
</style>


```

### 2.5、nextTick

1. 语法：```this.$nextTick(回调函数)```
2. 作用：在下一次 DOM 更新结束后执行其指定的回调。
3. 什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行。

### 2.6、动画与过度效果

1. 作用：在插入、更新或移除 DOM元素时，在合适的时候给元素添加样式类名。

2. 图示：

![image-20221227183743844](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/20221227183813.png)

1. 写法：

   1. 准备好样式：

      - 元素进入的样式：
        1. v-enter：进入的起点
        2. v-enter-active：进入过程中
        3. v-enter-to：进入的终点
      - 元素离开的样式：
        1. v-leave：离开的起点
        2. v-leave-active：离开过程中
        3. v-leave-to：离开的终点

   2. 使用```<transition>```包裹要过度的元素，并配置name属性：

      ```vue
      <transition name="hello">
      	<h1 v-show="isShow">你好啊！</h1>
      </transition>
      ```

   3. 备注：若有多个元素需要过度，则需要使用：```<transition-group>```，且每个元素都要指定```key```值。

   ---

   

   #### 动画

   ```vue
   <template>
   	<div>
   		<button @click="isShow = !isShow">显示/隐藏</button>
   		<transition name="hello" appear>
   			<h1 v-show="isShow">你好啊！</h1>
   		</transition>
   	</div>
   </template>
   
   <script>
   	export default {
   		name:'Test',
   		data() {
   			return {
   				isShow:true
   			}
   		},
   	}
   </script>
   
   <style scoped>
   	h1{
   		background-color: orange;
   	}
   
   	.hello-enter-active{
   		animation: atguigu 0.5s linear;
   	}
   
   	.hello-leave-active{
   		animation: atguigu 0.5s linear reverse;
   	}
   
   	@keyframes atguigu {
   		from{
   			transform: translateX(-100%);
   		}
   		to{
   			transform: translateX(0px);
   		}
   	}
   </style>
   ```

   #### 过度

   ```vue
   <template>
   	<div>
   		<button @click="isShow = !isShow">显示/隐藏</button>
   		<transition-group name="hello" appear>
   			<h1 v-show="!isShow" key="1">你好啊！</h1>
   			<h1 v-show="isShow" key="2">尚硅谷！</h1>
   		</transition-group>
   	</div>
   </template>
   
   <script>
   	export default {
   		name:'Test',
   		data() {
   			return {
   				isShow:true
   			}
   		},
   	}
   </script>
   
   <style scoped>
   	h1{
   		background-color: orange;
   	}
   	/* 进入的起点、离开的终点 */
   	.hello-enter,.hello-leave-to{
   		transform: translateX(-100%);
   	}
   	.hello-enter-active,.hello-leave-active{
   		transition: 0.5s linear;
   	}
   	/* 进入的终点、离开的起点 */
   	.hello-enter-to,.hello-leave{
   		transform: translateX(0);
   	}
   
   </style>
   ```

   #### 第三方动画库

   ```vue
   <template>
   	<div>
   		<button @click="isShow = !isShow">显示/隐藏</button>
   		<transition-group 
   			appear
   			name="animate__animated animate__bounce" 
   			enter-active-class="animate__swing"
   			leave-active-class="animate__backOutUp"
   		>
   			<h1 v-show="!isShow" key="1">你好啊！</h1>
   			<h1 v-show="isShow" key="2">尚硅谷！</h1>
   		</transition-group>
   	</div>
   </template>
   
   <script>
   	import 'animate.css'
   	export default {
   		name:'Test',
   		data() {
   			return {
   				isShow:true
   			}
   		},
   	}
   </script>
   
   <style scoped>
   	h1{
   		background-color: orange;
   	}
   	
   
   </style>
   ```

   

   

   

# 4、Vue中的ajax

#### 解决Ajax跨域问题

**方法一**

​	在vue.config.js中添加如下配置：

```js
devServer:{
  proxy:"http://localhost:5000"
}
```

说明：

1. 优点：配置简单，请求资源时直接发给前端（8080）即可。
2. 缺点：不能配置多个代理，不能灵活的控制请求是否走代理。
3. 工作方式：若按照上述配置代理，当请求了前端不存在的资源时，那么该请求会转发给服务器 （优先匹配前端资源）

**方法二**

​	编写vue.config.js配置具体代理规则：

```js
module.exports = {
	devServer: {
      proxy: {
      '/api1': {// 匹配所有以 '/api1'开头的请求路径
        target: 'http://localhost:5000',// 代理目标的基础路径
        changeOrigin: true,
        pathRewrite: {'^/api1': ''}
      },
      '/api2': {// 匹配所有以 '/api2'开头的请求路径
        target: 'http://localhost:5001',// 代理目标的基础路径
        changeOrigin: true,
        pathRewrite: {'^/api2': ''}
      }
    }
  }
}
/*
   changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
   changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:8080
   changeOrigin默认值为true
*/
```

说明：

1. 优点：可以配置多个代理，且可以灵活的控制请求是否走代理。
2. 缺点：配置略微繁琐，请求资源时必须加前缀。

代码示例：

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  lintOnSave:false ,/*关闭语法检查*/

  //开启代理服务器 (方式一)
  // devServer: {
  //   proxy: 'http://localhost:5000'
  // }
    //开启代理服务器 (方式二)
    devServer: {
      proxy: {
        '/api': {
          target: 'http://localhost:5000',
          pathRewrite:{'^/api':''},
          ws: true,  //用户支持websocket
          changeOrigin: true   //用于控制请求头中的host值
        },
        '/demo': {
          target: 'http://localhost:5001',
          pathRewrite:{'^/demo':''},
          ws: true,  //用户支持websocket
          changeOrigin: true   //用于控制请求头中的host值
        },
       
      }
    }
})

```



```vue
<template>
	<div >
		<button @click="getStudents">获取学生信息</button>
		<button @click="getCars">获取学生信息</button>
	</div>
</template>

<script>
	import axios from 'axios'

	export default {
		name:'App',
		methods:{
			getStudents(){
				axios.get('http://localhost:8081/api/students').then(
					response=>{
						console.log('请求成功了',response.data)
					},
					error=>{
						console.log('请求失败了',error.message)
					}
				)
			},
			getCars(){
				axios.get('http://localhost:8081/demo/cars').then(
					response=>{
						console.log('请求成功了',response.data)
					},
					error=>{
						console.log('请求失败了',error.message)
					}
				)
			}
		}
	}
</script>


```



# 5、vuex

# 6、vue-router

# 7、element-ui

# 8、vue3