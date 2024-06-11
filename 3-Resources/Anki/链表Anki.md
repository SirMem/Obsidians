---
Resource: "[[链表基本内容]]"
Status: 🟨
Deadline: 2024-03-18
CreateTime: 2024-03-18
tags:
  - Anki
---
DECK: 数据结构::链表
FILE TAGS: 链表
# 介绍
「链表 linked list」是一种线性数据结构，其中的每个元素都是一个节点对象，各个节点通过“引用”相连接。引用记录了下一个节点的内存地址，通过它可以从当前节点访问到下一个节点。
链表的设计使得各个节点可以被分散存储在内存各处，它们的内存地址是无须连续的。
![[BAvMXOSKbrXnYZww-3f7f4647-154f-5e47-e4be-70123a63ca3c.png|746]]
END
<!--ID: 1710726265201-->

# 初始化链表
```cpp
/* 链表节点结构体 */
struct ListNode {
	int val;
	// 节点值
	ListNode *next; // 指向下一节点的指针
	ListNode(int x) : val(x), next(nullptr) {} // 构造函数
};
/* 初始化链表 1 -> 3 -> 2 -> 5 -> 4 */
// 初始化各个节点
ListNode* n0 = new ListNode(1);
ListNode* n1 = new ListNode(3);
ListNode* n2 = new ListNode(2);
ListNode* n3 = new ListNode(5);
ListNode* n4 = new ListNode(4);
// 构建引用指向
n0->next = n1;
n1->next = n2;
n2->next = n3;
n3->next = n4;
```
END
<!--ID: 1710726265207-->

# 操作
```cpp
/* 在链表的节点 n0 之后插入节点 P */
void insert(ListNode *n0, ListNode *P) {
	ListNode *n1 = n0->next;
	P->next = n1;
	n0->next = P;
}
/* 删除链表的节点 n0 之后的首个节点 */
void remove(ListNode *n0) {
	if (n0->next == nullptr)
	return;
	// n0 -> P -> n1
	ListNode *P = n0->next;
	ListNode *n1 = P->next;
	n0->next = n1;
	// 释放内存
	delete P;
}
/* 访问链表中索引为 index 的节点 */
ListNode *access(ListNode *head, int index) {
	for (int i = 0; i < index; i++) {
		if (head == nullptr)
			return nullptr;
		head = head->next;
	}
	return head;
}
/* 在链表中查找值为 target 的首个节点 */
int find(ListNode *head, int target) {
	int index = 0;
	while (head != nullptr) {
		if (head->val == target)
			return index;
		head = head->next;
		index++;
	}
	return -1;
}
```

END
<!--ID: 1710726265213-->

## 双向链表
```cpp
/* 双向链表节点结构体 */
struct ListNode {
	int val;
	ListNode *next; // 指向后继节点的指针
	ListNode *prev; // 指向前驱节点的指针
	ListNode(int x) : val(x), next(nullptr), prev(nullptr) {} // 构造函数
};
```

END
<!--ID: 1710726265221-->
