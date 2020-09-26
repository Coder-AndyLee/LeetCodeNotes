# [Description](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof)
给定一棵二叉搜索树，请找出其中第k大的节点。

 

示例 1:
```python
输入: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
输出: 4
```
示例 2:
```python
输入: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
输出: 4
```

限制：

1 ≤ k ≤ 二叉搜索树元素个数

# Solution
## 1. 笨办法
- 获得中序遍历的节点取值数组，返回index=-k的元素
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def __init__(self):
        self.vals = []

    def LDR(self, root):
        if not root:
            return
        else:
            self.LDR(root.left)
            self.vals.append(root.val)
            self.LDR(root.right)

    def kthLargest(self, root: TreeNode, k: int) -> int:
        self.LDR(root)
        return self.vals[-k]
```

## 2. 对时间复杂度稍作优化
- 提前退出遍历
```python 
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def __init__(self):
        self.cnt = 0

    def LDR(self, root, k):
        if not root or self.cnt == k:
            return
        else:
            self.LDR(root.right, k)
            self.cnt += 1
            if self.cnt == k:
                self.res = root.val                

            self.LDR(root.left, k)

    def kthLargest(self, root: TreeNode, k: int) -> int:
        self.LDR(root, k)
        return self.res
```