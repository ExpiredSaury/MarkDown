* `pygame.init()`导入并初始化所欲的`pygame`模块，使用其他模块之前，必须先导入Init()

* `pygame.quit()`卸载所有`pygame`模块，再游戏结束之前调用
* `pygame`提供了一个类`pygame.Rect`用于描述**矩形区域**

```python
Rect(x,y,width,height)
```

需求：

1. 定义`	hero_tect	`矩形描述英雄的位置和大小
2. 输出英雄的坐标原点(x,y)
3. 输出英雄的尺寸(宽度和高度)

```python
import pygame
hero_rect=pygame.Rect(100,150,120,125)
print(hero_rect.x)
print(hero_rect.y)
print(hero_rect.width)
print(hero_rect.height)
```



**创建游戏主窗口**

| 方法                        | 说明               |
| :-------------------------- | ------------------ |
| `pygame.display.set_mode()` | 初始化游戏显示窗口 |
| `pygame.display.update()`   | 刷新屏幕内容显示， |

**图像绘制**

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/11 11:59
import pygame

pygame.init()

screen = pygame.display.set_mode((480, 700))
"""绘制图像"""
# 1.加载图像数据
image = pygame.image.load('../images/background.png')
# 2.blit绘制图像
screen.blit(image, (0, 0))  # 屏幕的左上角
# 3.更新屏幕显示
pygame.display.update()
while True:
    pass
pygame.quit()
```

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/11 11:59
import pygame

pygame.init()

screen = pygame.display.set_mode((480, 700))
"""绘制图像"""
# 1.加载图像数据
image = pygame.image.load('../images/background.png')
# 2.blit绘制图像
screen.blit(image, (0, 0))  # 屏幕的左上角
# 3.更新屏幕显示
pygame.display.update()
while True:
    pass
pygame.quit()
```

**update()方法**

* 可以在`screen`对象完成所有`blit`方法之后，同意调用一次`display.update`方法，

* 使用`display.set_mode()`创建的`screen`对象是一个内存中的屏幕数据对象
  * 可以理解成是**油画的画布**

* `screen.blit`方法可以在画布上绘制很多**图像**

* `pygame.display.update()`会将画布的最终结果绘制在屏幕上，**可以提高屏幕绘制效率，增加游戏的流畅度**

**游戏循环**

* 跟电影原理类似，游戏中的动画效果，本质上是**快速**的在屏幕上**绘制图像**

* 电影是将多张静止 的电影胶片连续、快速的播放，产生的连贯的视觉效果
* 一般在电脑上**每秒绘制60次**，就能达到**连续高品质**的动画效果
  * 每次绘制的结果被称为 **帧Frame**

![image-20220911123058594](C:\Users\31812\AppData\Roaming\Typora\typora-user-images\image-20220911123058594.png)

**游戏循环的作用：**

1. 保证游戏**不会直接退出**
2. **变化图像位置-**-动画效果
   1. 每个`1/60秒`移动一下所有图像的位置
   2. 调用`pygame.display.update()`更新屏幕显示
3. **检测用户交互**---按键，鼠标等等，，

**游戏时钟**

* 提供了一个类`pygame.time.Clock`可以设置屏幕的绘制速度---**刷新帧率**
* 时钟对象需要两部
  * 1)在游戏初始化创建一个时钟对象
  * 2）在游戏循环中让时钟对象调用`tick(帧率)`方法
* `tick`方法会根据上次被调用的时间，自动设置游戏循环中的延时

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/11 13:09

import pygame

pygame.init()
screen = pygame.display.set_mode((480, 700))
bg = pygame.image.load('../images/background.png')
screen.blit(bg, (0, 0))
hero = pygame.image.load('../images/me1.png')
screen.blit(hero, (150, 300))
pygame.display.update()

# 创建游戏时钟对象
clock = pygame.time.Clock()

# 游戏循环

while True:
    # 可以指定循环体内部的代码循环的频率
    clock.tick(60)  # 每秒钟执行60次

pygame.quit()
```

**英雄动画简单实现**

需求：

1. 在游戏初始化定义一个pygame.Rect的变量记录英雄的初始位置
2. 在游戏循环中每次让英雄的`y - 1 `-------向上移动
3. `y <= 0`将英雄移动到屏幕的底部

> * 每一次调用update方法之前，需要把**所有游戏图像都冲洗绘制一遍**
> * 而且应该**最先**绘制**背景图像**

```python
# -*- coding: UTF-8 -*-  @Date ：2022/9/11 13:17


import pygame

pygame.init()
screen = pygame.display.set_mode((480, 700))
bg = pygame.image.load('../images/background.png')
screen.blit(bg, (0, 0))
hero = pygame.image.load('../images/me1.png')
screen.blit(hero, (150, 300))
pygame.display.update()

# 创建时钟对象
clock = pygame.time.Clock()
# 1.定义rect记录飞机的起始位置
hero_rect = pygame.Rect(150, 300, 102, 126)
# 游戏循环
while True:
    # 可以指定循环体内部的代码循环的频率
    clock.tick(60)  # 每秒钟执行60次
    # 2.修改飞机位置
    hero_rect.y -= 1
     # 判断飞机的位置
    if hero_rect.y <= 0:
        hero_rect.y = 700
    # 3调用bulit方法绘制图像
    screen.blit(bg,(0,0))
    screen.blit(hero, hero_rect)

    # 4调用update更新显示
    pygame.display.update()
pygame.quit()
```

**游戏循环中监听事件**

**事件event**

* 就是游戏启动后，用户针对游戏所作的操作
* 例如：点击关闭按钮，点击鼠标，按下键盘

**监听**

* 在游戏循环中，判断用户的具体操作

**代码实现**

* `pygame`通过`pygame.event.get()`可以获得**用户当前所做的动作的 事件列表**

```python
# 创建时钟对象
clock = pygame.time.Clock()
while True:
    clock.tick(60)
    # 捕获事件
    # event_list=pygame.event.get()
    # if len(event_list)>0:
    #     print(event_list)
    for event in pygame.event.get():
        # 判断用户是否点击了关闭按钮
        if event.type == pygame.QUIT:
            print('退出游戏')
            pygame.quit()
            # 直接退出系统
            exit()
            
```

**精灵和精灵组**

![image-20220911134724398](C:\Users\31812\AppData\Roaming\Typora\typora-user-images\image-20220911134724398.png)

![image-20220911135615474](C:\Users\31812\AppData\Roaming\Typora\typora-user-images\image-20220911135615474.png)

![image-20220911141454395](C:\Users\31812\AppData\Roaming\Typora\typora-user-images\image-20220911141454395.png)

![image-20220911143545669](C:\Users\31812\AppData\Roaming\Typora\typora-user-images\image-20220911143545669.png)