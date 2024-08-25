Tensorflow使用手册
==================================

直接安装
------------------------------------------------

+ 安装CUDA和cuDNN,最方便的是使用 ``conda install cudnn`` ,可以一步安装好cuda和对应的cudnn
+ 使用pip安装tensorflow（tf2之后cpu gpu包名一样），如：

.. code-block:: bash

    pip install tensorflow==2.6.0

如果是CUDA环境，安装完成后需要找到libcudnn.so.8的路径，并添加到LD_LIBRARY_PATH环境变量中
+ 测试：

.. code-block:: python
    :linenos:

    import tensorflow as tf
    print(tf.test.is_gpu_available())
    print(tf.config.list_physical_devices())

如果正常输出了GPU和CUDA相关信息，表明可以使用

源码编译TF2
------------------------------------------------

软件版本

+ ubuntu	22.04	
+ python	3.9.12
+ gcc	11.3	
+ CUDA Toolkit	11.7.0
+ cuDNN	8.6.0.163
+ tensorflow	2.9	源码
+ bazel	5.0.0	

参考：

+ 使用bazel安装tensorflow https://xhhszc.github.io/2019/01/08/%E4%BD%BF%E7%94%A8bazel%E5%AE%89%E8%A3%85tensorflow/
+ Building Tensorflow from source. Step by step guide. https://medium.com/analytics-vidhya/building-tensorflow-from-source-step-by-step-guide-1075ef2d9356

+ 下载tf源码（https://github.com/tensorflow/tensorflow）

.. code-block:: bash
    :linenos:

    git clone -b r2.11 git@github.com:tensorflow/tensorflow.git

+ 安装bazel：查看并下载安装对应版本：https://mirrors.huaweicloud.com/bazel

.. code-block:: bash
    :linenos:

    VER=$(cat .bazelversion)
    wget https://mirrors.huaweicloud.com/bazel/${VER}/bazel_${VER}-linux-x86_64.deb
    sudo dpkg -i bazel_${VER}-linux-x86_64.deb

+ 运行./configure
+ 进行编译：

.. code-block:: bash
    :linenos:

    #https://cloud.tencent.com/developer/article/1967814
    bazel build --config=cuda --config=dbg //tensorflow/tools/pip_package:build_pip_package

在笔记本上编译时，要设置内存和cpu限制，如：--local_ram_resources=9012 -j 4 
设置不编译一些模块：

.. code-block:: bash
    :linenos:

    --config=nonccl
    --config=noaws
    --config=nohdfs
    --config=noignite
    --config=nokafka

+ 生成wheel包：

.. code-block:: bash
    :linenos:

    ./bazel-bin/tensorflow/tools/pip_package/build_pip_package .

+ 查看可以编译的项目：

.. code-block:: bash
    :linenos:

    bazel query 'kind(rule, //:*)' --output label_kind

bazel参考
------------------------------------------------

https://blog.csdn.net/ayqy42602/article/details/108378427
+ 可以使用conda直接安装bazel：

.. code-block:: bash
    :linenos:

    conda search bazel
    conda install bazel

+ bazel在构建过程中可能需要下载一些第三方库，有时会网络超时，可以设置让bazel从本地目录获取源码包：

.. code-block:: bash
    :linenos:

    bazel build ...... --distdir  dirname

+ 额外添加c和c++编译选项:

.. code-block:: bash
    :linenos:

    --copt="-g" --cxxopt="-g"
    --cxxopt="-mfma"
    --cxxopt="-mavx"
    --cxxopt="-mavx2"

+ 显示编译时详细失败原因：-

.. code-block:: bash
    :linenos:

    -verbose_failures

+ 只构建c++库:

.. code-block:: bash
    :linenos:

    bazel build -c opt/dbg/fastbuild //tensorflow:libtensorflow_cc.so

+ 只构建pythony库：

.. code-block:: bash
    :linenos:

    bazel build -c opt/dbg/fastbuild //tensorflow/tools/pip_package:build_pip_package