---
Project: "[[2-Area/STL]]"
Status: 🟨
tags:
  - Resources
Deadline: 2024-03-26
CreateTime: 2024-03-26
Connected: 
---



# STL 算法[​](https://getiot.tech/cplusplus/cpp-stl-algorithms#stl-%E7%AE%97%E6%B3%95 "STL 算法的直接链接")介绍

STL 采用泛化（Generic）的方式实现各种算法，实现了数据结构和算法分离的目的。正是这种分离设计，使得 STL 变得非常通用。包括排序、查找、排列组合算法以及用于数据移动、复制、删除、比较、组合、运算等的算法。例如，我们可以将 STL 的 `sort()` 函数用在几乎所有数据集合的排序上，包括链表，容器和数组等等。

函数库对数据类型的选择对其可重用性起着至关重要的作用。而 C++ 通过模板的机制允许推迟对某些类型的选择，直到真正想使用模板或者说对模板进行特化的时候，STL 就利用了这一点提供了相当多的有用算法。

STL 提供了大约 100 个实现算法的模版函数，比如算法 for_each 将为指定序列中的每一个元素调用指定的函数，stable_sort 以你所指定的规则对序列进行稳定性排序等等。这样一来，只要我们熟悉了 STL 之后，许多代码可以被大大的化简，只需要通过调用一两个算法模板，就可以完成所需要的功能并大大地提升效率。

STL 算法部分的头文件包括三个：`<algorithm>`、`<numeric>` 和 `<functional>`。

- 要使用 STL 中的算法函数必须包含头文件 `<algorithm>`，它是所有 STL 头文件中最大的一个，包含一堆模板函数，功能涉及比较、交换、查找、遍历操作、复制、修改、移除、反转、排序、合并等算法；
- 而 `<numeric>` 提供了数学运算的模板函数，比如加法和乘法在序列上的一些操作。
- `<functional>` 中则定义了一些模板类，用来声明函数对象。

在 C++ STL 中，所有泛型算法的前两个参数都是一对迭代器（iterators），通常称为 `first` 和 `last`，用以标示算法的操作区间。STL 习惯采用前闭后开区间表示法，写成 `[first, last)`。不过有一个必要条件就是，必须能够累加操作符从 `first` 到达 `last`。编译器本身无法强求这一点，因此需要程序员去保证，否则将导致不可预期的结果。

每一个 STL 算法的声明，都必须说明它所需要的最低程度的迭代器类型，例如 `find()` 需要一个 InputIterator ，这是它的最低要求，但是它也可以接受更高类型的迭代器，如 ForwardIterator、BidirectionalIterator 或者 RandomAccessIterator。但是如果给 `find()` 传递一个 OutputIterator，将会出错。此外，将无效的迭代器传递给某个算法，虽然是一种错误，但是不保证编译器就会被捕捉出来。

# 常用 STL 算法[​](https://getiot.tech/cplusplus/cpp-stl-algorithms#%E5%B8%B8%E7%94%A8-stl-%E7%AE%97%E6%B3%95 "常用 STL 算法的直接链接")

STL 涵盖了许多非常有用的算法，比如排序、查找、排列组合、数据移动、复制、删除、比较、组合、运算等等。这里介绍其中比较常用的一些。

### sort[​](https://getiot.tech/cplusplus/cpp-stl-algorithms#sort "sort的直接链接")

sort 将范围 `[first,last)` 中的元素按升序排序。第一个版本使用 `operator<` 比较元素，第二个版本使用 `comp` 进行比较。sort 是我们用的最多的一个算法，它接受两个 RandomAccessIterators（随机存取迭代器），然后将区间内所有元素以递增方式由大到小重新排列。它的仿函数版本，允许自定义排序标准。

关联式容器自动排序，序列式容器中 stack、queue、priority-queue 都是有特定的出入口，因此也不需要 sort。list 的迭代器属于 BidirectionalIterators，因此不能用 sort，剩下的 vector 和 deque 比较适合用 sort。

