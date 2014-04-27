---
layout: post
title: "Blog Setup"
date: 2014-04-14 20:34:00
categories: Technology
tags: Jekyll, Markdown
keywords: Jekyll, Markdown, Usage
---

终于需要架构我的个人博客了，根据以往经验，最快的方式就是直接把人家的模板带过来，边学习边看，在 [mojombo][mojombo] 推荐了很多模板，看了下，太多模板了，一时下不了手，所以果断先找自己觉得可以的，先入手，之后再慢慢调排版。看了[yansu][yansu] 的博客，关于目录结构的假设，觉得他的目录架构很清晰，想要用他的目录结构，但是他的主页有点复杂，东西太多了，然后又看到 [geeklu][geeklu] 的主页看起来非常简洁所以先利用yansu的把目录架构先架构起来，然后再决定好模板好了，另外看到有 [雁起平沙][yanping] 的关于R的博客，先收藏下，之后有机会再学习学习。

首先到 [github][github] 的主页上面注册一个帐号，然后创建一个库，名字叫username.github.io 接下来安装 ssh 和 git 与github建立起连接

{%highlight Ruby %}
sudo apt-get openssh-server
sudo apt-get git-core
{%endhighlight %}

安装完ssh 之后就需要在你自己的github主页里设置ssh密码了，这个密码是用验证的，具体步骤如下

* 检查是否已经安装ssh

{%highlight Ruby %}
cd ~/.ssh
ls -a
{%endhighlight %}

如果发现有`id_rsa.pub`的文件,那么就是已经安装了，如果记得密码就不用删除，不记得果断删除重装

* 接着产生一个新的SSH 密码

{%highlight Ruby %}
ssh-kengen -t rsa -C "your_email@example.com"
{%endhighlight %}

把新的密码加入到 ssh-agent中去

```
ssh-add ~/.ssh/id_rsa
```

* 最后ssh的密码加到GitHub中，直接把id_rsa.pub的东西复制到Github主页的ssh密码去，添加即可

{%highlight Ruby %}
sudo apt-get install xclip
xclip -sel clip < ~/.ssh/id_rsa.pub
{%endhighlight %}

完成之后就可以用git 跟github建立连接了，
创建一个文件夹和自己的github的名字一样也行，就`darranchen.github.com` 用git 把别人的文档拷贝下来了。拷贝之前到别人的github上把他的文件夹fork到自己的io库中，比如我拷贝了yansu的文档

```
git clone https://github.com/darranchen/suyan.github.io
```
拷贝完文档之后就要开始复制他的功能啦\~\~

#设定目录结构

他的目录结构

{% highlight Ruby %}
├── CNAME
├── README.md
├── _config.yml
├── _includes
│   ├── disqus.html
│   ├── footer.html
│   ├── googleanalytics.html
│   ├── header.html
│   └── navside.html
├── _layouts
│   ├── base.html
│   ├── book.html
│   ├── page.html
│   └── post.html
├── _posts
│   ├── Book
│   ├── Life
│   ├── Resource
│   ├── Technology
│   └── Tool
├── index.html
├── pages
│   ├── about.html
│   ├── archive.html
│   └── atom.xml
├── public
│   ├── css
│   ├── fonts
│   ├── img
│   ├── js
│   └── upload
└── sitemap.txt
{%endhighlight%}

## CNAME

从最外层的学起，他的CNAME 就是用来存域名的，如果不喜欢github的域名可以改成自己的域名，比如可以把内容改为

```
blog.xjchen.net
```

## README\.md

这个是一般的说明档，之后再慢慢补充说明了

## \_config.yml

