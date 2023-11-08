# C++ 面试题回答（持续更新中）

[toc]

---

## 请简要介绍一下 C++ 的历史及其与 C 语言的关系

C++ 是一种通用的程序设计语言，和 C 一样，也是诞生于 贝尔实验室，它在 C 的基础上，添加了面向对象编程以及泛型编程的特性，并提供了一套更完善，更健壮的标准库。

同时，C++ 也继承了 C 简洁高效，快速，可移植性高的传统。

时至今日，C++ 经过多年的发展，已经有大量成熟了类库和活跃的开发者社区，是开发复杂系统的首选，
被广泛的应用在操作系统，数据库，游戏引擎等领域。

## C++ 面向对象编程的三大特性是什么?并举例说明

面向对象编程的三大特性是：继承，多态，封装

首先是继承：在继承机制下形成有层级的类，可以使低层级的类可以沿用高层级的特征和方法。
继承的目的有两个：

（1）复用代码，减少代码的冗余，减少开发的工作量。
（2）使类与类之间产生联系，为多态的实现打下基础。

其次是多态：是指一个类的同名方法，在不同的情况下实现的细节不同。
多态有以下目的：

（1）一个外部接口可以被多个类同时使用
（2）不同对象调用同一个方法，可以有不同的实现

在 C++ 中，多态主要依靠虚函数实现，在一个类方法前用 virtual 关键字修饰，就表示该方法是一个虚方法，
如果有一个子类继承了这个类，就可以重写这个方法。因此，父类调用这个方法和子类调用这个方法产生的结果是不同的。

最后说一下封装：封装就是将一个客观的食物抽象为一个逻辑实体，实体的属性和功能相互结合，形成一个整体。
并对这个实体设置访问控制，通过外部开放的接口可以访问这个实体，而无需知道内部的具体实现细节。
C++ 中的结构体和类就很好的使用了封装的特性。

## 什么是类和对象的构造函数及析构函数? 它们的作用是什么?

构造函数和析构函数是 C++ 类中最重要的两类函数，他们分别负责类的创建和销毁。
首先讲一下构造函数，它通过传入的参数创建一个类，在 C++ 的类中，通常有以下几种构造函数：

（1）默认构造函数，一个类如果没有显示的声明构造函数，那么它就会自己创建一个默认构造函数，为类成员进行初始化，当然，也可以在某个构造函数后面加上 = defult 来显示的声明一个默认构造函数。

（2）参数构造函数，就是有参数列表的构造函数，该函数可以根据传入的参数来构建并初始化一个类的数据。

（3）拷贝构造含数，这类构建函数需要传入一个同类对象的引用，将对象 B 的数据深拷贝到对象 A 中。

（4）移动构造函数，这类构造函数需要传入一个对象引用的引用，将原对象的所有权转移到新对象中去，通俗的说就是将原对象的内容搬到新对象中去，而非拷贝。

接下来讲一下析构函数，它的任务很简单，在类的声明周期到头时销毁这个类，如果有一些申请在堆上面的内存，可以放到析构函数中去销毁。

用一段代码去展示：

```C++

class A
{
    private:
        int *_arr;
        std::string _disc;

    public:
        /*默认构造函数*/
        A() : _arr(nullptr), _disc("NULL DATA") = defult {}

        /*参数构造函数*/
        A(int *_array, int _a_size, std::string & _str);

        /*拷贝构造函数*/
        A(A & _a);

        /*移动构造函数*/
        A(A && __a);

        /*析构函数*/
        ~A() { delete[] _arr; }
};

```

## C++模板的作用是什么? 描述其语法形式

C++ 通过模板（Template）来提供泛型编程的支持，他可以让一个函数或类可以适应不同类型的参数，以达到更高的泛用性，像 C++ 的 STL 容器就是基于模板实现的S。

要使用模板，需要引入 template 和 typename 这两个关键字，语法如下：

```C++

/*模板函数*/
template <typename Type>
void function(Type & _type);

/*模板类*/
template <typename Type>
class A
{
    private:
        Type _data;

    public:
        A(Type & _ty);
};

/*从外部实现模板类方法*/
template <typename Type>
A::A(Type & _ty) : _data(_ty) {}
```

若要使用模板函数和模板类，最好显示的指定类型，语法如下（当然，不指定在某些情况下也是可以的，编译器会自动判断相应的类型并替换）：

```C++

/*使用模板函数*/
function<int>(value);

/*使用模板类*/
A<const int> _a;

```

## 什么是STL? 讲述你熟悉的几种STL容器

STL 全称为 （Standing Template Library），是 C++ 提供的一套标准模板库，该模板库提供了多种容器的实现（如 vector，list，array，string，stack，queue，map，set 等），还引入了迭代器的概念，使得 STL 更加健壮和通用。

