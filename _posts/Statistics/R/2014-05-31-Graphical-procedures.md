---
layout: post
title: "R 绘图功能"
date: 2014-05-31 22:00:00
categories: Statistics
tags: R  绘图
keywords: R control_statement 绘图
---

目前R的基本使用系列的基本应用知识加上这一章绘图功能之后就算结束了，为什么专门把绘图功能这一项放在最后面呢，因为这个基本知识介绍完之后,将会有一系列的统计基本图形使用，以及从图形看统计概念的文章，因为R 的绘图功能非常强大，以至于张惇皓同学觉得未来有可能R的绘图会成为R的主要应用方向，我也比较认可，以为它实在太flexible 了。基本应用系列有

* [R 的基本资料类型][data] 
* [R 中list的使用方式][list]
* [R 中图片档案存储方式以及Cairo的安装][Cair]
* [R 中读取资料][read]
* [R 控制语句与逻辑运算符][control]
* [R 函数的编写][function]
* [R loop-function][loop]

其实也可以把R 上的画图功能当作R 上的特有函数的一种，这里确实是觉得R 上的绘图功能很重要，所以就将这个功能重点介绍了，这里的介绍方式参照 《An Introduction to R》, R 上的画图命令一般分为三种不同的类型，
1. 高阶画图命令， 这些命令允许参数新的图形，如plot等
2. 低阶画图命令， 这些命令是用于已存在的图形上面添加自己所需要的信息，比如legend等
3. 交互型命令， R中的绘图命令还允许进行交互，比如我们需要标记那一个点，可以直接在图中点那个点的位置，这样就可以返回点的值了。 rgl() 这个三维的命令也是一样。

## 高阶画图命令

最常使用的高阶画图命令就是plot()了，他是直接画x,y 的散点图. 他的使用方式是：

{% highlight r%}
plot(x,y,...)
{% endhighlight %}

他的参数如下：

* x 就是x轴的值，也可以是只有一個x，这个时候就会画x与其index的散点图。

* y 值，可有可无

* ... 其他的图形参数, 例如

* log = "x"

* log = "y"

* log = "xy"

* type 什么类型的图
  * "p" 画点
  * "l" 画线
  * "b" 同时画点和线,不穿过点
  * "c" 只画线，但是不穿过点
  * "o" 画点和线，穿过点，
  * "h" 对于每一个x点，画垂直与x轴的线，这种方式可以让y轴的值更加明显，如果想要突出最大y值的x,可以使用这种方式
  * "s" 阶梯型函数，从x1开始到x2，然后再跳阶梯，
  * "S" 另一种阶梯型函数，从x1直接开始跳阶梯,
  * "n" 什么点都不画上去，这个方式在后期加点会很有用

如下图所示，使用下面代码可以画出下面对应于不同type的图

{% highlight r%}
t_type.png",width = 800, height =640, type = "cairo-png")
par(mfrow = c(3,3))
x <- c(1:8)
y <- c(4,7,9,8,7,6,4,2)
plot(x,y,type ="p", main= paste("type=", "\"","p","\"",sep=""))
plot(x,y,type ="l", main= paste("type=", "\"","l","\"",sep=""))
plot(x,y,type ="b", main= paste("type=", "\"","b","\"",sep=""))
plot(x,y,type ="c", main= paste("type=", "\"","c","\"",sep=""))
plot(x,y,type ="o", main= paste("type=", "\"","o","\"",sep=""))
plot(x,y,type ="h", main= paste("type=", "\"","h","\"",sep=""))
plot(x,y,type ="s", main= paste("type=", "\"","s","\"",sep=""))
plot(x,y,type ="S", main= paste("type=", "\"","S","\"",sep=""))
plot(x,y,type ="n", main= paste("type=", "\"","n","\"",sep=""))
dev.off()

{% endhighlight %}

![type][]

* xlab: 就是x轴的名称

* ylab: y轴的名称

* main 图的标题，主要在正上方

* sub 图的副标题， 在x轴名称下方

* asp y/x的比值,这个是为了更符合视觉视角或者说自身资料原因需要这样看资料

