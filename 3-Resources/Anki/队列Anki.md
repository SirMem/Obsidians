---
Resource:
Status:
Deadline:
CreateTime:
tags: Anki
---
DECK: 数据结构::队列
FILE TAGS: 队列
# 队列介绍
「队列 queue」是一种遵循先入先出规则的线性数据结构。顾名思义，队列模拟了排队现象，即新来的人不断加入队列的尾部，而位于队列头部的人逐个离开。
![[BAvMXOSKbrXnYZww-9e3bef12-7c94-cb5b-f8ef-0e3491a4960d.png|155]]

END
<!--ID: 1710726265157-->

# 实现
## 链表
![[BAvMXOSKbrXnYZww-4db17480-3d86-80f3-be51-79cc95a00fc9.png|957]]
```cpp
/* 基于链表实现的队列 */
class LinkedListQueue {
	private:
		ListNode *front, *rear; // 头节点 front ，尾节点 rear
		int queSize;
	public:
		LinkedListQueue() {
			front = nullptr;
			rear = nullptr;
			queSize = 0;
		}
		~LinkedListQueue() {
		// 遍历链表删除节点，释放内存
			freeMemoryLinkedList(front);//释放队列函数，未写出
		}
		/* 获取队列的长度 */
		int size() {
			return queSize;
		}
		/* 判断队列是否为空 */
		bool empty() {
			return queSize == 0;
		}
		/* 入队 */
		void push(int num) {
			// 尾节点后添加 num
			ListNode *node = new ListNode(num);
			// 如果队列为空，则令头、尾节点都指向该节点
			if (front == nullptr) {
				front = node;
				rear = node;
			}
			// 如果队列不为空，则将该节点添加到尾节点后
			else {
				rear->next = node;
				rear = node;
			}
			queSize++;
		}
		/* 出队 */
		void pop() {
			int num = peek();
			// 删除头节点
			ListNode *tmp = front;
			front = front->next;
			// 释放内存
			delete tmp;
			queSize--;
		}
		/* 访问队首元素 */
		int peek() {
			if (size() == 0)
				throw out_of_range(" 队列为空");
			return front->val;
		}
		/* 将链表转化为 Vector 并返回 */
		vector<int> toVector() {
			ListNode *node = front;
			vector<int> res(size());
			for (int i = 0; i < res.size(); i++) {
				res[i] = node->val;
				node = node->next;
			}
			return res;
		}
};
		
```

END
<!--ID: 1710726265164-->

## 数组
可以使用一个变量 front 指向队首元素的索引，并维护一个变量 size 用于记录队列长度。定义rear = front + size ，这个公式计算出的 rear 指向队尾元素之后的下一个位置。需要让 front 或 rear 在越过数组尾部时，直接回到数组头部继续遍历。

环形数组
![[BAvMXOSKbrXnYZww-a1ecc6f0-9587-16c4-2453-1290c56d05dd.png|948]]
```cpp
/* 基于环形数组实现的队列 */
class ArrayQueue {
	private:
		int *nums; // 用于存储队列元素的数组
		int front; // 队首指针，指向队首元素
		int queSize; // 队列长度
		int queCapacity; // 队列容量
	public:
		ArrayQueue(int capacity) {
			// 初始化数组
			nums = new int[capacity];
			queCapacity = capacity;
			front = queSize = 0;
		}
		~ArrayQueue() {
			delete[] nums;
		}
		/* 获取队列的容量 */
		int capacity() {
			return queCapacity;
		}
		/* 获取队列的长度 */
		int size() {
			return queSize;
		}
		/* 判断队列是否为空 */
		bool empty() {
			return size() == 0;
		}
		/* 入队 */
		void push(int num) {
			if (queSize == queCapacity) {
				cout << " 队列已满" << endl;
				return;
			}
			// 计算队尾指针，指向队尾索引 + 1
			// 通过取余操作，实现 rear 越过数组尾部后回到头部
			int rear = (front + queSize) % queCapacity;
			// 将 num 添加至队尾
			nums[rear] = num;
			queSize++;
		}
		/* 出队 */
		void pop() {
			int num = peek();
			// 队首指针向后移动一位，若越过尾部则返回到数组头部
			front = (front + 1) % queCapacity;
			queSize--;
		}
		/* 访问队首元素 */
		int peek() {
			if (empty())
				throw out_of_range(" 队列为空");
			return nums[front];
		}
		/* 将数组转化为 Vector 并返回 */
		vector<int> toVector() {
		// 仅转换有效长度范围内的列表元素
			vector<int> arr(queSize);
			for (int i = 0, j = front; i < queSize; i++, j++) {
				arr[i] = nums[j % queCapacity];
			}
			return arr;
		}
};
```

