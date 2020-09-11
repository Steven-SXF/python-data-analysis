## 1.pygame最小开发框架
- 引入模块
- 初始化
- 事件获取
- 屏幕刷新
```python
#引入模块
import pygame,sys
#初始化
pygame.init()
screen=pygame.display.set_mode((600,400))
pygame.display.set_caption("Hello pygame")
while True:
    #事件获取
    for event in pygame.event.get():
        if event.type==pygame.QUIT:
            sys.exit()
    #屏幕刷新
    pygame.display.update()
```

## 2.pygame屏幕绘制机制
> 屏幕绘制机制

1. pygame.display 操作屏幕
2. 笛卡尔坐标系
3. 屏幕控制要求
    - 全屏
    - 大小可调节
    - 无边框
    - 更改标题
    - 更改图标
    
> 屏幕尺寸和模式
1. pygame.display.set_mode(r=(0,0),flags=0) 设置相关屏幕尺寸信息
    - r是游戏屏幕的分辨率采用(width,height)方式输入
    
    - flags用来控制显示类型,可以用|组合 需要相关处理机制
    
        - pygame.RESIZABLE  窗口可调  需响应改变
        - pygame.NOFRAME   无边界   需设置退出按钮
        - pygame.FULLSCREEN 窗口全屏显示 需设置分辨率
        
2. pygame.display.Info() 生成屏幕相关信息

> 窗口标题和图标设置
1. pygame.display.set_caption()设置标题
2. pygame.display.set_icon(surface)设置图标
3. pygame.display.get_caption()获得标题

> 窗口感知和刷新应用
1. pygame.display.get_active()
2. pygame.display.flip()
3. pygame.display.update()

- 新事件
- pygame.VIDEORESIZE 窗口大小更改
    - 事件发生后,返回event.size元组 包含宽度和高度
    - event.size[0]宽度
    - event.size[1]高度

## 3.pygame事件处理机制
- 事件处理机制
    - 事件队列
    - 事件类型及属性
- 键盘事件及类型使用
1. KEYDOWN
2. KEYUP
   - 按键修饰符
- 鼠标事件及其类型的使用
    1. pygame.event.MOUSEBOTION鼠标移动事件
    2. pygame.event.MOUSEBUTTONDOWN鼠标按下事件
    3. pygame.event.MOUSEBUTTONUP鼠标释放事件
- 壁球小游戏鼠标型
- pygame事件处理函数
    - 处理事件
    1. pygame,event.get()
    2. pygame.event.poll()
    3. pygame.event.clear()
    - 操作事件队列
    1. pygame.event.set_blocked()
    2. pygame.event.get_blocked()
    3. pygame.event.set_allowed()
    - 生成事件
    1. pygame.event.post()
    2. pygame.event.Event()

## 4.pygame色彩与绘图机制

## 5.小游戏 小球碰壁展示型
   - 引入小球
   - 小球移动
   - 碰壁回弹


```python
import pygame
import sys

pygame.init()
size = width, height = 600, 400
speed = [1, 1]
screen = pygame.display.set_mode(size)
pygame.display.set_caption("Small ball hit the wall")
bg_color = (230, 230, 230)

#获取surface对象和rect矩形对象
ball = pygame.image.load("PYG02-ball.gif")
ball_rect = ball.get_rect()

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()
        #move()使小球运动
        ball_rect = ball_rect.move(speed[0], speed[1])
    if ball_rect.left < 0 or ball_rect.right > width:
        speed[0] = -speed[0]
    if ball_rect.top < 0 or ball_rect.bottom > height:
        speed[1] = -speed[1]
    #颜色填充
    screen.fill(bg_color)
    #blit()函数 将一个图像绘制在另一个图像上
    screen.blit(ball, ball_rect)
    pygame.display.update()
```

## 小球碰壁改良1-节奏型与屏幕帧率设置

