---
Project: "[[树的Anki整理]]"
Status: 🟨
tags:
  - Anki
Deadline: 2024-03-11
---
DECK: 数据结构::树

# 查找节点
## 操作
- 通过给定的num，以及根据二叉树的性质进行查找。
- 声明cur节点作为指针，进行比较cur.val和num的操作。
- ![[BAvMXOSKbrXnYZww-7a00f399-07c6-540f-4f19-bcaba335cc18.png|945]]

## 代码演示
```cpp
//在已有TreeNode树节点类和已知道root节点的操作
TreeNode *search(int num)
{
	TreeNode* cur = root; //声明cur节点，作为循环比较节点val和num值的指针
	while(cur != nullptr) //深入二叉树，使cur指向存在num的值
	{
		if(cur->val < num)
			cur = cur->right;
		else if(cur->val > num)
			cur = cur->left;
		else break;
	}
	return cur;
}
```

END
<!--ID: 1710165805487-->


# 插入节点
## 操作
- 给定一个num值，为了保持二叉搜索树"左子树<根节点<右子树"的性质有以下操作
- ![[BAvMXOSKbrXnYZww-c5d4da1f-cade-ab38-0038-771942e5a487.png|948]]
- ![[BAvMXOSKbrXnYZww-1ff89fdc-9290-e91b-8357-d31a53737df7.png|746]]
### 注意
- 二叉搜索树不允许存在重复节点，因此若num已存在，则直接返回，不执行插入
- 需要cur指针和pre指针进行向下搜索，从而完成二叉搜索树插入的操作



## 代码演示
```cpp
//已有TreeNode和已知root根节点的前提下

void insert(int num)
{
	//如果为空树，则直接将num值插入root中
	if(root == nullptr)
	{
		root = new TreeNode(num);
		return;
	}
	TreeNode *cur = root , *pre = nullptr;
	//向下搜索
	while(cur != nullptr)
	{
		if(cur->val == num)
			return;
		pre = cur;
		if(cur->val > num)
			cur = cur->left;
		if(cur->val < num)
			cur = cur->right;
	}
	//在pre指针到达底部后，创建指针
	TreeNode *Node = new TreeNode(num);
	if(pre->val < num)
		pre->right = node;
	else
		pre->left = node;
}
```

END
<!--ID: 1710165805508-->

# 删除节点
## 操作
- 与插入节点类似，先在二叉树查找到目标节点，再讲其从二叉树中删除
### 注意
- 在保证删除操作完成后，二叉搜索树的"左子树<根节点<右子树"的性质仍然成立
### 根据度完成操作
- 当待删除节点的度为0时，表示该节点是叶节点，可以直接删除
- ![[BAvMXOSKbrXnYZww-7cf487e2-2a64-4e41-85a8-3f8897fec82c.png|747]]
- 当待删除节点的度为1时，将待删除节点的子姐带你替换为pre父节点的子节点即可
- ![[BAvMXOSKbrXnYZww-b20ef75e-c8df-1696-94ac-a7788b4bc3cd.png|750]]
- 当待删除节点的度为2时，无法直接删除，需要使用一个节点替换该节点。这个节点可以是cur右子树最小节点或cur左子树下一层的最大节点
- ![[BAvMXOSKbrXnYZww-c7e9bc57-0da6-7d7a-eaa9-6b9b12513608.png|944]]


## 代码演示
```cpp
//在已有TreeNode类和已知root节点的情况下
void remove(int num)
{
	//若树为空，直接提前返回
	if(root == nullptr)
		return;
	TreeNode *cur = root,*pre = nullptr;
	//向下搜索
	while(cur != nullptr)
	{
		if(cur->val == num)
			break;
		pre = cur;
		if(cur->val < num)
			cur = cur->right;
		else 
			cur = cur->left;
	}
	//若没有待删除节点，则直接返回
	if(cur == nullptr)
		return;
	//子节点数量为0 or 1
	if(cur -> left == nullptr or cur->right == nullptr)
	{
		TreeNode *child = cur->left != nullptr ? cur->left : cur->right;
		//删除节点cur
		if(cur != root)
		{
			if(pre-> left == cur)
				pre->left = child;
			else
				pre->right = child;
		}else{
			root = child;
		}
		delete cur;
		cur = nullptr; //避免悬空指针
	}
	//子节点数量为2,修改节点的值实现
	else
	{
		TreeNode *tmp = cur->right; //若cur->right为nullptr,将cur->right的val赋值给cur,并删除cur->right
		while(tmp->left != nullptr)
		{
			tmp = tmp->left;
		}
		//若cur->right不为nullptr,将左子树最小值赋值给cur，并删除最左子树的节点
		int tmpVal = tmp->val;
		//递归删除节点tmp
		remove(tmp->val);
		cur->val = tmpVal;
	}
}
```

END
<!--ID: 1710165805518-->

# 有序中序遍历
- 二叉树中序遍历遵循"左->根->右"的遍历顺序，而二叉搜索树满足"左子节点<根节点<右子节点"的大小关系
- 也就是说，二叉搜索树的中序遍历序列是升序的



END
<!--ID: 1710165805523-->


