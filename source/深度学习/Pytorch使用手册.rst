Pytorch使用手册
==================================

安装
------------------------------------------------

直接安装
````````````````````````````````````````````````

步骤：https://pytorch.org/get-started/locally/

简单验证：

.. code-block:: ipython

    import torch
    x = torch.rand(5, 3)
    print(x)

查看编译选项：

.. code-block:: ipython

    print(torch.__config__.show())

典型输出如下：

.. code-block:: bash

    PyTorch built with:
    - GCC 9.3
    - C++ Version: 201402
    - Intel(R) Math Kernel Library Version 2020.0.0 Product Build 20191122 for Intel(R) 64 architecture applications
    - Intel(R) MKL-DNN v2.6.0 (Git Hash 52b5f107dd9cf10910aaa19cb47f3abf9b349815)
    - OpenMP 201511 (a.k.a. OpenMP 4.5)
    - LAPACK is enabled (usually provided by MKL)
    - NNPACK is enabled
    - CPU capability usage: AVX2
    - CUDA Runtime 11.7
    - NVCC architecture flags: -gencode;arch=compute_37,code=sm_37;-gencode;arch=compute_50,code=sm_50;-gencode;arch=compute_60,code=sm_60;-gencode;arch=compute_70,code=sm_70;-gencode;arch=compute_75,code=sm_75;-gencode;arch=compute_80,code=sm_80;-gencode;arch=compute_86,code=sm_86
    - CuDNN 8.5
    - Magma 2.6.1
    - Build settings: BLAS_INFO=mkl, BUILD_TYPE=Release, CUDA_VERSION=11.7, CUDNN_VERSION=8.5.0, CXX_COMPILER=/opt/rh/devtoolset-9/root/usr/bin/c++, CXX_FLAGS= -fabi-version=11 -Wno-deprecated -fvisibility-inlines-hidden -DUSE_PTHREADPOOL -fopenmp -DNDEBUG -DUSE_KINETO -DUSE_FBGEMM -DUSE_QNNPACK -DUSE_PYTORCH_QNNPACK -DUSE_XNNPACK -DSYMBOLICATE_MOBILE_DEBUG_HANDLE -DEDGE_PROFILER_USE_KINETO -O2 -fPIC -Wno-narrowing -Wall -Wextra -Werror=return-type -Werror=non-virtual-dtor -Wno-missing-field-initializers -Wno-type-limits -Wno-array-bounds -Wno-unknown-pragmas -Wunused-local-typedefs -Wno-unused-parameter -Wno-unused-function -Wno-unused-result -Wno-strict-overflow -Wno-strict-aliasing -Wno-error=deprecated-declarations -Wno-stringop-overflow -Wno-psabi -Wno-error=pedantic -Wno-error=redundant-decls -Wno-error=old-style-cast -fdiagnostics-color=always -faligned-new -Wno-unused-but-set-variable -Wno-maybe-uninitialized -fno-math-errno -fno-trapping-math -Werror=format -Werror=cast-function-type -Wno-stringop-overflow, LAPACK_INFO=mkl, PERF_WITH_AVX=1, PERF_WITH_AVX2=1, PERF_WITH_AVX512=1, TORCH_VERSION=1.13.1, USE_CUDA=ON, USE_CUDNN=ON, USE_EXCEPTION_PTR=1, USE_GFLAGS=OFF, USE_GLOG=OFF, USE_MKL=ON, USE_MKLDNN=ON, USE_MPI=OFF, USE_NCCL=ON, USE_NNPACK=ON, USE_OPENMP=ON, USE_ROCM=OFF,

查看torch包的编译选项：

.. code-block:: ipython

    import pprint
    pprint.pprint(torch.__config__._cxx_flags().split())

输出：

