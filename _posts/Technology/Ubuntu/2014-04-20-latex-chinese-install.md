---
layout: post
title: "Ubuntu 13.01 安装latex 中文版"
date: 2014-04-20 01:56:23
categories: Technology
tags: Vim, Lubuntu13.01, latex chinese
keywords: Lubuntu13.01, vim, latex
---

##Lubuntu 13.01 安装latex

在linux 上安装latex 很容易，主要是中文版的问题，latex基本的安装包是 `texlive-latex-base` 可以使用下面的命令查看有什么跟latex相关的安装包

    apt-cach search latex #查看与latex相关的信息
    sudo apt-get install texlive-latex-base # 安装latex的安装包


如果已经知道要安装texlive的话，那么直接安装texlive就好，但是我只安装上面的 `texlive` 发生了一个问题，rsfs5.tfm 没有找到，其实需要使用 `texlive-fonts-extra` 这些字体的外加包，更加便捷的方式，直接使用

```
sudo apt-get install texlive
```

把texlive 的相关包都安装下来，这样就没问题了。

##安装中文支持包

对于latex来说，如果需要它能支持中文，一般使用CJK这个安装包,直接下载 `latex-cjk-all` 就好了

```
sudo apt-get install latex-cjk-all
```

##在vim中编辑pdf 档

下面介绍vim 的插件 [latex\-suite][latex],这个可以直接在vim中用 `ll` 编译pdf 档，同时可以用 `lv` 查看 pdf 档， 下载latex-suite 插件，下载完之后解压到.vim下面

```
sudo tar zxvf latexSuite-1.5.tar.gz
```

lv默认的查看文档是dvi文档,需要修改默认查看文档为pdf文档。修改 `.vim/ftplugin/latex-suite/texrc` 文档中把88和92行改为默认生成pdf档

    if has('macunix')
        TexLet g:Tex_DefaultTargetFormat = 'pdf'
    else
        TexLet g:Tex_DefaultTargetFormat = 'pdf'
        endif

        " A comma seperated list of formats which need multiple compilations to be
        " correctly compiled.
        TexLet g:Tex_MultipleCompileFormats = 'pdf'

把查看的方式使用为 'evinve'

    TexLet g:Tex_ViewRule_pdf = 'evince'

然后用 '\lv' 可以看pdf档,这里是参考 [小小泪][xiaoxiao]的博客, 大功告成~~~~~~

[latex]: http://www.vim.org/scripts/script.php?script_id=475 "latex-suite"
[xiaoxiao]: https://lttt.blog.ustc.edu.cn
