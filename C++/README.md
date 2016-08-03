C++ 有着强大的抽象能力，它以奇妙的方式融合着 5 种编程范式（paradigm），即`面向过程、基于对象、面向对象、泛型和函数式`，将所有范型的优点提炼并发挥到极致的同时，又不拘泥于其中的任何一种。

同时 C++ 作为一种“高级的 C”而存在，任何一句 C++ 都有着深刻的 C 语言背景，可以直接落实为 C 语言，进而落实为任何一种计算机最底层的机器吗。这一点，任何解释型语言做不到，因而效率上和C差不多。

C++语言特点的详细内容可以参考 [Why](Why.md)。

# 基础概念

C++ 中有一些比较重要的基础概念需要仔细去理解，比如：

* 声明语句、定义语句；
* 左值还是右值；
* 引用传参和引用返回值；
* sizeof 运算符；
* 内存对齐机制；
* 联合体、CPU字节序；
* If 判断语句
* 逗号运算符

更多内容参考 [Basic](Basic.md)

# 关键字

关键字(keyword)又称保留字，是整个语言范围内预先保留的标识符。每个C++关键字都有特殊的含义。经过预处理后，关键字从预处理记号(preprocessing-token)中区别出来，剩下的标识符作为记号(token)，用于声明对象、函数、类型、命名空间等。

主要的关键字有以下几种：

* const： 编译器会检查const变量有没有被修改，如果有代码尝试修改一个const变量，编译器就会报错
* static：C/C++中面向过程程序设计中的static和面向对象程序设计中的static的5种具体使用方法
* extern：引用外部定义的变量
* extern "C"：函数名修饰机制
* volatile：CPU 的寄存器缓存机制
* register：寄存器变量
* inline：内联机制
* typedef：类型别名

C++ 关键字的详细内容可以参考 [Keywords](Keywords.md)。

# 类与对象

类的基本思想是数据抽象（封装）、继承和多态（动态绑定）。

* 数据抽象：把客观事物封装成抽象的类，同时将类的接口和实现分离。（优点：可以隐藏实现细节，使得代码模块化）
* 继承：定义相似的类型，并对其相似关系建模。（优点：可以扩展已存在的代码模块）
* 多态：一定程度上忽略相似类型的区别，以统一的方式使用它们的对象。

每一个类都必须有构造函数和析构函数，没有显式指定的话，编译器会帮我们生成默认的构造函数和析构函数。构造函数可以指定初始化列表，不应在构造函数和析构函数中抛出异常。

可以禁止在堆上或者在栈上生成类的对象。

继承是类的重要特性。通过继承联系在一起的类构成一种层次关系，通常在层次关系的根部有一个基类，其他类则直接或者间接地从基类继承而来，这些继承得到的类称为派生类。基类负责定义在层次关系中所有类公同拥有的成员，而每个派生类定义各自特有的成员。

C++中用虚函数实现多态机制。关于多态，简而言之就是用父类型的指针指向其子类的实例，然后通过父类的指针调用子类的成员函数。这种技术可以让父类的指针有“多种形态”，这是一种泛型技术。

有关类的更多内容，参见 [Class](Class.md)

# 数组

数组名指代一种数据结构。

    char str[10];
    cout << sizeof(str) << endl; // 输出 10

此外，数组名可以转换为指向其指代实体的指针，而且是一个常量指针，不能作自增、自减等操作；

    int nums[] = {1,2,3,4};
    *nums = 2;
    nums++; // cannot increment value of type 'int [4]'

再来看另一个例子：

    int a[5]={1,2,3,4,5};
    int *ptr=(int *)(&a+1);
    printf("%d",*(ptr-1));

这里 &a+1 并不是数组的首地址a+1，因为 &a 是指向数组的指针，其类型为int(* )[5]。而指针加1要根据指针类型加上一定的值，不同类型的指针+1之后增加的大小不同，a是长度为5的int数组指针，所以要加5 * sizeof(int)，所以ptr指向的位置是a+5。但是ptr与（&a+1）类型是不一样的，所以ptr-1只会减去sizeof(int*)。

## 数组形参

数组作为形参时，会退化为指针，这是因为数组的两个性质：

1. 不允许拷贝数组（无法以值传递的方式使用数组参数）；
2. 使用数组时会将其转换为指针。

所以给函数传递指针时，实际上传递的是指向数组首元素的指针。

    // 尽管形式不同，但是这三个 print 函数是等价的
    void print(const int*);
    void print(const int[]);   
    void print(const int[10]); // 纬度表示期望数组含有多少元素，实际并不一定

