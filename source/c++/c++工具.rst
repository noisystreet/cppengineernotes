=============
C++工具
=============

源代码管理：git
------------------------------------------------

git配置和仓库管理
````````````````````````````````````````````````

+ ``git config``
+ ``git clone``
+ ``git init``
+ ``git submodule``
+ ``git worktree``

代码修改与提交
````````````````````````````````````````````````

+ ``git add``
+ ``git status``
+ ``git commit``
+ ``git stash``
+ ``git clean``
+ ``git rm``
+ ``git reset``
+ ``git clang-format``

代码分析
````````````````````````````````````````````````

+ ``git diff``
+ ``git apply``
+ ``git cherry-pick``
+ ``git blame``
+ ``git bisect``

分支管理
````````````````````````````````````````````````

+ ``git branch``
+ ``git switch``
+ ``git checkout``
+ ``git rebase``
+ ``git cherry-pick``

远程仓库管理
````````````````````````````````````````````````

+ ``git remote``
+ ``git push``
+ ``git pull``
+ ``git fetch``
+ ``git prune``

提交记录管理
````````````````````````````````````````````````

+ ``git log``
+ ``git tag``
+ ``git reflog``
+ ``git show``

参考资料
````````````````````````````````````````````````

#. git文档 https://git-scm.com/docs

源代码阅读：ctags和gnu global
------------------------------------------------
空

源代码静态检查：cppcheck
------------------------------------------------

安装cppcheck

.. code-block:: bash
    :linenos:

    sudo apt install cppcheck cppcheck-gui

代码文档：doxygen
------------------------------------------------
空

参考资料
````````````````````````````````````````````````

#. https://www.doxygen.nl/manual/index.html

自动化编译：makefile和autotools
------------------------------------------------
空

跨平台编译构建：cmake
------------------------------------------------

基本选项：

+ ``cmake -S src -B build``
+ ``cmake --build build``
+ ``cmake --install <dir>``

其他常用选项：

+ ``-D <var>:<type>=<value>, -D <var>=<value>``
+ ``-G <generator-name>``
+ ``--install-prefix <directory>``
+ ``--graphviz=<file>``

trace相关选项:

+ ``--trace``
+ ``--trace-expand``
+ ``--trace-format=<format>``
+ ``--trace-redirect=<file>``

profile相关选项：

+ ``--profiling-output=<path>``
+ ``--profiling-format=<file>``

参考资料
````````````````````````````````````````````````

#. cmake文档 https://cmake.org/cmake/help/latest/guide/tutorial/index.html

调试：gdb
------------------------------------------------

使用gdb调试前应当在编译选项中加入-g或者-ggdb，以在运行时获得更多调试信息。

常用命令
````````````````````````````````````````````````

+ ``gdb file xx`` 启动gdb并加载可执行程序
+ ``gdb program pid_num`` 对某个pid号对应的进程进行调试，简写为-p
+ ``gdb --args python xx.py`` 启动并带参数运行Python脚本
+ ``help`` 查看某个命令的使用说明
+ ``break`` 在某个文件或者函数中设置断点，简写为b
+ ``clear`` 清空断点
+ ``delete`` 删除断点，简写为d
+ ``enabe/disable`` 启用/禁用断点
+ ``set args xxx`` 设置程序运行参数
+ ``run args`` 带参数运行程序，简写为r
+ ``backtrace`` 打印调用栈，简写为bt
+ ``continue`` 在断点后继续执行程序,简写为c
+ ``next [ns]`` 执行ns步，简写为n
+ ``list`` 打印源码
+ ``print`` 打印变量值，简写为p
+ ``watch`` 监视某个变量的值
+ ``whatis`` 显示变量的值和类型
+ ``quit`` 退出gdb环境，简写为q
+ ``start`` 在开始处设置断点
+ ``step`` 进入调用函数的内部，简写为s
+ ``starti`` 在程序最开始执行的地方设置临时断点，然后开始执行
+ ``rbreak`` 每个函数都设置断点
+ ``display var`` 列出某个变量
+ ``info locals`` 列出当前上下文的所有局部变量
+ ``info variables`` 列出static变量
+ ``backtrace full`` 列出所有调用栈上的变量
+ ``apropos`` 模糊查找命令，如：apropos registers

