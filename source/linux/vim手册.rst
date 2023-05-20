=============
vim手册
=============

安装vim
------------------------------------------------

直接安装最新版vim 

.. code-block:: bash
    :linenos:

    sudo add-apt-repository ppa:jonathonf/vim
    sudo apt install vim

编译安装

.. code-block:: bash
    :linenos:

    #下载源码 
    git clone -b master https://github.com/vim/vim.git

    #配置编译选项
    ./configure --with-features=huge --enable-cscope --enable-multibyte \
    --enable-python3interp \
    --with-python3-config-dir=\
    /usr/lib/python3.9/config-3.9-x86_64-linux-gnu \
    --enable-fail-if-missing

    #编译安装
    make && sudo make install

基本操作
------------------------------------------------

vim有3个模式 插入模式、命令模式、末行模式

+  命令模式 可以移动光标、删除字符等。
+  插入模式 在此模式下可以输入字符，按ESC将回到命令模式。
+  末行模式 可以保存文件、退出vim、设置vim、查找等功能

vim的配置文件
------------------------------------------------

vim的配置文件为 ``.vimrc`` ，一些基本设置如下 

.. code-block:: bash
    :linenos:

    " [Common Configration] 公共配置
    " [ui beautification] 界面美化
    syntax on           " 开启代码高亮
    set nu              " 开启行号
    set ruler           " 开启标尺
    set cursorline      " 开启高亮光标所在行
    set hlsearch        " 开启查找结果高亮显示
    set incsearch       " 开启查找逐字符高亮
    set encoding=UTF-8

    " [improve performance] 提示性能
    set viminfo=                   " 关闭 viminfo (用于加快 vim 启动速度)

    " [polyfill] 功能填补
    set clipboard=unnamedplus      " 开启系统剪贴板支持(需要手动编译最新版 vim 使其 +clipboard)
    set backspace=indent,eol,start " 开启 Backspace 键支持(否则 Backspace 无法删除字符)

    " [mouse support] 鼠标支持
    set mouse=a              " 开启鼠标支持
    set selection=inclusive  " 指定在选择文本时光标所在位置也属于被选中的范围
    set selectmode=mouse,key " 使鼠标和键盘都可以控制光标选择文本

    " [tab] tab键
    set tabstop=4     " 指定制表符(tab)等于的空格数
    set softtabstop=4 " 开启软制表(如果这4个空格是用tab键打出来的删除会一起删除)
    set shiftwidth=4  " 指定自动缩进时缩进4个空格
    set expandtab     "自动用空格代替tab

    " 缩进
    set smartindent   " 开启智能缩进
    set autoindent    " 开启自动缩进
    set cindent       " 开启C缩进(对C、C++语言文件有效)

    " [other] 其它配置
    set backupcopy=yes " 开启备份时行为为覆盖
    set cmdheight=1    " 设置命令行的高度为1

打开和关闭vim
------------------------------------------------

+ ``vim filename`` :打开或新建文件，并将光标置于第一行首
+ ``vim +n filename``  打开文件，并将光标置于第n行首
+ ``vim + filename``  打开文件，并将光标置于最后一行首
+ ``vim +/pattern filename`` 打开文件，并将光标置于第一个与pattern匹配的串处
+ ``vim -r filename``  恢复上次编辑时崩溃的文件
+ ``vim -o/O filename1 filename2 ...``  打开多个文件，依次进行编辑

+ ``:w``  保存文件 
+ ``:w xxx`` 保存为名叫xxx文件 
+ ``:x`` 保存当前文件并退出 
+ ``:q`` 退出当前文件 
+ ``:q!`` 退出且不保存 
+ ``:wq``  保存当前文件并退出 
+ ``:saveas file``   另存为 
+ ``:close`` 关闭当前窗口 
+ ``:qa`` 关闭所有窗口 
+ ``:wa`` 保存所有窗口 
+ ``:only`` 只保留当前窗口 
+ ``:split/vsplit``  水平/垂直分割当前窗口 
+ ``:new/vnew`` 水平/垂直新建窗口编辑空文件 
+ ``:sview`` 新建窗口只读方式查看文件 
+ ``ctrl+w`` 然后 ``hjkl`` 切换窗口

跳转和移动
------------------------------------------------

