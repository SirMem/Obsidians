---
Project: "[[2-Area/STL]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 
Connected: 
---


# 1list容器基本概念

链表是一种物理存储单元上非连续、非顺序的存储结构，数据元素的逻辑顺序是通过**链表** 中的指针链接次序实现的。链表由一系列结点（链表中每一个元素称为结点）组成，结点可以在运行时动态生成。每个结点包括两个部分：一个是存储数据元素的数据域，另一个是存储下一个结点地址的指针域。 相较于vector的连续线性空间，list就显得负责许多，它的好处是每次插入或者删除一个元素，就是配置或者释放一个元素的空间。因此，list对于空间的运用有绝对的精准，一点也不浪费。而且，对于任何位置的元素插入或元素的移除，list永远是常数时间。 List和vector是两个最常被使用的容器。**List容器是一个双向链表**。
![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic4.zhimg.com%2Fv2-d61af820b19525630e92784066ebc8e3_b.jpg&valid=true)

采用动态存储分配，不会造成内存浪费和溢出 链表执行插入和删除操作十分方便，修改指针即可，不需要移动大量元素 链表灵活，但是空间和时间额外耗费较大。

# 2list容器的迭代器（双向迭代器）

List容器不能像vector一样以普通指针作为迭代器，因为其节点不能保证在同一块连续的内存空间上。List迭代器必须有能力指向list的节点，并有能力进行正确的递增、递减、取值、成员存取操作。所谓"list正确的递增，递减、取值、成员取用"是指，递增时指向下一个节点，递减时指向上一个节点，取值时取的是节点的数据值，成员取用时取的是节点的成员。 由于list是一个双向链表，迭代器必须能够具备前移、后移的能力，所以list容器提供的是Bidirectional Iterators. List有一个重要的性质，插入操作和删除操作都不会造成原有list迭代器的失效。这在vector是不成立的，因为vector的插入操作可能造成记忆体重新配置，导致原有的迭代器全部失效，甚至List元素的删除，也只有被删除的那个元素的迭代器失效，其他迭代器不受任何影响。 **list容器不仅是一个双向链表，而且还是一个循环的双向链表**。

怎么判断一个迭代器是随机访问迭代器还是双向迭代器

简单判断：迭代器+1，如果成功，就是随机访问迭代器，否就是双向迭代器。

案例：

    int main() 
    { 
    	vector<int> v; 
    	v.begin()+1;//随机访问迭代器 

    	list<int> l; 
    	//l.begin() +1;//err 双向迭代器 不允许+1 
    }

# 3list容器常用的API

3.1list构造函数

    list<T> lstT;//list采用采用模板类实现,对象的默认构造形式： 
    list(beg,end);//构造函数将[beg, end)区间中的元素拷贝给本身。 
    list(n,elem);//构造函数将n个elem拷贝给本身。 
    list(const list &lst);//拷贝构造函数。

3.2list数据元素的插入和删除操作

    push_back(elem);//在容器尾部加入一个元素
    pop_back();//删除容器中最后一个元素
    push_front(elem);//在容器开头插入一个元素
    pop_front();//从容器开头移除第一个元素
    insert(pos,elem);//在pos位置插elem元素的拷贝，返回新数据的位置。
    insert(pos,n,elem);//在pos位置插入n个elem数据，无返回值。
    insert(pos,beg,end);//在pos位置插入[beg,end)区间的数据，无返回值。
    clear();//移除容器的所有数据 erase(beg,end);//删除[beg,end)区间的数据，返回下一个数据的位置。
    erase(pos);//删除pos位置的数据，返回下一个数据的位置。
    remove(elem);//删除容器中所有与elem值匹配的元素。

3.3list大小操作

    size();//返回容器中元素的个数 
    empty();//判断容器是否为空 
    resize(num);//重新指定容器的长度为num， 若容器变长，则以默认值填充新位置。 如果容器变短，则末尾超出容器长度的元素被删除。 
    resize(num, elem);//重新指定容器的长度为num， 若容器变长，则以elem值填充新位置。 如果容器变短，则末尾超出容器长度的元素被删除。

3.4list赋值操作

    assign(beg, end);//将[beg, end)区间中的数据拷贝赋值给本身。 
    assign(n, elem);//将n个elem拷贝赋值给本身。 
    list& operator=(const list &lst);//重载等号操作符 
    swap(lst);//将lst与本身的元素互换

3.5list的数据存取

    front();//返回第一个元素。 
    back();//返回最后一个元素。 3.6.4.6 list反转排序 
    reverse();//反转链表，比如lst包含1,3,5元素，运行此方法后，lst就包含5,3,1元素。 
    sort(); //list排序

3.6list反转排序

    reverse();//反转链表，比如lst包含1,3,5元素，运行此方法后，lst就包含5,3,1元素。 
    sort(); //list排序

使用案例：

```cpp

	#include<iostream>
    #include<list>
    using namespace std;

    void printListInt(list<int> &l)
    {
        list<int>::iterator it=l.begin();
        for(;it!=l.end();it++)
        {
            cout<<*it<<" ";
        }
        cout<<endl;
    }

    void test()
    {
        list<int> l1(5,100);
        printListInt(l1);

        list<int> l2;
        l2.push_back(10);
        l2.push_back(20);
        l2.push_back(30);
        l2.push_front(40);
        l2.push_front(50);
        l2.push_front(60);
        printListInt(l2);

        l2.insert(++l2.begin(), 3,10);
        printListInt(l2);
        l2.erase(l2.begin());
        printListInt(l2);
        //remove根据节点内容删除
        l2.remove(10);
        printListInt(l2);//50 40 20 30

        cout<<"front = "<<l2.front()<<endl;
        cout<<"back = "<<l2.back()<<endl;

        //链表反转
        l2.reverse();
        printListInt(l2);//30 20 40 50

        //链表排序
        //STL的sort排序算法 只能用随机访问迭代器
        //list容器的迭代器为双向迭代器 所以无法使用系统算法
        //sort(l2.begin(), l2.end());//err
        l2.sort();
        printListInt(l2);//20 30 40 50

    }
    int main(void)
    {
        test();
        system("Pause");
        return 0;

    }
```

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fpic4.zhimg.com%2Fv2-72d322bae57c9a3e5132af93cbb4a1a3_b.jpg&valid=true)



