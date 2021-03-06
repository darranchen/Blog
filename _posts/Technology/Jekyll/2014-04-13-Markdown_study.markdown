---
layout: post
title: "Markdown 总结"
date: 2014-04-13 21:00:00
categories: Technology
tags: Jekyll Markdown
keywords: Jekyll, Markdown, Usage
---
    
这里总结一下Markdown 用来写文章的用法,包括了:

1. 标题: 文件抬头应该怎么写
2. 段落: 
3. 序号: 
4. 强调:
5. 代码:
6. 数学符号:
7. 表格: 
8. 图片:
9. HTML:


# 1.标题

写的时候需要做标题,这个时候其实Markdown 有两种方式可以做标题的,第一种是加上#号，如下面所述

{%highlight Ruby%}
#
##
#####
{%endhighlight %}

几个#号代表第几层标题,也可在一段结束之后加上可以有多层标题

另一种方式就是直接在文本下面加上\_

{%highlight Ruby%}

---
layout: post
title: "Markdown 总结"
date: 2017-08-03 21:00:00
categories: Technology
tags: Jekyll Markdown
keywords: Jekyll, Markdown, Usage
---
{%endhighlight %}


# 2.段落

段落，也就是编写一整段的文字，只要空一行以上就是一段了(包括了里面有tab的情况)

# 3.序号(列表)

这里列表的功能比较像Latex中的enumerate和itemize的功能，写上列表之后会让文章看起来更加有条理性，有两种列表，无序列表和有序列表
可以使用\*,\+,\- 来表示比如下面的代码， 注意如果使用\{\% highlight Ruby \%\}会使内嵌结果出现问题，所以只能是使用空四格，而且最好是在列表标识符后面补充空到4格那么才不容易发生错误。
{% highlight Ruby %}
*   line one 
    continuous
  
    *   nest 

            nest code

    *   sub item2
*   item 2
{% endhighlight %}

*   line one 
    continuous
  
    *   nest 

            nest code

    *   sub item2
*   item 2

有序的也可以用1234来表示

{% highlight Ruby %}
1. 第一
2. 第二
4. 第三
{% endhighlight %}

1. 第一
2. 第二
4. 第三

# 4.强调:

强调可以通过星号或者下划线来表示， 

{% highlight Ruby %}
*a* ==> <em> a <em>
**a** ==> <strong> a <strong>
{% endhighlight %}

*a* 

**a**

# 5.代码

使用\{\% highlight Ruby \%\} 代码 \{\% endhighlight \%\}
这样就可以达到效果了

还有一种方式:

{% highlight Ruby %}
```javascript
var canvas = document.getElementByld("canvas")
```
{% endhighlight %}

```javascript
var canvas = document.getElementByld("canvas")
```

也可以用\`\`表示行内代码


{% highlight Ruby %}
这是`javascript`代码

{% endhighlight %}


# 6.数学符号

使用下面就可以表示了

{% highlight Ruby %}

$$a^2+b^2=c^2$$

$$a^2+b^2=c^2$$

{% endhighlight %}

# 7.注释

使用\> 就可以当引用或者注释了

{% highlight Ruby %}

> 这是一段引用
>> 这是二级引用
>>> 这是三级引用


{% endhighlight %}


> 这是一段引用


# 8.图片

{% highlight Ruby %}

 ![

{% endhighlight %}


[othree]: http://markwodn.tw/#p

