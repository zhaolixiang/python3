# 用python度过一个温馨的单身七夕节

> 纽约时间比加州时间早三个小时，New York is 3 hours ahead of California
>
> 但加州时间并没有变慢。but it does not make California slow.
>
> 有人22岁就毕业了，Someone graduated at the age of 22,
>
> 但等了五年才找到稳定的工作！but waited 5 years before securing a good job!
>
> 有人25岁就当上CEO，Someone became a CEO at 25,
>
> 却在50岁去世。and died at 50.
>
> 也有人直到50岁才当上CEO，While another became a CEO at 50,
>
> 然后活到90岁。and lived to 90 years.
>
> 有人单身，Someone is still single,
>
> 同时也有人已婚，while someone else got married,
>
> 也有人又恢复单身了。someone is single again.
>
> 欧巴马55岁就退休，Obama retires at 55,
>
> 川普70岁才开始当总统 。but Trump starts at 70.
>
> 世上每个人本来就有自己的发展时区。Absolutely everyone in this world works based on their Time Zone.
>
> 身边有些人看似走在你前面，People around you might seem to go ahead of you,
>
> 也有人看似走在你后面。some might seem to be behind you.
>
> 但其实每个人在自己的时区有自己的步程。But everyone is running their own RACE, in their own TIME.
>
> 不用嫉妒或嘲笑他们。Don’t envy them or mock them.
>
> 他们都在自己的时区里，你也是！They are in their TIME ZONE, and you are in yours!
>
> 生命就是等待正确的行动时机。Life is about waiting for the right moment to act
>
> 所以，放轻松。So, RELAX.
>
> 你没有落后。You’re not LATE.
>
> 你没有领先。You’re not EARLY.
>
> 在命运为你安排的属于自己的时区里，一切都准时。You are very much ON TIME, and in your TIME ZONE Destiny set up for you.

七夕到了，作为独自一人的你，是否会有那么一丢丢失落呢，在这个特殊的日子，再好的代码可能也无法挽救你失落的心，但诗和python也许可以。如果你认真读了上面的诗，会有那么一丝丝安慰，别着急，满满来，这一篇，绝对是最温情的Python教程。就像诗中所说，属于每个人的美好，总会到来，而这之前，请过好自己，做好迎接美好的准备，有时候，你不缺遇到美好的机遇，只差抓住美好的能力。让我们来提示能力吧，最后会有安利诗句和视频哦，温馨属于我们每个不放弃自我的人。

