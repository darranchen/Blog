---
layout: post
title: "Augment octave with vim"
date:  2014-04-16 11:33
categories: Technology
tags: Vim, Octave
---

之前对于R 使用r—plugin 感觉各种方便，因为课程的问题，我也经常需要用Matlab, 但是使用Matlab 经常需要使用界面化系统，编辑方面我还是想要跟R 一样用vim 进行编辑，后面发现了有Octave 也就是Matlab的开源版本，既然是开源版本，那么正常应该会有大神开发vim 的插件用于编辑 \.m (也就是Matlab\Octave)的文档，那我就想实现像在vim编辑R 文档一样，

1. 语法高亮，这个应该是基本配备吧
2. 从vim 可以直接执行octave 当前行代码
3. 从vim 可以执行用visual 选择到的一块区域代码
4. 执行当前文档

首先安装Octave 

```
sudo apt-get install octave
```

# 功能一

--------------------------------

接着就可以到 [octave.vim][octave.vim] 下载由Rik写的语法高亮插件octave.vim，有了这个插件之后，只要编辑.m的文档就可以有语法高亮了，无论是matlab 还是octave 的文档。

    #下载完成之后把octave.vim 放到~/.vim/syntax文档中
    mkdir -p ~/.vim/syntax
    cp octave.vim ~/.vim/syntax

添加下面的代码到~\/\.vimrc档里面，这个参照了[Embedded Programmer][Eoc]

    "enable syntax highlighting 这是用于语法高亮的，如果之前vimrc中已经有设置，那就需要了
    syntax on 
    " .m files are "octave" files
    augroup filetypedetect
      au! BufRead,BufNewFile *.m,*.oct set filetype=octave
    augroup END 


# 功能二

---------------------------------

这样就能实现第一个需求了，第二个需求是可以执行当前行的代码，这里我是参照了[Thor][Thor] 提出的方法，如果需要把vim中当前的代码行发送到octave中，需要启动一个octave的界面，到\.m 档的目录下面执行下面代码，就可以启动一个新的octave文档用于和vim进行交互

```
tmux new-session -s octave "octave -q"
```

然后在\.vimrc 的文档中加入

    map <buffer> <F5> <Esc>:!tmux send-keys -t octave "<C-r>=getline('.')<CR>"<C-r>=nr2char(13)<CR><CR>a
    nmap <buffer> <F5>      :!tmux send-keys -t octave "<C-r>=getline('.')<CR>"<C-r>=nr2char(13)<CR><CR>

这样打开一个\.m的文档，如test.m 内容如下

    test = zeros(2,1);
    test_fault = zeros(2,1); %不要把注释放在后面，Tmux似乎不能识别%号，会发生执行错误

打开text.m

    vim text.m

光标移到第一行，然后按F5，当前行的命令就可以发送到Tmux开的Octave界面了,这里需要注意，我在使用的时候发现Tmux开得Octave似乎不能识别%号，所以如果把注释放在后面会导致当前行执行失败，所以尽可能不要加在后面。

# 功能三

-----------------------------------------------

第三个功能是希望用visual选择一个代码区域，然后把整个代码区域送到octave中执行，但是我根据Thir讲的尝试了下，没有成功，但是第一个功能已经能够满足我的需求了。下面贴出Thir的方法，如果有人成功了，麻烦告知一下, 在\.vimrc 文档中添加

    vmap <buffer> <F2> call ExecSelection()<CR><CR>
    let s:octave_selection = '/var/tmp/vim_octave_selection.m'

    function! ExecSelection()
    if line(".") == line("'<'")
        exec ".write! " . s:octave_selection
    else
        exec ".write! >> " . s:octave_selection
    endif
    if line(".") == line("'>")
        exec "!tmux send-keys -t octave 'run " . s:octave_selection . "'" . nr2char(13)
    endif
    endfunction

然后到test.m文档中用visual模式选择到想要执行的代码执行就好，可以我这边没有成功。

#功能四

------------------------------------

最后的功能就是可以执行当前整个文档在\.vimrc中添加

    imap <buffer> <F3> <Esc>:!tmux send-keys -t octave "<C-r>=expand('%:t:r')<CR>"<C-r>=nr2char(13)<CR><CR>a
    nmap <buffer> <F3>      :!tmux send-keys -t octave "<C-r>=expand('%:t:r')<CR>"<C-r>=nr2char(13)<CR><CR>

按下F3就可以执行整个文档了

#附加功能

--------------------------------------
把vim 设置为\.m文档的默认编辑器，编辑octaverc档

    vim ./.octaverc

输入下面的代码

    # Matlab like prompt
    PS1(">> ")
    #Clear the startup screen
    clc;
    # Use vim as editor
    edit mode sync
    edit home .
    edit editor 'vim > /dev/tty 2>&1 < /dev/tty %s'

这样在octave 中输入 `edit test.m` 就可以直接打开vim 来编辑test.m文档了，但是这个功能我一般比较少用.

[octave.vim]: http://www.vim.org/scripts/script.php?script_id=3600 "Octave 语法插件"
[Eoc]: http://embeddedprogrammer.blogspot.tw/2013/01/augmenting-octave-with-vim.html
[Thor]: http://stackoverflow.com/questions/15241201/appending-a-line-at-the-end-of-a-file
