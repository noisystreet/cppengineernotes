Linux系统管理
=================

系统信息查看
------------------------------------------------

综合查看工具：

+ hwinfo
+ dmidecode
+ inxi
+ lshw
+ cpu-x：带有图形界面，可查看CPU，内存，主板，显卡等信息
+ hardinfo

系统性能查看工具：

+ systat 命令iostat/sar/mpstat/pidstat
+ procps 子命令free/uptime/watch/w/top/ps/pgrep/pmap/vmstat/
+ dstat	更加直观地报告系统状态	
+ atop	进程与系统查看	
+ iotop	类似top，查看io状态	
+ stacer		
+ nmon		
+ hwloc	查看hardware localty，子命令lstopo

CPU和进程工具：

+ cpuinfo包	查看系统的cpu信息，子命令cache-info/cpu-info/cpuid-dump/isa-info
+ cpuid	根据cpuid指令输出CPU信息	
+ x86info	查看x86兼容CPU信息	
+ cpustat	查看CPU正在运行的进程	
+ cpulimit	限制进程的CPU使用率	
+ cpu-x	查看系统和CPU信息	
+ idlestat	查看CPU空闲信息	
+ top	进程信息查看	
+ htop	比top更直观的工具
+ pstree 查看进程树

内存和带宽分析：

+ free	查看空闲内存
+ mbw	带宽测试
+ stream	带宽测试
+ babelstream	cpu/gpu带宽测试
+ numad	查看NUMA拓扑和使用
+ numatop	
+ memtester	内存威力测试与内存错误检查

内存带宽理论峰值计算公式：通道数*内存总线频率*内存控制器效率*数据总线位宽/8

intel mlc工具：https://zhuanlan.zhihu.com/p/367004094

https://www.intel.com/content/www/us/en/developer/articles/tool/intelr-memory-latency-checker.html

.. code-block:: bash

    ./mlc --peak_injection_bandwidth

网络分析工具：

+ netstat和tcpstat	网络状态报告	
+ tcpdump	网络包分析	
+ ss	socket statistics	
+ iftop	网络带宽分析	
+ sockperf		
+ ethstats		

Linux设备
------------------------------------------------

+ https://wiki.archlinux.org/title/Device_file
+ https://linuxjourney.com/lesson/dev-directory
+ https://www.debian.org/releases/buster/amd64/apds01.en.html
+ https://www.baeldung.com/linux/dev-directory

kvm、qemu和libvirt
------------------------------------------------
空

fwupdmgr
------------------------------------------------
空


NFS
------------------------------------------------
空

samba
------------------------------------------------

挂载samba
``sudo mount -t cifs -o "user=zd04,passwd=123456,uid=1001,gid=1001"  //ip/share/``


开发环境
------------------------------------------------

安装openGL开发环境:

+ OpenGL Library ``sudo apt install libgl1-mesa-dev``
+ OpenGL Utilities ``sudo apt install libglu1-mesa-dev``
+ OpenGL Utility Toolkit ``sudo apt install freeglut3-dev``