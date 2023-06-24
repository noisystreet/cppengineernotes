C++基础
=============

基础知识
------------------------------------------------

程序的编译和执行
````````````````````````````````````````````````
源代码文件->预处理->编译->汇编->链接->可执行文件

常见编译器：

+ msvc
+ icc
+ gcc
+ clang
  
hello world
````````````````````````````````````````````````

.. code-block:: c++
    :linenos:

    #include <iostream>

    using namespace std;

    int main() {
      cout<<"hello world!"<<endl;
    }

c++基本语法
````````````````````````````````````````````````

关键字
::::::::::::::::::::::::::

.. code-block:: c++

    and	and_eq	asm	auto	bitand	bitor
    bool	break	case	catch	char	class
    compl	const	const_cast	continue	default	delete
    do	double	dynamic_cast	else	enum	explicit
    export	extern	false	float	for	friend
    goto	if	inline	int	long	mutable
    namespace	new	not	not_eq	operator	or
    or_eq	private	protected	public	register	reinterpret_cast
    return	short	signed	sizeof	static	static_cast
    struct	switch	template	this	throw	true
    try	typedef	typeid	typename	union	unsigned
    using	virtual	void	volatile	wchar_t	while
    xor	xor_eq				


数据类型
::::::::::::::::::::::::::

+ 整型：有符号型： ``short,int,long,long long`` ，无符号型： ``unsigned``
+ 字符型： ``char``
+ 浮点型： ``float,double,long double``
+ 布尔型 ``bool``：	``true`` 和 ``false``
+ 指针
+ 空类型： ``void``
+ 枚举： ``enum``
+ 结构体：  ``struct``
+ 联合体： ``union``
+ 类： ``class``


常量与变量，const修饰符

数据类型的转换：自动转换，强制转换和static_cast

运算符
::::::::::::::::::::::::::

+ 算术运算符 ``+-*/``
+ 逻辑运算符&& || !
+ 位运算符 ``& ^ >> <<``
+ 地址运算符 ``&`` 和 ``*``
+ 其它: ``[] <> () = . ->`` , ``new delete sizeof``
+ 运算符的优先级

表达式
::::::::::::::::::::::::::

赋值，逗号表达式

流程控制
::::::::::::::::::::::::::

+ 分支：if-else、switch
+ 循环：for while do-while
+ 跳转：break、continue、goto

函数
::::::::::::::::::::::::::

+ 主函数
+ 函数的参数：参数传递方式，默认参数
+ 默认参数
+ 可变参数
+ 函数的重载(overload)
+ 函数的递归
+ 变量的作用域


数组
::::::::::::::::::::::::::

数组和元素
多维数组


指针
::::::::::::::::::::::::::
指针的类型
普通变量的指针 
函数指针
指针函数
多重指针
动态内存管理： ``malloc`` / ``free`` 、 ``new`` / ``delete``
智能指针



面向对象
------------------------------------------------

封装 
````````````````````````````````````````````````

+ 数据 
+ 成员函数
+ ``static`` 和 ``const`` 修饰符

+ 访问属性： ``public,private,protected``
+ 默认函数：构造函数，析构函数，=,==, 
+ 构造函数： ``explicit，default`` 和 ``delete``
+ 浅拷贝和深拷贝

继承
````````````````````````````````````````````````
+ 继承方式： ``public,private,protected``
+ 多继承
+ 基类和子类，虚基类和纯虚基类
+ 对象的初始化流程


多态
````````````````````````````````````````````````

+ ``virtual`` 关键字的作用，基类和虚基类、纯虚基类
+ ``override``


模板
------------------------------------------------

+ 模板的起源
+ 模板类和模板函数
+ 模板的特化


标准模板库（STL）
------------------------------------------------

容器
````````````````````````````````````````````````

+ 线性容器： ``vector list deque bitset``
+ 容器适配器： ``stack queue priority_queue``
+ 关联容器： ``set map mutiset mutimap``
+ 字符串： ``string``

算法
````````````````````````````````````````````````

+ 查找与替换
+ 排序
+ 集合
+ 拷贝

函数式编程
------------------------------------------------

+ 函数指针 
+ 函数对象
+ functor
+ ``std::bind``
+ lambda表达式
+ ``std::function``
+ ``std::invoke``


其他实用模块
------------------------------------------------

命名空间
````````````````````````````````````````````````

``namespace``

异常处理
````````````````````````````````````````````````

exception

IO操作
````````````````````````````````````````````````

+ ``iostream``：格式控制, ``cin/cout/cerr/clog``
+ ``fstream``：文本文件和二进制文件的读写
+ ``stringstream``

断言
````````````````````````````````````````````````
``assert``
``static_assert``

宏
````````````````````````````````````````````````
宏的三种作用：文件包含，宏变量定义，条件编译

+ 预处理指令:
  
