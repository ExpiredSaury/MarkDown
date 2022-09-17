<h1><center><strong>Tkinter</strong></center></h1>

### 主窗口和位置大小

通过`geometry('wxh±x±y')`进行设置，`w为宽度，h为高度，+x表示距离屏幕左边的距离，-x表示距离屏幕右边的距离，+y表示距离屏幕上边的距离，-y表示屏幕下边的距离`

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-BuQvJGlJ-1663334689040)(E:\MarkDown\markdown\Python\img\image-20220916093630428.png)\]](https://img-blog.csdnimg.cn/1da84ecb379149f8a3f110056ac68a06.png)


```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 9:18


from tkinter import *
from tkinter import messagebox

window = Tk()#创建一个顶层窗口对象；
window.title('第一个GUI程序')  # 窗口的标题
window.geometry('500x300+100+200')  # 宽度500像素，高度300像素，距离左边100像素，距离上边200像素
bunt1 = Button(window)
bunt1['text'] = 'click me '

bunt1.pack()


def flower(e):
    messagebox.showinfo("Message", "送你一朵玫瑰花")


bunt1.bind("<Button-1>", flower)
window.mainloop()#事件循环。
```

### GUI编程描述

![](E:\MarkDown\markdown\Python\img\54d8a6eb7eced27d4871d352d1edd346.png)

**Tkinter和Wm**

Tkinter的GUI组件有两个根父类，他们都直接继承了object类：

* Misc：是所有组件的根父类
* Wm:主要提供了一些与窗口管理器通信的功能函数

**Tk**
Misc和Wm派生出子类Tk，它代表应用程序的主窗口，一般用户程序都需要直接或间接使用Tk

**Pack、Place、Grid**

Pack、Place、Grid是布局管理器，布局管理器组件的，大小，位置，通过布局管理器可以将容器中的组件实现合理的排布

**BaseWidget**

BaseWidget是所有组件的父类

**Widget**

Widget是所有组件类的父类，Widget一共有四个父类：Base Widget、Pack、Grid、Place，意味着所有GUI组件同时具备者四个父类的属性和方法

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-9uWNoAKn-1663334689042)()\]](https://img-blog.csdnimg.cn/b850d1d0f8744d979467d7ae21831d4b.png)


### 常用组件汇总



| Tkinter     | 名称       | 简介                                                         |
| ----------- | ---------- | ------------------------------------------------------------ |
| Toplevel    | 顶层       | 容器类，可用于为其他组件提供单独的容器，Toplevel有点类似于窗口 |
| Button      | 按钮       | 代表按钮组件                                                 |
| Canvas      | 画布       | 提供绘图功能，包括直线，矩形，椭圆，多边形，位图             |
| Checkbutton | 复选框     | 可供用户勾选的复选框                                         |
| Entry       | 单行输入框 | 用户可输入内容                                               |
| Label       | 标签       | 用于显示不可编辑的文本或图标                                 |
| LabelFrame  | 容器       | 也是容器组件，类似于Frame，但它支持添加标题                  |
| Menu        | 菜单       | 菜单组件                                                     |
| Menubutton  | 菜单按钮   | 用来包含菜单的按钮（包括下拉式，层叠式）                     |
| OptionMenu  | 菜单按钮   | Menubotton的子类，也代表菜单按钮，可以通过按钮打开一个菜单   |
| Message     | 消息框     | 类似于标签，但可以显示多行文本，后来当Label也能显示          |
| Listbox     | 列表框     | 列出多个选项，供用户先择                                     |
| Frame       | 容器       | 用于装载其他GUI组件                                          |

### GUI应用程序类的经典写法

通过类**Application**组织整个GUI程序，类**Application**继承了**Frame**及通过继承拥有了父类的特性，通过构造函数`__init__()`初始化窗口中的对象，通过`createWidgests()`方法创建窗口中的对象。

**Frame**框架是`tkinter`组件，标识一个矩形的区域，**Frame**一般作为容器使用，可以放置其他组件，从而实现复杂的布局

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 10:15

from tkinter import *
from tkinter import messagebox


class Application(Frame):
    """一个经典的GUI程序类的写法"""

    def __init__(self, master=None):
        super().__init__(master)  # super代表是父类的定义，而不是父类对象
        self.master = master
        self.pack()

        self.createWidget()

    def createWidget(self):
        """创建组件"""
        self.btn01 = Button(self)
        self.btn01['text'] = '点击送花'
        self.btn01.pack()
        self.btn01['command'] = self.flower

        # 创建退出按钮
        self.btnQuit = Button(self, text='退出', command=root.destroy)
        self.btnQuit.pack()

    def flower(self):
        messagebox.showinfo('送花', '送你一朵玫瑰花')


if __name__ == '__main__':
    root = Tk()
    root.geometry('400x200+200+300')
    root.title("一个经典的GUI程序类的测试")
    app = Application(master=root)
    root.mainloop()

```

### 简单组件

#### Label标签

label主要用来显示文本信息，也可显示图像

**常用属性：**

1. width、height:

   用于显示区域大小，如果显示**文本，则以单个英文字符大小为单位**（==一个汉字占两个字符==）如果**显示图像，则以像素为单位**，默认值是根据具体显示的内容动态调整

2. font

   指定字体和字体大小 font=(font_name,size)

3. image:

   显示在Label上的图像，目前tkinter只支持gif格式

4. fg和bg

   fg(foreground)：前景色\  bg(background)背景色

5. justify

   针对多行文字的对齐，可设置justify属性，可选值:left，right,center

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 10:47
from tkinter import *
from tkinter import messagebox


class Application(Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.createWidget()

    def createWidget(self):
        """创建组件"""
        self.label01 = Label(self,
                             text='我学Python',
                             width=10,
                             height=2,
                             bg='black',
                             fg='yellow')
        self.label01.pack()
        self.label02 = Label(
            text='人生苦短',
            width=10,
            height=2,
            bg='blue',
            fg='red',
            font=('楷体', 20)
        )
        self.label02.pack()
        # 显示图像
        global photo  # photo声明成全局变量，如果是局部，本方法执行完毕后，图片对象销毁，窗口显示不出来图像
        photo = PhotoImage(file='img/img.png')
        self.label03 = Label(image=photo)
        self.label03.pack()
        #显示多行文本
        self.label04=Label(text='人生苦短，我学Python',
                           borderwidth=2,
                           relief='solid',
                           justify='right')
        self.label04.pack()

if __name__ == '__main__':
    root = Tk()
    root.title('Label标签测试')
    root.geometry('500x300+200+200')
    app = Application(master=root)
    root.mainloop()
```

**效果展示**

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-omYYasmD-1663334689043)()\]](https://img-blog.csdnimg.cn/a776bdacdacb474391fed6c7719049a7.png)


#### Option选项

1. 创建对象时，使用命名参数(也叫关键字参数)

```python
window=Button(self,fg='red',bg='blue')
```

2. 创建对象后，使用字典索引方式

```python
window['fg']='red'
window['bg']='blue'
```

3. 创建对象后，使用config()方法

```python 
window.config(fg='red',bg='blue')
```

#### Button

Button用来执行用户点击操作，Button可以包含文本，也可包含图像，按钮被单机后会自动调用对应事件绑定方法

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 11:36
from tkinter import *
from tkinter import messagebox


class Application(Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.createWidget()

    def createWidget(self):
        """创建组件"""
        self.butn01 = Button(root, text='登录', command=self.login)
        self.butn01.pack()

        global photo
        photo = PhotoImage(file='img/start.png')
        self.butn02 = Button(root, image=photo, command=self.login)
        self.butn02.pack()
        # self.butn02.config(state='disabled')  # 设置按钮为禁用

        # 点击图片退出
        global  photo2
        photo2=PhotoImage(file='img/end.png')
        self.butnQuit = Button(root,image=photo2,  command=root.destroy)
        self.butnQuit.pack()

    def login(self):
        messagebox.showinfo('自学网', '登录成功，开始学习吧！')


if __name__ == '__main__':
    root = Tk()
    root.title("Button按钮")
    root.geometry('600x400+100+200')
    app = Application(master=root)
    root.mainloop()
```

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-6awvzcSf-1663334689046)()\]](https://img-blog.csdnimg.cn/ecf4b405b57349c7a4fa3526fded4a60.png)


#### Entry 当行文本框

Entry用来接收一行字符串的控件，如果用户输入的文字长度长于Entry控件的宽度，文字会自动向后滚动，入宫输入多行文本，需要使用Text控件

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 13:34


from tkinter import *
from tkinter import messagebox


class Application(Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.createWidget()

    def createWidget(self):

        """创建登录界面的组件"""
        self.label01 = Label(text='用户名')
        self.label01.pack()

        # StringVar变量绑定到指定的组件
        # StringVar变量的值发生变换，组件内容也发生变化
        # 组件的内容发生变化，StringVar变量的值也发生变化
        v1 = StringVar()
        self.entry01 = Entry(textvariable=v1)
        self.entry01.pack()
        v1.set('admin')
        # print(v1.get());print(self.entry01.get())

        # 创建密码框
        self.label02 = Label(text='密码')
        self.label02.pack()
        v2 = StringVar()
        self.entry02 = Entry(textvariable=v2, show='*')
        self.entry02.pack()

        Button(self, text='登录', command=self.login).pack()

    def login(self):
        username = self.entry01.get()
        password = self.entry02.get()
       
        print('用户名' + username)
        print('密码' + password)
         """模拟数据库比对数据"""
        if username == 'zhao' and password == '123456':
            messagebox.showinfo("自学网欢迎您", '开始自学吧')
        else:
            messagebox.showinfo("自学网欢迎您", '登陆失败！用户名或密码错误')


if __name__ == '__main__':
    root = Tk()
    root.title('Entry单行文本')
    root.geometry('500x300+100+200')
    app = Application(master=root)
    root.mainloop()
```

#### Text多行文本框

Text的主要用户显示多行文本，还可以显示网页链接，图片，HTML页面，甚至CSS样式表，添加组件等，因此，也常被当做简单的文本处理器、文本编辑器或者网页浏览器来使用，比如：IDEL就是Text组件构成的

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 14:02


from tkinter import *
from tkinter import messagebox
import webbrowser

class Application(Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.createWidget()

    def createWidget(self):
        """"""
        self.w1 = Text(width=40, height=12, bg='gray')
        self.w1.pack()

        self.w1.insert(1.0, '一二三四五，\n上山岛老鼠')
        self.w1.insert(2.3, '人生苦短，我学Python')

        Button(text='重复插入文本', command=self.insertText).pack(side='left')
        Button(text='返回文本', command=self.returnText).pack(side='left')
        Button(text='添加图片', command=self.addImage).pack(side='left')
        Button(text='添加组件', command=self.addWidget).pack(side='left')
        Button(text='通过tag精确控制文本', command=self.testTag).pack(side='left')
        Button(text='退出', command=window.destroy).pack(side='left')

    def insertText(self):
        # INSERT索引表示在光标处插入
        self.w1.insert(INSERT, 'zhao')
        self.w1.insert(1.8, '糖衣炮弹')
        # END索引号表示在最后插入
        self.w1.insert(END, '[xxxx.com]')

    def returnText(self):
        # Indexes（索引）是用来指向Text组件中文本的位置，Text的组件索引也是对应事件字符之间的位置
        # 核心：行号以1开始，列号以0开始
        print(self.w1.get(1.2, 1.6))
        print('所有文本内容\n' + self.w1.get(1.0, END))

    def addImage(self):
        self.photo = PhotoImage(file='img/dog.png')
        self.w1.image_create(END, image=self.photo)

    def addWidget(self):
        b1=Button(self.w1,text='Python')
        #在text创建组件的命令
        self.w1.window_create(INSERT,window=b1)

    def testTag(self):
        self.w1.delete(1.0,END)
        self.w1.insert(INSERT,'girl girl girl girl girl day boy\n insert common\n shabi dsakfh fsdlkhaf\n hakfha\n'
                              '百度')
        self.w1.tag_add('girl',1.0,1.9)
        self.w1.tag_config('good',background='yellow',foreground='red')

        self.w1.tag_add('baidu',5.0,5.2)
        self.w1.tag_config('baidu',underline=True)
        self.w1.tag_bind('baidu','<Button-1>',self.webshow)
    def webshow(self,event):
        webbrowser.open('http://www.baidu.com')



if __name__ == '__main__':
    window = Tk()
    window.title('多行文本')
    window.geometry('500x400+100+200')
    app = Application(master=window)
    window.mainloop()
```

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-9ElW8ExB-1663334689048)()\]](https://img-blog.csdnimg.cn/8b233b3a8450486fadf429a7300a74fb.png)


#### Radiobutton单选按钮

Radiobutton控件用于选择通过一组单选按钮中的一个

Radiobutton可以显示文本，也可以显示图像

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 14:46


from tkinter import *
from tkinter import messagebox


class Application(Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.createWidget()

    def createWidget(self):
        self.v = StringVar()
        self.v.set('F')  # 默认设置为女性

        self.r1 = Radiobutton(text='男性', value='M', variable=self.v)
        self.r2 = Radiobutton(text='女性', value='F', variable=self.v)

        self.r1.pack(side='left')
        self.r2.pack(side='left')

        Button(text='确定', command=self.confirm).pack(side='left')

    def confirm(self):
        messagebox.showinfo('测试', '选择的性别为' + self.v.get())


if __name__ == '__main__':
    root = Tk()
    root.geometry('200x100+100+100')
    app = Application(master=root)
    root.mainloop()
```

#### Checkbutton 复选按钮

Checkbutton控件用于选择多个按钮的情况，

Checkbutton可以显示文本也可以显示图像

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 14:46


from tkinter import *
from tkinter import messagebox


class Application(Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.createWidget()

    def createWidget(self):
        self.codeHobby = IntVar()
        self.videoHobby = IntVar()

        self.c1 = Checkbutton(text='敲代码', onvalue=1, offvalue=0,
                              variable=self.codeHobby)  # onvalue,offvalue当选中的时候返回是1，不选是0
        self.c2 = Checkbutton(text='看小视频', onvalue=1, offvalue=0, variable=self.videoHobby)

        self.c1.pack(side='left')
        self.c2.pack(side='left')

        Button(text='确定', command=self.confirm).pack(side='left')

    def confirm(self):
        if self.videoHobby.get() == 1:
            messagebox.showinfo('测试', '我爱看电影,你呢')
        if self.videoHobby.get() == 1:
            messagebox.showinfo('测试', '人生苦短，我学Python')


if __name__ == '__main__':
    root = Tk()
    root.geometry('300x100+100+100')
    app = Application(master=root)
    root.mainloop()
```

#### canvas画布

是一个矩形区域，可以防止任何图形，图像，组件等

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 15:08
import random
from tkinter import *
from tkinter import messagebox


class Application(Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.createWidget()

    def createWidget(self):
        # 画一条直线
        self.canvas = Canvas(self, width=300, height=200, bg='green')
        self.canvas.pack()
        # 画一个矩形
        line = self.canvas.create_line(10, 10, 30, 20, 40, 50)
        # 画一个矩形
        rect = self.canvas.create_rectangle((50, 50, 100, 100))
        #画一个椭圆,坐标两双，为椭圆的边界矩形左上角和底部右下角
        oval=self.canvas.create_oval(50,50,100,100)

        global  photo
        photo=PhotoImage(file='img/img.png')
        self.canvas.create_image(150,170,image=photo)

        Button(self,text='画十个矩形',command=self.draw).pack(side='left')

    def draw(self):
        for i in range(0,10):
            num1=int(self.canvas['width'])/2
            num2=int(self.canvas['height'])/2
            x1=random.randrange(num1)
            y1=random.randrange(num2)
            x2=x1+random.randrange(num1)
            y2=y1+random.randrange(num2)
            self.canvas.create_rectangle(x1,y1,x2,y2)


if __name__ == '__main__':
    root = Tk()
    root.geometry('500x300+200+300')
    app = Application(root)
    root.mainloop()
```

### 布局管理器

#### **grid布局管理器**

grid表格布局，采用表格结构组织组件，子组件的位置由行和列的单元格来确定， 并且可以跨行和跨列，从而实现复杂的布局

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-g5rY4SML-1663334689050)()\]](https://img-blog.csdnimg.cn/ea23c40b97c14582b56d3a696f58b80c.png)


```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 15:44
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 15:08
import random
from tkinter import *
from tkinter import messagebox


class Application(Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.createWidget()

    def createWidget(self):
        self.label01 = Label(self, text='用户名')
        self.label01.grid(row=0, column=0)
        self.entry01 = Entry(self)
        self.entry01.grid(row=0, column=1)
        Label(self, text='用户名为手机号').grid(row=0, column=2)

        Label(self, text='密码').grid(row=1, column=0)
        Entry(self, show='*').grid(row=1, column=1)

        Button(self, text='登录').grid(row=2, column=1, sticky=EW)
        Button(self, text='取消').grid(row=2, column=2, sticky=E)


if __name__ == '__main__':
    root = Tk()
    root.geometry('500x300+200+300')
    app = Application(root)
    root.mainloop()
```

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-XWbIMjxk-1663334689053)()\]](https://img-blog.csdnimg.cn/b6072fefaf8b40b2a3e378107dd9a899.png)


Grid实现计算器页面

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 16:02
from tkinter import *
from tkinter import messagebox


class Application(Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.createWidget()

    def createWidget(self):
        """通过Grid 实现计算器界面"""
        btnTex = (('MC', 'M+', 'M-', 'MR'),
                  ('C', '±', '/', 'x'),
                  ('7', '8', '9', '-'),
                  ('4', '5', '6', '+'),
                  ('1', '2', '3', '='),
                  ('0', '.')
                  )
        Entry(self).grid(row=0, column=0, columnspan=4, pady=10)  # 跨4列
        for rindex, r in enumerate(btnTex):
            for cindex, c in enumerate(r):
                if c =='=':
                    Button(self,text=c,width=2).grid(row=rindex+1,column=cindex,rowspan=2,sticky=NSEW)
                elif c=='0':
                    Button(self,text=c,width=2).grid(row=rindex+1,column=cindex,columnspan=2,sticky=NSEW)
                elif c=='.':
                    Button(self,text=c,width=2).grid(row=rindex+1,column=cindex+1,sticky=NSEW)

                else:
                    Button(self, text=c, width=2).grid(row=rindex + 1, column=cindex,sticky=EW)



if __name__ == '__main__':
    root = Tk()
    root.geometry('250x250+200+300')
    app = Application(master=root)
    root.mainloop()
```

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-3PdgEeqN-1663334689056)()\]](https://img-blog.csdnimg.cn/3353fbe0d5eb4a8b8c65487ad5b3f027.png)


#### pack布局管理器

pack按照组件的创建顺序将子组件添加到父组件，按照垂直或者水平的方向自然排布，如果不指定任何选项，默认在父组件中自顶向下垂直添加组件

pack是代码量最少，最简单的一种，可以用于快速生成界面

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-unZmHSUH-1663334689057)()\]](https://img-blog.csdnimg.cn/f4fd65ab5b9344ff9c39ca249e74c318.png)


```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 17:00


from tkinter import  *
root=Tk()
root.geometry("700x220")
#Frame是一个矩形区域，就是用来防止其他子组件
f1=Frame(root)
f1.pack()
f2=Frame(root)
f2.pack()

btnText=('流行风','中国风','日本风','重金属','轻音乐')
for txt in btnText:
    Button(f1,text=txt).pack(side='left',padx='10')
for i in range(1,20):
    Label(f2,width=5,height=10,borderwidth=1,relief='solid',
          bg='black' if i%2==0  else 'white').pack(side='left',padx=2)


root.mainloop()
```

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-FeFm0Fbv-1663334689058)()\]](https://img-blog.csdnimg.cn/cab62827a27a431bb7b43614a83f52d9.png)


#### place布局管理器

可以通过坐标精确控制组件的位置，适用于一些布局更加灵活的场景

![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-2ydYpQm2-1663334689059)()\]](https://img-blog.csdnimg.cn/c09537eae6c04acfb355ed79e9116513.png)


```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 17:29
from tkinter import *

root = Tk()
root.geometry("500x300")
root.title(
    '布局管理器Place'
)
root['bg'] = 'white'
f1 = Frame(root, width=200, height=200, bg='green')
f1.place(x=30, y=30)

Button(root, text='Python').place(relx=0.2, x=100, y=20, relwidth=0.2, relheight=0.5)
Button(f1, text='面向对象').place(relx=0.6, rely=0.7)
Button(f1, text='zhao').place(relx=0.5, rely=0.2)
root.mainloop()
```

通过palce布局管理器对扑克牌进行位置控制

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 17:37
from tkinter import *
from tkinter import messagebox


class Application(Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.pack()
        self.createWidget()

    def createWidget(self):
        """通过place布局管理器实现扑克牌位置控制"""
        self.photo = PhotoImage(file='img/puke/puke1.png')
        self.puke1 = Label(root, image=self.photo)
        self.puke1.place(x=10, y=50)

        self.photos = [PhotoImage(file='img/puke/puke' + str(i + 1) + '.png') for i in range(3)]
        self.pukes = [Label(self.master, image=self.photos[i]) for i in range(3)]

        for i in range(3):
            self.pukes[i].place(x=10 + i * 60, y=50)

        # 对所有的Label增加事件处理
        self.pukes[0].bind_class("Label", "<Button-1>", self.chupai)

    def chupai(self, event):
        # print(event.widget.winfo_geometry())
        # print(event.widget.winfo_x())  # 获取主键的x坐标
        # print(event.widget.winfo_y())  # 获得y坐标
        if event.widget.winfo_y() == 50:
            event.widget.place(y=30)
        else:
            event.widget.place(y=60)


if __name__ == '__main__':
    root = Tk()
    root.geometry("700x600+200+300")
    app = Application(root)
    root.mainloop()
```

### 事件处理

一个GUI应用整个生命周期都处在一个消息循环(eventloop)中，它等待事件的发生，并作出相应的处理

Tkinter提供了用以处理相关事件的机制，处理函数可以被绑定给各个控件的各种事件

```python
widget.bind(event,handler)
```

如果相关事件发生，hadler函数会被触发，事件对象event会传递给handler函数



#### 鼠标和键盘事件

![\外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-0NtGtGrz-1663334689060)()\]](https://img-blog.csdnimg.cn/0ab772064716406ea801d0510648db3e.png)


#### event对象常用属性

![\外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-h9LHocGt-1663334689061)(](https://img-blog.csdnimg.cn/a3a07244c83c40ef89706797fbc51792.png)


```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 20:00

from tkinter import *

root = Tk()
root.geometry("530x300")
c1 = Canvas(root, width=200, height=200, bg='green')
c1.pack()


def mouseTest(event):
    print('鼠标左键单机位置(相对于父容器):{0},{1}'.format(event.x, event.y))
    print('鼠标左键单机位置(相对于屏幕):{0}{1}'.format(event.x_root, event.y_root))
    print('事件绑定的组件:{0}'.format(event.widget))


def testDrag(event):
    c1.create_oval(event.x, event.y, event.x + 1, event.y + 1)


def keyboardTest(event):
    print('键的keycode:{0},键的char:{1},键的keysym:{2}'
          .format(event.keycode, event.char, event.keysym))


def press_a_test(event):
    print('press_a')


def release_a_test(event):
    print('release_a')


c1.bind('<Button-1>', mouseTest)
c1.bind('<B1-Motion>', testDrag)

root.bind('<KeyPress>', keyboardTest)
root.bind("<KeyPress-a>", press_a_test)
root.bind("<KeyRelease-a>", release_a_test)
root.mainloop()
```

#### 多种事件绑定方式

* 组件对象的绑定

  * 通过command属性绑定，（适合简单不需要获取event对象）

  * ```python
    Button(root,text='登录',command=login0)
    ```

  * 通过bind()方法绑定（适合需要获取event 对象

  * ```python
    c1=Canvas();c1.bind("<Button-1>",drawLine)
    ```

* 组件类的绑定

  * 调用对象的bind_class函数，将改组件类所有的组件绑定事件：

  * ```python
    w.bind_class("Widget","event",eventhanler)
    ```

    

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 20:32


from tkinter import *
from tkinter import messagebox

root = Tk()
root.geometry("300x30")


def mouseTest1(event):
    print('bind()方式绑定,可以获取event对象')
    print(event.widget)


def mouseTest2(a, b):
    print('a={0},b={1}'.format(a, b))
    print('command方式绑定，不能直接获取event对象')


def mouseTest3(event):
    print('右键单机事件，绑定给所有按钮了')
    print(event.widget)


b1 = Button(root, text='测试bind()绑定')
b1.pack(side='left')
# bind直接绑定事件
b1.bind("<Button-1>", mouseTest1)

# command属性直接绑定事件
b2 = Button(root, text='测试command2', command=lambda: mouseTest2("zhao", "lisi"))
b2.pack(side='left')

# 给所有Button按钮都绑定右键单机事件<Button-2>
b1.bind_class('Button', "<Button-2>", mouseTest3)

root.mainloop()
```

![\外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-X0y5T8eg-1663334689062)()\]](https://img-blog.csdnimg.cn/3fa4fe664d9c4ab1a450794ff3f2b5fd.png)


### 其他组件

#### OptionMenu选择项

用来多选一，选中的项在顶部显示。

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 20:50


from tkinter import *

root = Tk()
root.geometry("200x100")
v = StringVar()
v.set('黑马程序员')
om = OptionMenu(root, v, '千锋教育', '字节跳动', '华为')
om['width'] = 10
om.pack()


def test1():
    print('最想选择哪一个:', v.get())


Button(root, text='确定', command=test1).pack()
root.mainloop()
```

![\外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-cImgpNvN-1663334689064)(\]](https://img-blog.csdnimg.cn/751c7d9d59bf4edba9a69564c7090c7d.png)


#### Scale移动滑块

用于在指定的数值区间，通过滑块的移动来选择值

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 20:58


from tkinter import *

root = Tk()
root.geometry("400x150")


def test1(value):
    print('滑块的值：',value)
    newFont = ("宋体", value)
    a.config(font=newFont)


s1 = Scale(root, from_=10, to=50, length=200, tickinterval=5, orient=HORIZONTAL, command=test1)
s1.pack()

a=Label(root,text='Python',width=10,height=1,bg='black',fg='white')
a.pack()
root.mainloop()
```

![\外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-HdgK18Zi-1663334689065)()\]](https://img-blog.csdnimg.cn/2b8eeb3296f64d8d95faf94fe93d16cd.png)


#### 颜色选择框

颜色选择框帮助我们设置前景色，背景色，画笔颜色，字体颜色等

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 21:06


from tkinter import  *
from tkinter.colorchooser import  *

root=Tk();root.geometry("400x150")
def text():
    s1=askcolor(color='red',title='选择背景色')
    print(s1)
    #s1的值是：（(0.0,0.0,255,99609375),"#0000ff"）
    root.config(bg=s1[1])

Button(root,text='选择背景色',command=text).pack()

root.mainloop()
```

#### 文件选择框

文件对话框帮助我们实现可视化的操作目录、操作文件。最后，将文件、目录的信息传入到程序中。文件

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/16 21:11

from tkinter import  *
from tkinter.filedialog import  *

root=Tk()
root.geometry("400x100")
def test():
    f=askopenfile(title='上传文件',
                  initialdir='f:',
                  filetypes=[("视频文件",'.mp4')])
    show['text']=f
Button(root,text='选择编辑的视频文件',command=test).pack()
show=Label(root,width=40,height=3,bg='green')
show.pack()
root.mainloop()
```

![在这里插入图片描述](E:\MarkDown\markdown\Python\img\210350f74045499599d613dc26bc36d2.png)
**命名参数option的常见值**
![在这里插入图片描述](E:\MarkDown\markdown\Python\img\c04e89321fd342b7a80d429a32e61813.png)

#### 简单对话框

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/17 8:26

from tkinter import *
from tkinter.simpledialog import *

root = Tk()
root.geometry("400x100")


def test():
    a = askinteger(title='输入年龄', prompt='请输入年龄', initialvalue=18, minvalue=1, maxvalue=150)
    show['text'] = a


Button(root, text='多大了你？', command=test).pack()
show = Label(root, width=40, height=3, bg='green')
show.pack()

root.mainloop()
```

#### 上下文菜单

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/17 8:32


from tkinter import *
from tkinter.filedialog import *


class Application(Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.textpad = None
        self.pack()
        self.createWidget()

    def createWidget(self):
        """创建主菜单栏"""
        menubar = Menu(root)

        # 创建子菜单
        menuFile = Menu(menubar)
        menuEdit = Menu(menubar)
        menuHelp = Menu(menubar)

        # 将子菜单加入到主菜单栏
        menubar.add_cascade(label='文件(F)', menu=menuFile)
        menubar.add_cascade(label='编辑(E)', menu=menuEdit)
        menubar.add_cascade(label='帮助(H)', menu=menuHelp)

        # 添加菜单项
        menuFile.add_command(label='新建', accelerator="ctrl+n", command=self.test)
        menuFile.add_command(label='打开', accelerator="ctrl+o", command=self.test)
        menuFile.add_command(label='保存', accelerator="ctrl+s", command=self.test)
        menuFile.add_separator()  # 添加分割线
        menuFile.add_command(label='退出', accelerator='ctrl+q', command=self.test)

        # 将主菜单加到根窗口
        root['menu'] = menubar

        # 文本编辑区
        self.textpad = Text(root, width=50, height=30)
        self.textpad.pack()

        # 创建上下菜单
        self.contextMenu = Menu(root)
        self.contextMenu.add_command(label='背景颜色', command=self.test)

        # 为右键绑定事件
        root.bind("<Button-3>", self.createContextMenu)

    def test(self):
        pass

    def createContextMenu(self, event):
        # 菜单在鼠标右键单机的座标处显示
        self.contextMenu.post(event.x_root, event.y_root)


if __name__ == '__main__':
    root = Tk()
    root.geometry("400x200")
    root.title("简易记事本")
    app = Application(master=root)
    root.mainloop()
```

简易记事本

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/17 8:32


from tkinter import *
from tkinter.colorchooser import askcolor
from tkinter.filedialog import *


class Application(Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.filename = None
        self.master = master
        self.textpad = None
        self.pack()
        self.createWidget()

    def createWidget(self):
        """创建主菜单栏"""
        menubar = Menu(root)

        # 创建子菜单
        menuFile = Menu(menubar)
        menuEdit = Menu(menubar)
        menuHelp = Menu(menubar)

        # 将子菜单加入到主菜单栏
        menubar.add_cascade(label='文件(F)', menu=menuFile)
        menubar.add_cascade(label='编辑(E)', menu=menuEdit)
        menubar.add_cascade(label='帮助(H)', menu=menuHelp)

        # 添加菜单项
        menuFile.add_command(label='新建', accelerator="ctrl+n", command=self.newfile)
        menuFile.add_command(label='打开', accelerator="ctrl+o", command=self.openfile)
        menuFile.add_command(label='保存', accelerator="ctrl+s", command=self.savefile)
        menuFile.add_separator()  # 添加分割线
        menuFile.add_command(label='退出', accelerator='ctrl+q', command=self.exit)

        # 将主菜单加到根窗口
        root['menu'] = menubar

        # 增加快捷键的处理
        root.bind('<Control-n>', lambda event: self.newfile())
        root.bind('<Control-s>', lambda event: self.savefile())
        root.bind('<Control-q>', lambda event: self.exit())
        root.bind('<Control-o>', lambda event: self.openfile())

        # 文本编辑区
        self.textpad = Text(root, width=50, height=30)
        self.textpad.pack()

        # 创建上下菜单
        self.contextMenu = Menu(root)
        self.contextMenu.add_command(label='背景颜色', command=self.openAskColor)

        # 为右键绑定事件
        root.bind("<Button-3>", self.createContextMenu)

    def newfile(self):
        self.filename = asksaveasfilename(title='另存为', initialfile='未命名.txt',
                                          filetypes=[("文本 文档", '.txt')],
                                          defaultextension='.txt')
        self.savefile()

    def openfile(self):
        self.textpad.delete("1.0", "end")
        with askopenfile(title='打开文本文件') as f:
            self.textpad.insert(INSERT, f.read())
            self.filename = f.name

    def savefile(self):
        with open(self.filename, 'w', encoding='utf-8') as f:
            c = self.textpad.get(1.0, END)
            f.write(c)

    def exit(self):
        root.quit()

    def openAskColor(self):
        s1 = askcolor(color='red', title='选择背景色')
        self.textpad.config(bg=s1[1])

    def createContextMenu(self, event):
        # 菜单在鼠标右键单机的座标处显示
        self.contextMenu.post(event.x_root, event.y_root)


if __name__ == '__main__':
    root = Tk()
    root.geometry("400x200")
    root.title("简易记事本")
    app = Application(master=root)
    root.mainloop()
```

#### 将python程序打包成exe可执行文件

可以使用pyinstaller模块实现将python项目打包成exe执行文件

```python
"""
先安装模块
1.pip install pyinstaller
"""
```

```python
"""
2.在Pycharm的Terminal终端输入命令：pyinstaller -F xxx.py
"""
```

```python
相关参数
--icon=图标路径（pyinstaller -F --icon=my.icon xxx.py）
-F 打包成一个exe文件
-w 使用窗口，无控制台
-c 使用控制台 ，无窗口
-D 创建一个目录，里面包含exe以及其他一些依赖性文件
```

```python
"""
3.在项目目录下会生成一个dist目录，里面有一个exe文件
"""
```

#### 记事本优化

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/17 8:32


from tkinter import *
from tkinter.colorchooser import askcolor
from tkinter.filedialog import *


class Application(Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.filename = None
        self.master = master
        self.textpad = None
        self.pack()
        self.createWidget()

    def createWidget(self):
        """创建主菜单栏"""
        menubar = Menu(root)

        # 创建子菜单
        menuFile = Menu(menubar)
        menuEdit = Menu(menubar)
        menuHelp = Menu(menubar)

        # 将子菜单加入到主菜单栏
        menubar.add_cascade(label='文件(F)', menu=menuFile)
        menubar.add_cascade(label='编辑(E)', menu=menuEdit)
        menubar.add_cascade(label='帮助(H)', menu=menuHelp)

        # 添加菜单项
        menuFile.add_command(label='新建', accelerator="ctrl+n", command=self.newfile)
        menuFile.add_command(label='打开', accelerator="ctrl+o", command=self.openfile)
        menuFile.add_command(label='保存', accelerator="ctrl+s", command=self.savefile)
        menuFile.add_separator()  # 添加分割线
        menuFile.add_command(label='退出', accelerator='ctrl+q', command=self.exit)

        # 将主菜单加到根窗口
        root['menu'] = menubar

        # 增加快捷键的处理
        root.bind('<Control-n>', lambda event: self.newfile())
        root.bind('<Control-s>', lambda event: self.savefile())
        root.bind('<Control-q>', lambda event: self.exit())
        root.bind('<Control-o>', lambda event: self.openfile())

        # 文本编辑区
        self.textpad = Text(root, width=50, height=30)
        self.textpad.pack()

        # 创建上下菜单
        self.contextMenu = Menu(root)
        self.contextMenu.add_command(label='背景颜色', command=self.openAskColor)

        # 为右键绑定事件
        root.bind("<Button-3>", self.createContextMenu)

    def newfile(self):
        self.filename = asksaveasfilename(title='另存为', initialfile='未命名.txt',
                                          filetypes=[("文本 文档", '.txt')],
                                          defaultextension='.txt')
        self.savefile()

    def openfile(self):
        self.textpad.delete("1.0", "end")
        with askopenfile(title='打开文本文件') as f:
            self.textpad.insert(INSERT, f.read())
            self.filename = f.name

    def savefile(self):
        with open(self.filename, 'w', encoding='utf-8') as f:
            c = self.textpad.get(1.0, END)
            f.write(c)

    def exit(self):
        root.quit()

    def openAskColor(self):
        s1 = askcolor(color='red', title='选择背景色')
        self.textpad.config(bg=s1[1])

    def createContextMenu(self, event):
        # 菜单在鼠标右键单机的座标处显示
        self.contextMenu.post(event.x_root, event.y_root)


if __name__ == '__main__':
    root = Tk()
    root.geometry("400x200")
    root.title("简易记事本")
    app = Application(master=root)
    root.mainloop()

```

#### 画图软件开发

```python
"""
包含功能如下：
1.画笔
2.矩形/椭圆绘制
3.清屏
4.橡皮擦
5.直线/带箭头的执行
6.修改画笔的颜色、背景颜色
"""
```

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/17 8:32


from tkinter import *

from tkinter import messagebox

# 窗口的宽度和高度
from tkinter.colorchooser import askcolor

win_width = 900
win_height = 450


class Application(Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.bgcolor = '#000'
        self.x = 0
        self.y = 0
        self.lastDraw = 0  # 表示最后绘制的图形的id
        self.fgcolor = 'red'
        self.startDraw = False
        self.pack()
        self.createWidget()

    def createWidget(self):
        """创建绘图区"""
        self.drawpad = Canvas(root, width=win_width, height=win_height * 0.9, bg=self.bgcolor)

        self.drawpad.pack()

        # 创建按钮
        btn_start = Button(root, text='开始', name='start')
        btn_start.pack(side='left', padx='10')
        btn_pen = Button(root, text='画笔', name='pen')
        btn_pen.pack(side='left', padx='10')
        btn_rect = Button(root, text='矩形', name='rect')
        btn_rect.pack(side='left', padx='10')
        btn_clear = Button(root, text='清屏', name='clear')
        btn_clear.pack(side='left', padx='10')
        btn_eraor = Button(root, text='橡皮擦', name='erasor')
        btn_eraor.pack(side='left', padx='10')
        btn_line = Button(root, text='直线', name='line')
        btn_line.pack(side='left', padx='10')
        btn_lineArrow = Button(root, text='箭头直线', name='lineArrow')
        btn_lineArrow.pack(side='left', padx='10')
        btn_color = Button(root, text='箭头直线', name='color')
        btn_color.pack(side='left', padx='10')

        # 事件处理
        btn_pen.bind_class("Button", "<1>", self.eventManger)
        self.drawpad.bind("<ButtonRelease-1>", self.stopDraw)

        #增加颜色切换的快捷键
        root.bind('<KeyPress-r>',self.kuaijiejian)
        root.bind('<KeyPress-g>',self.kuaijiejian)
        root.bind('<KeyPress-y>',self.kuaijiejian)

    def eventManger(self, event):
        name = event.widget.winfo_name()
        print(name)
        if name == 'line':
            self.drawpad.bind("<B1-Motion>", self.myline)
        elif name == 'lineArrow':
            self.drawpad.bind("<B1-Motion>", self.lineArrow)
        elif name == 'rect':
            self.drawpad.bind("<B1-Motion>", self.myRect)
        elif name == 'pen':
            self.drawpad.bind("<B1-Motion>", self.mypen)
        elif name == 'erasor':
            self.drawpad.bind("<B1-Motion>", self.Erasor)
        elif name == 'clear':
            self.drawpad.delete("all")
        elif name == 'color':
            c = askcolor(color=self.fgcolor, title='选择画笔颜色')
            self.fgcolor = c[1]

    def stopDraw(self, event):
        self.startDraw = False
        self.lastDraw = 0

    def Draw(self, event):
        self.drawpad.delete(self.lastDraw)
        if not self.startDraw:
            self.startDraw = True

            self.x = event.x
            self.y = event.y

    def myline(self, event):
        self.Draw(event)

        self.lastDraw = self.drawpad.create_line(self.x, self.y, event.x, event.y, fill=self.fgcolor)

    def lineArrow(self, event):
        self.Draw(event)
        self.lastDraw = self.drawpad.create_line(self.x, self.y, event.x, event.y, arrow=LAST, fill=self.fgcolor)

    def myRect(self, event):
        self.Draw(event)
        self.lastDraw = self.drawpad.create_rectangle(self.x, self.y, event.x, event.y, outline=self.fgcolor)

    def mypen(self, event):
        self.Draw(event)
        self.drawpad.create_line(self.x, self.y, event.x, event.y, fill=self.fgcolor)
        self.x = event.x
        self.y = event.y

    def Erasor(self, event):
        self.Draw(event)
        self.drawpad.create_rectangle(event.x - 4, event.y - 4, event.x + 4, event.y + 4, fill=self.bgcolor)
        self.x = event.x
        self.y = event.y
    def kuaijiejian(self,event):
        if event.char =='r':
            self.fgcolor='#ff0000'
        elif event.char=='g':
            self.fgcolor='#00ff00'
        elif event.char=='y':
            self.fgcolor='#ffff00'


if __name__ == '__main__':
    root = Tk()
    root.geometry(str(win_width) + 'x' + str(win_height) + "+200+300")
    root.title("程序员的画图软件开发")
    app = Application(master=root)
    root.mainloop()
```