如下图所示

![asp][]

代码如下

{% highlight r%}
png("plot_asp.png",width = 800, height =640, type = "cairo-png")
par(mfrow = c(2,2))
plot(x,y,type= "p", xlab = "x轴", ylab = "y轴", main = paste("标题：asp = 1/10"),sub = "副标题：这是用于测试的",asp = 1/10)
plot(x,y,type= "p", xlab = "x轴", ylab = "y轴", main = paste("标题：asp = 1"),sub = "副标题：这是用于测试的",asp=1)
plot(x,y,type= "p", xlab = "x轴", ylab = "y轴", main = paste("标题：asp = 4"),sub = "副标题：这是用于测试的",asp =4)
plot(x,y,type= "p", xlab = "x轴", ylab = "y轴", main = paste("标题：asp = 10"),sub = "副标题：这是用于测试的",asp = 10)
dev.off()

{% endhighlight %}

还有其他的高阶层的命令如pairs(x) 和 coplot，如下图

![pairs][] ![coplot][]

代码如下：

{% highlight r%}

png("pairs.png",width = 400, height =440, type = "cairo-png")
x.mat <- matrix(c(1:24),ncol= 3)
colnames(x.mat) <- list("x1","x2","x3")
x.mat <- as.data.frame(x.mat)
pairs(x.mat,main = "多变量画图pairs()")
dev.off()
png("coplot.png",width = 400, height =440, type = "cairo-png")
f <- gl(2,4) # f is factor
coplot(y~x |f,main = "给定条件下")
dev.off()
{% endhighlight %}

##  低阶画图命令

低阶画图命令是用于在已有的图上添加自己想要的基本元素的命令，我们需要添加的基本元素一般都有什么，就是在图上加点，加线，加备注，加标题，加坐标轴这些，因为是在图上加东西，怎么知道加在哪里，这样就需要根据点的位置(x,y)来标志了,下面是一般常用的命令

* points(x,y) 加点

* lines(x,y) 加线

* text(x,y,labels,...) 在点的地方加上备注

* abline(a,b) 就是画一条 $y = a+b*x$ 的线

* abline(h= y) 画一条垂直于y轴的水平线

* abline(v =x) 画一条垂直于x轴的垂直线

* abline(lm.obj) 画一条根据回归模型得到的线