END
<!--ID: 1710726265170-->


# 双向队列介绍
「双向队列 deque」提供了更高的灵活性，允许在头部和尾部执行元素的添加或删除操作。
![[BAvMXOSKbrXnYZww-776248b7-75a2-27e9-e68b-14209f5fc366.png|676]]

END
<!--ID: 1710726265177-->

# 常用操作
## STL
```cpp
/* 初始化双向队列 */
deque<int> deque;
/* 元素入队 */
deque.push_back(2);
// 添加至队尾
deque.push_back(5);
deque.push_back(4);
deque.push_front(3); // 添加至队首
deque.push_front(1);
/* 访问元素 */
int front = deque.front(); // 队首元素
int back = deque.back();
// 队尾元素
/* 元素出队 */
deque.pop_front(); // 队首元素出队
deque.pop_back();
// 队尾元素出队
/* 获取双向队列的长度 */
int size = deque.size();
/* 判断双向队列是否为空 */
bool empty = deque.empty();
```

END
<!--ID: 1710726265184-->

## 双向链表
![[BAvMXOSKbrXnYZww-4317b66e-ef83-42b6-090b-5ba62db2fa10.png|933]]
```cpp
/* 双向链表节点 */
struct DoublyListNode {
	int val;
	// 节点值
	DoublyListNode *next; // 后继节点指针
	DoublyListNode *prev; // 前驱节点指针
	DoublyListNode(int val) : val(val), prev(nullptr), next(nullptr) {
	}
};

/* 基于双向链表实现的双向队列 */
class LinkedListDeque {
	private:
		DoublyListNode *front, *rear; // 头节点 front ，尾节点 rear
		int queSize = 0; // 双向队列的长度		
	public:
		/* 构造方法 */
		LinkedListDeque() : front(nullptr), rear(nullptr) {}
		/* 析构方法 */
		~LinkedListDeque() {
			// 遍历链表删除节点，释放内存
			DoublyListNode *pre, *cur = front;
			while (cur != nullptr) {
				pre = cur;
				cur = cur->next;
				delete pre;
			}
		}
		/* 获取双向队列的长度 */
		int size() {
			return queSize;
		}
		/* 判断双向队列是否为空 */
		bool isEmpty() {
			return size() == 0;
		}
		/* 入队操作 */
		void push(int num, bool isFront) {
			DoublyListNode *node = new DoublyListNode(num);
			// 若链表为空，则令 front, rear 都指向 node
			if (isEmpty())
				front = rear = node;
			// 队首入队操作
			else if (isFront) {
				// 将 node 添加至链表头部
				front->prev = node;
				node->next = front;
				front = node; // 更新头节点
			// 队尾入队操作
			} else {
				// 将 node 添加至链表尾部
				rear->next = node;
				node->prev = rear;
				rear = node; // 更新尾节点
			}
			queSize++; // 更新队列长度
		}
		/* 队首入队 */
		void pushFirst(int num) {
			push(num, true);
		}
		/* 队尾入队 */
		void pushLast(int num) {
			push(num, false);
		}
		/* 出队操作 */
		int pop(bool isFront) {
			// 若队列为空，直接返回 -1
			if (isEmpty())
				return -1;
			int val;
			// 队首出队操作
			if (isFront) {
				val = front->val; // 暂存头节点值
				// 删除头节点
				DoublyListNode *fNext = front->next;
				if (fNext != nullptr) {
					fNext->prev = nullptr;
					front->next = nullptr;
					delete front;
				}
				front = fNext; // 更新头节点
			// 队尾出队操作
			} else {
				val = rear->val; // 暂存尾节点值
				// 删除尾节点
				DoublyListNode *rPrev = rear->prev;
				if (rPrev != nullptr) {
					rPrev->next = nullptr;
					rear->prev = nullptr;
					delete rear;
				}
				rear = rPrev; // 更新尾节点
			}
			queSize--; // 更新队列长度
			return val;
		}
		/* 队首出队 */
		int popFirst() {
			return pop(true);
		}
		/* 队尾出队 */
		int popLast() {
			return pop(false);
		}
		/* 访问队首元素 */
		int peekFirst() {
			return isEmpty() ? -1 : front->val;
		}
		/* 访问队尾元素 */
		int peekLast() {
			return isEmpty() ? -1 : rear->val;
		}
		/* 返回数组用于打印 */
		vector<int> toVector() {
			DoublyListNode *node = front;
			vector<int> res(size());
			for (int i = 0; i < res.size(); i++) {
				res[i] = node->val;
				node = node->next;
			}
			return res;
		}
};

```

