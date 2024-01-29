=============
CUDA编程
=============

简介
------------------------------------------------

支持CUDA的GPU： https://developer.nvidia.com/cuda-gpus

CUDA的软件栈：https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html

基础环境配置
------------------------------------------------

CUDA基础软件栈由 ``CUDA driver`` （包括GPU kernel mode driver和CUDA usermode driver）、 ``CUDA Toolkit`` 和 ``runtime`` 组成。
要运行CUDA程序，至少要有 ``CUDA driver`` 和 ``CUDA runtime`` ，要开发CUDA程序，还要有 ``CUDA Toolkit``

Linux下CUDA环境配置
````````````````````````````````````````````````

CUDA下载：https://developer.nvidia.com/cuda-toolkit-archive

首先查看是否有NVIDIA显卡：

.. code-block:: bash

    lspci | grep VGA

如果输出中发现 ``NVIDIA`` 字样，说明系统识别到了NVIDIA的GPU硬件，例如：

.. code-block:: bash

    00:02.0 VGA compatible controller: Intel Corporation CometLake-S GT2 [UHD Graphics 630] (rev 03)
    01:00.0 VGA compatible controller: NVIDIA Corporation TU117M (rev a1)

不同Linux环境的下CUDA的安装可以参考：https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html

在 ``ubuntu/debian`` 环境中，可以使用下面的步骤安装CUDA:

#. 添加contrib源（只有debian需要执行这一步）

.. code-block:: bash

    sudo apt install software-properties-common
    sudo add-apt-repository contrib

#. 添加GPG key

.. code-block:: bash

    distro=ubuntu2204 #或debian11
    arch=x86_64
    wget https://developer.download.nvidia.com/compute/cuda/repos/$distro/$arch/cuda-keyring_1.0-1_all.deb
    sudo dpkg -i cuda-keyring_1.0-1_all.deb

    #安装
    sudo apt update
    sudo apt -y install cuda  #安装软件源中最新版本的CUDA软件栈

#. 软件源中也包含了cudnn，可以同时安装

#. 设置环境变量：

.. code-block:: bash

    export CUDA_PATH=/usr

也可以下载独立安装包进行安装，以CUDA11.4为例：

.. code-block:: bash

    wget https://developer.download.nvidia.com/compute/cuda/11.4.0/local_installers/cuda_11.4.0_470.42.01_linux.run
    sudo <CudaInstaller>.run 

安装好了之后设置 ``CUDA_HOME`` 环境变量，指向cuda安装目录，并设置 ``PATH`` 和 ``LD_LIBRARY_PATH`` 环境变量：

.. code-block:: bash

    export PATH=$CUDA_HOME/bin:$PATH
    export LD_LIBRARY_PATH=$CUDA_HOME/bin:$LD_LIBRARY_PATH

执行 ``nvcc --version`` 查看是否安装成功，典型输出如下：

.. code-block:: bash

    nvcc: NVIDIA (R) Cuda compiler driver
    Copyright (c) 2005-2023 NVIDIA Corporation
    Built on Fri_Jan__6_16:45:21_PST_2023
    Cuda compilation tools, release 12.0, V12.0.140
    Build cuda_12.0.r12.0/compiler.32267302_0

安装 ``nvidia-smi``，用 ``nvidia-smi`` 查看GPU信息，典型的输出如下：

.. code-block:: bash

    +---------------------------------------------------------------------------------------+
    | NVIDIA-SMI 530.30.02              Driver Version: 530.30.02    CUDA Version: 12.1     |
    |-----------------------------------------+----------------------+----------------------+
    | GPU  Name                  Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf            Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |                                         |                      |               MIG M. |
    |=========================================+======================+======================|
    |   0  NVIDIA GeForce GTX 1650         On | 00000000:01:00.0 Off |                  N/A |
    | N/A   42C    P8                3W /  50W|      1MiB /  4096MiB |      0%      Default |
    |                                         |                      |                  N/A |
    +-----------------------------------------+----------------------+----------------------+
                                                                                             
    +---------------------------------------------------------------------------------------+
    | Processes:                                                                            |
    |  GPU   GI   CI        PID   Type   Process name                            GPU Memory |
    |        ID   ID                                                             Usage      |
    |=======================================================================================|
    |  No running processes found                                                           |
    +---------------------------------------------------------------------------------------+

