---
layout: post
title: R 读取资料
date: 2014-05-28 00:46:00
categories: Statistics
tags: R
keywords: 统计 R readtable
---

之前已经介绍过，学会使用一个语言基本上了解4样东西，资料类型，控制语句，输入输出，特有函数，这里再加上绘图功能，因为R的绘图功能已经强大到某种程度了，前面介绍了 

* [R 的基本资料类型][data] 
* [R 中list的使用方式][list]
* [R 中图片档案存储方式以及Cairo的安装][Cair]

这里将要介绍的是R 里面资料的读取方式,这里是参照 [coursera R Programming][R_pro] 上的介绍方式,先总结一下R 里面读取资料的方式以及对应的写入方式，之后再详述经常会使用的 `read.table` 和`read.csv` 的参数. 一般而言R 读取资料的方式如下

---

* [read.table][] , read.csv 这个使用来读取我们表格的资料的，与其对应的是 [write.table][]

---

* [readLines][]  这是将一个text 档案全部读入或者读入某一行为character类型的资料。其实目前还没有用到这个功能的样子

     {%highlight r%}
     cat("TITLE extra line", "2 3 5 7", "", "11 13 17", file = "ex.data",
                 sep = "\n") # 创建一个ex.data 的text 档案
     a <- readLines("ex.data", n = -1) # 把text 档案读为一个character 的向量,n=-1 表示将所有的资料读入
     class(a)  # character
     a <- readLines("ex.data",n=1) # 读入第一行
     print(a) # TITLE extra line
    {% endhighlight %}

---

* [source][] 这个函数就是用来载入已经写好的R function,这个函数其实就经常用到了.  与之对应的是 [dump][],dump的功能有点像matlab的save函数一样，保存一个r object, 下次需要使用到了再用source 读入.
    
     {%highlight bash%}
     vim hello_world.r # 创建一个hello_world.r 的文档
     hello.world <- function(){print("Hello World")} # 编写以下内容。
    {% endhighlight %}

在R 里面将这个函数读入，然后执行这个函数

     {%highlight bash%}
     source("hello_world.r")
     hello.world()  #记得要加入括号
     [1] "Hello World"
     x <- rnorm(10)
    y <- rnorm(10)
    xy <- list(x,y)
    dump(c("x","y","xy"),file ="dumpdata.r") #把 x,y xy保存在dumpdata.r中
    rm (xy) # 移除 xy，现在输入xy 也找不到了，
    source("dumpdata.r") #可以重新读入了
    {% endhighlight %}

* [dget][] 和source 近似，但是他是专门用来读取 [dput][]的object的，dput和dump的差别就是dput 将object 存为ASCII text， 这样原来object的名字并没有保留下来，虽然读取速度会快一些，但是感觉还是比dump不方便一点。


     {%highlight bash%}
     x <- c(1,2)
     y <- 2
     xy = list(x,y)
     dump("x,xy",file = "x.r") # 保留xy的名称
     rm(x) 
     source("x.r") # 依然返回x,xy
     dput(c(x,xy),file= "dputx.r") #
     a <- dget("dputx.r") # 会出问题，a就被当作一个list,其中原来的x成为了分开的两个element.不好,失去原本的资料
    {% endhighlight %}

---

* [load][] 前面提到的是保留某个object,如果我们需要保留当前的workspace,就要用save, 然后将保存好的目录load进来.

     {%highlight bash%}
    save(list = ls(all = TRUE), file= "all.RData")
    rm(list =ls())
    load("all.RData") #重新载入回来
    con <- url("http://some.where.net/R/data/example.rda") # 建立资料的链接端口
    ## print the value to see what objects were created.
    print(load(con))
    {% endhighlight %}

* [unserialize][] 将读入binary 的资料，与之对应的是 serialize,这个其实是可以作为底层交互时候使用的。

     {%highlight bash%}
     x <- serialize(list(1,2,3), NULL)
     prinf(x)
     [1] 58 0a ... #保存为2进制资料了
     con = file("dput.r","wr") 创建一个链接
     {% endhighlight %}


[data]: http://blog.xjchen.net/statistics/2014/05/20/R-datatype/
[list]: http://blog.xjchen.net/statistics/2014/05/24/subsetting-and-list/
[Cair]: http://blog.xjchen.net/statistics/2014/05/25/Install-Cairo/
[R_pro]: https://class.coursera.org/rprog-002/lecture
[read.table]: http://stat.ethz.ch/R-manual/R-devel/library/utils/html/read.table.html
[write.table]: http://stat.ethz.ch/R-manual/R-devel/library/utils/html/write.table.html
[readLines]: http://stat.ethz.ch/R-manual/R-devel/library/base/html/readLines.html
[source]: http://stat.ethz.ch/R-manual/R-patched/library/base/html/source.html
[dump]: http://stat.ethz.ch/R-manual/R-patched/library/base/html/dump.html
[dget]: http://stat.ethz.ch/R-manual/R-devel/library/base/html/dput.html
[dput]: http://stat.ethz.ch/R-manual/R-devel/library/base/html/dput.html
[load]: http://stat.ethz.ch/R-manual/R-devel/library/base/html/load.html
[unserialize]: http://stat.ethz.ch/R-manual/R-devel/library/base/html/serialize.html
