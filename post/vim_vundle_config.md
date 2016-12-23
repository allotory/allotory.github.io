Title: 使用Vundle和GitHub管理Vim配置
Date: 2016-02-25
Tags: Vim, GitHub
Category: IDE
Slug: vimrc_config
Author: Ellery

## 简介

Vim 作为史上最强大的编辑器之一，拥有数以万计的插件。Vim 的生态系统拥有完美的插件机制。Vundle 是现存最好的 Vim 插件管理方案，用 Vundle 管理插件的好处：

* 自动下载安装
* 自动更新
* 轻松异地重装
* 自动清理没用的插件

## 前提

使用 [Vundle][2] 管理 Vim 插件时，绝大多数插件都是使用的 Github 上的开源库直接安装的，并且配置好的 Vundle 插件也需要使用 Github 进行管理。所以如果在 Windows 上首先需要安装 Git 客户端并了解常用 Git 命令。

## 安装

Vim 可以轻松安装在 `Windows` 及 `*nix` 系统中，但两者的安装目录结构略有不同。所以安装插件时命令格式也略有不同，应该值得注意。

	Windows 系统：
		
		_vimrc	#配置文件	
		vmfiles	#插件目录	对应变量   $VIM

	Unix-like 系统：

		.vimrc	#配置文件
		.vim	#插件目录

### 在 Github 上创建 Vim 管理仓库

在 Github 上创建一个空白仓库作为 Vim 管理仓库，这样在我们更换系统环境时可以轻松重新恢复已有配置。例如我的仓库为 [doicha][1]。该仓库的传输协议地址为：

	git@github.com:allotory/doricha.git
	
### Vundle 安装（Windows 版本为例）

1. 备份 Vim 主目录中的 `_vimrc` 文件以及 `vimfiles` 目录（这点非常重要！）。

2. 使用 `git clone` 命令克隆先前创建的 Vim 管理仓库，该例子中为了与 Vim 原始目录名称一致方便管理，所以直接克隆到 `vimfiles` 目录，其实可以直接克隆到任何地方，只要在配置文件中表明相关路径即可。

		$ git clone git@github.com:allotory/doricha.git vimfiles

3. 切换到 `vimfiles` 目录下使用 `git submodule add` 命令以 [子模块][3]的形式安装 Vundle。

		$ git submodule add https://github.com/VundleVim/Vundle.vim.git bundle/vundle

4. 本例中新建了两个新的配置文件 `vundle_vimrc` 和 `basic_vimrc` ，分别用来保存自定义的 Vundle 配置和自定义的 Vim 基础的配置。

5. 配置 `vundle_vimrc` 文件具体内容请参考 Vundle 配置文档，其中应重点注意 `path` 路径问题。

		set nocompatible        " be iMproved, required
		filetype off            " required
		
		" 设置包括vundle和初始化相关的runtime path
		" 判断操作系统类型
		if(has('win32') || has('win64'))
			set rtp+=$VIM/vimfiles/bundle/Vundle.vim
			let path='$VIM/vimfiles/bundle'
		else
			set rtp+=~/.vim/bundle/Vundle.vim
			let path='~/.vim/bundle'
		endif
		call vundle#begin(path)
		" 另一种选择, 指定一个vundle安装插件的路径
		"call vundle#begin('~/some/path/here')
		
		" 让vundle管理插件版本,必须
		Plugin 'VundleVim/Vundle.vim'
		
		" 以下范例用来支持不同格式的插件安装.
		" 请将安装插的命令放在vundle#begin和vundle#end之间.
		
		" Github上的插件
		" 格式为 Plugin '用户名/插件仓库名'
		"Plugin 'tpope/vim-fugitive'
		
		" 来自 http://vim-scripts.org/vim/scripts.html 的插件
		" Plugin '插件名称' 实际上是 Plugin 'vim-scripts/插件仓库名' 只是此处的用户名可以省略
		"Plugin 'L9'
		
		" 由Git支持但不再github上的插件仓库 Plugin 'git clone 后面的地址'
		"Plugin 'git://git.wincent.com/command-t.git'
		
		" 本地的Git仓库(例如自己的插件) Plugin 'file:///+本地插件仓库绝对路径'
		"Plugin 'file:///home/gmarik/path/to/plugin'
		
		" 插件在仓库的子目录中.
		" 正确指定路径用以设置runtimepath. 以下范例插件在sparkup/vim目录下
		"Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
		
		" 避免插件名冲突,例如L9已存在,则可以指定
		"Plugin 'user/L9', {'name': 'newL9'}
		
		" 你的所有插件需要在下面这行之前
		call vundle#end()            " 必须
		filetype plugin indent on    " 必须 加载vim自带和插件相应的语法和文件类型相关脚本
		" 忽视插件改变缩进,可以使用以下替代:
		"filetype plugin on
		"
		" 简要帮助文档
		" :PluginList       - 列出所有已配置的插件
		" :PluginInstall    - 安装插件,追加 `!` 用以更新或使用 :PluginUpdate
		" :PluginSearch foo - 搜索 foo ; 追加 `!` 清除本地缓存
		" :PluginClean      - 清除未使用插件,需要确认; 追加 `!` 自动批准移除未使用插件
		"
		" 查阅 :h vundle 获取更多细节和wiki以及FAQ
		" 将你自己对非插件片段放在这行之后