在linux开发CUDA程序可以使用eclipse+nvidia nsight，后者可从CUDA安装目录下找到。

注意CUDA需要和特定版本的驱动、编译器结合使用，版本不匹配可能会出问题，

CUDA的兼容性： https://docs.nvidia.com/deploy/cuda-compatibility/index.html

参考 `cuDNN Support Matrix <https://docs.nvidia.com/deeplearning/cudnn/archives/index.html>`_，以安装正确的gcc/CUDA/cuDNN版本组合。

Windows下CUDA环境配置
````````````````````````````````````````````````

Windows：使用vs2017和cuda10
安装完成后，在系统的环境变量里可以看到，CUDA自动添加了以下环境变量：

.. code-block:: powershell

    CUDA_PATH
    CUDA_PATH_V10

并且已经将以下路径添加到了PATH：

.. code-block:: powershell

    %CUDA_PATH%\bin
    %CUDA_PATH%\libnvvp

进入 ``%CUDA_PATH%/extras/demo_suite`` 目录，在终端分别运行 ``deviceQuery.exe`` 和 ``bandwidthTest.exe`` ，若输出结果均为 PASS，表明CUDA已经安装成功。

例子：

在VS中新建一个CUDA项目，然后会自动产生一个 ``kernel.cu`` 文件，直接生成解决方案，然后运行，
这是一个矢量加法的例子，在使用VS2010编译CUDA程序时，可能遇到如下所示的C4819警告：

.. code-block:: bash

    warning C4819:The file contains a character that cannot be represented in the current
    codepage (936). Save the file in Unicode format to prevent data loss；

这个警告的意思是：在该文件中有一个或多个字符不是Unicode字符。要求把这个字符变成Unicode字符防止数据丢失。这个警告跟代码本身无关，不会影响代码运行，但刷屏的warning使得对程序debug变得困难起来。

解决方法：在 项目->属性 -> 配置属性 -> CUDA C/C++ ->Command Line的“其他选项”中添加：

.. code-block:: bash

    -Xcompiler "/wd 4819"

从编译过程的命令行输出可以看出，编译CUDA程序时，使用的是 ``nvcc`` 来进行编译，而非vs内置的编译程序。

cuDNN离线安装
````````````````````````````````````````````````

下载安装包（需要先注册登录nvidia账号）

.. code-block:: bash
    :linenos:

    tar -xvf cudnn-linux-x86_64-*.tar.xz
    sudo cp cudnn-*-archive/include/cudnn*.h /usr/local/cuda/include 
    sudo cp -P cudnn-*-archive/lib/libcudnn* /usr/local/cuda/lib64 
    sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*

pip安装cuda-python相关包

https://pypi.org/search/?q=nvidia

以CUDA11为例，常用的包有：

.. code-block:: bash
    :linenos:

    nvidia-cublas-cu11
    nvidia-cuda-nvrtc-cu11
    nvidia-cuda-runtime-cu11
    nvidia-cudnn-cu11

常用工具命令
````````````````````````````````````````````````

``nvidia-smi`` 命令

.. code-block:: bash

    nvidia-smi topo -m          #查看GPU和CPU和拓扑连接方式
    nvidia-smi -L               #列出所有GPU设备
    nvidia-smi --help-query-gpu #查看--query-gpu的所有可选参数

多个查询：

.. code-block:: bash

    nvidia-smi --query-gpu=timestamp,name,pci.bus_id,driver_version,pstate,pcie.link.gen.max,\
        pcie.link.gen.current,temperature.gpu,utilization.gpu,\
        utilization.memory,memory.total,memory.free,memory.used --format=csv -l 1

参考：

+ `Explained Output of Nvidia-smi Utility <https://medium.com/analytics-vidhya/explained-output-of-nvidia-smi-utility-fc4fbee3b124>`_
+ `nvidia-smi Cheat Sheet <https://www.seimaxim.com/kb/gpu/nvidia-smi-cheat-sheet>`_
+ `GPU Management and Monitoring <https://xcat-docs.readthedocs.io/en/2.16.2/advanced/gpu/nvidia/management.html>`_

``nvidia-settings`` 命令：

.. code-block:: bash

    nvidia-settings -q gpus -t #查询GPU的数目
    nvidia-settings -q CUDACores -t #查询GPU中CUDA core的数目
    nvidia-settings -q PCIEGen -t #查看PCIE接口
    nvidia-settings -q GpuUUID -t #查看GPU的uuid

