---
layout: post
title: "R 的基本资料类型"
date: 2014-05-20 
categories: Statistics
tags: 统计  R  
keywords: 统计 R Datatype 学习
---

虽然使用R已经两年了，对于R 有基本的了解以及使用，但是接近毕业了还是把之前学习R 的书面笔记整理到博客上面来吧，方便以后需要使用再来看。R 其实是一个统计工具，背后的统计方法的思想比起了解它的语法更加重要，统计工具其实有SPSS, SAS 等，但是R 的开源性让它变得越来越强大，很多功能都可以在上面实现，它对我来说慢慢从一个统计的工具变成了画图工具，甚至于曾经用R 把老师上课用的PDF 当作图片档案切半，方便上课使用。这里其实是想先从R 的基本使用方式入手，之后慢慢增加上我可能会用到的统计方法的程式，比如写一个画方程的形状的R code. 保留下来之后就不需要每一次都去写一次而可以直接用了。

这里再总结一下学统计或者数学出身的人与学计算机出身的人在学习R 的时候一个完全不同的方式， 对于R 的学习，我们班本科学统计或者数学的同学，乃至老师，在介绍R的时候完全是从方法入手，比如在多元分析这门课的时候，老师就是介绍因子分析(FA)或者主成分(PCA)分析的时候就是介绍R 上面有这样的package可以实现这样的功能，然后就是根据这个package的函数需要什么参数就去使用，而学习过C语言之类的，基本学习一个新的语言的方式就会像我的学习方式一样，从基本的数据类型学习起。学习一种新的语言其实主要了解几个关键的地方就可以开始使用了：

1. 资料类型， 也就是这个语言支持什么样的资料形态，怎么去定义这些资料
2. 判断语句和迴圈,也就是控制语句。
3. 语言自己特有的函数， 如R 里面的sapply，apply
4. input 和output 这是最近觉得很关键的，因为怎么去读取资料和输出资料能够让自己的工作效率提高很多。

这里就先从资料类型出发了。其实可以想象计算机语言设计出来就是用来帮助我们处理现实生活中的问题的。在现实生活中遇到的资料类型都会在R 上面有一定方式的呈现。所以在学习R的时候就可以跟实际需求联系在一起。所以先把R 上面有的数据类型在下面总结一下，它们都可以跟现实生活联系的。

* 基本元素(atomic classes): numeric, logical, character, integer, complex
* vectors, list
* factors
* missing values
* data frames
* names

## 基本元素

现实中需要遇到的基本元素有那些，是基本元素，也就是由这些组成的,那么就会有最简单的字符character, 就是a-z的字符，数学上面的逻辑符号logical (True(T) or False(F)) 小时候开始学习数学遇到的整数(0,1,2,3...), 接下来就把整数扩展为实数(numeric), 实数又扩展到复数。 有了这些基本元素才能实现组合成各式各样的数学和文字。

### character 

从字符开始入手，简单从字面上意思就是a-z这样的字母，给x赋值如下

{% highlight r%}
x <- "a" # character
{% endhighlight %}

在C 语言里面是会把字符和字符串分得很清楚，但是R 其实是一个统计工具，所以对于字符串的处理并不会像C 这么丰富，所以在这里，很多时候会把一整个字符串当做是一个character，这是和C 不一样的地方，可以接受。

{%highlight r%}
> x <- "test" #character
> x[1] 
[1] "test" # 在C中就会只有一个t
> x <- c("t","e","s","t") #这样就能做字符串处理了，虽然很笨。
> x[1] 
[1] "t"
{% endhighlight %}

这里还需要讨论一下R上的赋值功能 `<-` 和 `=` 的区别， 这两个方法都是赋值功能，但是 `=` 只允许在运算的最高级别使用，也就是在赋值在第一行就需要使用了，比如 `m <- matrix(c(1,1),nrow = 2)` 这样就不是赋值功能，而是传参数，如果是使用` m <- matrix(c(1,1), nrow <-2)` 就是将2 赋给nrow 这个量。这样的设置可以把传参数和赋值的功能分开来。 当我们需要在程式中间进行赋值功能时就需要 `<-`.