END
<!--ID: 1710726265191-->

## 数组
![[BAvMXOSKbrXnYZww-35d9cf16-cb4e-f357-a960-ba53d4664650.png|939]]
在队列的实现基础上，仅需增加“队首入队”和“队尾出队”的方法。
```cpp
/* 基于环形数组实现的双向队列 */
class ArrayDeque {
	private:
		vector<int> nums; // 用于存储双向队列元素的数组
		int front; // 队首指针，指向队首元素
		int queSize; // 双向队列长度
	public:
		/* 构造方法 */
		ArrayDeque(int capacity) {
			nums.resize(capacity);
			front = queSize = 0;
		}
		/* 获取双向队列的容量 */
		int capacity() {
			return nums.size();
		}
		/* 获取双向队列的长度 */
		int size() {
			return queSize;
		}
		/* 判断双向队列是否为空 */
		bool isEmpty() {
			return queSize == 0;
		}
		/* 计算环形数组索引 */
		int index(int i) {
		// 通过取余操作实现数组首尾相连
		// 当 i 越过数组尾部后，回到头部
		// 当 i 越过数组头部后，回到尾部
			return (i + capacity()) % capacity();
		}
		/* 队首入队 */
		void pushFirst(int num) {
			if (queSize == capacity()) {
				cout << " 双向队列已满" << endl;
				return;
			}
			// 队首指针向左移动一位
			// 通过取余操作，实现 front 越过数组头部后回到尾部
			front = index(front - 1);
			// 将 num 添加至队首
			nums[front] = num;
			queSize++;
		}
		/* 队尾入队 */
		void pushLast(int num) {
			if (queSize == capacity()) {
				cout << " 双向队列已满" << endl;
				return;
			}
			// 计算尾指针，指向队尾索引 + 1
			int rear = index(front + queSize);
			// 将 num 添加至队尾
			nums[rear] = num;
			queSize++;
		}
		/* 队首出队 */
		int popFirst() {
			int num = peekFirst();
			// 队首指针向后移动一位
			front = index(front + 1);
			queSize--;
			return num;
		}
		/* 队尾出队 */
		int popLast() {
			int num = peekLast();
			queSize--;
			return num;
		}
		/* 访问队首元素 */
		int peekFirst() {
			if (isEmpty())
				throw out_of_range(" 双向队列为空");
			return nums[front];
		}
		/* 访问队尾元素 */
		int peekLast() {
			if (isEmpty())
				throw out_of_range(" 双向队列为空");
			// 计算尾元素索引
			int last = index(front + queSize - 1);
			return nums[last];
		}
		/* 返回数组用于打印 */
		vector<int> toVector() {
			// 仅转换有效长度范围内的列表元素
			vector<int> res(queSize);
			for (int i = 0, j = front; i < queSize; i++, j++) {
				res[i] = nums[index(j)];
			}
			return res;
		}
};		
```

END