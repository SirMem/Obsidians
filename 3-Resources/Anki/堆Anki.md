---
Resource: "[[堆的结构]]"
Status: 
Deadline: 
CreateTime: 
tags:
  - Anki
---
#flashcards 

# 堆的基本结构
「堆 heap」是一种满足特定条件的完全二叉树。其中
- 「大顶堆 max heap」：任意节点的值≥其子节点的值
- 「小顶堆 min heap」：任意节点的值≤其子节点的值
- ![[BAvMXOSKbrXnYZww-b0bfee23-824e-80c8-d7de-890f42bb264d.png|749]]
堆作为完全二叉树的一个特例，具有以下特性
- 最底层节点靠左填充，其他层的节点都被填满。
- 我们将二叉树的根节点称为"堆顶"，将底层最靠右的节点称为"堆底"。
- 对于大顶堆(小顶堆)，堆顶元素(即根节点)的值分别是最大(最小)的。

END



# 优先队列

## 简介
**优先队列（Priority Queue）**：一种特殊的队列。在优先队列中，元素被赋予优先级，当访问队列元素时，具有最高优先级的元素最先删除。
优先队列与普通队列最大的不同点在于 **出队顺序**。
- 普通队列的出队顺序跟入队顺序相关，符合「先进先出（First in, First out）」的规则。
- 优先队列的出队顺序跟入队顺序无关，优先队列是按照元素的优先级来决定出队顺序的。优先级高的元素优先出队，优先级低的元素后出队。优先队列符合 **「最高级先出（First in, Largest out）」** 的规则。

# 堆实现优先队列
堆通常用作实现优先队列，大顶堆相当于元素按从大到小顺序出队的优先队列。从使用角度来看，我们可以将“优先队列”和“堆”看作等价的数据结构。

END 
# 堆的初始化
```cpp
/*初始化堆*/
//初始化小顶堆
priority_queue<int,vector<int>,greater<int>> minHeap;
//初始化大顶堆
priority_queue<int,vector<int>,less<int>> maxHeap;

/*元素入堆*/
maxHeap.push(1);
maxHeap.push(3);
maxHeap.push(2);
maxHeap.push(5);
maxHeap.push(4);

//获取堆顶元素
int peek = maxHeap.top(); //5

//堆顶元素出堆
//出堆元素会形成一个从大到小的序列
maxHeap.pop(); //5
maxHeap.pop(); //4
maxHeap.pop(); //3
maxHeap.pop(); //2
maxHeap.pop(); //1

//获取堆大小
int size = maxHeap.size();

//判断堆是否为空
bool isEmpty = maxHeap.empty();

//输入列表并建堆
vector<int> input{1,3,2,5,4};
priority_queue<int,vector<int>,greater<int>> minHeap(input.begin(),input.end());
//优先队列创建

```

END



# 堆的实现 
因为堆是一种完全二叉树，而完全二叉树非常适合用数组表示，因此采用数组来存储堆
给定索引 𝑖 ，其左子节点索引为 2𝑖 + 1 ，右子节点索引为 2𝑖 + 2 ，父节点索引为 (𝑖 − 1)/2（向下取整）。当索引越界时，表示空节点或节点不存在。
![[BAvMXOSKbrXnYZww-ba9ef284-e35f-b297-7339-467ab978f17b.png|749]]
```cpp
//获取左子节点索引
int left(int i)
	return 2*i+1;
//获取右子节点索引
int right(int i)
	return 2*i+2;
//获取父节点索引
int parent(int i)
	return (i-1)/2;//向下取整
```

END


# 访问堆顶元素
```cpp
int peek()
	return maxHeap[0];
```

END


# 元素入堆
给定元素val,首先将其添加到堆底。添加后，由于val可能大于堆中其他元素，堆的成立条件可能已被破坏。因此，需要修复从插入节点到根节点的路径上的各个节点，这个操作被称为「堆化 heapify」。
考虑从入堆节点开始，从底至顶执行堆化。比较插入节点与其父节点的值，如果插入节点更大，则将它们交换。然后继续执行此操作，从底至顶修复堆中的各个节点，直至越过根节点或遇到无须交换的节点时结束。
![[BAvMXOSKbrXnYZww-be3d89b7-6a43-6128-38f7-0f52f7918674.png|942]]
![[BAvMXOSKbrXnYZww-cc2ca3c1-5b19-7354-e1e7-08528c076268.png|468]]
```cpp
//元素入堆
void push(int val)
{
	//添加节点
	maxHeap.push_back(val);
	//从底至顶堆化
	siftUp(size()-1);
}

//从节点i开始，从底至顶堆化
void siftUp(int i)
{
	while(true)
	{
		//获取节点i的父节点
		int p = parent(i);
		//当"越过根节点"或"节点无须修复"时，结束堆化
		if(p < 0 or maxHeap[i] <= maxHeap[p])
			break;
		//交换两节点
		swap(maxHeap[i],maxHeap[p]);
		i = p;
	}
}
```

