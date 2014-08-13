---
layout: post
title: "R 特有函数loop-function-apply"
date: 2014-05-30 13:45:00
categories: Statistics
tags: R 
keywords: R control_statement
---

目前R的基本使用系列文章已经把R 的基本语法介绍完了，接下来就需要学习R上特有的函数，这才是R作为一个统计分析软件的精髓所在，这里将会介绍特有的loop function

* [R 的基本资料类型][data] 
* [R 中list的使用方式][list]
* [R 中图片档案存储方式以及Cairo的安装][Cair]
* [R 中读取资料][read]
* [R 控制语句与逻辑运算符][control]
* [R 函数的编写][function]

为什么会有loop function这东西呢，其实我打算先从lapply开始讲起，之前介绍过对于R来说，list是一个很特别的存在，他可以处理长度不同的向量合并在一起， 但是当我们想要对每一列取值的时候，不能直接应用已经写好的mean() 这个函数，而是每一次都要写一个for-loop 循环，这样非常麻烦，代码写起来也很复杂,那么怎么办呢。于是就开发除了loop function取代了一长串的代码，这样就能允许mean()这个函数接受像list这种类型资料的输入了。 特别用来做list这种类型资料的loop-function 有lapply 和sapply

## lapply 和sapply

这两个接受的输入都是list,即使输入的不是list，他们也会自动转为list，因为list 的便利性，所以几乎什么输入都可以接受。唯一不同的是sapply的输入是比 [lapply][] 的输出更加简单一些，lapply的输出都是一个list, 这样有时候我们是针对一个类似与矩阵的data.frame取mean,那么希望是输出一个mean 的向量，但是输出了一个每一个元素都是长度为1的向量的list, 这样不是很符合直观，而sapply对于相同的list 处理完之后，考虑直观性的问题，输出方式就会如下：

1. 如果输出list是一个每一个元素都是长度为1的向量,那么就直接输出一个向量
2. 如果输出list是一个每一个元素都是长度相同(大于1)的向量,那么就输出一个矩阵.3. 如果输出是其他的，那么就输出一个list.

基本上lapply 和sapply 的使用方式如下

{% highlight r %}

lapply(X,FUN,...) # x 是输入，如果不是一个list也会自动转为一个list, Fun是要执行的function，x每一列都会作为一个输入，...是作为function所需要的参数的输入

x <- list(rnorm(3),rnorm(4),rnorm(5))
mean.x <- lapply(x, mean)
print(mean.x) 输出是一个list
[[1]]
[1] -0.999
[[2]]
[1] -0.256
[[3]]
[1] 0.1909
mean.x <- sapply(x,mean)
print(mean.x) # 输出是一个向量
[1]  -0.999 -0.256 0.1909 
# 传参数到使用的function
lapply(x <- 1:3,runif,min=0,max = 10) #不想要默认的0-1
[[1]]
[1] 6.33
[[2]]
[1] 4.58  6.066
[[3]]
[1] 2.95  7.419 7.98
sapply(x <-1:3,runif,min=0,max = 10) # sapply 输出一个同样的list
[[1]]
[1] 6.33
[[2]]
[1] 4.58  6.066
[[3]]
[1] 2.95  7.419 7.98

{%endhighlight %}

loop-function还有一个非常重要的功能，可以使用未定义名称的function，这样可以这样可以节省可以命名空间

{% highlight r%}
x <- list(rnorm(3),rnorm(4),rnorm(5))
max.x <- lapply(x, function(x) {y <- max(x)
        y})
print(max.x)
[[1]]
[1] 0.518
[[2]]
[1] 0.943
[[3]]
[1] 1.131

{%endhighlight %}

## apply

既然对于list 都已经有loop-function,那么对于matrix 和array这种类型的，也可以使用loop-function [apply][]


{% highlight r%}
apply(X,MARGIN,FUN,...)
{%endhighlight %}
参数意义如下：

* X:  矩阵或者array

* MARGIN 决定对那个维度的资料做 1 比较对行做，2 表示对列做，c(1,2) 表示保存行和列，对第三个维度做.

* FUN  所要执行的函数

* ... 对与FUN 的可选参数


{% highlight r%}
x.mat <- matrix(1:4,nrow=2,byrow = T)
print(x.mat)
    [,1] [,2]
[1,]  1    2
[2,]  3    4
sum.row <- apply(x.mat,1,sum)
print(sum.row)
[1] 3 7
sum.column <- apply(x.mat,2,sum)
print(sum.column)
[2] 4 6
x.array <- array(1:8,dim = c(2,2,2))
prinf(x.array)
, , 1
     [,1]  [,2]
[1,]    1     3
[2,]    2     4
, , 2
     [,1]  [,2]