6. 配置 `baisc_vimrc` 文件，这里提供了我自己常用的一些配置仅供参考。

		set nocompatible
		
		" 主题
		colorscheme molokai
		let g:molokai_original = 1
		
		" 基础设置
		set guifont=Monaco:h14			" 字体 && 字号
		syntax on 						" 打开语法高亮
		set number						" 显示行号
		set autoindent					" 设置缩进有三个取值 cindent (c风格)
										" smartindent (智能模式)、
										" autoindent (简单的与上一行保持一致)
		set tabstop=4					" 设置 tab 键的宽度
		set shiftwidth=4				" 换行时行间交错使用4个空格
		set backspace=indent,eol,start	" 设置退格键可用
										" indent：如果用了 set indent, set ai 等自动缩进，想用退格键将字段缩进的删掉，必须设置这个选项，否则不响应。
										" eol：如果插入模式下在行开头，想通过退格键合并两行，需要设置eol。
										" start：要想删除此次插入前的输入，需设置这个。
		set smarttab					" 每次按 backspace 时删除4个空格
		set incsearch					" 增量式搜索(遍搜索遍显示内容)
		set hlsearch					" 高亮搜索
		filetype on 					" 打开文件类型检测功能
		filetype plugin on 				" 允许加载文件类型插件
		filetype indent on 				" 允许为不同类型的文件定义不同的缩进格式
		set showmatch					" 显示括号配对情况
		"set foldenable					" 开启代码折叠
		"set foldmethod=syntax 			" 自动语法折叠
		set nobackup					" 禁止生成备份
		set noswapfile					" 不产生 swp 文件
		set mouse=a 					" 启用鼠标
		set nowrap						" 设置不自动折行
		set cursorline					" 突出显示当前行
		set clipboard=unnamed			" 与 windows 共享剪贴板
		set cmdheight=1					" 命令行（在状态行下）的高度，默认为1
		set laststatus=2 			    " 开启状态栏信息
		
		" 设置窗口
		if has("gui_running")
			" au GUIEnter * simalt ~x 	" 窗口启动时自动最大化
			winpos 70 25             	" 指定窗口出现的位置，坐标原点在屏幕左上角
			set lines=55 columns=180 	" 指定窗口大小，lines 为高度，columns 为宽度
			set guioptions+=c 			" 使用字符提示框
			set guioptions-=m       	" 隐藏菜单栏
			set guioptions-=T        	" 隐藏工具栏
			set guioptions=L         	" 隐藏左侧滚动条
			set guioptions=r         	" 隐藏右侧滚动条
			set guioptions-=b       	" 隐藏底部滚动条
			set showtabline=0       	" 隐藏 Tab 栏
		endif
		
		" 设置编码
		set fenc=utf-8								" 设置编码
		set encoding=utf-8							" 内部的程序识别编码
		set fileencoding=utf-8						" 当前文件编辑时使用的文件编码
		set fileencodings=utf-8,gbk,cp936,latin-1	" gvim 打开文件是支持的编码
		language messages zh_CN.utf-8				" 解决 consle 输出乱码
		set ambiwidth=double						" 防止特殊符号无法显示
		set helplang=cn 							" 中文帮助	 