.. code-block:: c++
    :linenos:

    #define
    #error
    #warning
    #pragma
    #ifndef

+ 预定义宏:
  
.. code-block:: c++
    :linenos:

    __func__
    __FUNCTION__
    __FILE__
    __LINE__
    __TIME__
    __DATE__

C++的演进
------------------------------------------------

C++11
````````````````````````````````````````````````

语法
::::::::::::::::::::::::::

+ 类型推导： ``auto`` 和 ``decltype``
+ 容器的列表初始化： ``vector<int> a={1,2,3,4};``
+ 统一初始化（列表初始化）方法：使用大括号初始化 ``int a{5}``;
+ 枚举类：``enum class``
+ 模板别名using
+ 可变参数模板
+ ``constexpr``
+ 右值引用，移动语义和完美转发
+ range based loop
+ ``final`` 和 ``override``
+ ``delete`` 和 ``default``
+ ``nullptr``
+ type_traits
+ ``std::bind``
+ lambda表达式 

容器
::::::::::::::::::::::::::

+ ``unordered_set``
+ ``unordered_map``
+ ``forward_list``
+ ``tuple``
+ ``array``

智能指针
::::::::::::::::::::::::::

头文件 ``<memory>``
智能指针是为了解决内存自动回收的问题而引用的。

+ ``unique_ptr``：独占所有权
+ ``shared_ptr``：共享所有权，采用引用计数实现
+ ``weak_ptr``：用于解决循环引用问题，通常和 ``shared_ptr`` 配合使用,不会改变引用计数
+ ``auto_ptr``：c++14已弃用

参考：
https://iamsorush.com/posts/weak-pointer-cpp/


线程库
::::::::::::::::::::::::::

涉及头文件

.. code-block:: c++

    #include <thread>
    #include <mutex>
    #include <atomic>
    #include <future>
    #include <promise>
    #include <condition_variable>

线程的创建：

+ ``std::thread``
+ ``std::async`` 用来创建一个异步任务，可以通过 ``future`` 的 ``get`` 、 ``wait_for`` 、 ``wait`` 函数对子线程的结果和状态进行访问.
+ ``std::packaged_task`` 是个模板类。 ``std::packaged_task`` 包装任何可调用目标(函数、lambda表达式、bind表达式、函数对象)以便它可以被异步调用。它的返回值或抛出的异常被存储于能通过 ``std::future`` 对象访问的共享状态中， ``std::packaged_task`` 类似于 ``std::function`` ，但是会自动将其结果传递给 ``std::future`` 对象。

std::packaged_task的例子

.. code-block:: c++
    :linenos:

    #include <thread>   // std::thread
    #include <future>   // std::packaged_task, std::future
    #include <iostream> // std::cout

    int sum(int a, int b) {
        return a + b;
    }

    int main() {
        std::packaged_task<int(int,int)> task(sum);
        std::future<int> future = task.get_future();

        // std::promise一样，std::packaged_task支持move，但不支持拷贝
        // std::thread的第一个参数不止是函数，还可以是一个可调用对象，即支持operator()(Args...)操作
        std::thread t(std::move(task), 1, 2);
        // 等待异步计算结果
        std::cout << "1 + 2 => " << future.get() << std::endl;

        t.join();
        return 0;
    }
    /// 输出: 1 + 2 => 3


锁

+ ``std::mutex``
+ ``std::lock_guard``
+ ``std::unique_lock``
+ ``std::call_once``，保证 ``call_once`` 调用的函数只被执行一次。该函数需要与 ``std::once_flag`` 配合使用。
+ ``std::atomic``:原子变量

线程的同步

``std::condition_variable``，条件变量提供了两类操作： ``wait`` 和 ``notify``

异步

+ ``std::promise``：是一个模板类: ``template<typename R> class promise``。其泛型参数R为 ``std::promise`` 对象保存的值的类型，R可以是 ``void`` 类型。std::promise保存的值可被与之关联的std::future读取，与std::promise关联的std::future是通过std::promise::get_future获取到的，自己构造出来的无效。一个std::promise实例只能与一个std::future关联共享状态
+ ``std::future`` ：是一个类模型，用来保存一个异步操作的结果，即这是一个未来值，只能在未来某个时候进行获取。
+ ``get()``：等待异步操作执行结束并返回结果，若得不到结果就会一直等待。
+ ``wait()``：用于等待异步操作执行结束，但并不返回结果。
+ ``wait_for()``：阻塞当前流程，等待异步任务运行一段时间后返回其状态 ``std::future_status`` ，状态是枚举值：

chrono时间处理
::::::::::::::::::::::::::
+ clock

``clocks`` 表示当前的系统时钟，内部有time_point, duration, Rep, Period等信息。
``clocks`` 包含三种时钟：
``steady_clock`` 是单调的时钟，相当于教练手中的秒表；只会增长，适合用于记录程序耗时；

