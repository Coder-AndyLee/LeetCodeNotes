# [Description](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof)

请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

 
例如:
给定二叉树: [3,9,20,null,null,15,7],
```python
    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [20,9],
  [15,7]
]
 
```
提示：

- 节点总数 <= 1000


# Solution
```python
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        if not root:
            return []
        else:
            reverse = False
            ans, stack = [], [root]
            while stack:
                layer_node, layer_val = [], []
                while stack:
                    current = stack.pop(0)
                    if reverse:
                        layer_val.insert(0, current.val)
                    else:
                        layer_val.append(current.val)

                    if current.left:
                        layer_node.append(current.left)
                    if current.right:
                        layer_node.append(current.right)

                reverse = not reverse
                stack.extend(layer_node)
                ans.append(layer_val)
            
            return ans
```