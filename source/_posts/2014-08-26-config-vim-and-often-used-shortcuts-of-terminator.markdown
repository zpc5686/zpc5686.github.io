---
layout: post
title: "Config Vim and Often Used Shortcuts of Terminator"
date: 2014-08-26 23:04:52 +0800
comments: true
categories: 
---
## 将Vim打造成Python开发的编辑器

说起Python开发，你要是在windows平台下进行多少有点不专业。而且第三方的很多包啊，模块啊，都是针对Linux系统开发。要知道Linux是没有IDE的，开发的时候编辑器与编译器等模块是分开的。Vim作为一款历史悠久的编辑器，可以胜任Linux下任何语言的开发。在这里记录一下使用Python开发时所用到的插件。

### NerdTree

看这个名字Nerd难道还不够diors吗，不要小看它，它可以在Vim中扩展出一个目录结构来。安装方法如下。

```bash
cd ~/.vim/bundle
git clone https://github.com/scrooloose/nerdtree.git
```

我喜欢用F3来开关NerdTree,需要在 ~/.vimrc中添加

```bash
map <F3> :NERDTreeMirror<CR>
map <F3> :NERDTreeToggle<CR>
```

你可以从[这里](https://github.com/scrooloose/nerdtree)看到nerdtree的源代码。

### Taglist

这个插件用来快速定位类中函数，分析源代码的结构。可以从[这里](http://sourceforge.net/projects/vim-taglist/files/)下载。

### Pydiction

这个厉害了，我最初都在想有没有插件想VS那样可以打点进行调用的，答案是肯定的。利用这个插件很方便地实现。
首先，需要下载

```bash
cd ~/.vim/bundle
git clone https://github.com/rkulla/pydiction.git
```
下载好了发现是几个文件，需要对他们进行拷贝
```bash
cp pydiction-1.2/python_pydiction.vim   ~/.vim/after/ftplugin/
cp pydiction-1.2/complete-dict       ~/.vim/tools/pydiction/complete-dict
```
没有目录的要先建立目录，我今天就是吃了这个亏了。 然后在~/.vimrc中加入


```bash
let Tlist_Auto_Highlight_Tag=1
let Tlist_Auto_Open=1
let Tlist_Auto_Update=1
let Tlist_Display_Tag_Scope=1
let Tlist_Exit_OnlyWindow=1
let Tlist_Enable_Dold_Column=1
let Tlist_File_Fold_Auto_Close=1
let Tlist_Show_One_File=1
let Tlist_Use_Right_Window=1
let Tlist_Use_SingleClick=1
nnoremap <silent> <F8> :TlistToggle<CR>

filetype plugin on
autocmd FileType python set omnifunc=pythoncomplete#Complete
autocmd FileType javascrīpt set omnifunc=javascriptcomplete#CompleteJS
autocmd FileType html set omnifunc=htmlcomplete#CompleteTags
autocmd FileType css set omnifunc=csscomplete#CompleteCSS
autocmd FileType xml set omnifunc=xmlcomplete#CompleteTags
autocmd FileType php set omnifunc=phpcomplete#CompletePHP
autocmd FileType c set omnifunc=ccomplete#Complete


let g:pydiction_location='~/.vim/tools/pydiction/complete-dict'
set tabstop=4
set shiftwidth=4
set expandtab
set number
```

这么多插件，可能需要一个管理的，这里使用[pathogen](https://github.com/tpope/vim-pathogen)


## Terminator 快捷键

几个常用的
Ctrl+Shift+O
Split terminals Horizontally.（上下开新窗口）
Ctrl+Shift+E
Split terminals Vertically.（垂直开新窗口）
Alt+Up
Move to the terminal above the current one.（切换当前窗口）
Ctrl+Shift+W
Close the current terminal.
Ctrl+Shift+Q
Quits Terminator