+ ``k`` 光标上移一行 
+ ``j`` 光标下移一行 
+ ``h`` 光标左移一个字符 
+ ``l`` 光标右移一个字符 
+ ``Enter`` 光标下移一行 
+ ``w/W``  光标右移一个词至词首 
+ ``b/B`` 光标左移一个词至词首 
+ ``e/E`` 光标右移一个词至词尾 
+ ``)`` 光标移至句尾 
+ ``(`` 光标移至句首 
+ ``}`` 光标移至段落开头 
+ ``{`` 光标移至段落结尾 
+ ``n^`` 光标移至第n行首 
+ ``n$`` 光标移至第n行尾 
+ ``H`` 光标移至当前窗口顶行 
+ ``M`` 光标移至当前窗口中间行 
+ ``L`` 光标移至当前窗口最底行 
+ ``0`` 光标移至当前行首 
+ ``^`` 光标移动到当前行第一个非空字符 
+ ``$`` 光标移至当前行尾 
+ ``n+`` 向下跳n行 
+ ``n-`` 向上跳n行 
+ ``gg`` 跳到文件第一行 
+ ``G`` 跳到文件最后一行 
+ ``%``  跳转到匹配的符号 ``(){}[]if /**/`` 等 
+ ``fx`` 移动到字符 x 下次出现的位置 
+ ``Fx`` 移动到字符 x 上次出现的位置 
+ ``tx/Tx``  移动到字符 x 下次/上次出现的位置的前一个字符 
+ ``ctrl+]``  跳转到函数定义 
+ ``g然后ctrl+]`` 查看所有同名函数定义 
+ ``ctrl+i`` 跳转到声明处 
+ ``ctrl+t`` 跳回光标上个位置 
+ ``g  ctrl+]`` 有多个定义的跳转 
+ ``:tag <varname>`` 通过ctags跳转 
+ ``]]`` 跳转到下个函数定义 
+ ``[[`` 跳转到上个函数定义 
+ ``Ctrl+u`` 向文件尾翻半个屏幕 up
+ ``Ctrl+d`` 向文件头翻半个屏幕 down
+ ``Ctrl+f`` 向文件尾翻一个屏幕 front
+ ``Ctrl+b`` 向文件头翻一个屏幕 back
+ ``nz``  将第n行滚至屏幕顶部，不指定n时将当前行滚至屏幕顶部 
+ ``zz`` 移动屏幕使光标居中 
+ ``gf`` 转到光标文件名对应的文件 goto file
+ ``Ctrl+^`` 返回gf跳转前的文件

插入
------------------------------------------------

+ ``i`` 光标前插入 
+ ``a`` 光标后插入 
+ ``I``  在当前行首插入 
+ ``A`` 在当前行尾插入 
+ ``o`` 在当前行之下新开一行 
+ ``O`` 在当前行之上新开一行 
+ ``r`` 替换当前字符 
+ ``R`` 替换当前字符及其后的字符，直至按ESC键 
+ ``ns`` 删除n个字符，并进行插入模式 
+ ``nS`` 删除n行，并进入插入模式 
+ ``ncw或nCW`` 修改从光标起n个单词 包含光标所在词
+ ``nCC`` 修改光标起n行 包含光标所在行
+ ``c$`` 修改光标后的本行内容

删除（剪切）
------------------------------------------------

+ ``ndw或ndW`` 剪切光标处开始及其后的n-1个字 
+ ``ndd`` 剪切当前行及其后n-1行 
+ ``diw`` 剪切光标处单词 
+ ``di(`` 剪切括号内的内容 
+ ``d0`` 从光标剪切至行首 
+ ``d$`` 从光标剪切至行尾 
+ ``x或X`` 剪切一个字符，x剪切光标后的，而X剪切光标前的 
+ ``:n1,n2 d`` 剪切n1行到n2行之间的内容,末行模式
   
撤销和重复操作
------------------------------------------------

+ ``u``  撤销上一步操作 
+ ``U`` 撤销对当前行的所有操作 
+ ``.`` 重复上一步操作 

复制粘贴
------------------------------------------------

+ ``nyy``     复制当前行向下n行，也可以用 "anyy 复制，"a 为缓冲区，a也可以替换为a到z的任意字母，可以完成多个复制任务。
+ ``nyw``    复制从光标开始的n个单词。
+ ``y^``     复制从光标到行首的内容。  
+ ``y$``     复制从光标到行尾的内容。
+ ``p``    粘贴到光标后
+ ``P``   粘贴到光标前

末行模式粘贴

+ ``:n1,n2 co n3`` 将n1行到n2行之间的内容拷贝到第n3行下
+ ``:n1,n2 m n3`` 将n1行到n2行之间的内容移至到第n3行下

使用系统剪切板，安装vim-gtk3代替系统vim，复制时按 ``Y`` 即可

.. code-block:: bash 
    :linenos:

    sudo apt install vim-gtk3

查找和替换
------------------------------------------------

+ ``*`` 查找当前光标的单词
+ ``:noh`` 取消结果高亮
+ ``/pattern`` 从光标位置向文件结尾查找pattern
+ ``?pattern`` 从光标位置向文件开始查找pattern
+ ``n`` 在同一方向重复上一次查找命令
+ ``N`` 在反方向上重复上一次查找命令
+ ``:s/old/new``     用 ``new`` 替换行中首次出现的 ``old``
+ ``:s/old/new/g``       用 ``new`` 替换行中所有的 ``old``
+ ``:n,m s/old/new/g`` 用 ``new`` 替换从 ``n`` 到 ``m`` 行里所有的 ``old``
+ ``:%s/old/new/g``   用 ``new`` 替换当前文件里所有的 ``old``

可视模式
------------------------------------------------

三种可视模式

+ ``v`` 字符可视化
+ ``V`` 行可视化
+ ``ctr+v`` 块可视化

列操作，首先 ``ctr+v`` 进入块可视模式,按 ``hjkl`` 等调整选中的范围

+  删除 选择列块后按d
+  插入 选择列块后 ``shift+i`` ，插入内容后按两次 ``ESC``
+  增加、减少缩进 ``>`` 和 ``<``
  
选择
------------------------------------------------

+ ``vi{`` 选择括号内的内容 
+ ``ggvG`` 选择全文 
+ ``0 ctrl+v  $`` 或者 ``V`` 或者 ``shift+v`` ,选择一行 

末行模式
------------------------------------------------

+ ``:set list/set list!`` 显示/关闭不可见字符
+ ``:set rnu/set rnu!`` 打开/关闭相对行号
+ ``:help`` 打开帮助文档
+ ``:e filename`` 打开文件filename进行编辑
+ ``:!command`` 执行shell命令 ``command`` 
+ ``:n1,n2 w !command`` 将文件中 ``n1`` 行至 ``n2`` 行的内容作为 ``command`` 的输入并执行之，若不指定 ``n1`` 和 ``n2`` ，则表示将整个文件内容作为 ``command`` 的输入
+ ``:r !command`` 将命令 ``command`` 的输出结果放到当前行
+ ``:r file`` 将外部文件的内容读入到当前文件中
+ ``:set path+=./include`` 添加路径
+ ``:set path?`` 查看当前已经设置的 path 选项的值。

vim的插件
------------------------------------------------

推荐 https://vimawesome.com/,首先需要安装插件管理:vim-plug
推荐安装下面的一些 

+ ``vim-plug`` 插件管理工具 需要手工安装
+ ``coc-nvim`` 基于nodejs的插件管理工具，相关coc-clangd,coc-jedi 需要安装clangd和jedi(使用pip安装)
+ ``nerdtree`` 浏览文件目录 
+ ``leaderF`` 查找文件 
+ ``vim-airline-theme``  设置状态栏主题 
+ ``tagbar`` 显示当前文件中的函数概览 
+ ``vim-gutentags`` 代码跳转 需要安装universal-ctags
+ ``vim-devicons`` nerdtree图标美化 需要先安装nerd fonts，并更改终端的字体
+ ``gen_tags.vim`` 自动更新gtags 需要安装global
+ ``blamer.nvim`` 浮动显示每行代码的提交记录 需要安装git
+ ``vim-cpp-enhanced-highlight`` c++增强高亮 
+ ``indentLine`` 显示缩进级别 
+ ``vim-autopep8`` 按照pep8规范格式化python代码 需要安装pip安装autopep8


`vim-plug <https://github.com/junegunn/vim-plug>`_
````````````````````````````````````````````````````````````````````````````````````````````````

vim-plug是一个管理插件的插件,下载之后将 ``plug.vim`` 拷贝到 ``~/.vim/autoload/`` 目录下即可。

在末行模式下的常用命令有:

+  ``:PlugInstall``
+  ``:PlugUpgrade`` 更新vim-plug自身
+  ``:PlugUpdate`` 更新已经安装的其他插件
+  ``:PlugClean`` 清理已经安装的插件
+  ``:PlugStatus`` 检查已经安装的插件
  
安装插件，如https://github.com/vim-airline/vim-airline，只要在 ``.vimrc`` 中添加 

.. code-block:: bash
    :linenos:

    Plug 'vim-airline/vim-airline' #, {'branch': 'release'}可选

然后执行 ``:PlugInstall`` 即可安装。

`coc.nvim <https://github.com/neoclide/coc.nvim>`_
````````````````````````````````````````````````````````````````````````````````````````````````

它是一个基于nodejs的补全插件，可以安装许多语言扩展，安装之前先要安装nodejs 

.. code-block:: bash
    :linenos:

    curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
    sudo apt install nodejs
    #安装yarn,可选，上步已经安装
    sudo npm install -g yarn

coc.nvim常用扩展安装 
``CocInstall coc-clangd coc-jedi coc-cmake coc-sh``

`Nerdtree <https://github.com/preservim/nerdtree>`_
````````````````````````````````````````````````````````````````````````````````````````````````

常用命令

+ ``:r`` 刷新目录和文件 
+ ``:m`` 移动文件目录 
+ ``:a`` 新建文件或者目录 
+ ``:o`` 展开目录 
+ ``:O`` 递归展开目录 
+ ``:e`` 在新窗口打开目录 
+ ``:?`` 查看使用帮助 

`blamer.nvim <https://github.com/APZelos/blamer.nvim>`_
````````````````````````````````````````````````````````````````````````````````````````````````

+ ``:BlamerShow`` 显示代码的提交记录
+ ``:BlamerHide`` 不显示提交记录

`vim-clangformat <https://github.com/rhysd/vim-clang-format>`_
````````````````````````````````````````````````````````````````````````````````````````````````

先在可视化模式下选中要格式化的代码，然后在末行模式下执行 ``:ClangFormat``
``gtags``和 ``cscope``
先安装 ``global``

.. code-block:: bash
    :linenos:

    sudo apt install global cscope

然后将 ``gtags-cscope.vim`` 和 ``gtags.vim`` 拷贝到 ``$HOME/.vim/plugin`` 目录下（如不存在可手工创建），然后就可以使用 ``Gtags`` 命令。,基本用法 
+ ``gtags``  #生成tags
+ ``:Gtags [option] pattern``

可以参照对应的 ``global`` 命令
如 ``:Gtags -r func ctags`` 生成tag 
``ctags -R –c++-kinds=+px –fields=+iaS –extra=+q .``

cscope
生成数据库 
``cscope -Rbkq``

使用方法（末行模式） 
+ ``:cs help``
+ ``:cs find d func_name``

可选项有 
+ ``c`` 查找调用本函数的函数
+ ``d`` 查找本函数调用的函数
+ ``e`` Find this egrep pattern 查找egrep模式
+ ``f`` 查找文件
+ ``g`` 查找定义
+ ``i`` 查找包含了本文件的文件
+ ``s`` 查询符号，函数/枚举/宏
+ ``t`` 查找指定字符串

手工生成tags文件 

.. code-block:: bash
    :linenos:

    find /my/project/dir -name '*.c' -o -name '*.h' > /foo/cscope.files
    cd /foo
    cscope -b
    export CSCOPE_DB=/foo/cscope.out

`vim-gutentags <https://github.com/ludovicchabant/vim-gutentags>`_
````````````````````````````````````````````````````````````````````````````````````````````````

它是一个将 ``ctags, cscope, gtags`` 串起来的一个自动化工具，可以在后台自动生成数据库并 添加 ``gtags`` 数据库到。如果同时编辑多个项目, gutentags 会把两个数据库都连接到 vim 里, 搜索一个符号时可能会被干扰, 搭配 gutentags_plus 一起使用, 可以避免数据库重复加载。
用法 与 ``cscope`` 命令相同，将 ``cs find`` 换成 ``GscopeFind`` 即可
快捷键 
``<leader>`` 键即 ``\`` 键

+ ``<leader>cs`` Find symbol (reference) under cursor
+ ``<leader>cg`` Find symbol definition under cursor
+ ``<leader>cd`` Functions called by this function
+ ``<leader>cc`` Functions calling this function
+ ``<leader>ct`` Find text string under cursor
+ ``<leader>ce`` Find egrep pattern under cursor
+ ``<leader>cf`` Find file name under cursor
+ ``<leader>ci`` Find files #including the file name under cursor
+ ``<leader>ca`` Find places where current symbol is assigned
+ ``<leader>cz`` Find current word in ctags database

`LeaderF <https://github.com/Yggdroot/LeaderF>`_
````````````````````````````````````````````````````````````````````````````````````````````````

常用命令

+ ``:LeaderfFile`` 查找文件 
+ ``:LeaderfBuffer`` 查找当前的Buffer
+ ``:LeaderfMru`` 查找最近使用过的文件( search most recently used files) 
+ ``:LeaderfFunction`` 查找当前文件的函数 
+ ``:LeaderfLine`` 查找当前文件中有的某个单词（好处就是能把他们都列出来，不是很常用，其实，不过可以看看有多少行，也不错）

`gen_tags.vim <https://github.com/jsfaint/gen_tags.vim>`_
````````````````````````````````````````````````````````````````````````````````````````````````

进入c/c++源码工程目录，在vim中执行 ``:GenGTAGS`` 生成gtags, ``:GenCtags`` 则是生成ctags
生成的tags/ctags目录在 ``$HOME/.cache/tags_dir/``
执行一次就行了，后面当在vim中修改了源码时，会自动更新gtags\ctags

gen_tags的快捷键:

+ ``Ctrl+\ c`` Find functions calling this function
+ ``Ctrl+\ d`` Find functions called by this function
+ ``Ctrl+\ e`` Find this egrep pattern
+ ``Ctrl+\ f`` Find this file
+ ``Ctrl+\ g`` Find this definition
+ ``Ctrl+\ i`` Find files #including this file
+ ``Ctrl+\ s`` Find this C symbol
+ ``Ctrl+\ t`` Find this text string