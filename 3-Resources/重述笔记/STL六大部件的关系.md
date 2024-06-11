---
Project: "[[2-Area/STL]]"
Status: 🟨
tags:
  - Resources
Deadline: 2024-03-25
CreateTime: 2024-03-25
Connected: 
---
# C++ STL 六大组件

STL 提供了六大组件，彼此组合套用协同工作。这六大组件分别是：

* **容器**（Containers）：各种数据结构，如 vector、list、deque、set、map 等。从实现的角度来看，容器是一种 class template。
* **算法**（Algorithms）：各种常用算法，提供了执行各种操作的方式，包括对容器内容执行初始化、排序、搜索和转换等操作，比如 sort、search、copy、erase。从实现的角度来看，STL 算法是一种 function template。
* **迭代器** （Iterators）：迭代器用于遍历对象集合的元素，扮演容器与算法之间的胶合剂，是所谓的"泛型指针"，共有 5 种类型，以及其他衍生变化。从实现角度来看，迭代器是一种将 `operator*`、`operator->`、`operator++`、`operator--` 等指针操作予以重载的 class template。所有的 STL 容器附带有自己专属的迭代器，因为只有容器设计者才知道如何遍历自己的元素。
* **仿函数** （Functors）：也称为函数对象（Function object），行为类似函数，可作为算法的某种策略。从实现角度来看，仿函数是一种重载了 `operator()` 的 class 或者 class template。
* **适配器**（Adaptors）：一种用来修饰容器或者仿函数或迭代器接口的东西。例如 STL 提供的 queue 和 stack，就是一种空间配接器，因为它们的底部完全借助于 deque。
* **分配器**（Allocators）：也称为空间配置器，负责空间的配置与管理。从实现的角度来看，配置器是一个实现了动态配置空间、空间管理、空间释放的 class template。

# STL 六大组件的交互关系


![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fstatic.getiot.tech%2Fcpp-stl-6-components.png%23center&valid=true)

# 容器

一个容器就是一些特定类型对象的集合。STL 中容器分为两大类，序列式容器和关联式容器。

**序列式容器**（sequential container）为程序员提供了控制元素存储和访问顺序的能力。这种顺序不依赖于元素的值，而是与元素加入容器时的位置相对应。

除了序列式容器外，标准库还定义了三个序列式容器适配器：stack、queue 和 priority_queue。**适配器**是标准库中的一个通用概念，容器、迭代器和函数都有适配器。本质上，一个适配器是一种机制，能使某种事物的行为看起来像另外一种事物一样。

和序列式容器对应的是**关联式容器**（associative-container），关联容器中的元素是按关键字来保存和访问的。关联容器支持高效的关键字查找和访问，STL 有两个主要的关联容器：map 和 set。

[[容器的介绍]]

# 迭代器


迭代器提供对一个容器中的对象的访问方法，并且定义了容器中对象的范围。迭代器就如同一个指针。事实上，C++ 的指针也是一种迭代器。但是，迭代器不仅仅是指针，因此你不能认为他们一定具有地址值。例如，一个数组索引，也可以认为是一种迭代器。

迭代器有各种不同的创建方法。程序可能把迭代器作为一个变量创建。一个 STL 容器类可能为了使用一个特定类型的数据而创建一个迭代器。作为指针，必须能够使用 `*` 操作符类获取数据。还可以使用其他数学操作符如 `++` 操作符用来递增迭代器，以访问容器中的下一个对象。如果迭代器到达了容器中的最后一个元素的后面，则迭代器变成 past-the-end 值。使用一个 past-the-end 值得指针来访问对象是非法的，就好像使用 `NULL` 或为初始化的指针一样。

对于 STL 数据结构和算法，可以使用五种迭代器。下面简要说明了这五种类型：

|          迭代器类型          |         说明         |
|-------------------------|--------------------|
| Input iterators         | 提供对数据的只读访问。        |
| Output iterators        | 提供对数据的只写访问。        |
| Forward iterators       | 提供读写操作，并能向前推进迭代器。  |
| Bidirectional iterators | 提供读写操作，并能向前和向后操作。  |
| Random access iterators | 提供读写操作，并能在数据中随机移动。 |

更多内容请阅读 \>\> [C++ STL 迭代器](https://getiot.tech/cplusplus/cpp-stl-iterators) 。

# 算法


STL 通过函数模板提供了很多作用于容器的通用算法，例如查找、插入、删除、排序等，这些算法均需要引入头文件 `<algorithm>`。

所有的 STL 算法都作用在由迭代器 `[first, last)` 所标示出来的区间上，可以分为两大类：

* **质变算法**（mutating algorithms）：运算过程中会更改区间内迭代器所指的元素内容，如分割（partition）、删除（remove）等算法。
* **非质变算法**（nonmutating algorithms）：运算过程中不会更改区间内迭代器所指的元素内容，如匹配（search），计数（count）等算法。

所有泛型算法的前两个参数都是一对迭代器，通常称为 first 和 last，用以标示算法的操作区间。注意，将无效的迭代器传给某个算法，虽然是一种错误，但不保证能够在编译期间被捕捉出来。

更多内容请阅读 \>\> [C++ STL 算法](https://getiot.tech/cplusplus/cpp-stl-algorithms) 。

# 总结

STL 提供了六大组件，而从代码广义上讲，主要分为三类：container（容器）、iterator（迭代器）和 algorithm（算法），几乎所有的代码都采用了模板类和模版函数的方式，这相比于传统的由函数和类组成的库来说提供了更好的代码重用机会。

算法、容器、迭代器三者的关系是：算法操作数据，容器存储数据，迭代器是算法操作容器的桥梁，迭代器与容器一一对应。如下图所示。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fstatic.getiot.tech%2Fcpp-stl-algorithm-container-iterator.png%23center&valid=true)

STL 的中心思想在于将数据容器（Containers）和算法（Algorithm）分开，彼此独立设计，最后以一帖胶着剂将它们撮合在一起。

STL 的一个重要特点是**数据结构和算法的分离**。正是这种分离设计，使得 STL 变得非常通用。例如，由于 STL 的 sort() 函数是完全通用的，你可以用它来操作几乎任何数据集合，包括链表，容器和数组。

STL 另一个重要特性是它**不是面向对象**的。为了具有足够通用性，STL 主要依赖于模板，而不是 OOP 的三个要素 ------ 封装、继承和虚函数（多态性），这使得你在 STL 中找不到任何明显的类继承关系。