将其他类型的转化为character 和判断是否为character类型的

{% highlight r%}
> x <- 2
> y <- as.character(x)
> is.character(y)
[1] TRUE
> x <- "a"
> object.size(x) # 字符大小为64bytes
64 bytes
> x.cha <- "testtesttesttesttesttesttest"
> object.size(x) # 字符大小为88
88 bytes #字符大小似乎为随着字符个数的增多而增多


{% endhighlight %}

### logical 

logical (T or F) 是用来做控制语句必不可少的，有时我们觉得它可以当作两个character "TRUE" 和" FALSE", 有时候又觉得它其实只是integer 中的 “0” 和“1” 而已，但是它对于数学运算有很重要的意义，所以在R中给logical 设定了特别的很多功能，比如可以相加


{% highlight r%}
> x <- (2==1)
> is.character(x)
[1] FALSE
> is.logical(x)
[1] TRUE
> x+1 # 可以加的意义很多，这样就可以计算有多少个元素成立
[1] 1 
> object.size(x) #字符大小为32
32 bytes

{% endhighlight %}

### integer, numeric and complex

在R 里面默认的数字输入是使用numeric 如果是想要输入integer型的话需要定义L，为什么要分别整数和numeric呢，因为整数需要的储存单元比较少，一般是32bit(4 bytes),而用numeric 需要的储存单元要64bit(8bytes),但是测试完之后发现numeric 也是使用32bit的，这似乎是一个比较有趣去讨论的问题. 在 [Hadley Wickham][hadley]里面有提到如果是一个向量增加了一个整数，那么增加4bytes, 如果增加一个numeric就增加8bytes, 增加一个复数是增加16bytes.那么为什么看到的object.size 会是32呢，因为在R中的数字不再只是单纯的存一个数字而已，还需要一些存其他元素。比如指针类的。



{% highlight r%}
x.int <- 1L # 整数的定义需要在后面加上L
object.size(x.int) #32bytes
x.int <- c(1L,2L) # 32bytes #一个int 只需要4byte,一开始是4bytes,后面的4bytes会留空
x.int <- c(1L,2L,3L) # 40bytes
x.int <- c(1L,2L,3L,4L) # 40bytes 占用留空的bytes
x.int <- integer() # 24bytes,这24bytes其实是用来存一些其他的东西，在hadley是说有40 但是这里只有24可能是一些指针修改了。
is.integer(x.int) # True
x.int <- integer()
object.size(x.int)

x.num <- 1 #默认为numeric
object.size(x.num) # 32byte 还是32byte.
is.numeric(x.num) #True
x.com <- 1+i 
object.size(x.com)# 40 bytes 因为complex就是16bytes

{% endhighlight %}

## vector ,list 和factor

学完基本元素之后就可以用这些元素组成想要的向量， 向量运算会让计算变得方便很多，


{% highlight r%}
#创建向量
x.vec <- c(1,2,3) # 可以用c()函数用来创建向量
x.vec <- vector("numeric",length=10)A #这样也可以创建向量
#定义一个不知道什么的向量，方便后面添加可以用
x.vec <- vector()
x[1] <- 3
x[6] <- 3
x.vec <- c(1:10) # 可以用：创造有序的数列
x.vec <- seq(from=1,to = 10, by= 1) #也可以用seq 产生
x.vec <- seq(from=1,to = 10, length=10) #也可以根据自己想要的长度来建立

x <- c(0.5,0.6) #numeric
object.size(x) #40
x <- c(TRUE,FLASE) #logical
object.size(x) #32 logical 其实跟integer一样的
x <- c("a","b") #character
object.size(x) # 96怪怪的字符大小
x <- 1:2 #integer 
object.size(x) #32
x <- c(1+0i,2+4i) #complex
object.size(x) #56

