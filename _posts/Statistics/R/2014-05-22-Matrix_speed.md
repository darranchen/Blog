---
layout: post
title: "矩阵的取值方式对于速度的影响"
date: 2014-05-22 23:34:00
categories: Statistics
tags: 统计 R Matrix
keywords:  统计 R Matrix
---

对于R里面可以根据对于矩阵取值的时候可以根据row 来取值，也可以根据column来取值，也就是先固定一个column 来取值还是固定一个row来取值，但是不同的取值方式会对速度造成影响，因为我们在存资料的时候是先根据列来存资料，所以先固定列取资料这样子在寻址的时候下一个地址就是想要的元素了， 如果先固定row取值，那么取值的时候就会跳到下一列的同一个row,这样造成计算速度变慢，下面可以做一个模拟来看这件事，我们固定列对于前i行求和以及固定行对于前j列求和，得到的时间散布图，模拟的代码如下：

{% highlight r%}

x.mat <- matrix(rep(1,(5000*5000)), nrow = 5000)
times <- seq(500,5000,by =500)
time1 <- vector()
time2 <- vector()
sum.x <- 0 
ptm <- proc.time()
for (k in 1:length(times))
{
    for (i in 1:times[k])
    {
        for (j in 1:times[k])
        {
            sum.x = sum(x.mat[i,1:j]) # 固定行取列的值
        }
    }
    time1[k] <- proc.time() -ptm
    ptm <- proc.time()
}
sum.x <- 0 
ptm <- proc.time()
for (k in 1:length(times))
{
    for (i in 1:times[k])
    {
        for (j in 1:times[k])
        {
            sum.x = sum(x.mat[1:i,j]) # 固定列取行的值
        }
    }
    time2[k] <- proc.time() -ptm
    ptm <- proc.time()
}
png("matrix_time.png")
plot(times,time1, col = "blue",xlab = "times",ylab = "时间",lty = 1,lwd = 2,type= "l") #画图，固定行取列的值
lines(times, time2, col = "red",lty = 2,lwd = 2) #固定列取行的值
legend("topleft", legend=c("Sum by column","Sum by row"),col = c("blue","red"),lty=c(1,2),lwd = c(2,2))
dev.off()

{% endhighlight %}

得到的效果图如下所示,红色的线是固定列然后取行值的，可以随着次数的增多，他们之间计算的时间越差越大，那么怎么去处理这个问题呢？

![时间][matrix]

处理这个问题的方式可能可以从两方面，如果一定是要固定行，对于列取值，那么可以做转置， 一个是直接对于矩阵做转置，另一个是重新定义一个矩阵，这样地址编排的方式就可以发生改变了。

[Rblog]: http://www.r-bloggers.com "R-bloggers"
[sim]: http://www.r-bloggers.com/simpsons-paradox-is-back/
[matrix]: {{BASE_PATH}}/images/matrix_time.png
