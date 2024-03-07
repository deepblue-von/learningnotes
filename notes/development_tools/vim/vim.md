# vim

## 下载vim指定版本

```shell
wget https://ftp.nluug.nl/pub/vim/unix/vim-9.0.tar.bz2
```

## 解压

```shell
tar -jxvf vim-9.0.tar.bz2
```

## 配置

```shell
cd vim90/src
./configure
sudo yum install ncurses-devel
```

## 编译

```shell
make
```

## 安装

```shell
sudo make install
```







# coc.vim

## git

```shell
sudo yum install git
```

## vim-plug

```shell
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```



## nodejs

```shell
curl --silent --location https://rpm.nodesource.com/setup_lts.x | bash -
sudo yum install nodejs
```

## coc.vim

```shell
call plug#begin()
Plug 'neoclide/coc.nvim', {'branch': 'release'}
Plug 'neoclide/coc-snippets'
Plug 'preservim/nerdtree'
Plug 'Xuyuanp/nerdtree-git-plugin'
Plug 'honza/vim-snippets'
call plug#end()
```

```shell
:PlugInstall
```

## 配置

```shell
进入vim
:CocConfig
```

添加

```shell
"languageserver": {
  "ccls": {
    "command": "ccls",
    "filetypes": ["c", "cc", "cpp", "c++", "objc", "objcpp"],
    "rootPatterns": [".ccls", "compile_commands.json", ".git/", ".hg/"],
    "initializationOptions": {
        "cache": {
          "directory": "/tmp/ccls"
        }
      }
  }
}

```



~/.vimrc

```shell
" Use <Tab> and <S-Tab> to navigate the completion list
" tab补全
inoremap <expr> <Tab> pumvisible() ? "\<C-n>" : "\<Tab>"
inoremap <expr> <S-Tab> pumvisible() ? "\<C-p>" : "\<S-Tab>"
“ ctrl space 刷新
inoremap <silent><expr> <c-space> coc#refresh())
" enter确定补全
inoremap <expr> <cr> pumvisible() ? "\<C-y>" : "\<C-g>u\<CR>"
```



## ccls

```shell
yum install clang-devel
yum install llvm-devel
```



在/home/src目录下

```shell
git clone --depth=1 --recursive https://github.com/MaskRay/ccls
```

编译安装

```shell
cd ccls
 
cmake .
make
```

创建软连接

```shell
ln -s /home/src/ccls/ccls /usr/bin/ccls
```



启动查看是否安装成功

```shell
ccls
```





# vim其他配置

```shell
set softtabstop=4
set shiftwidth=4

"解决默认是replace模式的问题
nnoremap <esc>^[ <esc>^[]]

set autoindent
set smartindent
"设置字体

set guifont=YaHei\ Consolas\ Hybrid\ 11.5
" set guifont=Courier\ New:h10
colorscheme murphy
"设置真彩模式
set termguicolors

inoremap ' ''<ESC>i
inoremap " ""<ESC>i
inoremap ( ()<ESC>i
inoremap [ []<ESC>i
"inoremap { {<CR>}<ESC>O
inoremap jj <ESC>
"数组则不换行
inoremap { {}<ESC>i
"函数左括号加回车则换行
inoremap {<CR> {<CR>}<ESC>O


" coc tab键补全
inoremap <expr> <Tab> pumvisible() ? "\<C-n>" : "\<Tab>"
inoremap <expr> <S-Tab> pumvisible() ? "\<C-p>" : "\<S-Tab>
" 解决不能退格的问题
set backspace=indent,eol,start
" Use <c-space> to trigger completion.  ctrl + space 刷新
inoremap <silent><expr> <c-space> coc#refresh()
" 使用enter确认补全
inoremap <expr> <cr> pumvisible() ? "\<C-y>" : "\<C-g>u\<CR>"
“ tab跳到下一个参数
let g:coc_snippet_next = '<tab>'
" vim-plug
call plug#begin()
Plug 'neoclide/coc.nvim', {'branch': 'release'}
call plug#end()



" 开启文件类型侦测
filetype on
" 根据侦测到的不同类型加载对应的插件
filetype plugin on


" 定义快捷键前缀，即<Leader>
let mapleader=";"


" 定义快捷键保存当前窗口
nmap <Leader>w :w<CR>
" 定义快捷键关闭当前分割窗口
nmap <Leader>q :q<CR>

" 让配置立即生效
autocmd BufWritePost $MYVIMRC source $MYVIMRC

" 开启实时搜索功能
set incsearch
" 搜索时大小写不敏感
set ignorecase
" 关闭兼容模式
set nocompatible
" vim 自身命令行模式智能补全
set wildmenu

" 开启语法高亮
" syntax enable

"语法高亮
" syntax on
"set mouse=a
"高亮相匹配的括号
"set showmatch
"搜索时显示高亮
":set hlsearch
"搜索时忽略大小写
set ignorecase
" 开启实时搜索功能
set incsearch
" 关闭兼容模式
set nocompatible
" vim 自身命令模式智能补全
set wildmenu


"英语单词拼写检查
""set spell spelllang=en_us
"不创建交换文件
set noswapfile
""保存撤销历史
set undofile
"记住多少次操作历史
set history=1000
""将历史操作文件统一保存
"undodir=~/.undodir


" 总是显示状态栏
set laststatus=2
" 显示光标当前位置
set ruler
" 开启行号显示
set number
" 高亮显示当前行/列
" set cursorline
" set cursorcolumn
" 高亮显示搜索结果
set hlsearch

" 基于缩进或语法进行代码折叠
" za打开或关闭当前折叠，zM关闭所有折叠,zE打开所有折叠
set foldmethod=indent
set foldmethod=syntax
" 启动 vim 时关闭折叠代码
set nofoldenable


" 接口与实现之间快速切换
nmap <silent> <Leader>sw :FSHere<cr>
```

