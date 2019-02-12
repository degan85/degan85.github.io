---
layout: post
title:  "windows에서 clojure + vim"
author: Degan
categories: clojure
tags:   clojure windows lein 	
comments: true
---

## Leiningen 

윈도우용으로 다운로드
  [leiningen-win-installer](https://djpowell.github.io/leiningen-win-installer/)

---

## Vim

* Vundle 이용해서 다음 플러그인들 설치
    * tpope/vim-fugitive
    * tpope/vim-fireplace.git
    * kien/rainbow_parentheses.vim
    * guns/vim-clojure-static
    * tpope/vim-salve
    * nrepl/piggieback

* vimrc
(Vundle 외 필요없는 부분들 생략)

```
set nocompatible              
filetype off                  

Plugin 'tpope/vim-fugitive'
Plugin 'tpope/vim-fireplace.git'
Plugin 'kien/rainbow_parentheses.vim'
Plugin 'guns/vim-clojure-static'
Plugin 'tpope/vim-salve'
Plugin 'nrepl/piggieback'

imap jk <ESC>

set nu
set hls

set expandtab
set tabstop=4

set clipboard=unnamed
set nobackup
set noswapfile

let g:molokai_original = 1
let g:rehash256 = 1
colorscheme molokai

set title
set guifont=consolas:h14

let g:rbpt_colorpairs = [
    \ ['brown',       'RoyalBlue3'],
    \ ['Darkblue',    'SeaGreen3'],
    \ ['darkgray',    'DarkOrchid3'],
    \ ['darkgreen',   'firebrick3'],
    \ ['darkcyan',    'RoyalBlue3'],
    \ ['darkred',     'SeaGreen3'],
    \ ['darkmagenta', 'DarkOrchid3'],
    \ ['brown',       'firebrick3'],
    \ ['gray',        'RoyalBlue3'],
    \ ['black',       'SeaGreen3'],
    \ ['darkmagenta', 'DarkOrchid3'],
    \ ['Darkblue',    'firebrick3'],
    \ ['darkgreen',   'RoyalBlue3'],
    \ ['darkcyan',    'SeaGreen3'],
    \ ['darkred',     'DarkOrchid3'],
    \ ['red',         'firebrick3'],
    \ ]
let g:rbpt_max = 16

au VimEnter * RainbowParenthesesToggle
au Syntax * RainbowParenthesesLoadRound
au Syntax * RainbowParenthesesLoadSquare
au Syntax * RainbowParenthesesLoadBraces
```  

---

## 실행

* cmd에서 `lein new hello` 실행
* 해당 폴더에서 `lein repl` 실행
* vim으로 해당 폴더에 clj 파일 열음
* repl에 **name** 값과 파일의 맨 위에 `(ns name)`값이 같은지 확인
* vim에서 `:h fireplace` 실행