.. code-block:: bash

    ['-D_GLIBCXX_USE_CXX11_ABI=0',
    '-fabi-version=11',
    '-Wno-deprecated',
    '-fvisibility-inlines-hidden',
    '-DUSE_PTHREADPOOL',
    '-DNDEBUG',
    '-DUSE_KINETO',
    '-DLIBKINETO_NOROCTRACER',
    '-DUSE_FBGEMM',
    '-DUSE_QNNPACK',
    '-DUSE_PYTORCH_QNNPACK',
    '-DUSE_XNNPACK',
    '-DSYMBOLICATE_MOBILE_DEBUG_HANDLE',
    '-O2',
    '-fPIC',
    '-Wall',
    '-Wextra',
    '-Werror=return-type',
    '-Werror=non-virtual-dtor',
    '-Werror=bool-operation',
    '-Wnarrowing',
    '-Wno-missing-field-initializers',
    '-Wno-type-limits',
    '-Wno-array-bounds',
    '-Wno-unknown-pragmas',
    '-Wunused-local-typedefs',
    '-Wno-unused-parameter',
    '-Wno-unused-function',
    '-Wno-unused-result',
    '-Wno-strict-overflow',
    '-Wno-strict-aliasing',
    '-Wno-error=deprecated-declarations',
    '-Wno-stringop-overflow',
    '-Wno-psabi',
    '-Wno-error=pedantic',
    '-Wno-error=redundant-decls',
    '-Wno-error=old-style-cast',
    '-fdiagnostics-color=always',
    '-faligned-new',
    '-Wno-unused-but-set-variable',
    '-Wno-maybe-uninitialized',
    '-fno-math-errno',
    '-fno-trapping-math',
    '-Werror=format',
    '-Werror=cast-function-type',
    '-Wno-stringop-overflow'

CUDA相关：

.. code-block:: python

    import torch
    torch.cuda.is_available()           #检查CUDA是否可用
    torch.cuda.is_bf16_supported()      #检查GPU是否支持bfloat16
    torch.cuda.device_count()           #GPU数目
    torch.cuda.get_arch_list()          #打印计算力
    torch.cuda.get_device_capability()  #打印计算力,返回的是tuple，如(7,5)
    torch.cuda.get_device_name()        #设备名称
    torch.get_autocast_gpu_dtype()

CPU相关：

.. code-block:: python

    torch.get_default_dtype()       #默认数据类型
    torch.get_num_threads()         #线程数目
    torch.get_num_interop_threads() #op间线程数目

编译安装
````````````````````````````````````````````````

源码：https://github.com/pytorch/pytorch

环境：
软件	版本	备注

+ python	3.10	conda环境
+ gcc	11.3	
+ CUDA Toolkit	11.8.0
+ cuDNN	8.6.0.163
+ pytorch	2.0	release/2.0分支
+ torchvision	0.15.1	tag：v0.15.1
  
一些可选项：

.. code-block:: bash
    :linenos:

    sudo add-apt-repository non-free #对于debian，安装mkl之前需要添加non-free源
    sudo apt install libgmp-dev libmpfr-dev libfftw3-dev libnuma-dev intel-mkl-full clang ccache doxygen libssl-dev


使用conda创建基础python环境：

.. code-block:: bash
    :linenos:

    conda create -n ptdbg && conda activate ptdbg
    conda install pip

安装依赖包，在pytorch源码目录下执行：

.. code-block:: bash
    :linenos:

    pip -r requirements.txt
    conda install magma-cuda118 -c pytorch  #可选，注意cuda后缀要与CUDA的版本一致
    conda install doxyrest -c conda-forge   #可选

重要的依赖包说明：

+ cmake	构建工具
+ numpy	基础数据结构
+ mkl和mkl-include CPU的一些算子调用MKL实现
+ sphinx pytorch文档构建工具
+ magma可以手动下载安装：https://icl.utk.edu/projectsfiles/magma/downloads/，magma2.7.1使用cmake进行编译
+ doxyrest可以手动下载安装：https://github.com/vovkos/doxyrest/releases

编译pytorch

+ 获取源码，并切换到指定版本（此处为2.0）：

.. code-block:: bash
    :linenos:

    git clone -b release/2.0 https://github.com/pytorch/pytorch
    git submodule update --init --recursive #更新子模块代码

+ 设置编译的环境变量：

.. code-block:: bash

    export CMAKE_BUILD_TYPE=Debug
    export CMAKE_INCLUDE_PATH=/usr/include/mkl
    export USE_CUDA=1
    export USE_CUDNN=1
    export USE_MKLDNN=1
    export MAX_JOBS=32                   #设置编译使用的线程数
    export BUILD_TEST=0
    #下面两个环境变量要么都设置，或者都不设置
    export PYTORCH_BUILD_VERSION=2.0.0  #设置编译后的版本号
    export PYTORCH_BUILD_NUMBER=1

+ 生成wheel格式的python包：

.. code-block:: bash

    python setup.py build
    python setup.py bdist_wheel

编译成功后会在dist目录下生成wheel包，使用pip安装即可。

+ 也可以使用下面命令，安装时会直接将python源码软链接到安装目录下，方便debug

.. code-block:: bash

    python setup.py develop

编译文档

.. code-block:: bash

    cd docs && pip install -r requirements.txt
    sudo npm install -g katex
    make        #输出所有支持的文档格式
    make html   #生成html格式文档

生成的html 文档保存在 docs/build/html 目录下

编译torchvision

torchvision的版本要和pytorch对应，可参考：https://github.com/pytorch/vision

安装依赖：

.. code-block:: bash
    :linenos:

    sudo apt install libjpeg-dev libavcodec-dev libavformat-dev libswscale-dev ffmpeg
    pip install pillow

获取代码：

.. code-block:: bash

    git clone -b release/0.14 git@github.com:pytorch/vision.git

编译：

.. code-block:: bash

    export BUILD_VERSION=0.14.0
    python setup.py build
    python setup.py bdist_wheel

同样，可以直接将python源码文件软链接到安装目录：

.. code-block:: bash

    python setup.py develop

简介
------------------------------------------------


常用模块
````````````````````````````````````````````````

+ torch：torch核心库
+ torch.nn：神经网络相关接口
+ torch.nn.functional：神经网络算子的函数式接口
+ torch.autograd：自动求导
+ torch.optim：优化器
+ torch.distributed：分布式
+ torch.jit：即时编译
+ torch.backend：目前支持gloo mpi nccl三种后端
+ torch.amp：混合精度

tensor
````````````````````````````````````````````````

tensor是一种与数组和矩阵类似的数据结构，在pytorch中，输入输出和模型的参数都是用tensor来表示的。

tensor与numpy中的ndarray非常相似，但tensor能在GPU和其他硬件加速设备上运行。并且针对自动微分进行了优化。

tensor可以从python的list或者numpy的ndarray创建：

.. code-block:: python
    :linenos:

    import torch
    import numpy as np
    data=[[1,2],[3,4]]
    x_data=torch.tensor(data)
    np_array = np.array(data)
    x_np = torch.from_numpy(np_array)

也可以从另外一个tensor创建，与numpy也有很多相似的接口，如ones_like，ones，zeros_like,zeros等等
b=a.numpy()

其他接口：

.. code-block:: python
    
    torch.tensor()
    torch.empty()
    torch.rand()
    torch.randn()
    x=x.new_ones()
    x.item()        #获取标量的值

tensor的属性有shape,dtype，device等等，device代表tensor数据的存储位置，默认在cpu上

.. code-block:: python

    tensor=torch.ones(3,4)
    print(tensor.device)   #结果为cpu

如果GPU可用，可以显式地把数据拷贝到GPU上：

.. code-block:: python

    if torch.cuda.is_available():   
        tensor = tensor.to('cuda')

    #打印tensor的device属性,结果为cuda:0
    print(tensor.device)

to方法可以将tensor在不同device之间或者不同数据类型进行拷贝和转换

tensor类定义在torch/_tensor.py文件中，继承自torch._C._TensorBase类，它的一些常用成员方法有：

.. code-block:: python

    dim()
    size()
    numel()     #返回元素个数
    data_ptr()  #返回底层数据地址
    storage()
    stride()
    requires_grad()
    [] #切片和索引
    register_hook()
    backward()
    resize()
    view()
    reshape()
    permute()

自动微分
````````````````````````````````````````````````

在训练过程中，对于梯度下降法，需要根据梯度和学习率来更新权重系数。可以采用自动微分的方法来计算损失函数的梯度。

如下列代码：

.. code-block:: python
    :linenos:

    x=torch.rand(2,2,requires_grad=True)
    y=x**2
    dydx=2*x
    y.backward(torch.one_like(y))
    print(dydx==x.grad)

可以验证y=x^2用自动微分求出的导数。

自动微分是pytorch构建神经网络最核心的功能之一

数据操作
------------------------------------------------

pytorch中与此相关的主要模块torch.utils.data.DataLoader和torch.utils.data.Dataset

PyTorch 提供了一些特殊的库如TorchText, TorchVision和TorchAudio, 其中都包含了一些数据集。

操作数据集的一个例子：https://www.cnblogs.com/DeepRS/p/15737009.html

tensor数据结构
------------------------------------------------

#. tensor的一些属性：shape,stride,dtype,memory_format,storage
#. storage和共享storage
#. 深拷贝：clone操作
#. to操作
#. contiguous和stride概念

参考：

+ `Tensor Views <https://pytorch.org/docs/stable/tensor_view.html>`_
+ `view与reshape区别详解 <https://zhuanlan.zhihu.com/p/436892343>`_
+ `PyTorch中的contiguous <https://zhuanlan.zhihu.com/p/64551412>`_

pytorch中的算子
------------------------------------------------

算子主要集中在以下模块：

+ torch	基础算子	tensor的创建/索引/切片/聚合/判断/数学函数/归约/逻辑/谱函数/BLAS和LAPACK接口等等
+ torch.nn	与神经网络相关的对象接口	卷积，池化，激活函数，RNN层，线性，dropout，损失函数，裁剪
+ torch.nn.functional	与神经网络相关的函数式接口	卷积，池化，激活函数，线性，dropout，损失函数，CV函数
+ torch.nn和torch.nn.functional中的接口功能重合，但前者中定义的算子大部分是torch.nn.,odule的子类，是面向对象接口，调用前需要先实例化对象；而后者是函数式接口，不需要放入__init__进行构造，所以不具有可学习参数的部分可以使用nn.functional进行代替。

参考阅读：

https://dev-discuss.pytorch.org/t/where-do-the-2000-pytorch-operators-come-from-more-than-you-wanted-to-know/373

神经网络组件
------------------------------------------------

+ 数据集
+ DataLoader
+ nn.Module类
+ 优化器
+ 损失函数
+ weight初始化：torch.nn.init模块

PyTorch可复现/重复实验的相关设置 https://zhuanlan.zhihu.com/p/584208060

定义网络并训练
------------------------------------------------

根据基础一节中的流程，在pytorch中进行训练的流程大体如下：

#. 定义自己的网络模型(如继承torch.nn.Module)
#. 定义loss函数和optimizer
#. 迭代数据集中的数据
#. 计算模型输出和loss
#. 通过optimizer.zero_grad()清空梯度
#. 通过反向传播计算梯度：loss.backward()
#. 更新权重：optimizer.step()
#. 重复3-7步直到loss下降到期望阈值，然后保存模型，完成训练

模型保存、加载与应用
------------------------------------------------

.. code-block:: python
    :linenos:

    model.save()
    model.load()
    #
    torch.save(model,PATH)             #保存整个网络
    torch.save(model.state_dict(),PATH) #只保存网络中的权重参数
    #加载
    model.load_state_dict(torch.load(PATH))

性能
------------------------------------------------

intel提供的pytorch扩展：
https://github.com/intel/intel-extension-for-pytorch

性能分析：

+ torch.bottleneck https://zhuanlan.zhihu.com/p/435914083
+ pytorch profiler

分布式训练
------------------------------------------------

参考：https://pytorch.org/tutorials/beginner/dist_overview.html

主要步骤：
+ 初始化分布式环境,调用 ``torch.distributed.init_process_group`` 进行初始化,并设置当前进程的 ``device`` :

.. code-block:: python
    :linenos:

    torch.distributed.init_process_group("nccl")
    torch.cuda.set_device(local_rank)

+ 为dataloader设置分布式sampler

.. code-block:: python
    :linenos:

    train_sampler = torch.utils.data.distributed.DistributedSampler(train_dataset,
                                                                    num_replicas=world_size,
                                                                    rank=local_rank)
    train_loader = torch.utils.data.DataLoader(dataset=train_dataset,
                                            batch_size=batch_size,
                                            shuffle=True,
                                            num_workers=0,
                                            sampler=train_sampler)

+ 将model封装成DistributedDataParallel model

.. code-block:: python
    :linenos:

    model = torch.nn.parallel.DistributedDataParallel(model,  device_ids=[local_rank])

对于有batchnorm的模型，可以使用SyncBN：

.. code-block:: python
    :linenos:

    model = torch.nn.SyncBatchNorm.convert_sync_batchnorm(model)

+ 使用 ``torchrun`` 或者 ``python -m torch.distributed.launch`` 启动分布式训练

``torchrun -h`` #查看帮助

+ PyTorch分布式训练简明教程(2022更新版) https://zhuanlan.zhihu.com/p/113694038
+ Pytorch 分布式训练 https://zhuanlan.zhihu.com/p/76638962
+ Pytorch DDP分布式训练介绍 https://zhuanlan.zhihu.com/p/453798093
+ PyTorch分布式训练基础--DDP使用 https://zhuanlan.zhihu.com/p/358974461


horovod
------------------------------------------------

环境：ubuntu20.04 anaconda cuda11.1

参考：https://horovod.readthedocs.io/en/stable/gpus_include.html

安装openmpi:

.. code-block:: bash

    sudo apt install openmpi-bin libopenmpi-dev

下载安装NCCL并解压，然后通过pip安装horovod：

.. code-block:: bash
    :linenos:

    HOROVOD_NCCL_HOME=/path/to/nccl HOROVOD_GPU_OPERATIONS=NCCL \
    pip install --no-cache-dir horovod

辅助工具
------------------------------------------------

+ 查看网络和参数：torchsummary

例子：

.. code-block:: python
    :linenos:

    import torchvision.models as models
    from torchinfo import summary
    #查看cpu上的模型参数
    resnet18 = models.resnet18().cpu()
    summary(resnet18,(3,300,300),batch_size=32,device="cpu")
    #查看gpu上的模型参数
    resnet18 = models.resnet18().cuda()
    summary(resnet18,(3,300,300),batch_size=32,device="cuda")

参考资料
------------------------------------------------
+ `PyTorch developer's wiki <https://github.com/pytorch/pytorch/wiki/>`_
+ `PyTorch 101, Part 3: Going Deep with PyTorch <https://blog.paperspace.com/pytorch-101-advanced/>`_
+ `Intermediate Activations — the forward hook <https://web.stanford.edu/~nanbhas/blog/forward-hooks-pytorch/>`_
+ `PyTorch下的Tensorboard 使用 <https://zhuanlan.zhihu.com/p/103630393>`_
+ `Which GPU\(s\) to Get for Deep Learning: My Experience and Advice for Using GPUs in Deep Learning <https://timdettmers.com/2023/01/30/which-gpu-for-deep-learning/>`_
+ `A Quick PyTorch 2.0 Tutorial <https://www.learnpytorch.io/pytorch_2_intro/>`_
+ `记录一次keras与pytorch的源码比较 <https://www.dazhuanlan.com/icesma/topics/1152012>`_
+ `PyTorch工程的最佳实践 <https://zhuanlan.zhihu.com/p/371978706>`_
+ `torch.utils 系列 <https://zhuanlan.zhihu.com/p/375445552>`_
+ `Some important Pytorch tasks - A concise summary from a vision researcher <https://spandan-madan.github.io/A-Collection-of-important-tasks-in-pytorch/>`_
+ `Accelerating PyTorch distributed fine-tuning with Intel technologies <https://huggingface.co/blog/accelerating-pytorch>`_