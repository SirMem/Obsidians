---
Resource: "[[哈希表基本定义]]"
Status: 
Deadline: 
CreateTime: 
tags:
  - Anki
---
DECK: 数据结构::哈希表
FILE TAGS: 哈希表

# 定义
哈希表是一个通过建立键 key 与值 value 之间的映射，实现高效的元素查询的数据结构
![[BAvMXOSKbrXnYZww-766382f6-35f5-486c-0948-15fb345814d3.png|688]]

# 桶
哈希表是基于数组实现的，哈希表中每一个元素称为桶，桶内存有键和值。
# 基本结构
![[3-Resources/excalidraw/哈希表Excalidraw.md#^group=ohLYhmklM7FrTCJDeBXZT]]

END
<!--ID: 1710725561965-->


# 基本操作
## STL哈希表
```cpp
//STL方式创建哈希表
//初始化
unordered_map<int, string> map;
//添加键值对
map[12836] = " 小哈";
map[15937] = " 小啰";
map[16750] = " 小算";
map[13276] = " 小法";
map[10583] = " 小鸭";
//查询操作
// 向哈希表输入键 key ，得到值 value
string name = map[15937];
/* 删除操作 */
// 在哈希表中删除键值对 (key, value)
map.erase(10583);
/* 遍历哈希表 */
// 遍历键值对 key->value
for (auto kv: map) {
	cout << kv.first << " -> " << kv.second << endl;
}
// 单独遍历键 key
for (auto key: map) {
	cout << key.first << endl;
}
// 单独遍历值 value
for (auto val: map) {
	cout << val.second << endl;
}

```

END 
## vector实现哈希表
### 初始化创建
```cpp
//键值对初始化
class Pair
{
	public:
		int key;
		string val;
		Pair(int key,string val)
		{
			this->key = key;
			this->val = val;
		}
}
/* 基于数组简易实现的哈希表 */
class ArrayHashMap {
	private:
		vector<Pair *> buckets;
	public:
		ArrayHashMap() {
		// 初始化数组，包含 100 个桶
			buckets = vector<Pair *>(100);
		}
		~ArrayHashMap() {
		// 释放内存
			for (const auto &bucket : buckets) {
				delete bucket;
			}
			buckets.clear();
		}
		/* 哈希函数 */
		int hashFunc(int key) {
			int index = key % 100;
			return index;
		}
		/* 查询操作 */
		string get(int key) {
			int index = hashFunc(key);
			Pair *pair = buckets[index];
			if (pair == nullptr)
				return nullptr;
			return pair->val;
		}
		/* 添加操作 */
		void put(int key, string val) {
			Pair *pair = new Pair(key, val);
			int index = hashFunc(key);
			buckets[index] = pair;
		}
		/* 删除操作 */
		void remove(int key) {
			int index = hashFunc(key);
		// 释放内存并置为 nullptr
			delete buckets[index];
			buckets[index] = nullptr;
		}
		/* 获取所有键值对 */
		vector<Pair *> pairSet() {
			vector<Pair *> pairSet;
			for (Pair *pair : buckets) {
				if (pair != nullptr) {
					pairSet.push_back(pair);
				}
			}
			return pairSet;
		}

		/* 获取所有值 */
		vector<string> valueSet() {
			vector<string> valueSet;
			for (Pair *pair : buckets) {
				if (pair != nullptr) {
					valueSet.push_back(pair->val);
				}
			}
			return valueSet;
		}
		/* 打印哈希表 */
		void print() {
			for (Pair *kv : pairSet()) {
				cout << kv->key << " -> " << kv->val << endl;
			}
		}
};
```

END
<!--ID: 1710725561970-->


# 哈希冲突
## 定义
哈希函数的作用是将所有 key 构成的输入空间映射到数组所有索引构成的输出空间，而输入空间往往远大于输出空间。因此，理论上一定存在“多个输入对应相同输出”的情况

当输入的 key 后两位相同时，哈希函数的输出结果也相同。例如
```
12836 % 100 = 36
20336 % 100 = 36
```

我们将这种多个输入对应同一输出的情况称为「哈希冲突 hash collision」。

END
<!--ID: 1710725561975-->

## 解决方法

### 链式地址
「链式地址 separate chaining」将单个元素转换为链表，将键值对作为链表节点，将所有发生冲突的键值对都存储在同一链表中。
![[BAvMXOSKbrXnYZww-6d2fb0e1-7bcb-04b3-b278-3b0ba07df4fd.png|725]]

```cpp
/* 链式地址哈希表 */
class HashMapChaining {
	private:
		int size; // 键值对数量
		int capacity; // 哈希表容量
		double loadThres; 	// 触发扩容的负载因子阈值
		int extendRatio; 	// 扩容倍数
		vector<vector<Pair *>> buckets; // 桶数组
	public:
		/* 构造函数 */
		HashMapChaining() : size(0), capacity(4), loadThres(2.0 / 3), extendRatio(2) {
			buckets.resize(capacity);
		}
		/* 析构方法 */
		~HashMapChaining() {
			for (auto &bucket : buckets) {
				for (Pair *pair : bucket) {
					// 释放内存
					delete pair;
			}
		}
		/* 哈希函数 */
		int hashFunc(int key) {
			return key % capacity;
		}
		/* 负载因子 */
		double loadFactor() {
			return (double)size / (double)capacity;
		}
		/* 查询操作 */
		string get(int key) {
			int index = hashFunc(key);
			// 遍历桶，若找到 key 则返回对应 val
			for (Pair *pair : buckets[index]) {
				if (pair->key == key) {
					return pair->val;
				}
			}
			// 若未找到 key 则返回 nullptr
			return nullptr;
		}
		/* 添加操作 */
		void put(int key, string val) {
		// 当负载因子超过阈值时，执行扩容
			if (loadFactor() > loadThres) {
				extend();
			}
			int index = hashFunc(key);
			// 遍历桶，若遇到指定 key ，则更新对应 val 并返回
			for (Pair *pair : buckets[index]) {
				if (pair->key == key) {
					pair->val = val;
					return;
				}
			}
			// 若无该 key ，则将键值对添加至尾部
			buckets[index].push_back(new Pair(key, val));
			size++;
		}
		/* 删除操作 */
		void remove(int key) {
			int index = hashFunc(key);
			auto &bucket = buckets[index];
			// 遍历桶，从中删除键值对
			for (int i = 0; i < bucket.size(); i++) {
				if (bucket[i]->key == key) {
					Pair *tmp = bucket[i];
					bucket.erase(bucket.begin() + i); // 从中删除键值对
					delete tmp;
					// 释放内存
					size--;
					return;
				}
			}
		}
		/* 扩容哈希表 */
		void extend() {
			// 暂存原哈希表
			vector<vector<Pair *>> bucketsTmp = buckets;
			// 初始化扩容后的新哈希表
			capacity *= extendRatio;
			buckets.clear();
			buckets.resize(capacity);
			size = 0;
			// 将键值对从原哈希表搬运至新哈希表
			for (auto &bucket : bucketsTmp) {
				for (Pair *pair : bucket) {
					put(pair->key, pair->val);
					// 释放内存
					delete pair;
				}
			}
		}
		/* 打印哈希表 */
		void print() {
			for (auto &bucket : buckets) {
				cout << "[";
				for (Pair *pair : bucket) {
					cout << pair->key << " -> " << pair->val << ", ";
				}
				cout << "]\n";
			}
		}
}
		
```
当链表很长时，查询效率 𝑂(𝑛) 很差。此时可以将链表转换为“AVL树”或“红黑树”，从而
将查询操作的时间复杂度优化至 𝑂(log 𝑛) 。

END
<!--ID: 1710725561979-->

### 开放寻址
「开放寻址 open addressing」不引入额外的数据结构，而是通过“多次探测”来处理哈希冲突，探测方式主要包括线性探测、平方探测、多次哈希等。
#### 线性探测
##### 操作
插入元素：通过哈希函数计算数组索引，若发现桶内已有元素，则从冲突位置向后线性遍历（步长通常为 1 ），直至找到空位，将元素插入其中。
查找元素：若发现哈希冲突，则使用相同步长向后线性遍历，直到找到对应元素，返回 value 即可；如果遇到空位，说明目标键值对不在哈希表中，返回None 。
##### 缺点
不能直接删除元素。删除元素会在数组内产生一个空位，当查找该空位之后的元素时，该空位可能导致程序误判元素不存在。为此，通常需要借助一个标志位来标记已删除元素。不能直接删除元素。

容易产生聚集。数组内连续被占用位置越长，这些连续位置发生哈希冲突的可能性越大，进一步促使这一位置的聚堆生长，形成恶性循环，最终导致增删查改操作效率劣化。
END
<!--ID: 1710725561983-->

##### 代码实现
使用一个固定的键值对实例 removed 来标记已删除元素。也就是说，当一个桶内的元素为None或removed 时，说明这个桶是空的，可用于放置键值对

在线性探测时，我们从当前索引 index 向后遍历；而当越过数组尾部时，需要回到头部继续遍历。

```cpp
/* 开放寻址哈希表 */
class HashMapOpenAddressing {
	private:
		int size; 	// 键值对数量
		int capacity;// 哈希表容量
		double loadThres; // 触发扩容的负载因子阈值
		int extendRatio; // 扩容倍数
		vector<Pair *> buckets; // 桶数组
		Pair *removed; // 删除标记
	public:
		/* 构造方法 */
		HashMapOpenAddressing() {
			// 构造方法
			size = 0;
			capacity = 4;
			loadThres = 2.0 / 3.0;
			extendRatio = 2;
			buckets = vector<Pair *>(capacity, nullptr);
			removed = new Pair(-1, "-1");
		}
		/* 哈希函数 */
		int hashFunc(int key) {
			return key % capacity;
		}
		/* 负载因子 */
		double loadFactor() {
			return static_cast<double>(size) / capacity;
		}
		/* 查询操作 */
		string get(int key) {
			int index = hashFunc(key);
			// 线性探测，从 index 开始向后遍历
			for (int i = 0; i < capacity; i++) {
				// 计算桶索引，越过尾部返回头部
				int j = (index + i) % capacity;
				// 若遇到空桶，说明无此 key ，则返回 nullptr
				if (buckets[j] == nullptr)
					return nullptr;
				// 若遇到指定 key ，则返回对应 val
				if (buckets[j]->key == key && buckets[j] != removed)
					return buckets[j]->val;
				}
			return nullptr;
		}
		/* 添加操作 */
		void put(int key, string val) {
			// 当负载因子超过阈值时，执行扩容
			if (loadFactor() > loadThres)
				extend();
			int index = hashFunc(key);
			// 线性探测，从 index 开始向后遍历
			for (int i = 0; i < capacity; i++) {
				// 计算桶索引，越过尾部返回头部
				int j = (index + i) % capacity;
				// 若遇到空桶、或带有删除标记的桶，则将键值对放入该桶
				if (buckets[j] == nullptr || buckets[j] == removed) {
					buckets[j] = new Pair(key, val);
					size += 1;
					return;
				}
			// 若遇到指定 key ，则更新对应 val
				if (buckets[j]->key == key) {
					buckets[j]->val = val;
					return;
				}
			}
		}
		/* 删除操作 */
		void remove(int key) {
			int index = hashFunc(key);
			// 线性探测，从 index 开始向后遍历
			for (int i = 0; i < capacity; i++) {
				// 计算桶索引，越过尾部返回头部
				int j = (index + i) % capacity;
				// 若遇到空桶，说明无此 key ，则直接返回
				if (buckets[j] == nullptr)
					return;
				// 若遇到指定 key ，则标记删除并返回
				if (buckets[j]->key == key) {
					delete buckets[j]; // 释放内存
					buckets[j] = removed;
					size -= 1;
					return;
				}
			}
		}
		/* 扩容哈希表 */
		void extend() {
			// 暂存原哈希表
			vector<Pair *> bucketsTmp = buckets;
			// 初始化扩容后的新哈希表
			capacity *= extendRatio;
			buckets = vector<Pair *>(capacity, nullptr);
			size = 0;
			// 将键值对从原哈希表搬运至新哈希表
			for (Pair *pair : bucketsTmp) {
				if (pair != nullptr && pair != removed) {
					put(pair->key, pair->val);
				}
			}
		}
		/* 打印哈希表 */
		void print() {
			for (auto &pair : buckets) {
				if (pair != nullptr) {
					cout << pair->key << " -> " << pair->val << endl;
				} else {
					cout << "nullptr" << endl;
				}
			}
		}
};
```

END
<!--ID: 1710725561987-->
