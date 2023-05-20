C++基础
=============

基础知识
------------------------

程序的编译和执行
````````````````````````
源代码文件->预处理->编译->汇编->链接->可执行文件

常见编译器：

+ msvc
+ icc
+ gcc
+ clang


c++基本语法
````````````````````````

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

+ 整型：有符号型：short,int,long,long long，无符号型：unsigned 
+ 字符型：char
+ 浮点型：float,double,long double 
+ 布尔型bool：	true和false
+ 指针
+ 空类型：void
+ 枚举：enum
+ 结构体：struct
+ 联合体：union
+ 类：class


常量与变量，const修饰符

数据类型的转换：自动转换，强制转换和static_cast

运算符
::::::::::::::::::::::::::

+ 算术运算符+-*/
+ 逻辑运算符&& || !
+ 位运算符& ^ >> <<
+ 地址运算符&和*
+ 其它:[] <> () = . ->,new delete sizeof
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
动态内存管理：malloc/free、new/delete
智能指针



面向对象
------------------------


模板
------------------------


标准模板库
------------------------


其他实用模块
------------------------



参考阅读
------------------------
#. `cppreference <https://en.cppreference.com/w/>`_
#. `cplusplus <https://cplusplus.com/reference/>`_
#. `C++重载底层原理 <https://www.cnblogs.com/whiteBear/p/17180339.html>`_