查看寄存器
````````````````````````````````````````````````

+ ``maintenance print register-groups`` Print the internal register configuration including each register's group.
+ ``info registers`` 列出整数寄存器变量，简写info r
+ ``info all-registers`` 列出所有寄存器变量，
+ ``maintenance print reggroups`` 打印内部寄存器的组名称

.. code-block:: bash
    :linenos:

    Group      Type      
    sse        user      
    mmx        user      
    general    user      
    float      user      
    all        user      
    save       internal  
    restore    internal  
    vector     user      
    system     user

``info registers vector`` 查看vector寄存器变量

打印变量
````````````````````````````````````````````````

``print/fmt expr`` ，常用的fmt有：

+ ``/x`` 以十六进制的形式打印出整数。
+ ``/d`` 以有符号、十进制的形式打印出整数。
+ ``/u`` 以无符号、十进制的形式打印出整数。
+ ``/o`` 以八进制的形式打印出整数。
+ ``/t`` 以二进制的形式打印出整数。
+ ``/f`` 以浮点数的形式打印变量或表达式的值。
+ ``/c`` 以字符形式打印变量或表达式的值

打印数组 ``print *arr@100`` 

打印C++STL容器
````````````````````````````````````````````````

参考：https://sourceware.org/gdb/wiki/STLSupport

首先，需要gdb编译时启用了 ``--with-python`` 选项。

查找printers.py路径：

.. code-block:: bash

    locate python/libstdcxx/v6/printers.py

创建$HOME/.gdbinit文件，加入以下内容：

.. code-block:: bash

    python
    import sys
    import os
    if os.path.exists('/usr/share/gcc/python'):
        sys.path.insert(0, '/usr/share/gcc/python')
        from libstdcxx.v6.printers import register_libstdcxx_printers
        register_libstdcxx_printers (None)
    end

启动gdb，执行 ``info pretty-printer`` ,典型输出如下：

.. code-block:: bash

    global pretty-printers:
    builtin
        mpx_bound128
    libstdc++-v6
        __gnu_cxx::_Slist_iterator
        __gnu_cxx::__8::_Slist_iterator
        __gnu_cxx::__8::__normal_iterator
        __gnu_cxx::__8::slist
        __gnu_cxx::__normal_iterator
        __gnu_cxx::slist
        __gnu_debug::_Safe_iterator
        std::_Bit_const_iterator
        std::_Bit_iterator
        std::_Bit_reference
        std::_Deque_const_iterator
        std::_Deque_iterator
        std::_Fwd_list_const_iterator
        std::_Fwd_list_iterator
        ......

然后就可以以美观简洁的方式打印STL容器

条件断点
````````````````````````````````````````````````

设置方法：

#. ``break ... if expr``
#. ``condition num expr``

c++字符串的条件断点：

.. code-block:: bash
    :linenos:

    cond 1 ((int)strcmp(pName.c_str(), "abc")) == 0

或者：

.. code-block:: bash
    :linenos:

    condition 1 str.compare("foo") == 0

调试系统软件
````````````````````````````````````````````````

以ls为例,先要下载源码和debug符号：

.. code-block:: bash
    :linenos:

    sudo apt source coreutils
    sudo apt install coreutils-dbgsym

然后就可以debug ls

调试多线程
````````````````````````````````````````````````

+ ``info inferiors`` 查看进程信息
+ ``info threads`` 查看线程信息
+ ``thread ID`` 切换到编号为ID的线程
+ ``set scheduler-locking on`` 只运行当前线程
+ ``break thread_test.c:123 thread all`` 在所有线程中相应的行上设置断点
+ ``thread apply all command`` 让所有被调试线程执行GDB命令command
+ ``thread apply ID1 ID2 command`` 让一个或者多个线程执行GDB命令command
+ ``set follow-fork-mode child`` 在目标应用调用fork之后接着调试子进程而不是父进程