```cpp
template <class RandomAccessIterator>
void sort (RandomAccessIterator first, RandomAccessIterator last);

template <class RandomAccessIterator, class Compare>
void sort (RandomAccessIterator first, RandomAccessIterator last, Compare comp);
```

一定要注意第二个参数是一个仿函数对象，直接用仿函数类是不可以的。下面是一个简单的示例：

```cpp
int myints[] = {32, 71, 12, 45, 26, 80, 53, 33};
std::vector<int> myvector (myints, myints+8);               // 32 71 12 45 26 80 53 33

// using default comparison (operator <):
std::sort (myvector.begin(), myvector.begin()+4);           //(12 32 45 71)26 80 53 33
// using function as comp
std::sort (myvector.begin()+4, myvector.end(), myfunction); // 12 32 45 71(26 33 53 80)
// print out content:
// using object as comp
std::sort (myvector.begin(), myvector.end(), myobject);     //(12 26 32 33 45 53 71 80)
```

参考手册：[sort](http://www.cplusplus.com/reference/algorithm/sort/)

### lower_bound[​](https://getiot.tech/cplusplus/cpp-stl-algorithms#lower_bound "lower_bound的直接链接")

这是二分查找的一种版本，试图在已经排序的 `[first, last)` 中寻找元素 value。它返回一个迭代器，指向第一个 “不小于 value” 的元素。如果 value 大于 `[first, last)` 内的任何一个元素，则返回 last。该函数接受两个版本，版本一采用 `operator <` 进行比较，版本二采用仿函数 `comp`，完整的声明如下。

```cpp
template <class ForwardIterator, class T>
ForwardIterator lower_bound (ForwardIterator first, ForwardIterator last,
                            const T& val);

template <class ForwardIterator, class T, class Compare>
ForwardIterator lower_bound (ForwardIterator first, ForwardIterator last,
                            const T& val, Compare comp);
```

此外，还有一个和 `lower_bound` 函数十分类似的 `upper_bound`，返回的迭代器指向区间内第一个大于 value 的元素。

下面是一个简单的示例：

```cpp
#include <iostream>     // std::cout
#include <algorithm>    // std::lower_bound, std::upper_bound, std::sort
#include <vector>       // std::vector

int main(void)
{
    int myints[] = {10, 20, 30, 30, 20, 10, 10, 20};
    std::vector<int> v(myints,myints+8);           // 10 20 30 30 20 10 10 20
    std::sort(v.begin(), v.end());                 // 10 10 10 20 20 20 30 30

    std::vector<int>::iterator low,up;
    low=std::lower_bound (v.begin(), v.end(), 20); //          ^
    up= std::upper_bound (v.begin(), v.end(), 20); //                   ^

    std::cout << "lower_bound at position " << (low- v.begin()) << '\n';
    std::cout << "upper_bound at position " << (up - v.begin()) << '\n';

    return 0;
}
// lower_bound at position 3
// upper_bound at position 6
```

此外，还有一些用到二分查找的其他算法，比如 `binary_search` 用来检查指定值是否存在于排序后的序列中，`equal_range` 返回排序后的序列中值等于 value 的下标范围。

参考手册：[lower_bound](http://www.cplusplus.com/reference/algorithm/lower_bound/)

### 其他算法[​](https://getiot.tech/cplusplus/cpp-stl-algorithms#%E5%85%B6%E4%BB%96%E7%AE%97%E6%B3%95 "其他算法的直接链接")

- `reverse`：反转范围 `[first,last)` 中元素的顺序。
- `swap`：交换 a 和 b 的值。
- `binary_search`：如果范围 `[first,last)` 中的任何元素等效于 val，则返回 true，否则返回 false。
- `find`：返回指向范围 `[first,last)` 中比较等于 val 的第一个元素的迭代器。如果没有找到比较相等的元素，函数返回 last。
- `find_first_of`：返回一个指向范围 `[first1,last1)` 中与 `[first2,last2)` 中的元素匹配的第一个元素的迭代器。如果没有找到这样的元素，函数返回 last1。

# 算法大全
STL 标准模板库 [`<algorithm>`](https://www.cplusplus.com/reference/algorithm/) 定义了一组基于算法的函数。这些算法函数可以通过迭代器或指针访问的任何对象序列，例如数组或某些 [STL 容器](https://getiot.tech/cplusplus/cpp-stl-containers) 的实例。但是请注意，算法通过迭代器直接对值进行操作，不会影响容器的结构（它永远不会影响容器的大小或存储分配）。

下面我们直接看一下这些函数的功能介绍。 

## 不修改序列操作[​](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#%E4%B8%8D%E4%BF%AE%E6%94%B9%E5%BA%8F%E5%88%97%E6%93%8D%E4%BD%9C "不修改序列操作的直接链接")

Non-modifying sequence operations

|函数|说明|
|---|---|
|[all_of](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|以下函数判断参数内所有元素的条件。|
|[any_of](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|以下函数判断参数内某些或任何元素的条件|
|[none_of](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|以下函数检查是否所有元素都不符合条件。|
|[for_each](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数对参数的所有元素应用运算。|
|[find](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数在参数内找到一个值。|
|[find_if](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数查找元素在参数内。|
|[find_if_not](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数在该参数内找到一个元素，但方式与上述相反。|
|[find_end](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数用于返回参数的最后一个元素。|
|[find_first_of](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数查找满足条件并首先出现的元素。|
|[adjacent_find](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数进行搜索以查找参数内的相等和相邻元素。|
|[count](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数返回参数内的值的计数。|
|[count_if](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数返回满足条件的值的计数。|
|[mismatch](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数按顺序返回第一个不匹配的值。|
|[equal](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数用于检查两个参数是否所有元素都相等。|
|[is_permutation](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数检查参考参数是否为其他参数的排列。|
|[search](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数在一个参数内搜索子序列。|
|[search_n](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数在参数内搜索元素的出现。|

## 修改序列操作[​](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#%E4%BF%AE%E6%94%B9%E5%BA%8F%E5%88%97%E6%93%8D%E4%BD%9C "修改序列操作的直接链接")

Modifying sequence operations

| 函数                                                                       | 说明                          |
| ------------------------------------------------------------------------ | --------------------------- |
| [copy](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)            | 该函数复制元素的参数。                 |
| [copy_n](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)          | 该函数复制参数内的n个元素               |
| [copy_if](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)         | 如果满足特定条件，该函数将复制参数的元素。       |
| [copy_backward](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)   | 函数以向后的顺序复制元素                |
| [move](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)            | 该函数移动元素的参数。                 |
| [move_backward](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)   | 该函数将元素参数向后移动                |
| [swap](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)            | 该函数交换两个对象的值。                |
| [swap_ranges](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)     | 该函数交换两个参数的值。                |
| [iter_swap](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)       | 该函数交换参考下两个迭代器的值。            |
| [transform](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)       | 该函数转换一个参数内的所有值。             |
| [replace](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)         | 该函数将参数内的值替换为特定值。            |
| [replace_if](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)      | 如果满足特定条件，该函数将替换参数的值。        |
| [replace_copy](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)    | 该函数通过替换元素来复制值的参数。           |
| [replace_copy_if](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#) | 如果满足特定条件，该函数将通过替换元素来复制值的参数。 |
| [fill](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)            | 该函数用一个值填充参数内的值。             |
| [fill_n](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)          | 该函数填充序列中的值。                 |
| [generate](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)        | 该函数用于生成参数值。                 |
| [generate_n](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)      | 该函数用于生成序列的值。                |
| [remove](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)          | 该函数从参数中删除值。                 |
| [remove_if](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)       | 如果满足条件，该函数将删除参数的值。          |
| [remove_copy](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)     | 该函数通过删除参数值来复制它们。            |
| [remove_copy_if](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)  | 该函数通过在满足条件的情况下删除参数来复制参数的值。  |
| [unique](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)          | 该函数标识参数的唯一元素。               |
| [unique_copy](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)     | 该函数复制参数的唯一元素。               |
| [reverse](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)         | 该函数可逆转参数。                   |
| [reverse_copy](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)    | 该函数通过反转值来复制参数。              |
| [rotate](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)          | 该函数将参数的元素向左旋转。              |
| [rotate_copy](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)     | 该函数复制向左旋转的参数的元素。            |
| [random_shuffle](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)  | 该函数随机调整参数。                  |
| [shuffle](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)         | 该函数借助生成器随机调整参数。             |

## 分区[​](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#%E5%88%86%E5%8C%BA "分区的直接链接")

Partitions

|函数|说明|
|---|---|
|[is_partitioned](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数用于推断参数是否已分区。|
|[partition](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数用于划分参数。|
|[stable_partition](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数将参数分为两个稳定的一半。|
|[partition_copy](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数在分区后复制参数。|
|[partition_point](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数返回参数的分区点。|

## 排序[​](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#%E6%8E%92%E5%BA%8F "排序的直接链接")

Sorting

|函数|说明|
|---|---|
|[sort](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数对参数内的所有元素进行排序。|
|[stable_sort](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数在保持相对等效顺序的参数内对元素进行排序。|
|[partial_sort](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数对参数的元素进行部分排序。|
|[partial_sort_copy](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数对参数进行排序后将其复制。|
|[is_sorted](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数检查参数是否已排序。|
|[is_sorted_until](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|函数检查直到参数排序的元素。|
|[nth_element](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|函数对参数内的元素进行排序。|

## 二分搜索[​](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#%E4%BA%8C%E5%88%86%E6%90%9C%E7%B4%A2 "二分搜索的直接链接")

Binary search (operating on partitioned/sorted ranges)

|函数|说明|
|---|---|
|[lower_bound](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|返回参数的下界元素。|
|[upper_bound](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|返回参数的上限元素。|
|[equal_range](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数返回等于元素的子参数。|
|[binary_search](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数判断参数内的值是否按排序顺序存在。|

## 合并函数[​](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#%E5%90%88%E5%B9%B6%E5%87%BD%E6%95%B0 "合并函数的直接链接")

Merge (operating on sorted ranges)

|函数|说明|
|---|---|
|[merge](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数合并两个已排序的参数。|
|[inplace_merge](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数合并两个已排序的连续参数。|
|[includes](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数搜索排序参数是否包括另一个参数。|
|[set_union](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数返回已排序的两个参数的并集。|
|[set_intersection](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数返回已排序的两个参数的交集。|
|[set_difference](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数返回已排序的两个参数的差。|
|[set_symmetric_difference](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|函数返回已排序的两个参数的对称差。|

## 堆函数[​](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#%E5%A0%86%E5%87%BD%E6%95%B0 "堆函数的直接链接")

Heap

|函数|说明|
|---|---|
|[push_heap](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数将新元素压入堆中。|
|[pop_heap](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数在堆中弹出新元素。|
|[make_heap](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数用于创建堆。|
|[sort_heap](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数对堆进行排序。|
|[is_heap](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数检查参数是否为堆。|
|[is_heap_until](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|该函数检查参数直到堆的哪个位置。|

## 最小/最大[​](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#%E6%9C%80%E5%B0%8F%E6%9C%80%E5%A4%A7 "最小/最大的直接链接")

Min/max

|函数|说明|
|---|---|
|[max](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|返回参数的最大元素。|
|[min](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|返回参数的最小元素。|
|[minmax](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|返回参数的最小和最大元素。|
|[min_element](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|返回参数的最小元素。|
|[max_element](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|返回参数的最大元素。|
|[minmax_element](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|返回参数的最小和最大元素。|

## 其他[​](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#%E5%85%B6%E4%BB%96 "其他的直接链接")

Other

|函数|说明|
|---|---|
|[lexicographical_compare](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|字典序小于比较。|
|[next_permutation](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|将范围转换为下一个排列。|
|[prev_permutation](https://getiot.tech/cplusplus/cpp-stl-algorithms-all#)|将范围转换为先前的排列。|