入门例子
------------------------------------------------

从 `https://github.com/NVIDIA/cuda-samples <https://github.com/NVIDIA/cuda-samples>`_ 可以下载cuda的一些例子:

.. code-block:: bash

    git clone https://github.com/NVIDIA/cuda-samples.git
    #切换成与当前CUDA环境一致的代码版本
    version=v11.8
    git checkout $version && git switch -c $version
    #安装依赖项
    sudo apt install libopenmpi-dev -y
    #编译
    make -j

编译之后，可以先运行两个demo程序来检查一下CUDA是否可用。
生成的可执行文件在 ``bin/x86_64/linux/release`` 目录下

#. 查询设备信息 ``deviceQuery``

进入 ``bin/x86_64/linux/release`` 目录，执行 ``deviceQuery`` 程序，运行之后，典型输出如下：
  
.. code-block:: bash

    ./deviceQuery Starting...

    CUDA Device Query (Runtime API) version (CUDART static linking)

    Detected 1 CUDA Capable device(s)

    Device 0: "NVIDIA GeForce GTX 1650"
    CUDA Driver Version / Runtime Version          12.1 / 11.8
    CUDA Capability Major/Minor version number:    7.5
    Total amount of global memory:                 3904 MBytes (4093509632 bytes)
    (014) Multiprocessors, (064) CUDA Cores/MP:    896 CUDA Cores
    GPU Max Clock rate:                            1515 MHz (1.51 GHz)
    Memory Clock rate:                             6001 Mhz
    Memory Bus Width:                              128-bit
    L2 Cache Size:                                 1048576 bytes

    ......

    deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 12.1, CUDA Runtime Version = 11.8, NumDevs = 1
    Result = PASS

可以看出该GPU有896个 ``CUDA core`` ，最后的 ``Result=PASS`` 表明运行没有问题。

#. 带宽测试 ``bandwidthTest``

进入 ``bin/x86_64/linux/release`` 目录，执行 ``bandwidthTest`` 程序，输出如下：

.. code-block:: bash

    [CUDA Bandwidth Test] - Starting...
    Running on...

    Device 0: NVIDIA GeForce GTX 1650
    Quick Mode

    Host to Device Bandwidth, 1 Device(s)
    PINNED Memory Transfers
        Transfer Size (Bytes)	Bandwidth(GB/s)
        32000000			6.2

    Device to Host Bandwidth, 1 Device(s)
    PINNED Memory Transfers
        Transfer Size (Bytes)	Bandwidth(GB/s)
        32000000			6.5

    Device to Device Bandwidth, 1 Device(s)
    PINNED Memory Transfers
        Transfer Size (Bytes)	Bandwidth(GB/s)
        32000000			169.8

    Result = PASS

可以看到H2D、D2H和D2D的带宽数据。

GPU硬件和执行模型
------------------------------------------------


GPU的内存层次:

+ Register
+ L1/Shared memory (SMEM)
+ Read-only memory
+ L2 cache
+ Global memory

参考

+ `warp深度解析 <https://blog.51cto.com/u_15127500/3641722>`_
+ `Warp Scheduling and Divergence <https://cse.iitkgp.ac.in/~soumya/hp3/slides/warp-divr.pdf>`_
+ `CUDA Refresher <https://developer.nvidia.com/blog/tag/cuda-refresher>`_

CUDA API
------------------------------------------------

CUDA API可以分为 ``driver API`` 和 ``runtime API`` ，对应的函数分别以 ``cu`` 和 ``cuda`` 开头， ``driver API`` 是更加偏底层的接口。一般使用 ``runtime API`` 即可。下面介绍的均为 ``runtime API`` 。

一些概念
````````````````````````````````````````````````

+ ``grid`` 一个kernel所启动的所有线程称为一个网格
+ ``block`` grid由三维结构的block组成
+ ``thread`` 一个block由多个线程组成

grid、block和thread都是软件逻辑层面的概念。CUDA的设备在实际执行过程中，会以block为单位。把一个个block分配给SM进行运算；而block中的thread又会以warp（线程束）为单位，对thread进行分组计算。目前CUDA的warp大小都是32，也就是说32个thread会被组成一个warp来一起执行。同一个warp中的thread执行的指令是相同的，只是处理的数据不同。