调试c++和python混合程序
````````````````````````````````````````````````

在python脚本中打印进程号并暂停脚本

.. code-block:: python
    :linenos:

    print("pid=",os.getpid())
    os.system("read _")

然后使用 ``gdb python pid`` 进行调试，python脚本可以使用 ``py-bt`` 等等命令
如果提示py-bt找不到，需要先locate python3.x-gdb.py文件的位置，然后在gdb中执行：

.. code-block:: python
    :linenos:

    source /usr/share/gdb/auto-load/usr/bin/python3.x-gdb.py

或者在家目录下创建 ``.gdbinit`` 文件，加入上面的代码。

GDB原理
````````````````````````````````````````````````

https://zhuanlan.zhihu.com/p/336922639
系统首先会启动gdb进程，这个进程会调用系统函数fork()来创建一个子进程，这个子进程做两件事情： 1. 调用系统函数ptrace(PTRACE_TRACEME，[其他参数])； 2. 通过exec来加载、执行可执行程序。
不论是调试一个新程序，还是调试一个已经处于执行中状态的服务程序，通过ptrace系统调用，最终的结果都是：gdb程序是父进程，被调试程序是子进程，子进程的所有信号都被父进程gdb来接管，并且父进程gdb可查看、修改子进程的内部信息，包括：堆栈、寄存器等。

参考资料
````````````````````````````````````````````````

#. https://www.sourceware.org/gdb/current/onlinedocs/gdb.html
#. `100个GDB小技巧 <https://wizardforcel.gitbooks.io/100-gdb-tips/content/index.html>`_
#. https://hiberabyss.github.io/2018/04/04/gdb-internal/http://note.iawen.com/note/programming/gdb_ptrace
#. `debugging with gdb <https://sourceware.org/gdb/current/onlinedocs/gdb/>`_
#. https://sourceware.org/gdb/current/onlinedocs/gdb.html/Process-Record-and-Replay.html


程序执行流程分析：uftrace
------------------------------------------------

编译安装：

.. code-block:: bash

    sudo apt install libelf-dev libdw-dev libcapstone-dev libtraceevent-dev libunwind-dev libluajit-5.1-dev
    git clone -b master https://github.com/namhyung/uftrace.git

直接安装：

.. code-block:: bash

    sudo apt install uftrace

使用过程：

1.编译源码时使用-pg参数
2.执行程序uftrace record ./xx，如果程序编译时未使用-pg，运行时可以加上--force选项，加上-a可以记录函数返回值，最终数据会记录在uftrace.data文件中

使用-F func指定只跟踪某个函数，使用-D设置跟踪函数调用的深度
使用-t过滤掉执行时间较短的函数，如：

.. code-block:: bash
    :linenos:

    uftrace -t 5us hello

常用命令：

+ ``uftrace report`` 查看执行时间统计
+ ``uftrace replay`` 查看执行过程
+ ``uftrace graph`` 查看整个或者某个函数的call graph，如：

.. code-block:: bash
    :linenos:

    uftrace graph main

参考资料
````````````````````````````````````````````````

#. https://uftrace.github.io/slide/#1
#. https://github.com/namhyung/uftrace/wiki/Tutorial

二进制文件分析工具
------------------------------------------------

libc-bin
````````````````````````````````````````````````

+ ``pldd`` 查看进程依赖的动态链接库
+ ``ldd`` 查看elf文件依赖的动态链接库
+ ``ldconfig`` 管理ld加载的动态链接库，-p打印所有库

binutils
````````````````````````````````````````````````

