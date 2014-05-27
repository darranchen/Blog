---
layout: post
title: "Markdown study note2"
date: 2014-04-14 20:34:00
categories: Technology
tags: Jekyll Markdown
keywords: Jekyll, Markdown, Usage
---
    
介于上面把Markdown 的process 改为kramdown 之后，竟然让我上一个文档的格式几乎都跑掉了，真不能忍，还是要忍，很有可能是kramdown要求更加严格的格式，所以导致有一些格式跑掉了，于是开了一个新的档重新打，打完中间出现问题再找原因，边打边学

#强调

有时候需要强调一个东西的时候，需要用斜字体或者粗体字来表示，下面就介绍怎么使用这些可以用* 或者是_ 来表示
{% highlight Ruby %}
*斜字体*
_斜字体_
**粗体**
__粗体__
{% endhighlight%}
效果如\: *斜字体* _斜字体_**粗体**__粗体__

---------------------------

#分隔行

分割行能让文章看起来更加清晰，如上面加了个分隔行表示两边不一样分隔行记得两边要留空白。

#数学符号

正是因为数学符号，所以就需要使用kramdown 来做Markdown 的process. 严格很多,这里是参照 [yanping][yanping] 想要用kramdown 需要先安装:

```
gem install kramdown
```


安装后修改 `_config.yml` 中把markdown 修改为

```
markdown: kramdown
```

然后在`_layout/default.html` 的head 中间添加下面的代码

{% highlight HTML %}
<!-- mathjax config similar to math.stackexchange -->

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      processEscapes: true
    }
  });
</script>

<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code']
      }
    });
</script>

<script type="text/x-mathjax-config">
    MathJax.Hub.Queue(function() {
        var all = MathJax.Hub.getAllJax(), i;
        for(i=0; i < all.length; i += 1) {
            all[i].SourceElement().parentNode.className += ' has-jax';
        }
    });
</script>

<script type="text/javascript"
   src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

{%endhighlight %}

## Vim highlight for Mardown

从 [vim-Markdown][Markdown] 下载 `markdown-1.2.2.vba.gz` 直接用vim 打开之后用 `:so %` 来解压即可

[yanping]: http://yanping.me/cn/blog/2012/03/10/octopress-with-latex/
[Markdown]: http://www.vim.org/scripts/script.php?script_id=2882 "Markdown syntax highlight"
