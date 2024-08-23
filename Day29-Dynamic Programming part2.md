# [62. Unique Paths](https://leetcode.com/problems/unique-paths/description/)
題目

<img width="531" alt="image" src="https://github.com/user-attachments/assets/dcea1807-a039-463f-9c6a-1f5b488d54ac">

- 五部曲
  1. dp[i][j] 代表從 (0,0) 出發，到 (i,j) 有 dp[i][j] 種可能的路徑
  2. dp[i][j] = dp[i][j-1] + dp[i-1][j]
  3. 由於第一排跟第一列只有一種路徑，因此 dp[i][0] = 1, dp[0][j] = 1
  4. 左 -> 右, 上 -> 下
  5. 畫圖檢查
```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[0] * n for _ in range(m)]  

        for i in range(m):
            dp[i][0] = 1
        for j in range(n):
            dp[0][j] = 1

        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = dp[i -1][j] + dp[i][j-1]
        
        return dp[m-1][n-1]
```

# [63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/descriptiono/)
題目

<img width="530" alt="image" src="https://github.com/user-attachments/assets/f8971128-c8c3-4ee9-8cc0-de235512805f">

- 類似題: [62. Unique Paths](https://leetcode.com/problems/unique-paths/description/)
```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        n, m = len(obstacleGrid[0]), len(obstacleGrid)
        dp = [[0] * n for _ in range(m)]

        for i in range(m):
            if obstacleGrid[i][0] != 0:
                break
            dp[i][0] = 1

        for j in range(n):
            if obstacleGrid[0][j] != 0:
                break
            dp[0][j] = 1
        
        for i in range(1, m):
            for j in range(1, n):
                if obstacleGrid[i][j] == 1:
                    continue
                dp[i][j] = dp[i][j-1] + dp[i-1][j]

        return dp[m - 1][n - 1]
```

# [343. Integer Break](https://leetcode.com/problems/integer-break/description/)
題目

<img width="534" alt="image" src="https://github.com/user-attachments/assets/4b7c7fc4-387c-4c60-8024-6adfeb3163b1">

- 留到二刷寫
```python
class Solution:
    def integerBreak(self, n: int) -> int:
        
```

# [96. Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/description/)
題目

<img width="532" alt="image" src="https://github.com/user-attachments/assets/1436d01e-de16-44e6-8338-d282a04d88e6">

- 留到二刷寫
```python
class Solution:
    def numTrees(self, n: int) -> int:
        
```
- 留到二刷寫
```python
```
