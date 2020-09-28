# [Description](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof)
输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

例如:
```python
给定的树 A:

     3
    / \
   4   5
  / \
 1   2
给定的树 B：

   4 
  /
 1
 ```
返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。

示例 1：
```python
输入：A = [1,2,3], B = [3,1]
输出：false
```
示例 2：
```python
输入：A = [3,4,5,1,2], B = [4,1]
输出：true
```
限制：

```0 <= 节点个数 <= 10000```

# Solution
### 1. 解法一（行数看着很多）
- 本题的关键在于A和B为空时的判断，并且它们在两个函数中的情况是不一样的。例如，在```isSubStructure()```中，A和B只要有一个为NULL，结果都应为False（和判断B是否是A的子树那题类似）；而在```isSub()```中，若B为NULL，则已遍历完了B，结果为True；反之，若A为NULL但B不为NULL，则结果为False。
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isSub(self, A, B):
        if not B:
            return True
        elif not A:
            return False
        else:
            if A.val != B.val:
                return False
            else:
                return self.isSub(A.left, B.left) and self.isSub(A.right, B.right)
                
    def isSubStructure(self, A: TreeNode, B: TreeNode) -> bool:
        if not A and not B:
            return True
        elif not A or not B:
            return False
        elif A.val == B.val:
            if self.isSub(A, B):
                return True
            else:
                return self.isSubStructure(A.left, B) or self.isSubStructure(A.right, B)
        else:
            return self.isSubStructure(A.left, B) or self.isSubStructure(A.right, B)        
```

### 2. 解法二（优雅的代码，from题解）
- 该题解把上述繁琐的代码的多个情况做了合并。若进行重构的话，```isSub()```是很容易优化到题解的程度的，但```isSubStructure()```需要优化到如此简洁还需要继续努力。
```python
class Solution:
    def isSubStructure(self, A: TreeNode, B: TreeNode) -> bool:
        def recur(A, B):
            if not B: return True
            if not A or A.val != B.val: return False
            return recur(A.left, B.left) and recur(A.right, B.right)

        return bool(A and B) and (recur(A, B) or self.isSubStructure(A.left, B) or self.isSubStructure(A.right, B))
```