接下来将介绍几个我熟悉的 STL 容器：

``` md

1. std::vector
    
vector 本质上就是一个可以自动扩容，缩减的数组容器，该容器的常用方法如下：

    1.push_back() 在末尾插入元素，如果数组以满则扩容，一般是扩大原数组长度的一半，然后将原数组中的数据拷贝到新数组后，删除原数组。

    2.pop_back() 删除数组末尾的元素，同时执行上述拷贝操作缩减数组的长度

    3.size() 求数组长度

    4.capacity() 求数组容量

    5.insert() 从数组指定位置插入元素，该操作由于需要移动数组元素位置，会很慢，复杂度在 O(n)

    6.erase() 从数组指定位置删除元素，该操作和 5 一样，需要移动数组元素，会很慢。

    7.begin() end() 返回数组第一个元素和最后一个元素的迭代器 (iterator)

    8.swap() 交换两个元素的值

    9.at() 返回容器内一个索引值的引用

    10.reserve() 重设该容器的大小

2. std::stack 一个栈的实现，常用方法如下：

    1.push() 入栈，将数据放入栈头，并偏移 top 指针指向该数据，该数据就成为新的栈头。

    2.pop() 出栈，将栈头数据弹出，并偏移指针 top 指针指向下一个数据，下一个数据就成为新的栈头。

    3.empty() 检查是否为空栈，返回一个 bool 类型。

    4.top() 返回栈头元素值的引用。

    5.swap() 交换两个栈元素的值。

    6.emplace() 可以直接在栈顶构造并插入一个元素，省去了拷贝的开销。

```

## 如何理解和使用 C++ 中的引用(reference)，引用和指针有什么区别?

引用是 C++ 的新特性，可以避免传递数据时拷贝所带来的开销。
在 C++ 中，有 3 种传递方式：按值传递，按指针传递，按引用传递，按值和按指针都需要将右值的数据拷贝一份再传递到左值中。
而按引用传递不一样，它直接将原变量名修改为要传递的变量名，这样就避免了拷贝带来的开销。

代码示例：

```C++
const int a = 10;

/*直接将变量 a 的名称改为 ref_a，或者说将 ref_a 绑定在 a 上面，避免了值传递的拷贝所带来的开销*/
const int & ref_a = a;

ref_a = 10; /*错误，不可修改*/

/*在函数中，使用引用变量作为参数列表也可以避免拷贝*/
std::vector<int> & function(std::array<int> & _array);

```

## 什么是内联函数? 它与宏的区别是什么?

内联函数（inline function）是指在编译环节中展开，而非跳转的函数。
在 C++ 中，使用 inline 关键字显式声明或者在类内部实现方法的就是内联函数。
内联函数和普通函数最大的不同就是执行的流程不一样，普通的函数是跳转到函数的入口，然后执行函数，函数执行完毕后再跳转回主函数中。而内联函数则直接将函数体替换到主函数之中，省去了跳转所带来的开销。当然，内联函数也有缺点，频繁的使用会让代码过于臃肿，因此内联函数非常适合那种短小，但调用十分频繁的函数。

内联函数也有一些有别于其他类型函数的特点：

（1）内联函数不能取地址

（2）虚函数不可以是内联的

代码示例：

``` C++

class A
{
    private:
        int *_arr;
        size_t size;
        size_t capacity;

    public:
        A() = default;
        A(const int *_a, size_t _s, size_t _c) : size(_s), capacity(_c);

        /*像 size，capacity 这样的，只有一条 return 语句的函数，非常适合作为内联函数*/
        size_t size() const { return size; }

        size_t capacity() const { return capacity; }

        ~A();
};

```

当然内联函数和宏（macro）的区别也很多，如下：

（1）宏实在预处理阶段展开，而内联函数实在编译阶段展开。

（2）内联函数可以重载，但宏不能。

（3）内联函数遵循作用域，命名空间规则，但是宏不会

（4）内联函数在运行时可调试，但是宏函数不能。

（5）内联函数在编译时会有类型检查，而在预处理阶段的宏不会。

## 请简述几种常用的设计模式及其应用场景

### 单例模式

是使用最广泛的模式。
其意图是保证一个类仅有一个实例，并提供一个访问它的全局访问点，该实例被所有程序共享。
一般情况下，C++ 的单例类定义如下：

1. 私有化它的构造函数，防止外界创建单例类对象；

2. 使用类的私有静态指针变量指向类的唯一实例；

3. 使用一个公有静态方法获取该实例；

单例模式的实现分为 懒汉版 和 饿汉版，代码如下：

