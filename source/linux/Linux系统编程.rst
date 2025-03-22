=============
Linux系统编程
=============

概述
------------------------------------------------

系统编程的主要内容：

+ 文件I/O
+ 进程管理
+ 进程间通信（IPC）
+ 信号处理
+ 多线程编程（pthread）
+ 网络编程


hello world
------------------------------------------------

系统调用
------------------------------------------------

`A system call is a request for service that a program makes of the kernel. The service is generally something that only the kernel has the privilege to do, such as doing I/O. Programmers don’t normally need to be concerned with system calls because there are functions in the GNU C Library to do virtually everything that system calls do. These functions work by making system calls themselves. For example, there is a system call that changes the permissions of a file, but you don’t need to know about it because you can just use the GNU C Library’s chmod function.` [#syscall_intro]_

可以通过 ``man 2 <name>`` 查看系统调用的说明

可以通过 ``strace`` 命令跟踪一个程序执行过程中使用的系统调用，如：

.. code-block:: bash

    strace mkdir tmp_dir
    strace chmod 755 some_file

glibc接口
------------------------------------------------

glibc和系统调用的关系：https://sys.readthedocs.io/en/latest/doc/07_calling_system_calls.html

glibc的syscall函数执行系统调用，示例：

.. code-block:: c

    #include <errno.h>
    #include <sys/stat.h>
    #include <sys/types.h>
    #include <stdio.h>

    int main() {
        int rc = syscall(SYS_chmod, "/etc/passwd", 0444);
        if (rc == -1) fprintf(stderr, "chmod failed, errno = %d\n", errno);
    }

glibc为系统调用提供了封装好的接口，便于使用，可以通过命令 ``dpkg -L libc6-dev`` 查看，对于x86_64架构，主要在以下目录：

.. code-block:: bash

    /usr/include
    /usr/include/x86_64-linux-gnu

上述的syscall示例等价于以下示例：

.. code-block:: c

    #include <sys/types.h>
    #include <sys/stat.h>
    #include <errno.h>
    #include <stdio.h>

    int main() {
        int rc = chmod("/etc/passwd", 0444);
        if (rc == -1) fprintf(stderr, "chmod failed, errno = %d\n", errno);
    }

ucontext
------------------------------------------------
空


参考资料
------------------------------------------------

+ `Linux system calls <https://man7.org/linux/man-pages/man2/syscalls.2.html>`_
+ https://lxr.missinglinkelectronics.com/linux
+ https://sys.readthedocs.io/en/latest/index.html
+ `Linux系统调用表 <https://github.com/torvalds/linux/blob/master/arch/x86/entry/syscalls/syscall_64.tbl>`_
+ `Linux 驱动 | 重新理解一波设备驱动 <https://www.51cto.com/article/708502.html>`_
+ `The 101 of ELF files on Linux: Understanding and Analysis <https://linux-audit.com/elf-binaries-on-linux-understanding-and-analysis/>`_

.. [#syscall_intro] https://www.gnu.org/software/libc/manual/html_node/System-Calls.html