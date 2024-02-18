=============
Linux常用命令
=============

linux的发行版
------------------------------------------------

+ redhat系列 redhat/centos/fedora
+ debian系列 debian ubuntu
+ 其他 suse getoo arch

linux的目录结构
------------------------------------------------

典型的linux目录结构：

.. code-block:: bash
 :linenos:

 /
 ├── bin -> usr/bin
 ├── boot
 ├── dev
 ├── etc
 ├── home
 ├── lost+found
 ├── media
 ├── mnt
 ├── opt
 ├── proc
 ├── root
 ├── run
 ├── srv
 ├── sys
 ├── tmp
 └── usr
      ├── bin
      ├── sbin
      ├── include
      ├── lib
      └── local
            ├── bin
            ├── etc
            ├── include
            └── lib
 └── var


文件/目录管理
------------------------------------------------

+ ``pwd`` 显示当前所在路径
+ ``ls`` 显示目录下的文件
+ ``tree`` 以树型结构显示目录下的目录和文件
+ ``file`` 查看文件类型
+ ``cd`` 切换目录
+ ``pushd/popd`` 操作目录栈，常用于在两个目录间切换
+ ``dirs`` 显示目录栈中的目录
+ ``ln`` 创建文件/目录的链接
+ ``touch`` 创建文件或者更新已有文件时间
+ ``rm`` 删除文件/目录
+ ``mkdir`` 创建目录
+ ``rmdir`` 删除目录
+ ``cp`` 复制文件/目录
+ ``scp`` 远程复制文件/目录
+ ``mv`` 移动/重命名文件/目录
+ ``mc`` 文本模式的文件管理器

文件权限管理
````````````````````````````````````````````````

+ ``chmod`` 更改文件权限
+ ``chown`` 更改文件所有者
+ ``chgrp`` 更改文件所属组
+ ``tar`` 打包、压缩或解压文件
+ ``which`` 查找程序所在路径，-a查看所有
+ ``whereis`` 查找程序所在路径（所有同名的程序）
+ ``locate`` 根据linux的数据库查找文件
+ ``find`` 文件查找
+ ``grep`` 使用正则表达式查找

文件编辑和查看
------------------------------------------------

查看文件
````````````````````````````````````````````````

+ ``cat``
  ``cat`` -A显示所有不可见字符
+ ``more``
+ ``less``
+ ``head``
+ ``tail``
+ ``fx/jq`` 处理json

编辑文件
````````````````````````````````````````````````

+ ``nano`` 文本编辑程序
+ ``vim`` 文本编辑程序
+ ``split`` 文件分割
+ ``dos2unix/unix2dos`` 转换windows和unix文本格式
+ ``seq`` 产生数字序列
+ ``cut`` 分割数据
+ ``sort`` 排序
+ ``wc`` word count
+ ``convert`` 图像变换

进程管理
------------------------------------------------

+ ``pstree`` 以树形结构显示进程
+ ``ps`` 查看进程
+ ``pidof`` 查看某个进程的进程号
+ ``pmap`` 查看进程内存
+ ``kill`` 向进程发送信号
+ ``nohup`` 让程序在后台运行
+ ``lsof`` 查看进程打开的文件
+ ``taskset`` 设置进程的cpu亲和性
+ ``bg/fg`` 将程序切换到后台/前台执行
+ ``nice`` 调整进程优先级

用户管理
------------------------------------------------

+ ``sudo`` 以管理员权限执行命令
+ ``who`` 查看登入系统的用户
+ ``whoami`` 查看当前用户名
+ ``logname`` 查看当前用户名
+ ``logout`` 注销
+ ``lastlog`` 报告所有用户的最近登录情况，或者指定用户的最近登录情况
+ ``adduser`` 添加用户，并添加用户的家目录
+ ``useadd`` 添加用户
+ ``usedel`` 删除用户
+ ``su`` 切换用户
+ ``passwd`` 更改用户密码

系统管理
------------------------------------------------

开关机
````````````````````````````````````````````````

+ ``halt`` 关机
+ ``shutdown`` 关机
+ ``poweroff`` 直接断电关机
+ ``reboot`` 重启
+ ``dmesg`` 查看开机信息

查看系统信息
````````````````````````````````````````````````
+ ``lsb_release`` 查看发行版信息
+ ``uname`` 查看操作系统信息
+ ``runlevel`` 查看当前系统运行等级
+ ``arch`` 显示计算机体系结构
+ ``hostname`` 查看主机名
+ ``date`` 查看系统时间
+ ``cal`` 查看日历
+ ``iconv -l`` 查看系统支持的编码格式
+ ``locale`` 查看系统使用的locale信息

查看硬件设备
````````````````````````````````````````````````

