=============
优秀C++库赏析
=============

protobuffer
------------------------------------------------

+ https://developers.google.com/protocol-buffers
+ https://github.com/protocolbuffers/protobuf
+ https://www.tizi365.com/archives/374.html

ubuntu安装：

.. code-block:: bash
    :linenos:

    sudo apt install libprotobuf-dev protobuf-compiler

打印编译和链接选项：

.. code-block:: bash
    :linenos:

    pkg-config --cflags --libs protobuf

主要步骤：

#. 编写.proto文件
#. 使用protoc将.proto文件编译成其他语言源文件

如使用 `protoc --cpp_out=. xx.proto` ，可以在当前目录生成 `xx.pb.h` 文件和 `xx.pb.cc` 文件。
`--java_out` 和 `--python_out` 同理

#. 引用生成的源文件

`.proto` 文件中，字段后面的序号不能重复，定义了就不能修改，可以理解成字段的唯一ID。

cmake引用protobuf：

.. code-block:: cmake
    :linenos:

    find_package(Protobuf REQUIRED)
    include_directories(${Protobuf_INCLUDE_DIRS})
    include_directories(${CMAKE_CURRENT_BINARY_DIR})
    protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS foo.proto)
    protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS EXPORT_MACRO DLL_EXPORT foo.proto)
    protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS DESCRIPTORS PROTO_DESCS foo.proto)protobuf_generate_python(PROTO_PY foo.proto)
    add_executable(bar bar.cc ${PROTO_SRCS} ${PROTO_HDRS})
    target_link_libraries(bar ${Protobuf_LIBRARIES})


其他
------------------------------------------------

+ 并行计算 MPI OpenMP kokkos
+ 异构加速	CUDA ROCm OpenCL SYCL openAcc
+ 游戏开发	AV3D cocos2d-x KlayGE Unity3D DirectX
+ 界面开发	GTK OpenGL MFC QT WxWindows WTL
+ 多线程	ZThreads pthread
+ 日志	https://github.com/google/glog

+ Boost C++准标准库 http://www.boost.org/ 
+ eigen C++线性代数库 http://eigen.tuxfamily.org/index.php?title=Main_Page
+ wxwidgets 跨平台界面库 http://www.wxwidgets.org/
+ catch 单元测试工具 https://github.com/philsquared/Catch 
+ libevent 高性能网络库 http://libevent.org/ 
+ ACE 重量级网络库 http://www.cs.wustl.edu/schmidt/ACE.html 