[1,]    5     7
[2,]    6     8
sum.1 <- apply(x.array,1,sum)
prinf(sum.1)  # 对于1求和起来,将row 但做一个独立的vector 输入进去
[1] 16 20
sum.2 <- apply(x.array,2,sum) 
prinf(sum.2)  # 对于2求和，将第二维度的当作一个独立的向量输入进去，这里第二维度的向量为， c(1,2,5,6)和c(3,4,7,8)
[1] 14 22 
sum.3 <- apply(x.array,3,sum)
prinf(sum.3)  # 对第三个维度求和
[1] 10,  26
sum.12 <- apply(x.array,c(1,2),sum)
prinf(sum.12) 对于维度1，2组成的元素当作一个向量输入，维度1,2组成的向量有c(1,5) c(3,7), c(2,6), c(4,8)
      [,1]  [,2]
[1,]    6     10
[2,]    8     12 
sum.13 <- apply(x.array,c(1,3),sum)
prinf(sum.13) 对于维度1，3组成的元素当作一个向量输入，维度1,3组成的向量有c(1,3) c(5,7), c(2,4), c(6,8)
      [,1]  [,2]
[1,]    4     10
[2,]    6     14 
sum.23 <- apply(x.array,c(2,3),sum)
prinf(sum.23) 对于维度2，3组成的元素当作一个向量输入，维度2,3组成的向量有c(1,2) c(3,4), c(5,6), c(7,8)
      [,1]  [,2]
[1,]    3     11
[2,]    7     15 
{% endhighlight %}

## tapply

上面的lapply 和apply 是针对list和array这些类型的，这里tapply 这个方法就真的是针对统计上面的资料，也就是我们在做crosstable analysis的时候，table里面会有类别型的资料，如会有性别资料，如果我们需要对不同性别的人求分数和，那么就是 [tapply][] 了。


{% highlight r%}
tapply(X, INDEX, FUN = NULL, ..., simplify = TRUE)
x <- data.frame(score = 1:4,gender = c(F,F,T,T))
aver.score <- tapply(x$score, x$gender,mean)  # INDEX 是需要和X 等长的向量

{%endhighlight %}

## split

这里既然介绍了tapply, 那么顺便介绍另一个有用的function, [split][] 就是我们可以根据我们想要的不同level的factor 将一个list 或者data.frame 分成几组,它经常与lapply 或者sapply一起使用

{% highlight r%}
split(x,f)
x <- c(rnorm(10,runif(10,min=1,max=10),rnorm(10,mean=10))
f <- gl(3,10) # 产生一个长度为30的,各个不同level为10的向量
split.x <- split(x,f) # 把x根据f分类分出来
x.mean <- sapply(split.x,mean) 
print(x.mean)
[1] 0.618, 4.98 10.415
{% endhighlight %}

## mapply

上面是传一个参数的时候使用的，当我们需要的输入有多个参数怎么办呢，可以使用 [mapply][] , 在mapply的function的介绍的时候是这样描述的: mapply is a multivariate version of [sapply][lapply]


{% highlight r%}
mapply(FUN, ..., MoreArgs = NULL, SIMPLIFY = TRUE,
           USE.NAMES = TRUE)
{% endhighlight %}
其中的參數意義是，

* FUN 是指需要輸入的function, 這個和lapply 等不同，是寫在前面的.
* ... 是指需要輸入的參數個數
* MoreArgs 是指FUN所需要的其他參數
* SIMPLIFY 是指sapply 和tapply的區別
* USE.NAMES 是指我們輸入的參數可以用名字來辨別

{% highlight r%}
mapply(rep, times = 1:2, x= 2:1)
[[1]]
[1]  2
[[2]]
[1]  1 1 
mapply(rep, time = 1:2, MoreArgs = list(x=4))
[[1]]
[1]    4 4 
[[2]]
[1]    4 4
{% endhighlight %}


[data]: http://blog.xjchen.net/statistics/2014/05/20/R-datatype/
[list]: http://blog.xjchen.net/statistics/2014/05/24/subsetting-and-list/
[Cair]: http://blog.xjchen.net/statistics/2014/05/25/Install-Cairo/
[read]: http://blog.xjchen.net/statistics/2014/05/27/Read-and-write/
[control]: http://blog.xjchen.net/statistics/2014/05/29/control/
[function]: http://blog.xjchen.net/statistics/2014/05/30/function/
[lapply]: http://stat.ethz.ch/R-manual/R-devel/library/base/html/lapply.html
[apply]: http://stat.ethz.ch/R-manual/R-devel/library/base/html/apply.html
[tapply]: http://stat.ethz.ch/R-manual/R-devel/library/base/html/tapply.html
[mapply]: http://stat.ethz.ch/R-manual/R-devel/library/base/html/mapply.html
[split]: http://astrostatistics.psu.edu/su07/R/html/base/html/split.html