这个是设置文档可以在里面定义自己喜欢的功能，目前先用着他的，之后再学习怎么修改

    permalink: /:year/:month/:day/:title.html
    paginate: 10
    pygments: true
    markdown: kramdown
    # 作者信息
    author: 
    name: 新杰
    email: zsdxchenxinjie@gmail.com
    link: http://blog.xjchen.org
    github: http://github.com/darranchen
    # 站点信息
    title: Darran's Blog 
    description: Darran's Blog
    url: http://blog.xj.org
    rss_url: /pages/atom.xml

    # gavatar头像及Favicon
    gavatar: /public/upload/gavatar/gavatar.png
    favicon: /public/upload/gavatar/favicon.ico

    # google analytics 设置
    ga:
      id: UA-50000128-1
      url: blog.xjchen.net

    # disqus 设置
    #disqus:
    #  shortname: xinjie-zh

    # 主题设置，自动激活某个标签
    #active: 技术

    # 首页除了最新文章外显示分类
    cates: 
    - Python
    - OpenStack

    # 中文本地化
    locals:
    tags: 标签
    about: 关于
    newest: 最新文章

就根据他的模板做稍微的改动，之后在慢慢学习，里面有一个google analysis的工具比较好玩，可以追踪你的网站观察情况。

* Include 文件夹

这个文件夹主要是用于添加各种HTML的添加功能的模板，就是当我们需要网页功能的时候把模板放在这里，就可以读到了。里面有几个文件，就是用来添加网页功能的，

* disques.html  是用来添加评论的。 评论功能比较喜欢 [geeklu][geeklu] 的.
* footer.html 是用来增加右上角的那个缩进功能的。以及旁边的分类的框架的
* header.html  用于编辑表头的。
* navside.html  使用来添加最左边的分类格的
* googleanalystic.html 是用来添加google 分析功能的。

* \_layout 这个用于设置主页的布局模式
* pages 站内固定页面
* public 公共的资源以及调用的图片都会放在这里。
* sitemap.txt 给搜索看如何爬取这个网站 不懂

# 建构自己网站

认真了解了yansu的网页框架之后，觉得想改一些东西还是需要对HTML有一定了解，最快捷的方式还是找一个自己喜欢的模板复制过来好了，参考了 [mojombo][mojombo]上面的模板之后，先选择了 [ever][ever]的，觉得很干净，但是还是不能满足我的需求，所以又用了 [Skydark][Skydark] 的模板,开始修改成我要的模板

* 首先需要修改\_config.yml 文档这个是我的个人文件，
* 然后修改about\.md 做自我的声明。其中学到一个技巧就是把图片档到放在images这个文件夹中，需要就是用下面的定义就能载到

```
[logo]: {{BASE_PATH}}/image/game/yz2demo.jpg
```

就可以载到图片了

*修改首页 `index.html`文档,把首页显示成我的信息
*删除原来的文章，放入我的文章

## 上传到Github

修改完成之后就需要上传到Github。这个是参照 [ruanyi][ruanyi]的博客

* 首先，到github 创建一个帐号，然后在帐号下面创建一个仓库，如我创建了一个Blog的仓库。

* 到'_config.yml' 所在的目录下，使用git让本地的Blog目录与github 的Blog仓库建立链接

    cd darranBlog
    git init

* 创建一个没有父节点的分支gh-pages. 因为github 规定有该分支的上传的才会自动生成网页文件

```
git checkout --orphan gh-pages
```

* 把当前目录的东西上传到github.

{%highlight Ruby%}
    git add .
    git commit -m "first post"
    git remote add origin https://github.com/darranchen/Blog
    git push origin gh-pages
{%endhighlight %}


要等10分钟左右就可以看到blog生成了，但是我一开始没有成功，因为我还没有购买我的域名 [blog.xjchen.net][xj]. 

# 购买与设置域名

我是购买了万网的域名，直接到 [万网][万网] 的网站上面购买的，购买步骤就不详细叙述了。购买完之后到域名管理下面的 解析记录增加三个解析, 如我下图所示 ![域名][wan] 设置完成之后可能需要一到两天才可以访问。这样就完成了。

[xj]: http://blog.xjchen.net
[ever]: http://blog.evercoding.net/
[mojombo]: https://github.com/mojombo/jekyll/wiki/sites
[Skydark]: http://blog.skydark.info/ 
[yansu]: http://yansu.org/2014/02/12/how-to-deploy-a-blog-on-github-by-jekyll.html
[yanping]: http://yanping.me/cn
[geeklu]: http://geeklu.com
[github]: http://github.com
[ruanyi]: http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html
[万网]: http://www.net.cn/
[wan]: {{BASE_PATH}}/images/yuming.png