基本上warp 分组的动作是由SM自动进行的，会以连续的方式来做分组。比如说如果有一个block 里有128 个thread 的话，就会被分成四组warp，实际上，warp 也是CUDA 中每一个SM 执行的最小单位；
kernel在调用时必须通过 ``<<<grid, block>>>`` 来指定kernel所使用的线程数及结构。
可以使用nvprof分析CUDA程序中的函数的执行开销

+ `CUDA 深入理解threadIdx <https://www.cnblogs.com/zzzsj/p/14866103.html>`_

CUDA程序和编译
````````````````````````````````````````````````

编译时一定要根据硬件的 ``compute capability`` 设置匹配的编译选项，否则可能计算结果错误。

由于GPU是异构模型，需要区分host和device上的代码，在CUDA中对C语言进行的扩展，通过函数类型修饰符开区别host和device上的函数，主要的三个函数类型修饰符如下：

+ ``__global__`` 从host调用，在device上执行，（一些特定的GPU也可以从device上调用），返回类型必须是 ``void`` ，不支持可变参数参数，不能是类的成员函数。用 ``__global__`` 定义的kernel函数是异步的，这意味着host不会等待kernel执行完就执行下一步。
+ ``__device__`` 从device调用，在device上执行，且只能，不可以和 ``__global__`` 同时用。
+ ``__host__`` 从host上调用，在host上执行，一般省略不写，不可以和 ``__global__`` 同时用，但可和 ``__device__`` 同时用，此时函数会在device和host都编译。

变量定义：

+ ``__shared__`` ：用来定义共享内存变量
+ ``__constant__`` ：用来定义常量内存
+ thread_local变量，定义在kernel函数内，被线程私有。
  
kernel函数内可以使用一些c++11语法，如 ``auto``
内置 ``dim3`` 结构体和 ``uint3`` 结构体：

.. code-block:: c++
    :linenos:

    struct __device_builtin__ uint3
    {
        unsigned int x, y, z;
    };
    struct __device_builtin__ dim3
    {
        unsigned int x, y, z;
    #if defined(__cplusplus)
    #if __cplusplus >= 201103L
        __host__ __device__ constexpr dim3(unsigned int vx = 1, unsigned int vy = 1, unsigned int vz = 1) : x(vx), y(vy), z(vz) {}
        __host__ __device__ constexpr dim3(uint3 v) : x(v.x), y(v.y), z(v.z) {}
        __host__ __device__ constexpr operator uint3(void) const { return uint3{x, y, z}; }
    #else
        __host__ __device__ dim3(unsigned int vx = 1, unsigned int vy = 1, unsigned int vz = 1) : x(vx), y(vy), z(vz) {}
        __host__ __device__ dim3(uint3 v) : x(v.x), y(v.y), z(v.z) {}
        __host__ __device__ operator uint3(void) const { uint3 t; t.x = x; t.y = y; t.z = z; return t; }
    #endif
    #endif /* __cplusplus */
    };

一些内置变量
````````````````````````````````````````````````

+ ``gridDim``
+ ``blockDim``
+ ``blockIdx`` 线程块的索引
+ ``threadIdx`` 线程块内线程的索引
+ ``warpSize``

这些内置变量常用于在kernel函数中获取线程和blockID。


常用头文件：

.. code-block:: c++

    #include <cuda_runtime.h>
    #include <device_launch_parameters.h>


设备管理
````````````````````````````````````````````````

.. code-block:: c++

    __host__            cudaError_t cudaGetDeviceProperties(cudaDeviceProp *prop, int device)
    __host__ __device__ cudaError_t cudaGetDeviceCount (int* count)
    __host__ __device__ cudaError_t cudaGetDevice(int* device)
    __host__            cudaError_t cudaSetDevice(int device)
    __host__ __device__ cudaError_t cudaDeviceSynchronize(void)
    __host__            cudaError_t cudaDeviceReset(void)

内存管理
````````````````````````````````````````````````