```C++
/**
*   懒汉版 （Lazy Singleton）
*   即单例实例在第一次使用的时候才初始化（延迟初始化）
*/
class Singleton
{
    private:
        static Singleton * instance;
    
    private:
        Singleton() {}

        Singleton(const Singleton & _ins);

        Singleton & operator=(const Singleton & _ins);

        ~Singleton() {}

    public:
        static Singleton * get_instance()
        {
            if ( !instance ) { instance = new Singleton(); }

            return instance;
        }
};

Singleton * Singleton::instance = nullptr;

```

```C++

/**
*   饿汉版 （Eager Singleton）
*   即单例实例在程序运行时被立即执行初始化
*/
class Singleton
{
    private:
        static Singleton instance;
    
    private:
        Singleton() {}

        Singleton(const Singleton & _ins);

        Singleton & operator=(const Singleton & _ins);

        ~Singleton() {}

    public:
        static Singleton & get_instance() { return instance; }
};

Singleton Singleton::instance;

```

单例模式的应用场景也有很多，

由于一个操作系统中有且只能有一个任务管理器，因此最经典单例模式的应用场景莫过于操作系统的任务管理器。

### 工厂模式（简单工厂模式，抽象工厂模式）

一种创建型设计模式，它提供一种在不指定具体类的情况下创建对象的接口，这使得客户端与类的具体实现解耦，提高了代码的可扩展性和可维护性。

在 C++ 中，工厂模式最常见的操作就是设置一个抽象基类，然后派生多个子类去完成不同的实现，代码就省略了。。。

最常见的应用场景比如 C++ STL 中的 List Map Stack 容器，可以避免暴露底层的实现细节。

### 观察者模式

一种行为设计模式，它定义了一种一对多的依赖关系，当一个对象的状态发生变化时，对所有观察者都会收到通知并更新。

这种模式在 UI 更新上面有广泛的应用。

### 职责链模式

........

### 适配器模式

........

## 如何理解 C++ 中命名空间的概念及其作用

命名空间（namespace）是 C++ 一种组织代码的方法，它允许将一组相关的符号（变量，结构体，类 等）划分到一起，以便更好地管理和访问，同时也避免了命名冲突 ，提高代码的可读性和可维护性，像 C++ 标准库的所有内容都放在一个叫 std 的命名空间内。

此外，命名空间还可以随时扩展，只需要重新声明命名空间即可。

代码举例如下：

```C++
namespace Space_01
{
    int number;
};

/**
*   命名空间可以随时扩展，
*   扩展完后的命名空间有 number 和 _disc 两个成员
*/
namespace Space_01
{
    std::string _disc;
};

namespace Space_02
{
    int number;

    /*当然，命名空间也可以嵌套*/
    namespace Space_03 { size_t value; }
};

int main(int argc, char const *argv[], char const *envp[])
{
    /*
        如果要使用命名空间，可以使用 using 关键字和 作用域解析运算符 ::
        
        1.  using Space_01::number;     
        using 声明，表示使用命名空间 Space_01 中的 number 成员，
        在声明后使用成员 number 就无需带上 Space_01 的前缀。

        2.  using namespace Space_02;   
        using 编译指令，表示使用整个 Space_02 命名空间，
        在声明后使用这个命名空间的所有成员都不需要带上 Space_02 的前缀，
        当然这也会加剧命名冲突的问题。
    */

    
    std::cout << Space_01::number;
    std::cout << Space_02::number;

   return EXIT_SUCCESS; 
}
```

## C++异常处理机制的工作流程是什么

C++ 异常处理机制一般分为 3 个步骤，分别是：

### 异常抛出    （throw）

在程序执行过程中，如果发生异常（exception）情况，（如 除数为 0，访问已经 delete 的内存，解引用 nullpter 指针等），
就会抛出异常，一般是使用 throw 关键字。

### 异常捕获    （try）

try 用于发现异常，若异常在 try 语句块内，就可以被捕获到，然后跳转到 catch 语句块进行处理。

### 异常处理    （catch）

catch 语句块用于处理异常，catch 语句块用于处理 try 语句块中发生的异常，并执行相应的代码来提示或处理，
当然，catch 语句可以有多个，由于处理不同情况的异常。

举个例子

```C++
#include <iostream>

//#include <exception>

int function(int number)
{
    if (!number) { throw "number is ZERO!"; }

    return number;
}

int main(int argc, char const *argv[])
{
    int num = 0;

    try
    {
        num = function(0);
    }
    catch (const char * _message)
    {
        std::cerr << _message << std::endl;
    }

    return 0;
}
```

### Latest update: Jesse 2023_11_08

![image](./img/swing.gif "GanYu Swing~")

EOF
