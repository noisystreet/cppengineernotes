rust编程
==============

rust环境配置
------------------------------------------------

安装命令：

.. code-block:: bash
    :linenos:

    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

相关组件：

+ ``rustup`` 命令：

.. code-block:: bash
    :linenos:

    rustup help  #查看使用说明
    rustup xxx help #子命令的使用说明
    rustup doc --book #查看rust教程
    rustup check  #检查
    rustup update #升级

+ ``rustc`` ``rust`` 的编译器
+ ``cargo``

常用用法：

.. code-block:: bash
    :linenos:

    cargo new xxx #新建项目
    cargo build <-r, --release> #编译项目，可以加上release选项
    cargo run #执行项目
    cargo fix #修复代码错误

hello world
````````````````````````````````````````````````
空

基础语法
------------------------------------------------

基本语法
````````````````````````````````````````````````

+ ``.rs`` 文件名
+ 注释： ``//``
+ fn关键字和main函数
+ ``println!`` 宏 
+ 变量的声明： ``let`` 和 ``let mut``
+ ``std`` 模块的使用： ``using std::io`` ; 默认导入的模块 ``prelude`` （https://doc.rust-lang.org/std/prelude/index.html）
+ 等号的作用：变量绑定（区别于其他语言的赋值）

数据类型
````````````````````````````````````````````````

整型：

Length	Signed	Unsigned
8-bit	i8	u8
16-bit	i16	u16
32-bit	i32	u32
64-bit	i64	u64
128-bit	i128	u128
arch	isize	usize

整型的字面量：

.. code-block:: rust
    :linenos:

    Number literals	Example
    Decimal	98_222
    Hex	0xff
    Octal	0o77
    Binary	0b1111_0000
    Byte (u8 only)	b'A'

``bool`` 型：

.. code-block:: rust
    :linenos:

    let t = true;
    let f: bool = false; 

浮点型：

.. code-block:: rust
    :linenos:

    let x = 2.0; // f64
    let y: f32 = 3.0; // f32

数组的声明：

.. code-block:: rust
    :linenos:

    let a = [1, 2, 3, 4, 5];
    let a: [i32; 5] = [1, 2, 3, 4, 5];
    let a = [3; 5];
    let months = ["January", "February", "March", "April", "May", "June", "July","August", "September", "October", "November", "December"];

``tuple`` 的声明：

.. code-block:: rust
    :linenos:

    let tup: (i32, f64, u8) = (500, 6.4, 1);
    let (x, y, z) = tup;

切片：切片是对集合的部分引用，字符串、数组等等集合类型都有切片的概念。如：

.. code-block:: rust
    :linenos:

    let a = [1, 2, 3, 4, 5];
    let slice = &a[1..3];

    let s = String::from("hello world");
    let len = s.len();
    let slice = &s[0..2]; //取下标0-2之间的子串
    let slice = &s[..2]; //取下标0-2之间的子串
    let world = &s[6..11]; //取区间[6,11)的子串，即world
    let slice = &s[0..len];  //取整个字符串
    let slice = &s[..];  //取整个字符串

字符串字面量就是切片：

.. code-block:: rust
    :linenos:

    let s = "Hello, world!";
    let s: &str = "Hello, world!";

当谈到字符串时，一般指的是 ``String`` 类型和 ``&str`` 字符串切片类型，这两个类型都是 UTF-8 编码。

分支
````````````````````````````````````````````````

``if`` 语句：

.. code-block:: rust
    :linenos:

    if cond0 {
        ...
    } else if cond1 {
        ...
    } else if cond2 {
        ...
    } else {
        ...
    }

``condition`` 语句不需要像 ``c/c++`` 语句一样加括号

在 ``let`` 中使用：

.. code-block:: rust
    :linenos:

    let number = if condition { 5 } else { 6 };

循环
````````````````````````````````````````````````

``loop`` 语句：

.. code-block:: rust
    :linenos:

    loop {
        println!("again!");
    }

    let mut counter = 0;
    let result = loop {
        counter += 1;
        if counter == 10 {
            break counter * 2;
        }
    };

``while`` 语句：

.. code-block:: rust
    :linenos:

    let mut number = 3;
    while number != 0 {
        println!("{number}!");
        number -= 1;
    }

``for`` 语句：

.. code-block:: rust
    :linenos:

    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }

    for i in 1..11  
    {  
        print!("{} ",i); 
    } 