```python
import pygame
import sys

pygame.init()
size = width, height = 600, 400
speed = [1, 1]
screen = pygame.display.set_mode(size)
pygame.display.set_caption("Small ball hit the wall")
bg_color = (230, 230, 230)
fps=300
#创建了一个Clock对象 用于操控时间
fclock=pygame.time.Clock()

#获取surface对象和rect矩形对象
ball = pygame.image.load("PYG02-ball.gif")
ball_rect = ball.get_rect()

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()
        #move()使小球运动
        ball_rect = ball_rect.move(speed[0], speed[1])
    if ball_rect.left < 0 or ball_rect.right > width:
        speed[0] = -speed[0]
    if ball_rect.top < 0 or ball_rect.bottom > height:
        speed[1] = -speed[1]
    #颜色填充
    screen.fill(bg_color)
    #blit()函数 将一个图像绘制在另一个图像上
    screen.blit(ball, ball_rect)
    pygame.display.update()
    #控制帧的速度,即窗口的刷新速度 fps=100 每秒刷新100次
    fclock.tick(fps)
```

## 小球碰壁改良2-操控型与键盘的基本使用


```python
import pygame
import sys
#初始化窗体及设置
pygame.init()
size = width, height = 600, 400
speed = [1, 1]
screen = pygame.display.set_mode(size)
pygame.display.set_caption("Small ball hit the wall")
bg_color = (230, 230, 230)
fps=300
#创建了一个Clock对象 用于操控时间
fclock=pygame.time.Clock()

#获取surface对象和rect矩形对象
ball = pygame.image.load("PYG02-ball.gif")
ball_rect = ball.get_rect()

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()
         #定义键盘敲击事件
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                speed[0] = speed[0] if speed[0] == 0 else (
                    abs(speed[0])-1)*int(speed[0]/abs(speed[0]))
            elif event.key == pygame.K_RIGHT:
                speed[0] = speed[0]+1 if speed[0] > 0 else speed[0]-1
            elif event.key == pygame.K_UP:
                speed[1] = speed[1]+1 if speed[1] > 0 else speed[1]-1
            elif event.key == pygame.K_DOWN:
                speed[1] = speed[1] if speed[1] == 0 else (
                    abs(speed[1])-1)*int(speed[1]/abs(speed[1]))
        #move()使小球运动
        ball_rect = ball_rect.move(speed[0], speed[1])
    if ball_rect.left < 0 or ball_rect.right > width:
        speed[0] = -speed[0]
    if ball_rect.top < 0 or ball_rect.bottom > height:
        speed[1] = -speed[1]
    #颜色填充
    screen.fill(bg_color)
    #blit()函数 将一个图像绘制在另一个图像上
    screen.blit(ball, ball_rect)
    pygame.display.update()
    #控制帧的速度,即窗口的刷新速度 fps=100 每秒刷新100次
    fclock.tick(fps)
```

## 小球碰壁改良3-全屏
```python
import pygame
import sys

pygame.init()
#Info()获取屏幕信息
vInfo = pygame.display.Info()
#获取当前屏幕信息的高宽和高
size = width, height = vInfo.current_w, vInfo.current_h
speed = [1, 1]
fps = 300
fclock = pygame.time.Clock()
#设置全屏模式
screen = pygame.display.set_mode(size, pygame.FULLSCREEN)
pygame.display.set_caption("小球碰壁")

bg_color = (230, 230, 230)

ball = pygame.image.load("PYG02-ball.gif")
ballrect = ball.get_rect()

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                speed[0] = speed[0] if speed[0] == 0 else (
                    abs(speed[0])-1)*int(speed[0]/abs(speed[0]))
            elif event.key == pygame.K_RIGHT:
                speed[0] = speed[0]+1 if speed[0] > 0 else speed[0]-1
            elif event.key == pygame.K_UP:
                speed[1] = speed[1]+1 if speed[1] > 0 else speed[1]-1
            elif event.key == pygame.K_DOWN:
                speed[1] = speed[1] if speed[1] == 0 else (
                    abs(speed[1])-1)*int(speed[1]/abs(speed[1]))
            #设置按ESC退出游戏
            elif event.key == pygame.K_ESCAPE:
                sys.exit()
    ballrect = ballrect.move(speed[0], speed[1])
    if ballrect.left < 0 or ballrect.right > width:
        speed[0] = -speed[0]
    if ballrect.top < 0 or ballrect.bottom > height:
        speed[1] = -speed[1]

    screen.fill(bg_color)
    screen.blit(ball, ballrect)
    pygame.display.update()
    fclock.tick(fps)
```

