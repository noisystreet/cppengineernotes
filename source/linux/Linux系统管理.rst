Linux系统管理
=================

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