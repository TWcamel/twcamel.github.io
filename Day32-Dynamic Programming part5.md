# [52. 携带研究材料（第七期模拟笔试）](https://kamacoder.com/problempage.php?pid=1052)
題目

- 完全背包 v.s. 0-1 背包問題
  - 完全背包定義： 每項物品有無限多個，求背包容量的最大價值為多少?
  - 跟 0-1 背包的差異： **差異在於遍歷順序**，因此也可以使用五部曲來分析此類型的題目
  - 遍歷順序：一定要先遍歷容量再物品嗎？ 不一定! **dp[j] 是根據 dp[j-X] 計算得出，只要保證 dp[j] 前經過計算即可！**
## 先遍歷背包容量再遍歷物品
```python
n, v = map(int, input().split())
items = [list(map(int, input().split())) for _ in range(n)]

dp = [0] * (v + 1)

for j in range(v + 1):
    for i in range(len(items)):
        wi, vi = items[i]
        if j - wi >= 0:
            dp[j] = max(dp[j], dp[j-wi] + vi)

print(dp[v])
```
## 先遍歷物品再遍歷背包
```python
n, v = map(int, input().split())
items = [list(map(int, input().split())) for _ in range(n)]

dp = [0] * (v + 1)

for i in range(len(items)):
    wi, vi = items[i]
    for j in range(wi, v + 1):
        dp[j] = max(dp[j], dp[j-wi] + vi)

print(dp[v])
```

# [518. Coin Change II](https://leetcode.com/problems/coin-change-ii/description/)
題目

- 思路
  - 本題求給定的硬幣種類及總量，求最多組合數量的硬幣種類
  - 這題看到組合，應該可以直接連想到回溯，但會超時！因此用 DP 來解此題
  - 那麼為什麼本題不是 0-1 背包呢？ 因為本題求解的是 **硬幣湊齊的組合個數，而不是最大價值（排列）**
- 五部曲
  1. dp[j]: 湊齊金額為 j 的硬幣組合數量為 dp[j]
  2. 遞推公式：d[j] += dp[j - coins[i]] (dp[j] 為考慮所有的 dp[j - coins[i]] 相加)
  3. dp 初始化: dp[0] = 1
  4. 遍歷順序: 先遍歷物品再遍歷容量 (先遍歷物品再遍歷容量，這樣是求組合數；但先遍歷容量，在遍歷物品就是排列了！)
  5. dry run (amount = 5, coints[1, 2, 5])
```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        dp = [0] * (amount + 1)
        dp[0] = 1
        
        for coin in coins:
            for j in range(coin, amount + 1):
                dp[j] += dp[j - coin]
        
        return dp[amount]
```

# [377. Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/description/)
題目

與 [518. Coin Change II](https://leetcode.com/problems/coin-change-ii/description/) 不同的點，本題是求排列數量，因此要先遍歷容量再遍歷物品
```python
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        dp = [0] * (target + 1)
        dp[0] = 1
        
        for j in range(target + 1):
            for num in nums:
                if (j - num) >= 0: 
                    dp[j] += dp[j - num]
        
        return dp[target]
```

# [57. 爬楼梯（第八期模拟笔试）](https://kamacoder.com/problempage.php?pid=1067)
題目

```python
n, m = map(int, input().split())

dp = [0] * (n + 1)
dp[0] = 1

for i in range(1, n + 1):
    for j in range(1, m + 1):
        if (i - j) >= 0:
            dp[i] += dp[i - j]
    
print(dp[n])
```