## 小球碰壁改良4-感知型  可伸缩型  改变图标
```python
import pygame
import sys

pygame.init()

size = width, height = 600, 400
speed = [1, 1]
fps = 300
f_clock = pygame.time.Clock()
#加载图标及设置图标
icon = pygame.image.load("PYG03-flower.png")
pygame.display.set_icon(icon)
screen = pygame.display.set_mode(size, pygame.RESIZABLE)
pygame.display.set_caption("Small ball hit the wall")
bg_color = (230, 230, 230)
ball = pygame.image.load("PYG02-ball.gif")
ball_rect = ball.get_rect()

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                speed[0] = speed[0] if speed[0] == 0 else (
                    abs(speed[0])-1)*int(speed[0]/abs(speed[0]))
            elif event.key == pygame.K_RIGHT:
                speed[0] = speed[0]+1 if speed[0] > 0 else speed[0]-1
            elif event.key == pygame.K_UP:
                speed[1] = speed[1]+1 if speed[1] > 0 else speed[1]-1
            elif event.key == pygame.K_DOWN:
                speed[1] = speed[1] if speed[1] == 0 else (
                    abs(speed[1])-1)*int(speed[1]/abs(speed[1]))
            elif event.key == pygame.K_ESCAPE:
                sys.exit()
        #对窗口改变的新事件发生响应
        elif event.type == pygame.VIDEORESIZE:
            size = width, height = event.size[0], event.size[1]
            screen = pygame.display.set_mode(size, pygame.RESIZABLE)
    #感知变化  图标化时静止
    if pygame.display.get_active():
        ball_rect = ball_rect.move(speed[0], speed[1])
    if ball_rect.left < 0 or ball_rect.right > width:
        speed[0] = -speed[0]
    if ball_rect.top < 0 or ball_rect.bottom > height:
        speed[1] = -speed[1]

    screen.fill(bg_color)
    screen.blit(ball, ball_rect)
    pygame.display.update()
    f_clock.tick(fps)
```

##  小球碰壁改良5-鼠标型

```python
import pygame
import sys

pygame.init()

size = width, height = 600, 400
speed = [1, 1]
fps = 300
f_clock = pygame.time.Clock()
#加载图标及设置图标
icon = pygame.image.load("PYG03-flower.png")
pygame.display.set_icon(icon)
screen = pygame.display.set_mode(size, pygame.RESIZABLE)
pygame.display.set_caption("Small ball hit the wall")
bg_color = (230, 230, 230)
ball = pygame.image.load("PYG02-ball.gif")
ball_rect = ball.get_rect()
still=False

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                speed[0] = speed[0] if speed[0] == 0 else (
                    abs(speed[0])-1)*int(speed[0]/abs(speed[0]))
            elif event.key == pygame.K_RIGHT:
                speed[0] = speed[0]+1 if speed[0] > 0 else speed[0]-1
            elif event.key == pygame.K_UP:
                speed[1] = speed[1]+1 if speed[1] > 0 else speed[1]-1
            elif event.key == pygame.K_DOWN:
                speed[1] = speed[1] if speed[1] == 0 else (
                    abs(speed[1])-1)*int(speed[1]/abs(speed[1]))
            elif event.key == pygame.K_ESCAPE:
                sys.exit()
        #增加对鼠标事件的响应
        elif event.type==pygame.MOUSEBUTTONDOWN:
            if event.button==1:
                still=True
        elif event.type==pygame.MOUSEBUTTONUP:
            still=False
            if event.button==1:
                ball_rect=ball_rect.move(event.pos[0]-ball_rect.left,event.pos[1]-ball_rect.top)
        elif event.type==pygame.MOUSEMOTION:
            if event.buttons[0]==1:
                ball_rect=ball_rect.move(event.pos[0]-ball_rect.left,event.pos[1]-ball_rect.top)
        #对窗口改变的新事件发生响应
        elif event.type == pygame.VIDEORESIZE:
            size = width, height = event.size[0], event.size[1]
            screen = pygame.display.set_mode(size, pygame.RESIZABLE)
    #感知变化  图标化时静止
    if pygame.display.get_active()and not still:
        ball_rect = ball_rect.move(speed[0], speed[1])
    if ball_rect.left < 0 or ball_rect.right > width:
        speed[0] = -speed[0]
    if ball_rect.top < 0 or ball_rect.bottom > height:
        speed[1] = -speed[1]

    screen.fill(bg_color)
    screen.blit(ball, ball_rect)
    pygame.display.update()
    f_clock.tick(fps)

```