# 向量元素的取得
x.vec[5]  # 取得第5个元素,matlab 写多了经常使用（）其实在R里面意味着函数。
x.vec[x.vec == 5] # 中间可以使用逻辑判断语句来选择想要的元素，非常方便
x.vec[x.vec == c(3,6)] # 本来想让里面等于3，6的元素取出来的，但是发现没有元素,这个只会判x.vec会不会等于(3,6)这个向量 那么怎么去取得离散的值呢 使用%in%
x.vec[x.vec %in% c(3,6)] # 这个用起来非常方便，尤其是面对类别型资料时。

{% endhighlight %}

上面讲了对于每一个object除了数值之外还存了其他东西，因为对于每一个object都会有自己的attribute, 所以object会存了一个指向attribute 的指针。这些特有的属性可以让这些object操作起来更加方便。那么有什么样的attribute呢

* names, dimnames 
* dimensions (matrices, arrays)
* class 也就是是由什么基本元素组成的
* length 向量也有的，长度
* other user-defined attribute/metadata

对于向量来说，每个element 都可以给它一个名字这样找这个element 的时候会方便很多

{% highlight r%}
> x <- c(1+1i,2+2i)
> names(x) = c("first","second")
> x 
first second
1+1i  2+2i
> x["first"] # 取得名字为"first"的元素

{% endhighlight %}

另外向量其实区别于list这样object 主要是因为想要有规定它一定是要在同一个class,那么当不同的class 放在一起的时候会把向量转化成为同一个class的东西

{% highlight r%}
class(x) #"complex"
length(x) # 2
x <- c("a",2) # 转化为character
x <- c(2,TRUE) # 转化为numeric
x <- c("a",TRUE) # 转化为character "TRUE"

{% endhighlight %}

上面也讲了list 的存在是为了实现能在同一个向量里面存不同的class 这在现实中也有意义，有时候我们的output 可能包括数字和字符，这样就可以存在list中输出了。

{% highlight r%}
x.list <- list("a",2,TRUE)
names(x.list) <-  c("char","num","log") #这样显示list比较清晰。
length(x.list) #3 
{% endhighlight %}

### factor

factor 是一种比较特殊的类型，它是专门用来处理类别型资料的(categorical),包括有序型和无序型， 为什么也要用到它呢，主要有两个原因，第一是因为用factor 会比直接使用整数1，2表示更加有意义， 比如使用"Male" 和"Female" 会比使用1，2 更有意义。第二个原因是有些特殊的函数可以用来专门处理这种类型的资料，比如做回归的时候的lm() 如果是factor, 它自动会使用一个baseline,然后使用dummy variable 来做回归。这个就很有用。这里需要先将一下有序的类别型资料，有序的类别型资料比如我们看的衣服的尺寸，有"small","medium","big",在R中将这种有序型的资料,在使用factor 的时候先介绍一下Order的使用


