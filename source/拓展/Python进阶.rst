Python进阶
===============

Python深入理解
------------------------------------------------

参考

+ https://www.cnblogs.com/jiakecong/p/16869432.html
+ https://www.cnblogs.com/jiakecong/p/16908795.html
+ https://www.cnblogs.com/Chang-LeHung/p/16988279.html

Python分析工具
------------------------------------------------

pystack: https://github.com/bloomberg/pystack

inspect模块
````````````````````````````````````````````````

trace模块
````````````````````````````````````````````````

python -m trace --ignore-dir=../lib --trace mymain.py

python解释器
````````````````````````````````````````````````

参考文档：

#. https://docs.python.org/3/library/language.html
#. https://www.cnblogs.com/Chang-LeHung/p/17299754.html
#. https://github.com/Chang-LeHung/dive-into-cpython

标准库中与解释器相关的重要模块：

+ ast	抽象语法树	https://docs.python.org/zh-cn/3/library/ast.html
+ symtable	符号表	https://docs.python.org/zh-cn/3/library/symtable.html
+ token		https://docs.python.org/zh-cn/3/library/token.html
+ keyword	检测关键字	https://docs.python.org/zh-cn/3/library/keyword.html
+ tokenize	python词法分析	https://docs.python.org/zh-cn/3/library/tokenize.html
+ py_compile		
+ dis	反汇编	

字节码分析
------------------------------------------------

参考：

#. https://www.geeksforgeeks.org/deconstructing-interpreter-understanding-behind-the-python-bytecode/
#. https://docs.python.org/3/library/dis.html
#. https://opensource.com/article/18/4/introduction-python-bytecode
#. https://python.plainenglish.io/introduction-to-python-bytecode-42648932b347
#. https://towardsdatascience.com/understanding-python-bytecode-e7edaae8734d

简单分析

dis.dis()：反汇编一段代码
dis.ByteCode()：生成某段代码的字节码
dis.code_info()：显示详细代码信息
dis.get_instructions()：返回代码对应的指令
python -m dis xxx.py：反编译某个脚本

例子代码：

.. code-block:: python
    :linenos:

    import dis

    def showMeByte(name):
        name.replace("h","j")
        return "hello "+name+" !!!"

    print("dis.dis"+"="*50)
    dis.dis(showMeByte)

打印出来字节码如下所示，4和5为行号，右边为字节码和对应语句

.. code-block:: python
    :linenos:

    4           0 LOAD_FAST                0 (name)
                2 LOAD_METHOD              0 (replace)
                4 LOAD_CONST               1 ('h')
                6 LOAD_CONST               2 ('j')
                8 CALL_METHOD              2
                10 POP_TOP

    5          12 LOAD_CONST               3 ('hello ')
                14 LOAD_FAST                0 (name)
                16 BINARY_ADD
                18 LOAD_CONST               4 (' !!!')
                20 BINARY_ADD
                22 RETURN_VALUE

cpython解释器是基于栈执行字节码的。使用inspect.stack()可以获取栈信息。
The stack is the data structure used as internal working storage for the  virtual machine. There are different classes of virtual machines and  one of them is called a stack machine. CPython’s virtual machine is an  implementation of such a stack machine. 
测试给上面函数加上一个class，成为类的成员方法，打印出字节码仍然相同。

dis.opname：返回所有的指令名称
dis.opmap：指令名称和对应编号构成的map

Bytecode包
````````````````````````````````````````````````

源码：https://github.com/MatthieuDartiailh/bytecode

文档：https://bytecode.readthedocs.io/en/latest/

cpython源码分析
````````````````````````````````````````````````

源码： https://github.com/python/cpython

代码分支：3.11，commid ID:1633aea0e430f4c0d115b1ea5baac5daaecf81ff

c/c++代码可达几十万行，如下：

.. code-block:: bash

    ---------------------------------------------------------------------------------------
    Language                             files          blank        comment           code
    ---------------------------------------------------------------------------------------
    Python                                2008         134507         156077         623396
    C                                      326          56976          54417         383814
    C/C++ Header                           446          18002          10691         170603

重要的目录有：

+ Grammer：语法文件和token
+ Parser：词法分析+语法分析
+ Include：
+ Python：python解释器源码
+ Lib：python标准库
+ Modules：C编写的python模块
+ Objects：python内置类型
+ Programs：python解释器对应的可执行程序

参考：https://devguide.python.org/internals/exploring/#cpython-source-code-layout

python解释器：https://devguide.python.org/internals/compiler/

解释执行代码的步骤

#. Tokenize the source code (https://github.com/python/cpython/blob/main/Parser/tokenizer.c)
#. Parse the stream of tokens into an Abstract Syntax Tree (https://github.com/python/cpython/blob/main/Parser/parser.c)
#. Transform AST into a Control Flow Graph (https://github.com/python/cpython/blob/main/Python/compile.c)
#. Emit bytecode based on the Control Flow Graph (https://github.com/python/cpython/blob/main/Python/compile.c)

https://www.flyabledev.com/articles.html

https://www.cnblogs.com/whiteBear/p/16698276.html

每个阶段的详细说明：

+ 词法分析tokenize ：python -m tokenize -e xxx.py
+ 语法分析生成ast： https://www.cnblogs.com/yssjun/p/10069199.html

其他语法分析库：stunparse, codegen, unparse,astpretty

ast的可视化：https://ucb-sejits.github.io/ctree-docs/ipythontips.html

有了ast和符号表，就可以生成字节码，生成 CFG 和字节码的代码在 Python/compile.c 中

参考资料
------------------------------------------------

#. 系列博客：https://tenthousandmeters.com/tag/cpython/
#. 内存管理和垃圾回收算法 https://devguide.python.org/internals/garbage-collector/
#. https://realpython.com/cpython-source-code-guide/
#. https://devguide.python.org/internals/
#. https://cython.readthedocs.io/en/latest/