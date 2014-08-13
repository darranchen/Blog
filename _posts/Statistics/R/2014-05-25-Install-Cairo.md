---
layout: post
title:  R 畫圖功能以及安装 Cairo package
date: 2014-05-25 20:34:00
categories: Statistics
tags: R  绘图
keywords: R 绘图 Cairo
---

参照 [R: grDevices][png], R 绘图中保存图片功能可以使用


* jpeg(filename = "R.plot.jpeg",width=480,height=480,unit = "px",pointsize =12,quality = 75, bg = "white",res = NA, type = c("cairo","Xlib","quartz"),antiatlias)



* png(filename = "R.plot.png",width=480,height=480,unit = "px",pointsize =12,bg = "white",res = NA, type = c("cairo","cairo-png","Xlib","quartz"),antiatlias)


* bmp(filename = "R.plot.bmp",width=480,height=480,unit = "px",pointsize =12,bg = "white",res = NA, type = c("cairo","Xlib","quartz"),antiatlias)

* tiff(filename = "R.plot.tiff",width=480,height=480,unit = "px",pointsize =12,compressinon = c("nono","rle,"lzw","jpeg,"zip","lzw+p","zip+p"),bg = "white",res = NA, type = c("cairo","Xlib","quartz"),antiatlias)

目前比较常用的是使用jpeg 和png, 因为他们的文档大小较小，所以比较方便做传输，相反对于其他两种虽然图片质量比较高，但是他们太大了，详细的可以参照 [格式比较][compare], 其中各个参数的意义是：

* filename\: 不用说就是文档名字，

* width height\: 就是图片保留多少px，默认是480\*480的图

* unit\:  默认是px, 也可以是in(inches),cm 或者mm

* pointsize\: 点的大小

* bg\: 图的背景颜色

* quality\: 是jpeg 压缩图档的比例

* compression\: 对于tiff 档案可以有不同的压缩格式，可以使用不同方式，如果类型是 "quartz"那么就默认忽略

* res\: 这个其实是跟我们显示的字体大小有关的，R中是根据inches 来决定每张图上的字体大小, inches 越大，字体就越小, R 默认是72 pixels 每inch,如果是480/72 这张图就是6.6 inch, 人比较适应的字体大概在7-10之间，详情可以参照 [10 tips for making your R graphics look their best][10tips]

* type\: 默认使用的cairo, 如果系统中有装cairo的话，否则就会使用xlib

* antialias: 去锯齿化，如果有使用cairo 就会自动去锯齿化。

可以先看一下使用不同的类型画出来的图档,使用`Xlib`, 考虑下面的代码,刚好是我这次要帮一个老师画的一张图.
{%highlight r%}
png("Xlib.png",width = 640,height= 480,type= "Xlib")
par(mar = c(4.1,2.1,2.1,2.1),mgp = c(1,1,0),las=1)
x <- seq(from = -0.32,to =10,by = 0.01)

beta <- c(-53.75,-3.10,-7.89,9.44,0.18,-5.25,-6.70,-1.12,13.22,8.42,1.07,0.65,-0.66,-17.06,0.16,-16.66)

X <- c(1,0,0,6.11,36,0,0,0.534,0.14748,0.512,0.213,0.310,0.280,0.91,46.89,0.459)

v.y <- sapply(1:length(x), function(i)  y = t(X)%*%beta-33.10*x[i]+9.87*x[i]^2-0.63*x[i]^3)

plot(x,v.y,type='l',xlim = c(-2,14),ylim <- c(-30,50),main= "png(type=Xlib)",xlab = 'Alliance intensity',ylab='Parent Performance',axes=F,lty = 2)

axis(2,seq(-30,50,by = 10),label =c ('-30','-20','-10','','10','20','30','40','50'),pos=0)

par (las =2)

axis(1,seq(-1,10,by=1),label = c('-1',' ','1','2','3','4','5','6','7','8','9','10'),pos=0)

beta <- c(-30,-1,-3,4.7,0.068,-1.783,-2.283,0.620,7.03,1.375,0.08,-1.612,-0.435,-1.689,0.053,-6.97)

X <- c(1,0,0,6.11,36,0,0,0.534,0.14748,0.512,0.213,0.310,0.280,0.91,46.89,0.459)

text(12,30,labels = paste("ROI as",'\n', "dependent variable"))

text(12,10,labels = paste("ROA as",'\n', "dependent variable"))

v.y <- sapply(1:length(x), function(i)  y = t(X)%*%beta-15.31*x[i]+4.57*x[i]^2-0.3*x[i]^3)

lines(x,v.y,lty = 1)
dev.off()
{%endhighlight %}

可以看到图片效果并没有很好，主要是出现锯齿化的问题(alias),同样的使用quatz也会有一样的问题，当使用 `cairo`时就会使用antialias,反锯齿化使得更加光滑.

![merge1][]

使用 `quartz`

![merge4][]

使用 `cairo`

![merge2][]

使用 `cairo-png`，这样的文档会比原来的大一点

![merge3][]

使用Cairo package 的,可以更上面的 `cairo-png`比较一下，发现这里使用的字体更加细致，坐标轴而言更加清晰，其实我是比较喜欢 `cairo pacakge` 这个的画图工具的。那么这个工具所使用的函数是

{% highlight r%}
library(Cairo)
Cairo(width = 640,height =480, file = "", type = "png", pointsize = 12,bg="transparent",canvas = "white",unit= px,dpi= "auto")
{% endhighlight %}

![merge5][]

怎么安装Cairo 这个package 呢，直接在R里面安装

{% highlight r%}
install.package("Cairo")
{% endhighlight %}
出现问题， 找不到cairo.h
{% highlight r%}
checking cairo.h presence... no
checking for cairo.h... no
configure: error: Cannot find cairo.h! Please install cairo
(http://www.cairographics.org/) and/or set
CAIRO_CFLAGS/LIBS correspondingly.
ERROR: configuration failed for package Cairo
{% endhighlight %}
解决方法, 下载lib-cairo2-dev

{% highlight bash%}
sudo apt-get install libcairo2-dev
{% endhighlight %}

还是发生错误，找不到xlib

{% highlight bash%}
xlib-backend.c:34:74: error: X11/Intrinsic.h: No such file or directory
xlib-backend.c: In function ‘Rcairo_init_xlib’:
xlib-backend.c:158: warning: implicit declaration of function
‘XrmUniqueQuark’
make: *** [xlib-backend.o] Error 1
ERROR: compilation failed for package ‘Cairo’
* Removing ‘/usr/local/lib/R/site-library/Cairo’
{% endhighlight %}
解决方式

{% highlight bash%}
sudo apt-get install libxt-dev
library(Cairo) #解决.
{% endhighlight %}
解决.




[png]: http://stat.ethz.ch/R-manual/R-devel/library/grDevices/html/png.html
[compare]: blog.yam.com/changshuwei/aritcle/35103064
[merge1]: {{BASE_PATH}}/images/merge1.png
[merge2]: {{BASE_PATH}}/images/merge2.png
[merge3]: {{BASE_PATH}}/images/merge3.png
[merge4]: {{BASE_PATH}}/images/merge4.png
[merge5]: {{BASE_PATH}}/images/merge5.png
[10tips]: http://blog.revolutionanalytics.com/2009/01/10-tips-for-making-your-r-graphics-look-their-best.html
