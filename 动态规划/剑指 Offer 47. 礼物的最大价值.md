# [description](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof)
在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

 

示例 1:
```python
输入: 
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 12
解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物
```

提示：

```0 < grid.length <= 200```
```0 < grid[0].length <= 200```


# Solution
- 该题可用dp求解，状态转移方程为
$$
dp(i,j)=\left\{
\begin{array}{**lr**}  
             grid(i, j), & i=0,j=0  \\  
             grid(i, j)+dp(i, j-1) & i=0,j\neq0\\  
             grid(i, j)+dp(i-1, j), & i\neq0,j=0   \\
			 grid(i, j)+max[dp(i, j-1),dp(i-1, j)] & i\neq0,j\neq0 \\
             \end{array}  
\right. 
$$
- 由于grid数组中的值使用完后便不再被用到，所以若允许改变原数组的值，可用用grid代替新建的dp被操作，节省空间复杂度
```python
class Solution:
    def maxValue(self, grid: List[List[int]]) -> int:
        for i in range(1, len(grid)):
            grid[i][0] = grid[i-1][0] + grid[i][0]
        for i in range(1, len(grid[0])):
            grid[0][i] = grid[0][i-1] + grid[0][i]

        for i in range(1, len(grid)):
            for j in range(1, len(grid[i])):
                grid[i][j] = max(grid[i-1][j], grid[i][j-1]) + grid[i][j]

        return grid[-1][-1]
```