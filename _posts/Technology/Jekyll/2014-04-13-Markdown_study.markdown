---
layout: post
title: "Markdown study note"
date: 2014-04-13 20:34:00
categories: Technology
tags: Jekyll Markdown
keywords: Jekyll, Markdown, Usage
---
    
# 博客

首先是博客的需求：

1. 比较好的文档结构编排。
2. 自动的文档内容编排，像Latex一样，根据自己学好的模板，然后编辑文档，自动生成格式。
3. 输入中文支持。文字编辑，比如像Latex 中的enumerate 或者item 还有加粗字体。
4. 数学符号支持，因为是希望写成类似技术贴的，所以会用到比较多数学符号，所以需要数学符号支持。
5. 插入图片支持。
6. 插入代码支持。

有了这些需求之后非常感谢清平大神推荐我使用Jekyll， 感觉Jekyll 能满足我大部分的需求啊。开始学习怎么编写Markdown.

# 标题2

阅读完 [a][othree] 的繁体说明档之后，对于Markdown 有基本的了解，Markdown 是一种纯编写的格式，尽可能简单易写，容易读懂就好，尽管和HTML有千丝万缕的关系，不过，我要Markdown 就是来写博客的，所以就不用考虑太多啦，直接开始学习了。不过还是试试在Markdown 中插入一个HTML的格式看看

    <table>
        <tr>
            <td>Foo</td>
        <tr>
    <table>

显示结果就跟下面一样，问题是上面这样写Markdown 文档可阅读性太差了，而且当作代码段还需要刻意空4格，太麻烦了，所以还是要用一下highlight会比较合适。

# 标题3


{%highlight Ruby%}
<table>
    <tr>
       <td>Foo</td>
    <tr>
<table>
{%endhighlight %}

# 标题4

其实需要的简单就是一个

# 段落

    段落，也就是编写一整段的文字，只要空一行以上就是一段了(包括了里面有tab的情况)

#标题

写的时候需要做标题,这个时候其实Markdown 有两种方式可以做标题的，但是我还是喜欢这种

{%highlight Ruby%}
#
##
#####
{%endhighlight %}

几个#号代表第几层标题,也可在一段结束之后加上可以有多层标题

# 列表

这里列表的功能比较像Latex中的enumerate和itemize的功能，写上列表之后会让文章看起来更加有条理性，有两种列表，无序列表和有序列表
可以使用\*,\+,\- 来表示比如下面的代码， 注意如果使用\{\% highlight Ruby \%\}会使内嵌结果出现问题，所以只能是使用空四格，而且最好是在列表标识符后面补充空到4格那么才不容易发生错误。
{% highlight Ruby %}

*   line one 
    continuous
  * nest 
    nest code

    *   sub item2
*   item 2
{% endhighlight %}

* line

    * nest 

            nest code

    *   sub item2

*   item 2

# 代码区块
如果要在一小段里面加入代码你，那么需要反引号(`` ` ``) 比如：

    use the `highlight` function
如果代码中有反引号，那么就可以使用多个反引号来包住，如

    `` There is a literal backtick(`) here.``

加入代码区块的话可以有两个方式，一种方式是加入`{\%highlight\%}` 另一种是空出几行。

# 图片区块
怎么加入图片呢
可以使用

    ![Alt text][id]
跟前面的链接一样，id是定义好的标志符，指向文档所在位置 ![图片][testimg]



* * *

$$a^2+b^2=c^2$$
$$
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}
$$

为什么这样子呢

[othree]: http://markwodn.tw/#p

