---
{"author":"aming","email":"jikcheng@163.com","title":"Linux Initial configuration of the work","creation_date":"2022-06-27 15:57","Last modified date":"2022-11-25 16:00","tags":"Linux Initial configuration of the work","File Folder with relative path":"system/Doc/Linux/Linux Doc/Linux Configer","remark":null,"other":null,"dg-publish":true,"permalink":"/system/doc/linux/linux-doc/linux-configer/linux-initial-configuration-of-the-work/","dgPassFrontmatter":true}
---


# 基础安装包
```bash
yum update -y
```
```bash
yum install git vim net-tools sysstat  redhat-lsb-core
```
## 安装epel 源
## 官方网站
[epel 源](https://fedoraproject.org/wiki/EPEL)
```bash
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
```
# 配置VIM

## 配置vbundle
```bash
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```
## vimrc 配置
# 1. vimrc
```vim
" vim 官方配置
" 1.设置<TAB> 的格式.
set shiftwidth=4 
set softtabstop=4
" 2. 设置补全方式
set wildmode=longest,list
" 3. 保留命令的历史长度.
set history=2000
" 4. map 映射键
cnoremap <C-p> <Up>
cnoremap <C-n> <Down>

" 5.设置不兼容模式
set nocompatible              " be iMproved, required
" 6.打开自动文件高亮
filetype on                  " required
" 7. 设置缓存区的是否能够被隐藏.
"
set hidden
" 8. 设置打开文件时的快捷路径
cnoremap <expr> %% getcmdtype( ) == ':' ? expand('%:h').'/' : '%%'
" 9.跳转增强插件
runtime macros/matchit.vim
" 10.设置常用shell
set shell=/bin/bash

" Vbundle 插件 Plugin  管理插件
" :h Vundle

set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
call vundle#end()            " required
filetype plugin indent on    " required
Plugin 'VundleVim/Vundle.vim'




" 表格自动格式化插件
Plugin 'godlygeek/tabular'


" 快捷键插件
Plugin 'tpope/vim-unimpaired'
" find 的增强插件
Plugin 'tpope/vim-rails'
" 成对出现的符号添加\删除插件
Plugin 'tpope/vim-surround'
" sql code surround
let g:surround_113 = "```sql \n \r \n```"
let g:surround_115 = "```console \n \r \n```"
let g:surround_105 = "::: alert-info \n \r \n:::" 
let g:surround_100 = "::: alert-danger \n \r \n:::" 
let g:surround_119 = "::: alert-warning \n  \r \n:::" 
let g:surround_114 = ">>> \n  \r \n>>>" 
"""""""""""""""""" shell 编写IDE""""""""""""""""""""""
Plugin 'WolfgangMehner/bash-support'
"""""""""""""""""" awk 编写IDE""""""""""""""""""""""
Plugin 'WolfgangMehner/awk-support'

""""""""""""""" markdown """""""""""""""""""""""""'
Plugin 'vim-voom/VOoM'

"1. 根据文件类型进行自动窗口打开
let g:voom_ft_modes = {'markdown': 'markdown', 'tex': 'latex'}
```


# git 配置
```bash
 git config --global user.email "jikcheng@163.com"
 git config --global user.name "Aming"
```