+ ``lshw`` 查看计算机硬件信息，sudo lshw -C display 查看显示设备。https://ezix.org/project/wiki/HardwareLiSter
+ ``lscpu`` 查看cpu信息
+ ``lsusb`` 查看usb接信息
+ ``lsblk`` 查看块设备文件
+ ``lspci`` 查看总线设备
+ ``mount`` 挂载/查看设备
+ ``hwloc`` hardware localty
+ ``lstopo`` 列出系统cpu拓扑结构

查看系统性能
````````````````````````````````````````````````

+ ``free`` 查看内存使用情况
+ ``top`` 查看各进程的内存使用情况
+ ``htop`` 查看进程和内存使用情况（多核）
+ ``mpstat`` 查看cpu使用情况
+ ``turbostat`` 查看cpu频率
+ ``sar`` （System Activity Reporter）是目前Linux上最为全面的系统性能分析工具之一，可以从多方面对系统的活动进行报告，包括 文件的读写情况、系统调用的使用情况、磁盘IO、CPU、内存、进程活动及IPC有关的活动等

+ ``du`` 查看目录空间大小
+ ``duf`` 查看磁盘和目录空间
+ ``df`` 查看硬盘空间使用情况
+ ``env`` 查看已加载的环境变量
+ ``uptime`` 查看系统运行时间和负载

内核配置和模块
````````````````````````````````````````````````

+ ``lsmod`` 查看内核已加载模块
+ ``insmod/rmmod`` 安装/卸载内核模块
+ ``sysctl`` 配置内核运行时参数
+ ``getconf`` 查看配置（页表 getconf PAGESIZE）
+ ``logrotate`` 日志管理

系统服务管理
````````````````````````````````````````````````

systemd软件包中包含的重要命令:

+ ``timedatectl``
+ ``hostnamectl``
+ ``bootctl``
+ ``localectl``
+ ``resolvectl``
+ ``systemd-analyze``

sysctemctl的主要用法：

+ 启动服务 ``systemctl start xx.service``
+ 关闭服务 ``systemctl stop xx.service``
+ 重启服务 ``systemctl restart xx.service``
+ 显示服务的状态 ``systemctl status xx.service``
+ 在开机时启用服务 ``systemctl enable xx.service``
+ 在开机时禁用服务 ``systemctl disable xx.service``
+ 查看服务是否开机启动 ``systemctl is-enabled xx.service``
+ 查看已启动的服务列表 ``systemctl list-unit-files|grep enabled``
+ 查看启动失败的服务列表 ``systemctl --failed``


网络相关
------------------------------------------------

网络端口介绍：

+ https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/4/html/security_guide/ch-ports
+ `Service Name and Transport Protocol Port Number Registry <http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml>`_
+ `De Facto Ports <https://matt-rickard.com/de-facto-ports>`_

网络相关命令：

+ ``ping`` 测试主机的连通性
+ ``w3m`` 文本模式的浏览器
+ ``netstat`` 查看网络信息
+ ``route`` 路由管理
+ ``ip`` 网络管理
+ ``nmap``
+ ``ssh`` 远程登录，ssh免密码登录 ``scp ~/.ssh/id_rsa.pub remote_host:/some_user_home/.ssh/authorized_keys``，打开远程主机的gui: ``ssh -Y user@IP``，然后输入命令即可，如 ``cmake-gui``

+ ``talk`` 与其他用户聊天
+ ``mail`` 查看邮件
+ ``curl`` 获取URL链接。 ``-H`` 自定义请求头； ``-I`` 只打印响应头； ``-o`` 保存到文件； ``-L`` 跟随链接重定向； ``-d`` 发送POST请求
+ ``wget`` 下载文件

shell终端快捷键
------------------------------------------------

+ 查看所有快捷键：``man readline``
+ 复制 ``ctrl+shift+c``
+ 粘贴 ``ctrl+shift+v``
+ 补全 ``tab``
+ 移到行首 ``ctrl+a``
+ 移到行尾 ``ctrl+e``
+ ``Ctrl+f`` 光标向前移动一个字符,相当于 ``->``
+ ``Ctrl+b`` 光标向后移动一个字符,相当于 ``<-``
+ ``Alt+f`` 光标向前移动一个单词
+ ``Alt+b`` 光标向后移动一个单词
+ 移动到当前单词的开头 ``Esc+b``
+ 移动到当前单词的结尾 ``Esc+f``
 
+ 删除此处至末尾所有内容 ``ctrl+k``
+ 删除此处至开始所有内容 ``ctrl+u``
+ 剪切光标之前的词  ``Ctrl+w``
+ 剪切光标之后的词 ``Alt+d``
+ 把当前词转化为大写 ``Alt+U``
+ 把当前词转化为小写 ``Alt+L``
+ 清屏 ``Ctrl+L``
+ 杀死当前任务 ``Ctrl+C``
 
