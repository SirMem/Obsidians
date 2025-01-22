---
Project: "[[算法训练营]]"
Status: 🟨
tags:
  - Resources
Deadline: 2025-01-17
CreateTime: 2025-01-17
Connected:
---

#review

## 环境设置
电脑设置

- Google
  - Mac: iTerm2 + zsh (oh my zsh)
    Windows: Microsoft new terminal: 

- VSCode; Java: IntelliJ; Python: Pycharm
  - LeetCode plugin (VSCode & IntelliJ)
  - https://vscodethemes.com/

## Code Style
if后面一定要有空格
左括号前面一定要写空格
loop后面一定要有空格
所有的括号和关键字前后都要有空格
运算符前后都有空格


## LeetCode 
- leetcode.com 和 Discuss board
每一道题目写完后，到第三第四遍时，看leetcode国际票数Most votes前三最高的题解

## 指法和小操作
- home end (行头，行尾)
- delete删除光标右边内容
- Control/Option + left/right 单词的左右切分
- shift进行选中
- IDE 的自动补全
- Top tips for `<IDE-NAME> `

## 自顶向下的编程方式

```java
class Solution {
	public boolean isRailudrome(String s) {
    // 高层次（主干）逻辑。
    // 1. filter out number & char; 2. reverse and compare

	    String filteredS = _filterNonNumberAndChar(s);
	    return _reverseString(filteredS).equalsIgnoreCase(filteredS);
}

	private String _reverseString(String s) {
	    return new StringBuilder(s).reverse().toString();
	}

	private String _filterNonNumberAndChar(String s) {
	    return s.replaceAll(regex: "[^A-Za-z0-9]", replacement: "");
	}
}
```


升维，空间换时间