控制语句： ``break和continue``

迭代器

函数
````````````````````````````````````````````````

Rust 的函数体是由一系列语句组成，最后由一个表达式来返回值，表达式结尾没有分号。通过 ``;`` 结尾的表达式会返回一个空的 ``tuple``

.. code-block:: rust
    :linenos:


    fn another_function(x: i32) {
    }

    //带返回值的函数
    fn plus_one(x: i32) -> i32 {
        x + 1;
    }

泛型和 ``trait``
````````````````````````````````````````````````

函数的泛型：

.. code-block:: rust
    :linenos:

    fn add<T>(a:T, b:T) -> T {
        a + b
    }

    fn main() {
        println!("add i8: {}", add(2i8, 3i8));
        println!("add i32: {}", add(20, 30));
        println!("add f64: {}", add(1.23, 1.23));
    }

不是所有 ``T`` 类型都能进行相加操作，因此我们需要用 ``std::ops::Add<Output = T>`` 对 ``T`` 进行限制：

.. code-block:: rust
    :linenos:

    fn add<T: std::ops::Add<Output = T>>(a:T, b:T) -> T {
        a + b
    }

结构体中的泛型：

.. code-block:: rust
    :linenos:

    struct Point<T> {
        x: T,
        y: T,
    }

    impl<T> Point<T> {
        fn x(&self) -> &T {
            &self.x
        }
    }

    fn main() {
        let integer = Point { x: 5, y: 10 };
        let float = Point { x: 1.0, y: 4.0 };
    }

Rust 中泛型是零成本的抽象，意味着你在使用泛型时，完全不用担心性能上的问题。

面向对象
````````````````````````````````````````````````

``c++`` 语言中所有定义都在 ``class`` 中，但是 Rust 的对象定义和方法定义是分离的，这种数据和使用分离的方式，会给予使用者极高的灵活度。
Rust使用 ``impl`` 来定义方法，如：

.. code-block:: rust
    :linenos:

    #[derive(Debug)]
    struct Rectangle {
        width: u32,
        height: u32,
    }

    impl Rectangle {
        fn area(&self) -> u32 {
            self.width * self.height
        }
    }

需要注意的是，self 依然有所有权的概念：
+ self 表示 Rectangle 的所有权转移到该方法中，这种形式用的较少
+ &self 表示该方法对 Rectangle 的不可变借用
+ &mut self 表示可变借用

定义在 impl 中且没有 self 的函数被称之为关联函数。如下面的new：

.. code-block:: rust
    :linenos:

    impl Rectangle {
        fn new(w: u32, h: u32) -> Rectangle {
            Rectangle { width: w, height: h }
        }
    }
    //调用：let sq = Rectangle::new(3, 3);

所有权
````````````````````````````````````````````````

借用：分为可变借用和不可变借用。
借用的规则：当已经有了可变借用时，就无法再拥有不可变的借用

标准库
------------------------------------------------

Vec
````````````````````````````````````````````````

声明方式：使用 ``Vec`` 或者 ``vec!`` 宏。

.. code-block:: rust
    :linenos:

    let v: Vec<i32> = Vec::new();
    let v: Vec<i32> = vec![];
    let v = vec![1, 2, 3, 4, 5];
    let v = vec![0; 10]; // ten zeroes

常用方法: ``push()/pop()/from()``

实用工具
------------------------------------------------

包和模块

错误处理

文件IO

其他功能
获取命令行参数：

.. code-block:: rust
    :linenos:

    let args: Vec<String> = env::args().collect();

混合编程
------------------------------------------------
空

工具配置
------------------------------------------------

+ rustup-components https://rust-lang.github.io/rustup-components-history/
+ rust-analyzer：https://rust-analyzer.github.io/manual.html#toolchain
  
安装：

.. code-block:: bash
    :linenos:

    rustup component add rust-analyzer #安装
    rustup which --toolchain stable rust-analyzer #查看安装路径

为vim配置coc-rust-analyzer：

https://github.com/fannheyward/coc-rust-analyzer

在vim中输入:CocConfig或者直接修改~/.vim/coc-settings.json进行配置，

.. code-block:: json
    :linenos:

    {
        "rust-analyzer.server.path":"/usr/local/bin/rust-analyzer"
    }

cargo包管理
------------------------------------------------
空

第三方库
------------------------------------------------

tauri界面库：https://github.com/tauri-apps/tauri