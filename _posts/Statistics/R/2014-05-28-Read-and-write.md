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

## read.table

介绍完前面的一般方式之后，下面介绍比较常用到的 [read.table][] 和 [write.table][], 读取之后的资料会自动保存为data frame, 读取资料的时候会将同一个列的资料当作一个变数来表示，这个跟在进行统计分析的时候想法比较一致， 他的基本使用方式如下

{%highlight bash%}
read.table(file, header = FALSE, sep = "", quote = "\"'",
           dec = ".", numerals = c("allow.loss", "warn.loss", "no.loss"),
           row.names, col.names, as.is = !stringsAsFactors,
           na.strings = "NA", colClasses = NA, nrows = -1,
           skip = 0, check.names = TRUE, fill = !blank.lines.skip,
           strip.white = FALSE, blank.lines.skip = TRUE,
           comment.char = "#",
           allowEscapes = FALSE, flush = FALSE,
           stringsAsFactors = default.stringsAsFactors(),
           fileEncoding = "", encoding = "unknown", text, skipNul = FALSE)

# ex
a1 <- c(1,2,3,4)
a2 <- c("'32,1',"2","3","4")
a <- data.frame(a1,a2)
write.table (a, "Writetable.txt",sep=",")
rm(a)
a <- read.table("Writetable.txt",sep =",")
{% endhighlight %}


其中各个参数的意义如下:

* file 自然而然就是文档名称了

* header 是指第一行为名称行

* sep="" 是指文档资料是以什么为间隔的，这里默认是空格为单元分隔符号 read.csv 就是默认为逗号( `,`)的读取格式

* quote 是引用的意思，他会自动将引用符号中间的单元当作是character,在网上看到一个文章讲了自己原来输入200000行，结果出来只有100000行，原因就是没有把 `quote= ""`
   
{%highlight bash%}
# 修改Writetable 有两行
1,2,3,4
'32,1',2,3,4 # 其中32,1被引用符号包起来，当作一个string

a <- read.table("Writetable.txt",sep =",")
print(a)
    V1 V2 V3 V4
'32 1' 2  3  4 # 因为如果是第一行的单元个数比所有的column数目少，那么就会默认设置header =T. 所以1, 2 ,3 ,4 就会设为header了。而‘32 也被当成了行的名称，如果要设置行名很简单，就先在第一个行里面加入比所有column数少一个的column的名称，之后将行名放在第一列里就可以了

a <- read.table("Writetable.txt",sep =",",quote = "'")
print(a)
    V1     V2 V3 V4
[1] 1      2  3  4
[2] '32,1' 2  3  4  #V1 这一列被当为factor
{% endhighlight %}

* dec="." 就是标志小数点的符号，默认为.

* numerials 就是当我们读取的文档里面是用double 存的数字, 读入时候可能会失去一定的数字位数，numerials就是来衡量可以损失的范围 allow.loss, warn.loss, no.loss

* row.names 就是行名
* col.names 列名

* as.is 在R 中默认会将charactor 的资料转为factor,这个可以将不想要转为factor的charactor 保留原有格式


{%highlight bash%}
a <- read.table("Writetable.txt",sep =",",quote = "'",as.is = T)
str(a)
$a1: int 1 2 3 4
$a2: chr "32.1","2","3","4"
a <- read.table("Writetable.txt",sep =",",quote = "'",as.is = T)
str(a)
$a1: int 1 2 3 4
$a2: Factor "32.1","2","3","4"
{% endhighlight %}

* na.string 如果是missing value 将要怎么去表示他

* colClasses 就是告诉R 所有的column 是什么类型的资料，注意，如果提前讲会让R的读取速度加快

* nrows 最大读取的行数，这样在当读取的表格超过系统的memory时比较有用.

* skip  默认跳出的行数的个数

* check.names 如果是TRUE ，就会检查name 的命名方式是否符合闺房.

* fill logical 如果是真的，那么就会默认允许所有行的个数不一致，自动填充确实的空格。

* strip.white  只有当sep 定义的时候才有用，不过不知道什么意思。

* blank.lines.skip logical 如果是真的的话，那么就直接跳过空白行

* comment.char character: 用来定义作为备注的行，比如这里默认是用\#,如果需要设置没有comment 则为""

* allowEscapes logical 允许使用C类型的分界符。

* flush logical

* stringsAsFactor True, 是否将string 转为factor, 默认是允许的，这个的权限比as.is 和colClasses小。

*  fileEncoding 

* encoding 就是文档的类型是什么编码方式，可以是UTF-8

* text 

* skitNul 

在读入大资料的时候需要注意目前R的memory 是否足够。需要了解的信息如下：

1. R 有多少memory可以使用。
2. 有什么applications 正在使用
3. 有多少其他使用者登录到这个系统中
4. 是使用什么OS lubuntu13.01
5. 是32位还是64位

## 与文档建立链接

有时候需要与文档或者网页进行链接，获得所需要的资料，有多次存取又不需要每次都去输入文档名字，就可以建立一个connection.

* file 与file 建立链接， 有三种方式
  * r 就是读取文档
  * w 就是写入文档
  * a appending 就是在文档后面写入

        con <- file("Writetable.txt","r") # 读取文档
        x <- readLines(con) # 从文档中读取

* bzfile 与gzip的文档建立链接

* gzfile 与bzip2 的文档建立链接

* url 与网页建立链接
        con <- url("http://blog.xjchen.net","r")

    


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