［[数组合法参数](http://www.nowcoder.com/questionTerminal/e2ac8bddb9e5434a92511320221c8513)］

# 指针

如果在程序中定义了一个变量，在编译时就给这个变量分配内存单元。系统根据程序中定义的变量类型，分配一定长度的空间。在程序中一般是通过变量名来对内存单元进行存取操作的。程序经过编译以后已经将变量名转换为变量的地址，对变量值的存取都是通过地址进行的。这种按变量地址存取变量值的方式称为`直接存取`方式，或直接访问方式。

此外可以采用另一种称为`间接存取(间接访问)`的方式。可以在程序中定义这样一种特殊的变量，它是专门用来存放地址的。我们称专门用来存放另一变量地址(即指针)的变量为`指针变量`。指针变量的值(即指针变量中存放的值)是地址(即指针)。

利用指针变量可以表示各种数据结构；能很方便地使用数组和字符串；并能像汇编语言一样处理内存地址，从而编出精练而高效的程序，可以说指针极大地丰富了C/C++的功能。

指针的更多内容参考 [Pointer](Pointer.md)

由于C、C++没有自动内存回收机制，关于内存的操作的安全性依赖于程序员的自觉。程序员每次new出来的内存块都需要自己使用delete进行释放，复杂的流程可能会导致忘记释放内存而造成内存泄漏。

此外，当有多个指针指向同一个对象时，如果某个指针delete了该对象，对这个指针来说明确了它所指的对象被释放掉了，所以不会再对所指对象进行操作，但是对于剩下的其他指针来说还指向已经被删除的对象。于是悬垂指针就形成了，再次访问已经释放的内存空间，可能会导致程序崩溃。

为了避免普通指针可能带来的各种问题，C++标准库中引入了智能指针。关于智能指针的更多内容，参考 [SmartPoint](SmartPoint.md)。

# 函数

函数是一组一起执行一个任务的语句，函数还有很多叫法，比如方法、子例程或程序，等等。每个 C++ 程序都至少有一个函数，即主函数 main() ，所有简单的程序都可以定义其他额外的函数。

函数声明告诉编译器函数的名称、返回类型和参数，函数定义提供了函数的实际主体。

函数的更多内容参考： [Function](Function.md)

# 内存管理

内存管理是C++最令人切齿痛恨的问题，也是C++最有争议的问题，C++高手从中获得了更好的性能，更大的自由，C++菜鸟的收获则是一遍一遍的检查代码和对C++的痛恨，但内存管理在C++中无处不在，内存泄漏几乎在每个C++程序中都会发生。

内存的更多内容参考：[Memory](Memory.md)

# C++ STL 标准库

C++ STL（标准模板库）是一套功能强大的 C++ 模板类，提供了通用的模板类和函数，这些模板类和函数可以实现多种流行和常用的算法和数据结构，如向量、链表、队列、栈。

STL的一个重要特点是`数据结构和算法的分离`。尽管这是个简单的概念，但这种分离确实使得STL变得非常通用。例如，由于STL的sort()函数是完全通用的，你可以用它来操作几乎任何数据集合，包括链表，容器和数组。

STL另一个重要特性是它不是面向对象的。为了具有足够通用性，STL主要依赖于模板而不是封装，继承和虚函数（多态性）——OOP的三个要素。你在STL中找不到任何明显的类继承关系。

标准库的更多内容参考 [STL](STL.md)。

# C++ 11 新特性

C++11 曾经被叫做 C++0x，是对目前C++语言的扩展和修正，C++11不仅包含核心语言的新机能，而且扩展了C++的标准程序库（STL）。C++11包括大量的新特性：包括lambda表达式，类型推导关键字auto、decltype，和模板的大量改进。

C++11修复大量缺陷和降低代码拖沓，比如lambda表达式的支持将使代码更简洁。新的标准库同时也会包含新的特性，包括对多线程的支持和优化智能指针，后者将给那些还没用类似boost::shared_ptr的人提供更简单的内存管理方法。

详细内容参考 [11_Features](11_Features.md)


# 牛客网问题

［[代码膨胀问题](http://www.nowcoder.com/questionTerminal/f6ee5023f5334873980247cf496aa641)］  
［[C++不是类型安全](http://www.nowcoder.com/questionTerminal/f80ec593dcbd44e7a13975b53e9bdaab)］  

# 更多阅读

《C++ Primer（第五版）》  
《深度探索 C++ 对象模型》  
《STL 源码剖析》   
《Effective C++》   
《More Effective C++》  
[C++_More](More/C++_More.md)
[C/C++内存管理详解](http://chenqx.github.io/2014/09/25/Cpp-Memory-Management/)  
[那些不能遗忘的知识点回顾——C/C++系列](http://www.cnblogs.com/webary/p/4754522.html)  
[Can we change the value of a constant through pointers?](http://stackoverflow.com/questions/3801557/can-we-change-the-value-of-a-constant-through-pointers/3801601#3801601)   




