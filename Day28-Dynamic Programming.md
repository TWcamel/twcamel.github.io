# [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/description/)
題目

<img width="520" alt="image" src="https://github.com/user-attachments/assets/7e0d3cd4-1f1b-4fe0-a18b-dbd9ca48b493">

- 本題對理解 DP 很幫助，在進行 DP 題目時，可以參考以下五部曲
  1. 確定下標 i 在 dp 中的含義
  2. dp 的公式是什麼
  3. 公式如何初始化
  4. 確認迭代/遞回的順序
  5. 舉例 dry run 看看
```python
class Solution:
    def fib(self, n: int) -> int:
        if n < 2:
            return n

        dp = [0] * (n + 1)
        dp[0], dp[1] = 0, 1

        for i in range(2, n + 1):
            dp[i] = dp[i-1] + dp[i-2]
        
        return dp[n]
```

# [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/description/)
題目

<img width="524" alt="image" src="https://github.com/user-attachments/assets/bf394bd8-ef47-4e92-8047-6c5c25d1cc83">

- 如何判斷此題可以用 DP?
  - 先看 n = 1, n = 2 的情況，分別有 1, 2 兩種步數可以到終點，如果多看幾個例子，會發現規律是 f(n) = f(n-1) + f(n-2)
  - 目前狀態可以用先前狀態做推導，那就用 DP
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n == 1:
            return 1
        if n == 2:
            return 2
        
        dp = [0] * (n + 1)
        dp[1], dp[2] = 1, 2

        for i in range(3, n + 1):
            dp[i] = dp[i-1] + dp[i-2]
        
        return dp[n]
```

# [746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/description/)
題目

<img width="530" alt="image" src="https://github.com/user-attachments/assets/913fd9be-17bb-4f80-91cc-60ede3d3bc60">

- 這邊注意題目敘述，到達最後一階之後還要再多走一步才會到頂端，因此 `size(dp) = len(cost) + 1`
```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        dp = [0] * (len(cost) + 1)

        for i in range(2, len(cost) + 1):
            dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])
        
        return dp[len(cost)]
```