``system_clock`` 是系统的时钟；因为系统的时钟可以修改；甚至可以网络对时； 所以用系统时间计算时间差可能不准。

``high_resolution_clock`` 是当前系统能够提供的最高精度的时钟；它也是不可以修改的。相当于 ``steady_clock`` 的高精度版本。

+ ``chrono::duration`` 类 （ ``/usr/include/c++/11/chrono`` ）代表一段时间间隔

.. code-block:: c++

    /// `chrono::duration` represents a distance between two points in time
    template<typename _Rep, typename _Period = ratio<1>>
    struct duratio

模板参数_Rep为数据类型，如 ``int``, ``double``。 ``_Period`` 默认为 ``chrono::ratio`` ， ``ratio`` 也是是一个模板类，代表时间精度(一秒的几分之一)。成员函数 ``.count()`` 返回ratio的数目。

不同 ``duration`` 的转换：

.. code-block:: c++

    std::chrono::seconds s=std::chrono::duration_cast<std::chrono::seconds>(ms);

+ ``time_point`` 类

也是一个模板类：

.. code-block:: c++

    template<typename _Clock, typename _Dur>
    struct time_point

+ ``time_since_epoch()`` 用来得到当前时间点到1970年1月1日00:00的时间距离
+ ``to_time_t()`` ``time_point`` 转换成 ``time_t`` 秒
+ ``from_time_t()`` 从 ``time_t`` 转换成 ``time_point``

计时例子：

两个 ``time_point`` 对象之间的距离是 ``duration`` 类型，因此，得到代码执行前后的 ``time_point`` ，然后计算 ``duration`` ，就可以得到时间：

.. code-block:: c++
    :linenos:

    using namespace std::chrono;
    auto t1=high_resolution_clock::now();
    //call some func
    auto t2=high_resolution_clock::now();
    //时间间隔（毫秒）
    auto dt=duration_cast<milliseconds>(t2-t1);
    std::cout<<"duration/ms="<<dt.count()<<std::endl;


C++14
````````````````````````````````````````````````

+ ``std::make_unique``
+ ``std::quoted``
+ ``std::exchange``
+ ``auto`` 和泛型lambda表达式

  + ``auto lambda = [](auto a, auto&& b) { return a < b; };``
  + ``auto lambda = []<class T>(T a, auto&& b) { return a < b; };``

C++17
````````````````````````````````````````````````

+ ``filesystem``
+ ``std::any``
+ ``std::optional``
+ ``std::string_view``
+ ``std::apply``：将 ``tuple`` 的成员转变成函数参数，并调用函数
+ polymorphic allocators
+ searchers
+ fold expression
+ 结构化绑定

C++20
````````````````````````````````````````````````

新增头文件：

.. code-block:: c++

    #include <bit>      //位运算
    #include <compare>
    #include <concepts>
    #include <coroutine> //协程
    #include <format>
    #include <numbers> //数字常量
    #include <ranges>
    #include <source_location>
    #include <span>
    #include <syncstream>
    #include <version>
    //多线程相关 
    #include <barrier>
    #include <latch>
    #include <semaphore>
    #include <stop_token>

新增功能：

+ Modules
+ Coroutine
+ Concept
+ Ranges
+ ``consteval`` 和 ``constinit``
+ 数字常量，在 ``<number>`` 头文件
+ 格式化 ``std::format`` ，在 ``<format>`` 头文件
+ 位运算
+ ``jthread``
+ ``[[likely]]`` 和 ``[[unlikely]]``

C++23
````````````````````````````````````````````````

新增头文件：

.. code-block:: c++

    #include <expected>
    #include <flat_map>
    #include <flat_set>
    #include <generator>
    #include <mdspan>
    #include <print>
    #include <spanstream>
    #include <stacktrace>
    #include <stdfloat>     //float16和bfloat16支持

c++设计模式
------------------------------------------------

#. `设计 C++ 接口文件的小技巧之 PIMPL <https://www.cnblogs.com/tengzijian/p/17473602.html>`_

参考阅读
------------------------------------------------
#. `C++ FAQ <https://isocpp.org/wiki/faq>`_
#. `cppreference <https://en.cppreference.com/w/>`_
#. `cplusplus <https://cplusplus.com/reference/>`_
#. `GCC文档 <https://gcc.gnu.org/onlinedocs/>`_
#. `MSVC C++ Language Reference <https://learn.microsoft.com/en-us/cpp/cpp/cpp-language-reference?view=msvc-170>`_
#. `SGI-STL实现 <http://www.sgi.com/tech/stl/index.html>`_
#. `C++重载底层原理 <https://www.cnblogs.com/whiteBear/p/17180339.html>`_
#. `All About Lambda Function in C++(From C++11 to C++20) <http://www.vishalchovatiya.com/learn-lambda-function-in-cpp-with-example/>`_