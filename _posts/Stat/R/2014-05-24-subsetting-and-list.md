---
layout: post
title: "R note: Subsetting and list"
date: 2014-05-24
categories: Statistics
tags: 统计 R 
keywords: 统计 R subsetting list
---

前面一个章节介绍了R的[基本资料类型][R-data], 这里将要学习怎么取子集以及重新介绍一下list的这种类型, 一开始可能会觉得list比较陌生，不知道该怎么用，像在上一章将的，list只是简单的从只能固定的一个基本元素的向量扩展到可以包含不同基本数据元素的特殊向量么，我请教了一下我的R软件大神同学张惇皓，他给list这个数据类型很高的评价"这是一个R 上面一个伟大的发明"。他觉得 data.frame 只是list的一个特例而已，list可以用来包括所有的数据类型,把不同长度的向量合并在一起。而data.frame 只是包括了不同的向量， 每一个向量的长度是固定的，而list中向量长度不需要一样, 这个概念很像在学C语言里面的结构体(class), 但是这个使用起来更加直接，不需要复杂的定义，而像定义一个向量一样定义。 我们在使用函数的时候就可以使用list返回一个list, 里面包括了所有我们想要关心的量。

讲了list 这么多好话，那么怎么去使用list，怎么取list 里面的元素呢，在R 里面取元素的方法有三种

* 使用中括号 [], 这个比较适合在matrix, vector等，其实也可以用于list。
* 使用双中括号[[ ]]， 这个是在特别用于list的。
* 使用\$ 加上名字，取出对应这个名字下面的元素

第一种方法在前面已经有提到了，下面简单提一下

{% highlight r %}
x <- c(1,2,3,4)
x[1]  # 直接加上一个数字
u <- (x>3) # 产生一个logical 的向量
x[u] # 取值的时候可以根据一个logical 的向量取值
4

# matrix
> x <- matrix(c(1:6), 3,2)
> x[1,2] 
[1] 4 # 这是一个向量
> x[1,2, drop = FALSE]
     [,1]
[1,] 4    # 返回一个矩阵而不是一个向量

{% endhighlight %}


下面详细介绍一下对于list怎么取子集

{% highlight r %}
> x <- list(foo= c(1:4), bar =0.6,baz = "hello")
> x[c(1,3)] # 单中括号其实跟vector 和matrix的功能一样，就是取出第1，第3个元素
$foo
[1] 1 2 3 4
$baz
"hello"
> x[[1]] # 双中括号也能返回元素，但是返回的是一个整数的向量
[1] 1 2 3 4 
> class(x[[1]])
[1] "integer"
> class(x[1]) #返回第一个list
[1] "list"
# 那么怎么取地一个向量元素里面的第n个单元呢
> x[[c(1,3]] # 取得第一个向量的第三个元素
[1]  3
> x[[c(1,2:3)]] # 这样取值会出错
> x[[1]][2:3] # 这样就不会发生错误
[1] 2 3

> x$bar #可以返回名字为bar的元素
> x[["bar"]] # 也是一种方式
> x["bar"]]
[1] 0.6
> name <- "bar"  # $ 的操作不允许运算
> x$name #NULL

# 不完整引用
> x$f
[1] 1 2 3 4
> x[["f"]] 
NULL
> x[["f", exact =FALSE]]

{% endhighlight %}


那么最后怎么去消去Na呢
{% highlight r%}
x <- c(1,2,3, NA,NA,6)
x[!is.na(x)] #可以去除一个行的na值，如果是两行呢
y <- c("a","b", NA, NA, "e","f")
good <- complete.cases(x,y)
good
T T F F F T
x[good]
1 2 6
### for data.frame
z <- c(1:6)
data <- data.frame(x,y,z)
print(data)
   x   y   z
1  1   a   1
2  2   b   2
3  3   NA  3
4  NA  NA  4
5  NA  e   5
6  6   f   6
good <- complete.case(data) # 只返回有complete 的row 的logical值
print(good)
T T F F F T
> data[good,]
   x   y   z
1  1   a   1
2  2   b   2
6  6   f   6

{% endhighlight %}


[R-data]: http://blog.xjchen.net/statistics/2014/05/20/R-datatype/
