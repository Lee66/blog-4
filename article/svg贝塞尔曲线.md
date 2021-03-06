弧形的特点是整洁，也能达到我们想要的效果，但是很少有人用“优雅”这个词来形容它。如果想要优雅，则需要通过二次和三次方程绘图生成曲线。数学家在几百年前就知道这些曲线了，但是绘制它们始终是一个计算很复杂的任务。法国汽车制造商雷诺公司的工程师皮埃尔·贝塞尔和雪铁龙公司的物理学家和数学家Paul de Casteljau 改变了这一状况，他们开发并推广了一种更简便的方式来生成这些曲线。

如果你用过Adobe Illustrator这类绘图程序，可以通过指定两个点以及移动如下图所示的“控制柄”来绘制这些贝塞尔曲线。控制柄的端点称为控制点，因为它控制着曲线的状态。当我们移动控制柄时，曲线以一种在外行看来非常神秘的方式变化着。Key Point软件公司的图形设计师迈克·伍德（Mike Woodburn），提出了下图这种形象的表示控制点和曲线关系的方式：想象一下线是由柔性金属制造的。控制点内部是一个磁铁，与控制点越近，吸引力越强。

![image](https://cloud.githubusercontent.com/assets/1744713/26445562/2a228ac2-4173-11e7-94cc-8fc96e658eef.png)

## 二次贝塞尔曲线
最简单的贝塞尔曲线就是二次曲线。我们要指定起点、终点和控制点。假设有两个支柱放在线的两个端点，这两个支柱的交点就是控制点。拉紧两个支柱中点的是一根橡皮筋。其中曲线弯曲的地方正好和橡皮筋的中点相连。如下图所示：
![image](https://cloud.githubusercontent.com/assets/1744713/26445488/f876d9ce-4172-11e7-92bf-5e81bb23e193.png)


起点/终点以及以及控制点之间的线与曲线的起点/终点成切线。曲线首先沿着到达控制点的线，然后弯曲到达中点，再顺着“支柱”线的方向。曲线最终从控制点沿着线向上滑到终点。Adobe Illustrator这类程序只会展示一个“支柱”。下次使用这类程序时，可以在心里添加第二个支柱，这样产生的曲线就不那么神秘了。

可以通过在`<path>`中使用`Q`或者`q`命令指定一个二次曲线。这个命令后面紧跟着两组指定控制点和终点的坐标。大写命令意味着绝对坐标，小写意味着相对坐标。
可以先体验一下这个在线示例：http://oreillymedia.github.io/svg-essentials-examples/ch07/quadratic-bezier.html

绘制一个最简单的示例，曲线的起点是在`(30,75)`，终点是在`(300,120)`，控制点是在`(240,30)`，`stroke: black`表示线条的颜色为黑色，`fill: none`表示不需要填充任何颜色。
```svg
<svg>
    <path d="M30,75 Q240,30 300,120" style="stroke: black; fill: none;" />
</svg>
```
![image](https://cloud.githubusercontent.com/assets/1744713/26441606/c5c8122c-4163-11e7-98d9-93ba4609ef44.png)

还可以在二次曲线命令后面指定多组坐标，这会生成一个多边贝塞尔曲线。下面的示例是同时绘制了两条贝塞尔曲线，一条是从`(300,100)`到`(100,100)`，控制点是在`(80,30)`，然后紧接着绘制了一条终点在`(200,80)`，控制点在`(130,65)`的曲线：
```svg
<svg>
    <path d="M30,100 Q80,30 100,100 Q130,65 200,80" style="stroke: black; fill: none;" />
</svg>
```
![image](https://cloud.githubusercontent.com/assets/1744713/26442037/70ab71a6-4165-11e7-8cc4-b8ee7155846b.png)

你可能心想：“哪里优雅了？该曲线都是凹凸不平的。”这个评价是正确的，这是因为曲线相连接并不意味着它们在一起会好看。这也是为什么SVG还提供了流畅的二次曲线命令，用字母`T`表示（想要使用相对坐标，就用`t`），这个命令后面紧跟的是曲线的下一个端点。控制点会自动计算，方法是“使新的控制点与上一条命令中的控制点相对于当前点中心对称”。
```svg
<svg>
    <path d="M30,100 Q80,30 100,100 T200,80" style="stroke: black; fill: none;" />
</svg>
```
![image](https://cloud.githubusercontent.com/assets/1744713/26442563/a18f22ac-4167-11e7-951f-077d005617f3.png)

从数学的角度讲，新的控制点`(x2,y2)`基于上一条线段的端点`(x,y)`和上一个控制点`(x1,y1)`，按照如下规则计算：
```math
x2 = x + (x - x1) = 2 * x - x1
y2 = y + (y - y1) = 2 * y - y1
```

## 三次贝塞尔曲线

单个二次贝塞尔曲线有且只有一个顶点，或者每个曲线段都只有一个凹谷。换句话说，三次曲线可以包含一个拐点：曲线从该点开始从一个方向往另一个方向弯曲。二次曲线和三次曲线的区别是三次曲线有两个控制点，每个端点对应一个。

要指定条三次曲线，使用`C`或者`c`命令。这个命令后面紧跟三组坐标，用来指定起点的控制点、终点的控制点以及端点。绘制一条曲线从`(20,80)`到`(200,120)`，控制点分别在`(50,20)`和`(150,60)`：
```svg
<svg>
    <path d="M20,80 C50,20 150,60 200,120" style="stroke: black; fill: none;" />
</svg>
```
![image](https://cloud.githubusercontent.com/assets/1744713/26443205/00324b2a-416a-11e7-8cfb-88bcd99c4395.png)

可以体验在线示例尝试更多的组合：http://oreillymedia.github.io/svg-essentials-examples/ch07/cubic-bezier.html

和二次曲线一样，也可以通过在三次曲线命令后面指定多组坐标，来构建多条连接在一起的三次曲线。第一条曲线的最后一个点会变成下一条曲线的第一个点，以此类推。下面绘制一条从`(30,100)`到`(100,100)`，控制点在`(50,50)`和`(70,20)`的三次曲线；后面又紧跟着一条曲线，折回到`(65,100)`，控制点在`(110,130)`和`(45,150)`处。
```svg
<svg>
    <path d="M30,100 C 50,50 70,20 100,100 110,130 45,150 65,100" style="stroke: black; fill: none;" />
</svg>
```
![image](https://cloud.githubusercontent.com/assets/1744713/26444916/dba610be-4170-11e7-9bda-618d67c3ede3.png)

如果想要保证曲线之间的连接平滑，可以使用`S`命令。在某种程度上，它和二次曲线的`T`命令类似，新的曲线会把上一条曲线的端点作为起点，并且它的控制点是上一个终点控制点的中心对称点。我们需要提供的只是曲线的下一个端点的控制点，然后紧跟着的是下一个端点。

再画一个示例，从`(30,100)`到`(100,100)`，控制点为`50,30`和`(70,50)`。然后它平滑过渡到`(200,80)`，使用`(150,40)`作为终点控制点。
```svg
<svg>
    <path d="M30,100 C 50,30 70,50 100,100 S 150,40 200,80" style="stroke: black; fill: none;" />
</svg>
```
![image](https://cloud.githubusercontent.com/assets/1744713/26445143/a49192fa-4171-11e7-869e-a227caf1a3df.png)

## 非常强的几个可以用来练习贝塞尔曲线的网站
* http://cubic-bezier.com/#.64,.61,1,-1.03
* http://bezier.method.ac/
* http://dayu.pw/svgcontrol/