本篇文章启发和代码来源：[https://segmentfault.com/a/1190000016048640](https://segmentfault.com/a/1190000016048640)

![](/assets/Jietu20180817-153038.jpg)

### 1、代码与解释

```
"""
Tkinter库属于Python的GUI编程部分。
Python提供了多个图形开发界面的库，
常用的有Tkinter，xwPython，Jython。Tkinter是Python的标准GUI库，内置在Python中，
不需要额外安装，对于一些简单的图形界面可以轻松实现。

如果PyCharm安装PIL安装失败的话，请在Pyharm下面的控制台直接命令安装：pip install Pillow
"""
import tkinter as tk #Tkinter：最终的GUI实现；
from PIL import Image, ImageTk #处理图像，在最后画布背景中使用；
from time import time, sleep #处理时间，完成时间生命周期的更新迭代；
from random import choice, uniform, randint #随机产生数字，定义燃放过程中的随机变量；
from math import sin, cos, radians #数学函数方法，计算燃放移动使用；

# 设置重力参数
GRAVITY = 0.05
# 设置随机的颜色列表
colors = ['red', 'blue', 'yellow', 'white', 'green', 'orange', 'purple', 'seagreen', 'indigo', 'cornflowerblue']

#定义一个通用的烟花颗粒的类
class part:
    def __init__(self, cv, idx, total, explosion_speed, x=0., y=0., vx=0., vy=0., size=2., color='red', lifespan=2,
                 **kwargs):
        self.id = idx #每个烟花中颗粒的标识；
        self.x = x #烟花的x轴；
        self.y = y #烟花的y轴；
        self.initial_speed = explosion_speed
        self.vx = vx #在x轴中颗粒的速度；
        self.vy = vy #在y轴中颗粒的速度；
        self.total = total #每个烟花的颗粒数量；
        self.age = 0 #颗粒已经在背景度过的时间；
        self.color = color #颜色；
        self.cv = cv #背景；
        self.cid = self.cv.create_oval(
            x - size, y - size, x + size,
            y + size, fill=self.color)
        self.lifespan = lifespan

    #通过判断颗粒状态更新颗粒的生命时间；
    def update(self, dt):
        self.age += dt

        # 颗粒爆炸
        if self.alive() and self.expand():
            move_x = cos(radians(self.id * 360 / self.total)) * self.initial_speed
            move_y = sin(radians(self.id * 360 / self.total)) * self.initial_speed
            self.cv.move(self.cid, move_x, move_y)
            self.vx = move_x / (float(dt) * 1000)

        # 颗粒降落
        elif self.alive():
            move_x = cos(radians(self.id * 360 / self.total))

            self.cv.move(self.cid, self.vx + move_x, self.vy + GRAVITY * dt)
            self.vy += GRAVITY * dt

        # 如果颗超过最长持续时间，颗粒消失
        elif self.cid is not None:
            cv.delete(self.cid)
            self.cid = None

    # 定义爆炸的时间
    def expand(self):
        return self.age <= 1.2

    # 检查颗粒在生命周内是否还存在
    def alive(self):
        return self.age <= self.lifespan
"""
上面完成了一个通用的烟花颗粒类的实现，下面就开始烟花燃放的模拟循环过程：通过递归不断循地在背景中产生新的烟花。

首先定义一个 simulate 模拟的函数，在函数中定了一些参数：

t：时间戳；
explode_points：烟花爆炸点列表，供后续更新使用；
num_explore：随机的烟花数量；
然后在所有的烟花数量中循环创建所有的烟花颗粒类，当然在每次循环中颗粒类都需要设置一定的属性参数，参数多是随机产生：

objects：存放所有的颗粒对象；
x_cordi，y_cordi：随机产生烟花在背景中的x，y坐标位置（50，550）；
speed：随机产生颗粒移动速度（0.5，1.5）；
size：随机产生颗粒大小（0.5，3）；
color：选择颜色随机列表中的颜色；
total_particles：随机产生每个烟花中所有颗粒的数量；
有了这些参数，我们就可以定义循环产生每个颗粒对象了，并将每个烟花的所有颗粒对象储存在objects中。也就是说explore_points是列表中套列表，内层列表是每个烟花的所有颗粒对象，外层列表是所有烟花。

所有的颗粒对象完成后，就开始对每个颗粒的生命时间进行更新，且总时间设定在1.8秒以内。最后通过root递归使烟花可以一直在背景中燃放。
"""
def simulate(cv):
    t = time()
    explode_points = []
    wait_time = randint(10, 100)
    numb_explode = randint(6, 10)
    # 循环创建所有的烟花颗粒
    for point in range(numb_explode):
        objects = []
        x_cordi = randint(50, 950)
        y_cordi = randint(50, 150)
        speed = uniform(0.5, 1.5)
        size = uniform(0.5, 3)
        color = choice(colors)
        explosion_speed = uniform(0.2, 1)
        total_particles = randint(10, 50)
        for i in range(1, total_particles):
            r = part(cv, idx=i, total=total_particles, explosion_speed=explosion_speed, x=x_cordi, y=y_cordi,
                     vx=speed, vy=speed, color=color, size=size, lifespan=uniform(0.6, 1.75))
            objects.append(r)
        explode_points.append(objects)

    total_time = .0
    # 保持在1.8秒内进行更新
    while total_time < 1.8:
        sleep(0.01)
        tnew = time()
        t, dt = tnew, tnew - t
        for point in explode_points:
            for item in point:
                item.update(dt)
        cv.update()
        total_time += dt
    # 通过递归持续不断的在背景中添加新烟花
    root.after(wait_time, simulate, cv)

def close(*ignore):
    """停止模拟循环，关闭窗口"""
    global root
    root.quit()

"""
以上代码部分均与Tkinter无关，只是定义了颗粒对象以及模拟颗粒生命周期的全过程，下面将使用Tkinter完成最终的效果。

root：Tkinter类的对象；
cv：定义了Tkinter中背景画布对象，其中height和width参数可根据实际进行调整；
image：打开的图像对象，图像将被作为画布中的背景，图像可根据自己喜好自行选择；
photo：使用ImageTk定义了Tkinter中的图像对象；
然后将在画布对象上创建一个图像（使用定义的photo对象作为参数），最后调用Tkinter对象root进行持续不断地simulate模拟过程。
"""
if __name__ == '__main__':
    root = tk.Tk()
    cv = tk.Canvas(root, height=640, width=959)
    # 自己选择一个好的图像背景填充画布
    image = Image.open("image.jpeg")
    photo = ImageTk.PhotoImage(image)
    cv.create_image(0, 0, image=photo, anchor='nw')

    cv.pack()
    root.protocol("WM_DELETE_WINDOW", close)

    root.after(100, simulate, cv)

    root.mainloop()
```

### 2、最终效果
![](/assets/77细节礼物.gif)
### 3、安利1：《热爱生命》

> 我不去想，是否能够成功，既然选择了远方，便只顾风雨兼程。
>
> 我不去想，能否赢得爱情，既然钟情于玫瑰，就勇敢地吐露真诚。
>
> 我不去想，身后会不会袭来寒风冷雨，既然目标是地平线，留给世界的只能是背影。
>
> 我不去想，未来是平坦还是泥泞，只要热爱生命，一切，都在意料之中。

### 4、安利2：电影《心灵捕手》

[温馨解读版](https://www.bilibili.com/video/av22771407/?share_source=weixin&ts=1534492342&share_medium=iphone&bbid=723f2deb274f10cdcbf15fda6894381e)

### 5、如果还感觉有那么一丝丝失落，欢迎在下面留言，可三陪：陪唠嗑、陪吃鸡、陪lol



