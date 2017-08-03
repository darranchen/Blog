---
layout: post
title: "Lubuntu13.01 Setup!"
date: 2014-04-13 01:56:23
categories: Technology
tags: Vim Ubuntu
keywords: Lubuntu13.01, vim, gedit
---

太久没有重装系统了，主要是想要装一下jekyll, 同时由于之间系统安装时候有个bug 所以果断重装了。下面是要加上我的系统重装后需要加上的功能： 
# gedit
这是简单需要用到的阅读文档工具，比较像windows 中的记事本工具，有时候vim对于复制粘帖不太方便的时候会使用它：
{% highlight ruby %}
	sudo apt-get install gedit
{%endhighlight%}
# vim 
这个就是我需要的了，
{% highlight ruby %}
	sudo apt-get install vim
{%endhighlight%}
安装完成之后将vim的配置档.vimrc 从[我的github][my_github]下载下来
或者可以使用git 直接拷下来，学完git 之后再补上,应该是
{% highlight ruby %}
git colone
{%endhighlight%}
#git & openssh &jekyll
为了写blog 所以需要安装git
{% highlight ruby%}
sudo apt-get install git-core
{%endhighlight%}
详细的git 和github 的交互在之后的blog会做学习说明

安装openssh 这个主要是用于git 和github的交互:
{% highlight ruby %}
	sudo apt-get install openssh-server
{%endhighlight%}

安装jekyll 我的安装之基于[宁雨:ubuntu 12.40 安装jekyll][宁雨] 以及[Install Jekyll on Ubuntu 12.10][michaelchelen] 中间还是发生了一些奇奇怪怪的问题.勉强能用: 先装
- ruby1.9.3 
- python.pygments pygments 主要是用于用于不同的语法高亮
- rdiscount kramdown 是用于支持Markdown的:

{% highlight ruby %}
sudo apt-get install ruby1.9.3
sudo apt-get install python-pygments
sudo gem1.9.3 install rdiscount kramdown
{%endhighlight%}

接下来安装rdoc 和jekyll, 不先装rdoc会发生错误，所以就装了:

    sudo gem1.9.3 install rdoc
    sudo gem1.9.3 install jekyll

#安装中文输入法
我喜欢安装lubuntu 系统的时候用全英的，这样默认建立的文件夹的名称都会是英文的，安装完之后就要开始安装中文输入法，因为在台湾的关系，所以需要简繁体共同支持的输入法，所以选择了google 输入法。这个输入法的安装是根据[pluto418 Google输入发的安装][pluto] 安装步骤：

- 安装语言包： System Settings--\>Language Support--\>Install/Remove Languages 选择中文简体和繁体。安装
- 安装ibus 框架：
{% highlight ruby %}
sudo apt-get install ibus ibus-clutter ibus-gtk ibus-gtk3 ibus-qt4
{%endhighlight%}
- 安装google 拼音法:
{% highlight ruby %}
sudo apt-get install ibus-googlepinyin
{%endhighlight%}
- 设置ibus
安装gnome language support
{% highlight ruby %}
sudo apt-get install language-selector-gnome
{%endhighlight%}
把默认输入方式修改为ibus

#安装截图工具 gnome\-screenshot

{% highlight ruby %}
sudo apt-get install gnome-screenshot
{%endhighlight%}

设置快捷键, 原本以为lubuntu 上有直接设置快捷键的图形化界面，发现它管理快捷键的方式是由openbox来管理的

    vim ~/.config/openbox/lubuntu-rc.xml

加入,这个时候重新登录用户之后就可以 `Ctrl+Alt+a`就可以开始截图了。
{% highlight ruby %}
<keybind key="C-A-a">
    <action name="Execute">
        <command>gnome-screenshot -a</command>
     </action>
 </keybind>
{%endhighlight%}

还有一种方式是直接找到 系统-设置-键盘 自定义快捷键， 设定名称和对应的命令， 对应的命令是 gnome-screenshot -a, 应用， 看到后面的禁用， 禁用处添加上 Ctrl+Alt+a就可以设置对应的快捷键了。

[my_github]: https://github.com/darranchen/vim "Optional Title"
[宁雨]:http://ningyuwhut.blogspot.tw/2013/09/ubuntu-1240-jekyll.html
[michaelchelen]: http://michaelchelen.net/articles/install-jekyll-ubuntu-12-10.html
[pluto]: http://pluto418.iteye.com/blog/1772256