7. 最后将我们自定义的配置文件引入到 Vim 的配置文件 `_vimrc` 中， 同样注意其中路径写法。

		" 引用自定义的vundle配置文件,存放vimrc的地方(不是固定写法，可自定义)
		" 判断操作系统类型
		if(has('win32') || has('win64'))
			source $VIM/vimfiles/vundle_vimrc
			source $VIM/vimfiles/basic_vimrc
		else
			source ~/.vim/vundle_vimrc
			source ~/.vim/basic_vimrc
		endif	

至此，我们已经完成 Vundle 安装。

### 插件安装

Vundle 安装完成后，就可以使用其进行其他插件的安装。这里我们以 [NERDTree][4] 为例，`NERDTree` 是一个非常好用的 Vim 树形浏览插件。

1. 在 `vundle_vimrc` 中 `Github` 插件部分添加如下内容：

		Plugin 'scrooloose/nerdtree'

2. 重新打开 Vim 命令模式下执行如下命令即可完成安装：

		：PluginInstall

3. 在 `basic_vimrc` 中按需要自行配置 `NERDTree`， 如：

		autocmd vimenter * NERDTree
		let NERDTreeWinSize=25

### 将已安装的插件以子模块形式进行管理

最初我们安装 Vundle 时是以 `Git` 子模块形式进行管理的，但是使用 Vundle 安装的插件并没有以子模块形式进行管理，所以为了方便日后更新管理插件，需要手动将其添加为子模块。

	$ git submodule add https://github.com/scrooloose/nerdtree.git bundle/nerdtree
	Adding existing repo at 'bundle/nerdtree' to the index

此时我们已经完成了所有的安装配置操作，可以正常的提交到 `Github`。

## 重新安装（Windows 版本为例）

1. 克隆主项目到 `vimfiles` 目录。

		$ git clone git@github.com:allotory/doricha.git vimfiles

2. 此时，所有的插件目录均为空，需要重新安装，这是可以使用三种方法安装：

	* 使用 Vundle 命令安装，这种方法需要重新安装 Vundle。
	
			$ git submodule add https://github.com/VundleVim/Vundle.vim.git bundle/vundle
	
		然后再使用 Vundle 命令重新安装其他插件。

			：PluginInstall

	* 由于之前所有插件均添加为主项目的子模块，所以可以使用 `Git` 子模块相关命令进行安装。

			$ git submodule —-查看当前项目用到的子模块
			$ git submodule init —-只在首次检出仓库时运行一次就行
			$ git submodule update —-更新子模块（子模块会重新检出）

	* 更简单的一种方法是克隆主仓库时将其所包含的子模块同时克隆下来。

			 $ git clone --recursive git@github.com:allotory/doricha.git vimfiles

3. 最后不要忘了将我们自定义的配置文件引入到 Vim 的配置文件 `_vimrc` 中，因为此时的 `_vimrc` 文件是全新的，需要重新设置。

## 总结

Vim 是一个神器，但用好不宜。个人觉得不应该安装过多的插件使其成为一个 IDE ，过多的插件会使 Vim 卡顿、启动缓慢等。反而不如真正的 IDE 好用。最后附上我的界面。

![screenshot](/images/10-vim-vundle-config.png)

[1]: https://github.com/allotory/doricha
[2]: https://github.com/VundleVim/Vundle.vim
[3]: https://git-scm.com/book/en/v2/Git-Tools-Submodules
[4]: https://github.com/scrooloose/nerdtree