+ ``addr2line``
+ ``ar`` 目标文件打包
+ ``as``  汇编器
+ ``c++filt`` 根据文件中的符号名称查找源程序中对应的名称
+ ``dwp``
+ ``elfedit``
+ ``gold``
+ ``gprof`` 程序性能分析工具
+ ``ld`` 链接-加载器
+ ``nm``  列出一个目标文件中的各种符号
+ ``objcopy`` 从elf文件中拷贝信息
+ ``objdump`` 查看elf文件文件汇编信息
+ ``ranlib``
+ ``readelf`` 读取elf文件内容
+ ``size``

pax-utils
````````````````````````````````````````````````

+ ``dumpelf``
+ ``lddtree`` 以树形方式显示可执行文件的依赖
+ ``pspax``
+ ``scanelf``
+ ``scanmacho``
+ ``symtree``

execstack
````````````````````````````````````````````````
包含的工具为execstack，用来查看elf文件的栈信息

elfutils
````````````````````````````````````````````````

包含的工具有：

+ ``eu-addr2line``
+ ``eu-ar``
+ ``eu-elfclassify``
+ ``eu-elfcmp``
+ ``eu-elfcompress``
+ ``eu-elflint``
+ ``eu-findtextrel``
+ ``eu-make-debug-archive``
+ ``eu-nm``
+ ``eu-objdump``
+ ``eu-ranlib``
+ ``eu-readelf`` 如： ``eu-readelf –program-headers /bin/ps``
+ ``eu-size``
+ ``eu-stack``
+ ``eu-strings``
+ ``eu-strip``
+ ``eu-unstrip``
+ ``prelink`` ELF prelinking utility to speed up dynamic linking, 
+ ``binwalk`` 

参考资料
````````````````````````````````````````````````

#. https://linux-audit.com/elf-binaries-on-linux-understanding-and-analysis/
#. https://github.com/ReFirmLabs/binwalk
#. https://opensource.com/article/20/4/linux-binary-analysis

性能分析
------------------------------------------------

gprof
````````````````````````````````````````````````

gprof是bintuils包中的一个profile工具，可以运行于linux、AIX、Sun等操作系统进行C、C++、Pascal、Fortran程序的性能分析，用于程序的性能优化以及程序瓶颈问题的查找和解决。通过分析应用程序运行时产生的“flat profile”，可以得到每个函数的调用次数，每个函数消耗的处理器时间，也可以得到函数的“调用关系图”，包括函数调用的层次关系，每个函数调用花费了多少时间。

+ 编译时需要加上-pg参数
+ 执行程序，生成gmon.out文件
+ 使用gprof  xxx gmon.out分析程序的Flat profile

分析时，可以加上-p或者-q参数，区别在于：

+ ``-p`` 参数标识 ``flat profile`` 模式，在分析结果中不显示函数的调用关系
+ ``-q`` 参数标识 ``call graph`` 模式，在分析结果中显示函数的调用关系。

perf
````````````````````````````````````````````````

安装命令：

.. code-block::  bash
    :linenos:

    #ubuntu
    sudo apt install linux-tools-`uname -r`
    #debian
    sudo apt install linux-perf

perf的常用命令：

+ ``perf stat``
+ ``perf record``
+ ``perf report`` 查看PMU统计结果,--hierarchy:输出层次化的结果
+ ``perf list`` 查看PMU事件定义
+ ``perf bench`` 运行perf自带的基准测试

systemtap
````````````````````````````````````````````````
空

参考资料
````````````````````````````````````````````````

