---
Resource: "[[AVL树]]"
Status: 🟨
Deadline: 2024-03-12
CreateTime: 2024-03-12
tags:
  - Anki
---
DECK: 数据结构::树
# AVL的来源是什么
二叉搜索树在多次插入和删除操作后,可能退化为链表。这种情况下，所有操作的时间复杂度将从 𝑂(log 𝑛) 恶化为 𝑂(𝑛)。
![[BAvMXOSKbrXnYZww-49fcb223-1732-74e7-95a9-0ec028176e29.png|756]]
![[BAvMXOSKbrXnYZww-434ae2f2-aa63-8f47-fee8-173bb35dbfb9.png|963]]

END
<!--ID: 1710250273096-->

# AVL树的定义
AVL 树既是二叉搜索树也是平衡二叉树，同时满足这两类二叉树的所有性质，因此也被称「平衡二叉搜索树 balanced binary search tree」。
END
<!--ID: 1710250273100-->

# AVL树的声明

由于 AVL 树的相关操作需要获取节点高度，因此我们需要为节点类添加 height 变量。
```cpp
class TreeNode
{
	int val{};
	int hegiht = 0;
	TreeNode *left{};
	TreeNode *right{};
	TreeNode() = default; //默认隐式初始化
	explicit TreeNode(int x):val(x){} //显式构造函数
}
```

END
<!--ID: 1710250273104-->

# AVL的特征
## 节点高度
```cpp
//获取节点高度
int height(TreeNode *node)
{
	//空节点高度为-1,叶节点高度为0
	return node == nullptr ? -1 : node->height;
}

//更新节点高度
void updateHeight(TreeNode *node)
{
	//节点高度等于最高子树高度+1
	node->height = max(height(node->left),height(node->right)) + 1; //max返回两个比较值的最大值
}
```
END
<!--ID: 1710250273108-->


## 节点平衡因子
节点的「平衡因子 balance factor」定义为节点左子树的高度减去右子树的高度，同时规定空节点的平衡因子为 0 。一棵AVL 树的任意节点的平衡因子皆满足−1 ≤ 𝑓 ≤ 1
```cpp
/*获取平衡因子*/
int balanceFactor(Tree *node)
{
	//空节点平衡因子为0
	if(node == nullptr)
		return 0;
	return height(node->left) - height(node->right); //左树长度大于右树长度返回正值，反之则返回负值
}
```
END
<!--ID: 1710250273111-->

## AVL树旋转
- AVL 树的特点在于“旋转”操作，它能够在不影响二叉树的中序遍历序列的前提下，使失衡节点重新恢复平衡。换句话说，旋转操作既能保持“二叉搜索树”的性质，也能使树重新变为“平衡二叉树”。
- 我们将平衡因子绝对值> 1 的节点称为“失衡节点”。根据节点失衡情况的不同，旋转操作分为四种：右旋、左旋、先右旋后左旋、先左旋后右旋。
### 右旋
![[BAvMXOSKbrXnYZww-24320953-abb4-5f70-66d2-a51871a90f6a.png|940]]
二叉树中首个失衡节点是“节点3”。关注以该失衡节点为根节点的子树，将该节点记为 node ，其左子节点记为 child ，执行“右旋”操作。完成右旋后，子树已经恢复平衡，并且仍然保持二叉搜索树的特性。
![[BAvMXOSKbrXnYZww-f25b5c9c-ac2f-9ffa-06ee-27228054fe00.png|745]]
当节点 child 有右子节点（记为 grandChild ）时，需要在右旋中添加一步：将 grandChild作为 node 的左子节点。
```cpp
//右旋操作
TreeNode *rightRotate(TreeNode *node)
{
	TreeNode *child = node->left;
	TreeNode *grandChild = child ->right;
	//以child为原点,将node向右旋转
	child->right = node; //节点转移
	node->left = grandChild;
	//更新节点高度
	updateHeight(node);
	updateHeight(child);
	//返回旋转后子树的根节点
	return child;
}
```
END
<!--ID: 1710250273113-->