END


# 堆顶元素出堆
为了尽量减少元素索引的变动，我们采用以下操作步骤
- 交换堆顶元素与堆底元素(即交换根节点与最右叶节点)
- 交换完成后，将堆底从列表中删除
- 从根节点开始，从顶至底执行堆化。
![[BAvMXOSKbrXnYZww-4a2c2afb-2692-4f6d-c2d3-ff55e831a589.png|935]]
![[BAvMXOSKbrXnYZww-5c256adf-b4ec-2e40-964c-5c6939f250c2.png|939]]

```cpp
//元素出堆
void pop()
{
	//判空处理
	if(empty())
	{
		throw out_of_range("堆为空");
	}
	//交换根节点与最右叶节点(即交换首元素与尾元素)
	swap(maxHeap[0],maxHeap[size()-1]);
	//删除节点
	maxHeap.pop_back();
	//从顶至底堆化
	siftDown(0);
}
/*从节点i开始，从顶至底堆化*/
void siftDown(int i)
{
	while(true)
	{
		//判断节点i,l,r中值最大的节点,记为ma
		int l =left(i), r = right(i), ma = i;
		//若节点i最大或索引l,r越界,则无须继续堆化，跳出
		if(l<size() and maxHeap[l] > maxHeap[ma])
			ma = l;
		if (r < size() and maxHeap[r] > maxHeap[ma])
			ma = r;
		// 若节点 i 最大或索引 l, r 越界，则无须继续堆化，跳出
		if (ma == i)
			break;
		swap(maxHeap[i], maxHeap[ma]);
		// 循环向下堆化
		i = ma;
	}
}
```

END


# 建堆操作
## 自上而下构建
首先创建一个空堆，然后遍历列表，依次对每个元素执行“入堆操作”，即先将元素添加至堆的尾部，再对该元素执行“从底至顶”堆化。每当一个元素入堆，堆的长度就加一，因此堆是“自上而下”地构建的。
## 自下而上构建
- 将列表所有元素原封不动添加到堆中
- 倒序遍历堆(即层序遍历的倒序)，依次对每个非叶节点执行"从顶至底堆化"
在倒序遍历中，堆是“自下而上”地构建的，需要重点理解以下两点。
- 由于叶节点没有子节点，因此无需对它们执行堆化。最后一个节点的父节点是最后一个非叶节点。
- 在倒序遍历中，我们能够保证当前节点之下的子树已经完成堆化（已经是合法的堆），而这是堆化当前节点的前置条件。
```cpp
//根据输入列表建堆
//构建函数MaxHeap
MaxHeap(vector<int> nums)
{
	//将列表元素原封不动添加进堆
	maxHeap = nums;
	//堆化处叶节点以外的其他所有节点，从堆底堆化至堆顶
	for(int i = parent(size()-1);i >= 0; i--)
	{
		siftDown(i);//与索引节点的子节点进行比较交换
	}
}
```
## 自上而下和自下而上建堆的区别
自上而下建堆复杂度为O(nlogn)，而自下而上输入列表并建堆的时间复杂度为 𝑂(𝑛) ，非常高效。

END



# 小结
- 堆是一棵完全二叉树，根据成立条件可分为大顶堆和小顶堆。大（小）顶堆的堆顶元素是最大（小）的。
- 优先队列的定义是具有出队优先级的队列，通常使用堆来实现。
- 堆的常用操作及其对应的时间复杂度包括：元素入堆 𝑂(log 𝑛)、堆顶元素出堆 𝑂(log 𝑛) 和访问堆顶元素 𝑂(1) 等。
- 完全二叉树非常适合用数组表示，因此我们通常使用数组来存储堆。
- 
- 堆化操作用于维护堆的性质，在入堆和出堆操作中都会用到。
- 输入 𝑛 个元素并建堆的时间复杂度可以优化至 𝑂(𝑛) ，非常高效。
- Top‑K 是一个经典算法问题，可以使用堆数据结构高效解决，时间复杂度为 𝑂(𝑛 log 𝑘) 

# Q&A
## 数据结构的“堆”与内存管理的“堆”是同一个概念吗？
两者不是同一个概念，只是碰巧都叫堆。计算机系统内存中的堆是动态内存分配的一部分，程序在运行时可以使用它来存储数据。程序可以请求一定量的堆内存，用于存储如对象和数组等复杂结构。当这些数据不再需要时，程序需要释放这些内存，以防止内存泄露。相较于栈内存，堆内存的管理和使用需要更谨慎，不恰当的使用可能会导致内存泄露和野指针等问题。

END