* legend (x, y,legend, ...) 添加备注，标注不同情况

  * x,y 是坐标位置，可以使用"top","bottom","left","right"来表示

  * legend = c("a","b) 要标注的内容

  * ...其他参数，如fill = v 这个是box的背景颜色,col cex 和pch等

* title(main,sub) 标题

* axis(side, ...) 添加轴，使用方式如下

{% highlight r%}
axis(side, at = NULL, labels = TRUE, tick = TRUE, line = NA,
        pos = NA, outer = FALSE, font = NA, lty = "solid",
        lwd = 1, lwd.ticks = lwd, tck = 0.01, col = NULL, col.ticks = NULL,
        hadj = NA, padj = NA, las = 1,...)
{% endhighlight %}
参数的意义如下:
  
  * side: numeric 表示在那一个轴边，1， 2，3， 4 分别表示“下(x)“，“左(y)“，”上“，”右“. 比日下面4张图，图1到4 分别加了4个轴线
  * at:  vector 就是表示要在坐标的哪个位置加上点,因为有时候想要在不同的点上做标注,如下图都是在2,4,6,8上加点

  * labels: logical or character vector,是否加上labels, 这里有可以用一个char的向量，这样就能做标注点了，在画机率图的时候很重要。如图3 轴3的labels为F，不显示

  * tick: logical 是指需不需要加上tick和坐标轴线，如图2 在轴1的地方tick = F

  * line: 这个试了下不知道做什么用
  
  * pos: numeric 在哪个地方加线， 在图3 的地方定轴1的地方pos = 3

  * outer: 把坐标轴画到图形外面

  * font: 字体

  * lty: tick和坐标轴的方式

  * lwd 就是坐标轴的宽度

  * lwd.tick 就是tick的宽度
  
  * tck: 是指坐标轴的长度 如图4 定义tck 为0.3之后, tick的长度变长

  * col: 坐标周的颜色

  * col.tick: 坐标轴的颜色

  * hadj 是指tick labels 距离tick的位置是多少，水平移动，如图2 hadj = 9,向左移动了9个位置

  * padj 是指tick labels 垂直于tick的位置是多少，上下移动

  * las 决定labels 的朝向，如图2的y轴，las=1 那么垂直于tick，图3,las=0,就平行于y轴.

如下图所示

![lower][]

画图所用代码如下

{% highlight r%}
png("lower.png",width = 480, height =480, type = "cairo-png")
par(mfrow = c(2,2))
plot(x,y,axes = F, main = "轴1,tick =T, pos=2",sub = "图1: las = 1",pos = 2, las = 1)
axis(1,at = seq(0,10,by =2),lab= c("0","2",4,6,8,10),tck= 0.01,tick = T, pos = 2,lty = 1,lwd = 4,lwd.tick =4, col = "blue",col.ticks ="red",hadj = 0,pad = 0,las = 1 )
plot(x,y,axes = F, main = "轴1,2,tick1 =F, hadj =9",sub = "图2: las=1" )
axis(1,at = seq(0,10,by =2),lab= c("0","2",4,6,8,10),tck= 0.01,tick = F, pos = 2,lty = 1,lwd = 4,lwd.tick =4, col = "blue",col.ticks ="red",hadj = 9,las = 1 )
axis(2,seq(0,10,by =2),lab= c("0","2",4,6,8,10),tick = T, tck= 0.01,las = 1)
plot(x,y,axes = F, main = "轴1,2,labels =F,hadj =-9",sub = "图3: las2 = 0" )
axis(1,at = seq(0,10,by =2),lab= c("0","2",4,6,8,10),tck= 0.01,tick = T, pos = 3,lty = 1,lwd = 4,lwd.tick =4, col = "blue",col.ticks ="red",hadj = 0,las = 0)
axis(2,seq(0,10,by =2),labels = T,tick = T, tck= 0.01,las = 0)
axis(3,seq(0,10,by =2),lab= c("0","2",4,6,8,10),tck= 0.01,yaxs = "i")

plot(x,y,axes = F, main = "轴1,2,3,4 tck = 0.3",sub = "图4")
axis(1,at = seq(0,10,by =2),lab= c("0","2",4,6,8,10),tck= 0.3,tick = T, pos = 2,lty = 1,lwd = 4,lwd.tick =4, col = "blue",col.ticks ="red",outer =T ,las = 1)
axis(2,seq(0,10,by =2),lab= c("0","2",4,6,8,10),tick = T, tck= 0.3,las = 0)
axis(3,seq(0,10,by =2),lab= c("0","2",4,6,8,10),tck= 0.3,yaxs = "i")
axis(4,seq(0,10,by =2),lab= c("0","2",4,6,8,10),tck= 0.3,yaxs = "i")
dev.off()
{% endhighlight %}

## Interactive command

这里介绍两个交互型的命令 locator() 和 indetify(x,y) 

locator(n,type)的作用在于点击图形就可以返回点击点的坐标，实验如下：

{% highlight r %}
plot(x,y)
locator(1,type) # 左键点击一个点就可以返回了
$x
[1] 1.96
$y
[1] 6.97
text(locator(1),"Outlier",adj=0) # 点击就能标注outlier了
{% endhighlight %}

identity(x,y,labels)


{% highlight r %}
plot(x,y)
identify(x,y,"Outlier") #点击右键退出
{% endhighlight %}

## Parmanent change par()

最后介绍一个par()，这个是设定当前的drive 的图形参数，不用在使用plot等画图命令的时候添加很多参数，而是可以直接在画图之间改变默认的图片参数，
比如上面设定在一张图里面使用4个plot在同一张图用par(mfrow(2,2))。比较关键需要设定的是下面的参数

* pch = "1" 这个其实是用来设定所要的点的类型, 各个值对应的点如下图所示

![pch][]

* lty 是线的类型

* mgp = c(3,1,0)  这个是用来设定轴的距离，第一个3是指轴的标签到轴的位置距离,比如x离轴距离多少，为3个text line的距离, 第二个1是指轴的lables 到轴的位置的距离，第三个0是指要画的轴线到轴的位置的距离

* mai = c(1,0.5,0.5,0) 这个是用来设定图的边界多少，这个使用inch来衡量的，

* mar = c(4,2,2,1) 这个也是用来设定图的边界，不过这个是用text line 的长度来衡量的,分别对应的是bottom, left, top , right

* mfrow= c(2,2） 用来设定加入多少张图的，

* mfcol = c(2,2) 这个跟上面的命令一样，不过画图方式是先把col填满再直画下来

* mfrow = c(2,2) 这个就跟默认的mfrow 一样，先填满row 

* fig = c(4,9,1,4)/10 这个可以设定图形要画在那个位置，分别对应的是图形的4个点，左边，右边，bottom 和top, 这个可以参照 [programmingr][pro] 给的演示，非常漂亮，在下面可以看到图片效果

![fig][]


{% highlight r %}
png("fig.png",width = 800, height =640, type = "cairo-png")
# create data
sim.data <- cbind(replicate(5,runif(8,min=0, max=100)))
# place graph one in the bottom left
par(fig=c(0, .25, 0, .25), mar=c(2,.5,1,.5), mgp=c(0, 1, 0))
# #open plot
plot(c(0,100), c(-1,1), type = "n", ylab = "", yaxt = "n", xlab = "")
points(sim.data[,1], replicate(8, 0), pch = 19, col = 1:8)
# # add center reference line
abline(0,0)
# place graph two in the top right
# set graphing parameters for next plot and set new parameter to TRUE
par(fig=c(.75, 1, .75, 1), new = TRUE)
#open plot
plot(c(0,100), c(-1,1), type = "n", ylab = "", yaxt = "n", xlab = "")
points(sim.data[,2], replicate(8, 0), pch = 19, col = 1:8)
# add center reference line
abline(0,0)
# place main graph in the center
# set graphing parameters for next plot and set new parameter to TRUE
par(fig=c(.25, .75, .25, .75), new = TRUE)
#open plot
plot(c(0,100), c(-1,1), type = "n", ylab = "", yaxt = "n", xlab = "")
points(sim.data[,3], replicate(8, 0), pch = 19, col = 1:8, cex = 1.5)
# add center reference line
abline(0,0)
legend("bottomright", fill = c(1:8), legend = c(1:8), ncol = 4)
dev.off()
{% endhighlight %}

* oma = c(2,0,3,0) 这个就跟前面的mar 一样，不过这个是用来设置outer 的大小的

最后附上一张完成的图各个元素，包括了outer , figure 和plot area,这张图来自 [figure][]

![Figure][]

[figure]: http://exotica-taiwan.blogspot.tw/2014/05/blog-post_31.html
[data]: http://blog.xjchen.net/statistics/2014/05/20/R-datatype/
[list]: http://blog.xjchen.net/statistics/2014/05/24/subsetting-and-list/
[Cair]: http://blog.xjchen.net/statistics/2014/05/25/Install-Cairo/
[read]: http://blog.xjchen.net/statistics/2014/05/27/Read-and-write/
[control]: http://blog.xjchen.net/statistics/2014/05/29/control/
[function]: http://blog.xjchen.net/statistics/2014/05/30/function/
[loop]: http://blog.xjchen.net/statistics/2014/05/30/Loop-function/
[type]: {{BASE_PATH}}/images/plot_type.png
[asp]: {{BASE_PATH}}/images/plot_asp.png
[pairs]: {{BASE_PATH}}/images/pairs.png
[coplot]: {{BASE_PATH}}/images/coplot.png
[lower]: {{BASE_PATH}}/images/lower.png
[pch]: {{BASE_PATH}}/images/pch.jpg
[fig]: {{BASE_PATH}}/images/fig.png
[Figure]: {{BASE_PATH}}/images/Figure.gif
[pro]: http://www.programmingr.com/content/positioning-charts-fig-and-fin/