.. code-block:: c++

    __host__            cudaError_t cudaMemGetInfo(size_t* free, size_t* total)
    //memset
    __host__            cudaError_t cudaMemset(void* devPtr, int  value, size_t count)
    __host__ __device__ cudaError_t cudaMemsetAsync(void* devPtr, int  value, size_t count, cudaStream_t stream = 0)
    //malloc
    __host__ __device__ cudaError_t cudaMalloc(void** devPtr, size_t size) 
    __host__            cudaError_t cudaMallocManaged(void** devPtr, size_t size, unsigned int  flags = cudaMemAttachGlobal) 
    __host__            cudaError_t cudaMallocPitch(void** devPtr, size_t* pitch, size_t width, size_t height) 
    __host__            cudaError_t cudaHostAlloc(void** pHost, size_t size, unsigned int  flags)
    //memcpy 
    __host__            cudaError_t cudaMemcpy(void* dst, const void* src, size_t count, cudaMemcpyKind kind) 
    __host__ __device__ cudaError_t cudaMemcpyAsync(void* dst, const void* src, size_t count, cudaMemcpyKind kind, cudaStream_t stream = 0) 
    __host__            cudaError_t cudaMemPrefetchAsync(const void* devPtr, size_t count, int  dstDevice, cudaStream_t stream = 0) 
    __host__            cudaError_t cudaMemcpyToSymbol(const void* symbol, const void* src, size_t count, size_t offset = 0, cudaMemcpyKind kind = cudaMemcpyHostToDevice) 
    //free
    __host__ __device__ cudaError_t cudaFree(void* devPtr) 
    __host__            cudaError_t cudaFreeHost(void* ptr) 


共享内存 ``__shared__``

常量内存 ``__constant__``

事件管理
````````````````````````````````````````````````

.. code-block:: c++
    
    __host__            cudaError_t cudaEventCreate(cudaEvent_t* event)
    __host__ __device__ cudaError_t cudaEventCreateWithFlags(cudaEvent_t* event, unsigned int  flags)
    __host__ __device__ cudaError_t cudaEventDestroy(cudaEvent_t event)
    __host__            cudaError_t cudaEventElapsedTime(float* ms, cudaEvent_t start, cudaEvent_t end)
    __host__            cudaError_t cudaEventQuery(cudaEvent_t event)
    __host__ __device__ cudaError_t cudaEventRecord(cudaEvent_t event, cudaStream_t stream = 0)
    __host__            cudaError_t cudaEventRecordWithFlags(cudaEvent_t event, cudaStream_t stream = 0, unsigned int  flags = 0)
    __host__            cudaError_t cudaEventSynchronize(cudaEvent_t event) 

流管理
````````````````````````````````````````````````

.. code-block:: c++

    __host__            cudaError_t cudaStreamCreate(cudaStream_t* pStream) 
    __host__ __device__ cudaError_t cudaStreamDestroy(cudaStream_t stream) 
    __host__ __device__ cudaError_t cudaStreamCreateWithFlags(cudaStream_t* pStream, unsigned int  flags) 
    __host__            cudaError_t cudaStreamGetId(cudaStream_t hStream, unsigned long long* streamId) 
    __host__            cudaError_t cudaStreamQuery(cudaStream_t stream) 
    __host__            cudaError_t cudaStreamSynchronize(cudaStream_t stream) 
    __host__ __device__ cudaError_t cudaStreamWaitEvent(cudaStream_t stream, cudaEvent_t event, unsigned int  flags = 0) 

错误处理
````````````````````````````````````````````````
.. code-block:: c++

    cudaError_t 枚举
    cudaGetLastError()
    cudaGetErrorString()

更多例子
------------------------------------------------

数组相加
````````````````````````````````````````````````

矩阵乘法
````````````````````````````````````````````````

+ https://bluewaters.ncsa.illinois.edu/liferay-content/image-gallery/content/BLA-final
+ https://www.quantstart.com/articles/Matrix-Matrix-Multiplication-on-the-GPU-with-Nvidia-CUDA/
+ 矩阵乘法的 CUDA 实现、优化及性能分析
 
event
````````````````````````````````````````````````

https://www.bbsmax.com/A/mo5k6k1LJw/
CUDA  events可以用来控制同步，包括cpu/gpu的同步、gpu上不同engine的同步和gpu之间的同步。
此外，Event可以用来检查gpu的操作时长。它能够向CUDA  stream进行记录（record），cpu会等待event记录的这个地方完成才能执行下一步。所以Event可以统计GPU上面某一个任务或者代码段的精确运行时间。

