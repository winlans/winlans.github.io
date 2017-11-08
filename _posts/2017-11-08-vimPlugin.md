---
title: 强化vim
date: 2017-11-08 23:29:08
categories:
- PHP
tags:
- vim
- plugin
- Vundle
---

- 用Vundle 来管理我们的vim插件，以便动态的管理

  git ：https://github.com/VundleVim/Vundle.vim 

  将下面的内容写入到 .vimrc 中，可以注释掉不需要的插件

  ```bash
  set nocompatible              " be iMproved, required
  filetype off                  " required

  " set the runtime path to include Vundle and initialize
  set rtp+=~/.vim/bundle/Vundle.vim
  call vundle#begin()
  " alternatively, pass a path where Vundle should install plugins
  "call vundle#begin('~/some/path/here')

  " let Vundle manage Vundle, required
  " github上面的插件只需要项目地址就可以了
  " https://github.com/  VundleVim/Vundle.vim
  Plugin 'VundleVim/Vundle.vim'

  " The following are examples of different formats supported.
  " Keep Plugin commands between vundle#begin/end.
  " plugin on GitHub repo

  Plugin 'tpope/vim-fugitive'

  " plugin from http://vim-scripts.org/vim/scripts.html
  " Plugin 'L9'
  " Git plugin not hosted on GitHub
  Plugin 'git://git.wincent.com/command-t.git'
  " git repos on your local machine (i.e. when working on your own plugin)
  Plugin 'file:///home/gmarik/path/to/plugin'
  " The sparkup vim script is in a subdirectory of this repo called vim.
  " Pass the path to set the runtimepath properly.
  Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
  " Install L9 and avoid a Naming conflict if you've already installed a
  " different version somewhere else.
  " Plugin 'ascenator/L9', {'name': 'newL9'}

  " All of your Plugins must be added before the following line
  call vundle#end()            " required
  filetype plugin indent on    " required
  " To ignore plugin indent changes, instead use:
  "filetype plugin on
  "
  " Brief help
  " :PluginList       - lists configured plugins
  " :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
  " :PluginSearch foo - searches for foo; append `!` to refresh local cache
  " :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
  "
  " see :h vundle for more details or wiki for FAQ
  " Put your non-Plugin stuff after this line
  ```

- 安装插件， 进入vim ， 进入底行命令模式 ，执行`:PluginInstall` 

- 更新插件，执行`:PluginUpdate`

- 插件列表，`:PluginList`

- 查看帮助 ， 执行 `:h vundle`

#### 一些插件

- YouCompleteMe 一个自动不全插件，但是悲哀的不支持php，传送门

  `https://github.com/Valloric/YouCompleteMe`

  - 安装方法 在`.vimrc`中添加 `Plugin 'Valloric/YouCompleteMe'`,打开vim ，执行`:PluginInstall`

- ctrlp : 快速查找切换文件，传送门 `https://github.com/kien/ctrlp.vim`

  - 安装方法同上

  - 必要配置

    ```visual basic
    let g:ctrlp_map = '<c-p>'
    let g:ctrlp_cmd = 'CtrlP'
    ```

    ​

- vim-airline/vim-airline  状态栏插件，传送门 `https://github.com/vim-airline/vim-airline`

- nathanaelkane/vim-indent-guides  代码对齐引导条,传送门 `https://github.com/nathanaelkane/vim-indent-guides`

- ianva/vim-youdao-translater  有道翻译插件  ,传送门`https://github.com/ianva/vim-youdao-translater`

- `https://github.com/fatih/vim-go `

- `https://github.com/scrooloose/nerdtree`   文件管理器

- `https://github.com/majutsushi/tagbar`   tagbar 函数树什么的

- `https://github.com/jiangmiao/auto-pairs`   自动补全括号