### 左旋
左旋即是右旋的"镜像"
![[BAvMXOSKbrXnYZww-1d3f38ae-2031-cb07-f710-e90e21e269da.png|761]]
同理，当节点child有左子节点(记为grandChild)时，需要在左旋添加一步:将grandChild作为node的右子节点。
![[BAvMXOSKbrXnYZww-8e84d077-2c2c-b668-c235-219e1d302c4f.png|761]]
可以观察到，右旋和左旋操作在逻辑上是镜像对称的，它们分别解决的两种失衡情况也是对称的。基于对称性，我们只需将右旋的实现代码中的所有的 left 替换为 right ，将所有的 right 替换为 left ，即可得到左旋的实现代码。
```cpp
TreeNode *leftRotate(TreeNode *node)
{
	TreeNode *child = node->right;
	TreeNode *grandChild = child->left;
	//以child为原点,将node向左旋转
	child->left = node;
	node->right = grandChild;
	//更新节点高度
	updateHeight(node);
	updateHeight(child);
	//返回旋转后子树的根节点
	return child;
}
```
END
<!--ID: 1710250273116-->

### 先左旋后右旋和先右旋后左旋
![[BAvMXOSKbrXnYZww-3ad3bd36-8ba6-31cf-1cf1-0e6782311a5e.png|759]]
### 整体代码演示
```cpp
//执行旋转操作,使该子树重新恢复平衡
TreeNode *rotate(TreeNode *node)
{
	//获取节点node的平衡因子
	int _balanceFactor = balanceFactor(node);
	//左偏树
	if(_balanceFactor > 1)
	{
		if(balanceFactor(node->left)>=0)
		{
			//右旋
			return rightRotate(node);
		}else
		{
			//先左旋后右旋
			node->left = leftRotate(node->left);
			return rightRotate(node);
		}	
	}
	//右偏树
	if(_balanceFactor < -1)
	{
		if(balanceFactor(node->right) <=0 )
		{
			//左旋
			return leftRotate(node);
		}else
		{
			//先右旋后左旋
			node->right = rightRotate(node->right);
			return leftRotate(node);
		}
	}
	//平衡树，无须旋转，直接返回
	return node;
}

```
END
<!--ID: 1710250273119-->

# AVL树的操作
## 插入节点
AVL树的节点插入操作与二叉搜索树在主体上类似。唯一的区别在于，在AVL树中插入节点后，从该节点到根节点的路径上可能会出现一系列失衡节点。因此，我们需要从这个节点开始，自底向上执行旋转操作，使所有失衡节点恢复平衡。
```cpp
void insert(int val)
{
	root = insertHelper(root,val);
}
/*递归插入节点*/
TreeNode *insertHelper(TreeNode *node,int val)
{
	if(node == nullptr)
		return new TreeNode(val);
	//查找插入位置，并插入节点
	if(val<node->val)
		node->left = insertHelper(node->left,val);
	else if(val > node->val)
		node->right = insertHelper(node->right,val);
	else 
		return node;//重复节点不插入，直接返回
	//执行旋转操作，使该子树重新恢复平衡
	updateHeight(node);
	node = rotate(node);
	return node;
}
```
END
<!--ID: 1710250273122-->

## 删除节点
在二叉搜索树删除节点的基础上，需要从底至顶地执行旋转操作，使所有失衡节点恢复平衡。
```cpp
//删除节点
void remove(int val)
{
	root = removeHelper(root,val);
}
//递归删除节点
TreeNode *removeHelper(TreeNode *node,int val)
{
	if(node == nullptr)
		return nullptr;
	//1.查找节点，并删除之
	if(val < node->val)
		node->left = removeHelper(node->left, val)
	else if(val > node)
		node->right = removeHelper(node->right,val);
	else
	{
		if(node->left == nullptr or node->right == nullptr)
		{
			TreeNode *child = node->left !=nullptr ? node->left : node->right;
			//子节点数量 = 0，直接删除node并返回
			if(child == nullptr)
			{
				delete node;
				return nullptr;
			}else
			{
				delete node;
				node = child;
			}else
			{
				//子节点数量= 2,则将中序遍历的下个节点删除，并用该节点替换当前节点
				TreeNode *temp = node->right;
				while(temp->left != nullptr)
				{
					temp = temp->left;
				}
				int tempVal = temp->val;
				node->right = removeHelper(node->right,temp->val);
				node->val = tempVal;
			}
		}
	}
	updateHeight(node);
	node = rotate(node);
	return node;
}
```
END

## 查找节点
AVL 树的节点查找操作与二叉搜索树一致
![[二叉搜索树的各类操作#查找节点]]