.. code-block:: cuda
    :linenos:

    cudaEvent_t start_k1, stop_k1,
    //创建event
    cudaEventCreate(&start_k1);
    cudaEventCreate(&start_k2);

    cudaEventRecord(start_k1);
    ... //some device code
    cudaEventRecord(stop_k1);
    //计算时间之前进行event sync
    cudaEventSynchronize(start_k1);
    cudaEventSynchronize(stop_k1);
    cudaEventElapsedTime(&milliseconds_k1, start_k1, stop_k1);
    //销毁event
    cudaEventDestroy(start_k1)
    cudaEventDestroy(stop_k1)

stream
````````````````````````````````````````````````

#. https://developer.nvidia.com/blog/gpu-pro-tip-cuda-7-streams-simplify-concurrency/
#. https://lulaoshi.info/gpu/python-cuda/streams.html

CUDA streams用来管理执行单元的并发操作，在一个流中，操作是串行的按序执行的，但是在不同的流中操作就可以同时执行。前面的block和thread用于kernel内的并行，

由于异构计算的硬件特性，CUDA中以下操作是相互独立的：
+ 主机端上的计算
+ 设备端的计算（核函数）
+ 数据从主机和设备间相互拷贝
+ 数据从设备内拷贝或转移
+ 数据从多个GPU设备间拷贝或转移
  
针对这种互相独立的硬件架构，CUDA使用多流作为一种高并发的方案：把一个大任务中的上述几部分拆分开，放到多个流中，每次只对一部分数据进行拷贝、计算和回写，并把这个流程做成流水线。因为数据拷贝不占用计算资源，计算不占用数据拷贝的总线（Bus）资源，因此计算和数据拷贝完全可以并发执行。将数据拷贝和函数计算重叠起来，形成流水线，能获得非常大的性能提升。
通过使用stream，则可以实现：

+ 多个kernel的并发
+ kernel计算和数据拷贝的重叠
+ CPU和GPU的并发
+ 多GPU的并发

例子，memcpy和kernel执行分别在四个stream中并发执行：

.. code-block:: bash
    :linenos:

    cudaStream_t stream1, stream2, stream3, stream4 ;
    cudaStreamCreate(&stream1) ;
    cudaStreamCreate(&stream2) ;

    ...
    cudaMalloc(&dev1, size) ;
    cudaMallocHost(&host1, size) ;
    …
    cudaMemcpyAsync(dev1, host1, size, H2D, stream1) ;
    kernel2 <<< grid, block, 0, stream2 >>>(…, dev2, …) ;
    kernel3 <<< grid, block, 0, stream3 >>>(…, dev3, …) ;
    cudaMemcpyAsync(host4, dev4, size, D2H, stream4) ;

在cuda7之前，没有显式指定流，会隐式指定一个空流（默认流），它要同步设备上的所有操作。一个设备会产生一个空流。其它流的工作完成之后空流的工作才能开始，空流工作完成后其它流才能开始。cuda7版本增加了新的特性，可以选择每一个主机线程使用独立的空流，即一个线程一个空流，避免了原来空流的按序执行。

启动每个线程一个空流的方法:

#. 方法1

    .. code-block:: bash

        nvcc --default-stream per-thread

#. 方法2，在include CUDA头文件前加入以下内容

    .. code-block:: c++

        #define CUDA_API_PER_THREAD_DEFAULT_STREAM

CUDA instrinsics
````````````````````````````````````````````````

可以方便地实现一些常用操作，如fp16和bf16类型的数学函数，SIMD函数调用等等

+ https://ion-thruster.medium.com/an-introduction-to-writing-fp16-code-for-nvidias-gpus-da8ac000c17f
+ https://docs.nvidia.com/cuda/cuda-math-api/index.html

Tensor core编程
````````````````````````````````````````````````
空

其他常用库
------------------------------------------------

cuDNN
````````````````````````````````````````````````

基本概念

+ ``cuDNN handle`` create/destroy
+ ``tensor descriptor`` 3D、4D、5D、XYWZ等等

3D tensor的layout为BMN，B为batch size,b=1时即GEMM操作。
4D tensor的常用layout有NCHW、NHWC、CHWN。
5D tensor的常用layout有NCDHW、 NDHWC、CDHWN。

卷积：cudnn支持NCHW、NHWC、NC/32HW32。
matmul：使用3维tensor，即BMN，layout有：(1)Packed Row-major: dim [B,M,N] with stride [MN, N, 1], （2）Packed Column-major: dim [B,M,N] with stride [MN, 1, M]

+ ``tensor core`` 算子：卷积、RNN、Multi-Head Attention