{%highlight r%}
> x <- c("small","medium","big")
> is.character(x)
[1] TRUE
> is.vector(x)
[1] TRUE
> x <- ordered(x)
> print(x)
[1] small medium big
levels: big < medium < small # 默认的定义是这样定义的,当我们想要根据自己的喜好改变level的时候可以
> x <- ordered(x,levels = c("small","medium","big)
> print(x)
[1] small medium big
levels: small < medium < big # 这样做定义不单单有实际上面的意义，而且在做回归的时候可以定义谁是baseline
{% endhighlight %}

这些资料都是来源于R 的factor 说明档 [R:factor][factor]

{%highlight r%}
# usage
> x.fac <- factor(x.cha, levels, labels = levels,exclude = NA, ordered = is.ordered(x),nmax= NA)
> x.fac <- facror(x.cha, exclude = "small")
[1] <NA> big medium
levels: big, medium
{% endhighlight %}

* x.cha 要求输入的是cha类型的，
* levels 的功能和上面ordered 的一样，是用来定义baseline的
* labels 可以用来给不同的levels 取别名
* exclude 是指需要被删除在外的值
* ordered 来决定是否需要有ordered
* nmax 是值最多的类别个数



## missing value

讲完vector 之后有时候现实得到的资料里面有一些值是缺失的，但是又想保留它的位置，那么怎么办呢。R里面有了NA来表示


{% highlight r%}
x <- c(1,2,NA,3) # 这样就能定义一个长度为4的向量了，其中一个元素缺失
length(x) # 4
sum(x) # NA 缺失值加总也会缺失.
{% endhighlight %}

定义完NA 之后发现其实在R里面还有一个NAN的元素，那么这个NAN实际作用是什么呢，NAN 可以理解为Not A Number 也就是数学计算错误遗留下来的，NAN又属于NA的一种，但是NA不属于NAN

{% highlight r%}
x <- 0/0 # x 为NAN
y <- NA
is.na(X) # TRUE
is.nan(y) # FALSE
{% endhighlight %}

## matrix, array and data.frame

接下来介绍比较高维度的资料存储方式

### matrix

有了向量自然少不了矩阵就是将一维的向量影视到2维的矩阵上。

{% highlight r%}
> x.vec <- c(1:4)
> x.mat <- matrix(x.vec, nrow = 2,ncol = 2) # 默认是按column 来存的
> x.mat
1 3
2 4
> x.mat <- matrix(x.vec, nrow =2, ncol =2, byrow = T) #这样就可以按照row 来存了。
> x.mat
1 2
3 4
> x.mat <- matrix(,nrow =2,nrow=2) # 空的矩阵
> x.mat
NA NA
NA NA
> x.vec <- c("a","b",1,2)
> x.mat <- matrix(x.vec,nrow =2) # matrix 和vector 的意义一样，就是所有的元素都是需要同一个class 即可
> x.mat
"a" 1
"b" 2
{% endhighlight %}

介绍完基本定义方法之后需要讨论matrix 的attribute，有特别的dimension 属性和dimnames

{% highlight r%}
dim(x.mat) # 2,2 
rnames <- c("R1","R2")
cnames <- c("C1","C2")
dimnames(x.mat) <- list(rnames, cnames) # 这里一定要用list 规定的。用c 会报错
x.mat <- matrix(x.vec,nrow= 2, dimnames= list(rnames,cnames))# 这种方式也可以给名字
{% endhighlight %}

之前在Matlab 中了解到matrix的资料存储是一行col一行col 存的，也就是直接从col 开始取数会快一些，不需要使用指针跳到下一列去找，直接下一个元素就可以，这里在R中也测试一下

{%highlight r%}
ptm <- proc.time()
{% endhighlight %}

### array

刚才就在想既然有二维的，那肯定要有3维的资料啊， 3维这里就用array，基本定义方式是：


{%highlight r%}
x.array <- array(data_vector, dim_vector)
x.array <- array(c(1:24),dim=c(3,4,2))
{% endhighlight %}

### data.frame

终于讲到最后的data.frame了，像之前list 与matrix 的关系一样，data.frame 就像实际上面的表格资料一样，可以有连续型资料，类别型资料，合并在一起,在这里，每一个column 都会被当作一个list,长度为row 的个数，不够会自动补全。一般都会是有read.table()或者read.csv()来决定的.


{%highlight r%}
a <- c(1,2,3)
b <- c("a","b","c")
x.frame <- data.frame(a,b)
is.list(x.frame$b)
TURE
x.mat <- data.matrix(x.frame) #这个是使所有的资料变成数字，
print(x.mat)
  a b
1 1 1
2 2 2
3 3 3
x.mat <- as.matrix(x.frame)
print(x.mat)
  a    b
1 "1" "a"
2 "2" "b"
3 "3" "c"
{% endhighlight %}

[Rblog]: http://www.r-bloggers.com "R-bloggers"
[hadley]: http://adv-r.had.co.nz/memory.html
[factor]: http://stat.ethz.ch/R-manual/R-devel/library/base/html/factor.html
[sim]: http://www.r-bloggers.com/simpsons-paradox-is-back/