#. https://easyperf.net/notes/
#. https://www.agner.org/optimize/
#. Computer Systems Performance Analysis https://www.cse.wustl.edu/~jain/iucee/index.html
#. Linux Performance and Development Tools https://alephnull.com/perf.html
#. Linux Performance Analysis in 60,000 Milliseconds https://netflixtechblog.com/linux-performance-analysis-in-60-000-milliseconds-accc10403c55
#. https://access.redhat.com/documentation/en-us/red_hat_developer_toolset/7/html/user_guide/chap-binutils
#. https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/monitoring_and_managing_system_status_and_performance/recording-and-analyzing-performance-profiles-with-perf_monitoring-and-managing-system-status-and-performance
#. https://blog.51cto.com/xiamachao/1857696
#. http://manpages.ubuntu.com/manpages/jammy/man1/perf-stat.1.html
#. https://sourceware.org/systemtap/wiki
#. https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/systemtap_beginners_guide/index
#. https://www.codedump.info/post/20200128-systemtap-by-example/
#. https://github.com/gperftools/gperftools

内存分析：valgrind
------------------------------------------------

valgrind是一套工具合集，可以用 ``--tool`` 选项指定命令，可用的有：

+ ``memcheck``
+ ``cachegrind``
+ ``callgrind``
+ ``helgrind``
+ ``drd``
+ ``massif``
+ ``dhat``
+ ``lackey``
+ ``none``
+ ``exp-bbv``

配合 ``kcachegrind`` 查看调用栈，例子：

.. code-block:: bash
    :linenos:

    valgrind --tool=callgrind --dump-instr=yes --collect-jumps=yes ./xhpcg --nx=32

然后用 ``kcachegrind`` 打开 ``callgrind.out.*`` 文件即可

C++和python混合程序的性能分析
------------------------------------------------

可视化：speedscope
````````````````````````````````````````````````

简单例子：

.. code-block:: bash
    :linenos:

    py-spy record --format speedscope -o output.json --native -- python xx.py

生成的json文件用speedscope打开即可查看timeline
speedscope安装：

.. code-block:: bash
    :linenos:

    npm i -g speedscope

viztracer
````````````````````````````````````````````````

查看调用栈,用法: ``viztracer xx.py`` 然后 ``vizviewer result.json``，然后使用 ``AWSD`` 操作可视化界面
或者使用下面命令对某段代码进行trace：

.. code-block:: python
    :linenos:

    from viztracer import VizTracer

    tracer = VizTracer()
    tracer.start()
    # Something happens here
    tracer.stop()
    tracer.save() # also takes output_file as an optional argument

或者：

.. code-block:: python
    :linenos:

    with VizTracer(output_file="optional.json") as tracer:
        # Something happens here

参考资料：

#. pax-utils https://wiki.gentoo.org/wiki/Hardened/PaX_Utilities
#. valgrind https://valgrind.org/docs/manual/manual.html
#. Profiling Native Python Extensions https://www.benfrederickson.com/profiling-native-python-extensions-with-py-spy/
#. py-spy文档 https://docs.rs/crate/py-spy/latest
#. viztracer https://viztracer.readthedocs.io/en/latest/#

其他
------------------------------------------------

+ ``cppman`` c++帮助手册
+ ``graphviz`` 绘制有向图
+ ``hotspot`` 图形化热点分析工具
+ ``gstack/pstack``  查看进程的栈信息
+ ``ldd`` 查看可执行文件依赖的动态库
+ ``patchelf`` 修改已有的二进制可执行文件
+ ``hexdump`` 查看二进制文件 
+ ``cloc`` 统计代码行数
+ ``autodia`` 生成dia图表
+ ``ccache`` 编译缓存
+ ``ccbuild`` 自动编译工具
+ ``cccc`` 代码统计
+ ``cdecl`` 将短语转换成代码
+ ``cflow`` 代码控制流分析
+ ``complexity`` 代码复杂度分析
+ ``csmith`` 产生随机的c语言程序
+ ``global`` 代码搜索，浏览
+ ``heaptrack`` 堆分析工具
+ ``google-perftools`` 性能分析工具
+ ``ht`` 可执行文件编辑查看
+ ``visual-regexp`` 正则表达式debug
+ ``vtable-dumper`` 分析c++动态库中的vtable
+ ``tsort`` 拓扑排序工具
+ ``git-flow`` git工作流工具
+ ``pkg-config`` 显示头文件和库文件参数