+ 窗口操作
+ 新建标签页 ``Shift+Ctrl+T``
+ 关闭标签页 ``Shift+Ctrl+W``
+ 前一标签页 ``Ctrl+PageUp``
+ 后一标签页 ``Ctrl+PageDown``
+ 标签页左移 ``Shift+Ctrl+PageUp``
+ 标签页右移 ``Shift+Ctrl+PageDown``
+ ``Alt+1``:切换到标签页1
+ ``Alt+2``:切换到标签页2
+ ``Alt+3``:切换到标签页3
+ 新建窗口 ``Shift+Ctrl+N``
+ 关闭终端 ``Shift+Ctrl+Q``
 
+ 缩放
+ 放大 ``Ctrl+shift+plus``
+ 减小 ``Ctrl+minus``
+ 原始大小 ``Ctrl+0``
 
+ 显示上一条命令 ``↑`` 或者 ``Ctrl+p``
+ 显示下一条命令 ``↓`` 或者 ``Ctrl+n``
+ ``!num`` 执行命令历史列表的第num条命令
+ ``!!`` 执行上一条命令
+ ``!?string?`` 执行含有string字符串的最新命令
+ ``Ctrl+R`` 然后输入若干字符，向上搜索命令，继续按 ``Ctrl+R``，搜索上一条匹配的命令
+ ``Ctrl+S`` 与 ``Ctrl+R`` 类似,只是正向检索
+ ``Alt+<`` 历史列表第一项
+ ``Alt+>`` 历史列表最后一项
+ ``ls !$`` 执行命令 ``ls``，并以上一条命令的参数为其参数
 
+ ``Ctrl+d`` 删除光标所在处字符
+ ``Ctrl+h`` 删除光标所在处前一个字符
+ ``Ctrl+y`` 粘贴刚才所删除的字符 
+ ``Esc+w`` 删除光标所在处之前的字符至其单词尾（以空格、标点等为分隔符）
+ ``Ctrl+t`` 颠倒光标所在处及其之前的字符位置，并将光标移动到下一个字符
+ ``Alt+t`` 交换当前与以前单词的位置 
+ ``Alt+c`` 把当前词汇变成首字符大写
+ ``Ctrl+v`` 插入特殊字符,如 ``Ctrl+v+Tab`` 加入 ``Tab`` 字符键
+ ``Esc+t`` 颠倒光标所在处及其相邻单词的位置
+ ``Ctrl+(x u)`` 按住Ctrl的同时再先后按x和u，撤销刚才的操作
+ ``Ctrl+s`` 挂起当前shell
+ ``Ctrl+q`` 重新启用挂起的shell

grep
------------------------------------------------

用法 ``grep pattern some_file``

+ ``-n`` 显示行号
+ ``-i`` 不区分大小写
+ ``-r`` 递归搜索子目录中的文件
+ ``-w`` 匹配全词
+ ``-v`` 反向匹配

sed 
------------------------------------------------

+ ``sed -n '/pattern/=' file`` 输出file中匹配pattern的行号
+ ``sed -i '3 r b.txt' a.txt`` 在a.txt文件的第3行之后插入文件b.txt的内容
+ ``sed -i "2d" a.txt`` 删除a.txt的第2行
+ ``sed -i '/pattern/d' file`` 删除文本中符合pattern的行
+ ``sed -n '10,20p' file`` 查看指定行数范围

删除空行 ``sed -i '/^$/d' file_name``

awk
------------------------------------------------

用法 ``awk '/parttern/{action}' {filenames}``

参考资料
------------------------------------------------

#. https://linuxjourney.com/
#. `Linux Standard Base(LSB) <https://refspecs.linuxfoundation.org/lsb.shtml>`_
#. `Linux man pages <https://linux.die.net/man/>`_
#. `linux command library <https://linuxcommandlibrary.com/>`_
#. `The Linux man-pages project <https://www.kernel.org/doc/man-pages/>`_
#. `linux hint <https://linuxhint.com/>`_
#. `linuxopsys <https://linuxopsys.com/>`_
#. `linuxcapable <https://www.linuxcapable.com/>`_
#. `linuxhandbook <ttps://linuxhandbook.com/>`_
#. `debian-handbook <https://debian-handbook.info/browse/stable/>`_
#. `debian-reference <https://www.debian.org/doc/manuals/debian-reference/ch01.zh-cn.html>`_
#. `linux-man-page-guide <https://itsfoss.com/linux-man-page-guide/>`_
#. `Linux® technology reference <http://www.makelinux.net/reference/>`_
#. `tldr <https://manpages.ubuntu.com/manpages/jammy/man1/tldr-py.1.html>`_
#. `VITUX The Linux Compendium <https://vitux.com/>`_