tensor core的一些注意点：

+ Make sure that the convolution operation is eligible for Tensor Cores by  avoiding any combinations of large padding and large filters.                               
+ Transform the inputs and filters to NHWC, pre-pad channel and batch size to be a multiple of 8.                               
+ Make sure that all user-provided tensors, workspace, and reserve space are  aligned to 128-bit boundaries. Note that 1024-bit alignment may deliver better performance.  

精度：

For FP16 data, Tensor Cores operate on FP16 input, output in FP16, and may accumulate in FP16 or FP32. 如果最后需要的是fp16的输出，会将fp32进行转换，保证更高精度。

                
Graph API

分为front end和backend：

#. `cuDNN frontend <https://github.com/NVIDIA/cudnn-frontend>`_
#. `cuDNN backend <https://docs.nvidia.com/deeplearning/cudnn/api/index.html#cudnn-backend-api>`_
#. `New features and application from cuDNN V8 <https://medium.com/@billchenxi/cudnn-v8-2020-4-8-gtc-5a86365d33c3>`_

重要概念：

+ ``operation`` 和 ``operation graph``
+ ``engine`` 和 ``engine config``
+ ``Heuristics`` 启发式搜索，A heuristic is a way to get a list of engine configurations that are intended to be sorted from the most performant to least performant for the given operation graph


cuDNN文档

+ https://docs.nvidia.com/deeplearning/cudnn/developer-guide/index.html
+ https://medium.com/@rohitdwivedula/minimal-cudnn-c-hello-world-example
+ https://github.com/tbennun/cudnn-training
+ https://pypi.org/project/cudnn-python-wrappers/
+ https://developer.nvidia.com/blog/cuda-graphs/
+ https://nvidia.github.io/cudnn-frontend/

cuBLAS
````````````````````````````````````````````````

cuSparse
````````````````````````````````````````````````

Thrust
````````````````````````````````````````````````

Thrust是一个基于CUDA的类似c++STL的库，封装了各种常用的容器和算法

+ https://github.com/NVIDIA/thrust
+ https://thrust.github.io/


+ https://www.shuzhiduo.com/A/kmzLNoBY5G/
+ https://blog.csdn.net/Megvii_tech/article/details/122053556

多GPU编程
------------------------------------------------
空


参考文档
------------------------------------------------

+ CUDA python https://nvidia.github.io/cuda-python/index.html
+ https://docs.nvidia.com/deeplearning/performance/
+ https://docs.nvidia.com/cuda/
+ https://developer.nvidia.com/blog/?tags=accelerated-computing
+ https://developer.nvidia.com/cuda-faq
+ https://developer.nvidia.com/cuda-education-training
+ https://developer.nvidia.com/gpu-accelerated-libraries
+ Compiling CUDA with clang https://llvm.org/docs/CompileCudaWithLLVM.html
+ Rocm https://sep5.readthedocs.io/en/latest/index.html
+ https://developer.nvidia.com/zh-cn/blog/nvidia-ampere-architecture-in-depth/
+ GPU 兼容性的那些事 http://wsfdl.com/kubernetes/2019/05/08/versions_in_gpu.html
+ CUDATutorial https://cuda-tutorial.github.io/index.html
+ Matching CUDA arch and CUDA gencode for various NVIDIA architectures https://arnon.dk/matching-sm-architectures-arch-and-gencode-for-various-nvidia-cards/
+ https://carpentries-incubator.github.io/lesson-gpu-programming/
+ CUDA — Memory Model https://medium.com/analytics-vidhya/cuda-memory-model-823f02cef0bf
+ GPU Programming http://courses.cms.caltech.edu/cs179/

硬件规格说明
````````````````````````````````````````````````
#. H100 https://resources.nvidia.com/en-us-tensor-core/nvidia-tensor-core-gpu-datasheet
#. A100	https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/a100/pdf/nvidia-a100-datasheet-us-nvidia-1758950-r4-web.pdf
#. RTX A4000 https://www.nvidia.com/content/dam/en-zz/Solutions/gtcs21/rtx-a4000/nvidia-rtx-a4000-datasheet.pdf
#. RTX 3090	https://www.nvidia.com/en-us/geforce/graphics-cards/30-series/rtx-3090-3090ti/
#. https://www.techpowerup.com/gpu-specs/geforce-rtx-3090.c3622
