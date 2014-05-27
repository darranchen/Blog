---
layout: post
title: "Blog plugin"
titleline: "Disqus评论"
date: 2014-04-27 20:34:00
categories: Technology
tags: Jekyll Markdown
keywords: Jekyll, Markdown, Usage 
---

## Disqus

需要增加Disqus 评论功能，步骤如下:

* 到 [Disqus][] 的主页注册一个账户.
* 注册完成之后, 按下 ` Add Disqus to Your Site` 填写个人URL 比如xinjie
* 完成之后就会生成下面的代码

{% highlight HTML %}
<div id="disqus_thread"></div>
    <script type="text/javascript"> /* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
    var disqus_shortname = 'xinjie'; // required: replace example with your forum shortname
    /* * * DON'T EDIT BELOW THIS LINE * * */
    (function() 
    {
        var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
        dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
        (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
     })();
</script>
<noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
<a href="http://disqus.com" class="dsq-brlink">comments powered by <span class="logo-disqus">Disqus</span></a>

{% endhighlight %}


有很多语法高亮的插件

[wan]: {{BASE_PATH}}/images/yuming.png
[Disqus]: http://disqus.com/


