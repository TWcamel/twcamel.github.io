# [1049. Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/description/)
題目

<img width="538" alt="image" src="https://github.com/user-attachments/assets/3bed494e-98ca-428d-82ee-08c71fb5f01c">

- 思路
  1. 此題本質上是求解將石頭堆分為重量相同的兩堆, 兩兩相撞後剩下重量為最小
  2. dp[j] = 最多可以容納的重量為 dp[j]
  3. dp[j] = max(dp[j], dp[j - stone[i]] + stone[i])
  4. 題目中有說, 1 <= stones.length <= 30，1 <= stones[i] <= 1000，所以最大重量就是 30 * 1000, 將 dp 初始化大小為 15000 即可
  5. 遍歷順序為物品放外層
  6. [2,4,1,1] 在紙上做 dry run
```python
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        _sum = 0
        dp = [0] * 15001
        for stone in stones:
            _sum += stone
        
        target = _sum // 2

        for stone in stones:
            for j in range(target, stone - 1, -1):
                dp[j] = max(dp[j], dp[j-stone] + stone)
        
        return (_sum - dp[target]) - dp[target]
```

# [494. Target Sum](https://leetcode.com/problems/target-sum/description/)
題目

<img width="530" alt="image" src="https://github.com/user-attachments/assets/34a53793-2f5d-4c8f-a7ea-022a4a86b656">

- 思路
  1. 本題前，都是求容量為 j 的背包，最多能裝多少重量；但本題為求幾種裝背包方法（組合數量）的題目
  2. dp 含義: dp[j] = 填滿背包 j (包含 j)，有幾種裝法
  3. 公式: dp[j] = dp[j - num[i]], 組合題目數量都可以用這個公式
  4. 初始化: 由公式可以看出，因為所有的 dp 皆是由上個 dp 來，因此 dp[0] 一定要 = 1, 而其他元素一定要 = 0, 不然覆蓋會有問題
  5. 遍歷方式：nums 在外層迴圈，裡面遍歷元素種類 j
  6. 拿紙畫例題 `nums = [1,1,1,1,1], target = 5`
```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        _sum = 0
        for num in nums:
            _sum += num

        if abs(target) > _sum: return 0
        if (target + _sum) % 2 == 1: return 0
        bagSize = (target + _sum) // 2
        dp = [0] * (bagSize + 1)
        dp[0] = 1

        for i in range(len(nums)):
            for j in range(bagSize, nums[i] - 1, -1):
                dp[j] += dp[j-nums[i]]

        return dp[bagSize]
```

# [474. Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/description/)
題目

<img width="531" alt="image" src="https://github.com/user-attachments/assets/0d571345-68a6-4a13-a5c0-9b098cace97a">

- 思路
  1. 看到題目首先問自己，這是什麼類型的背包問題：如果把 m, n 看成不同東西，那會變成多多重背包，但 m, n 本質上就是同一個字串延伸出來的東西，因此可以視為同一個物品，因此是 0-1 背包！
  2. **本題中的 strs 中的元素就是物品，每個物品都是一個**
  3. **m, n 可視為一個兩維度的背包**
- 五部曲
  1. dp 意義： dp[i][j]: 最多有 i 個 0 和 j 個 1 的 strs 的最大子數組的大小為 dp[i][j]
  2. 公式： dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1) (對比之前一維 0-1 背包問題就會發現，這裡的zeroNum和oneNum相當於物品的重量 weight[i], 最後的 1 相當於本身字符串的價值 value[i])
  3. dp = [0] * [m * n]
  4. 遍歷順序：本題的物品為 strs, 須在外層進行遍歷，背包容量 m, n 在內層遍歷
  5. dry run
```python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        for s in strs:
            zeroNum = s.count('0')
            oneNum = len(s) - zeroNum
            for i in range(m, zeroNum - 1, -1):
                for j in range(n, oneNum - 1, -1):
                    dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1)
        return dp[m][n]
```

# 總結 0-1 背包的應用
  1. 純 0-1 背包: 求 給定容背包容量，看裝滿背包的最大價值為多少 [46. 攜帶研究資料（第六期模擬筆試）](https://kamacoder.com/problempage.php?pid=1046)
  2. 分割等和子集: 求 給定背包容量，看能不能裝滿這個背包 [416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/description/)
  3. 求 給定背包容量，盡可能裝，看能裝多少 [1049. Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/description/)
  4. 求 給定背包容量，裝滿背包有幾種裝法 [494. Target Sum](https://leetcode.com/problems/target-sum/description/)
  5. 求 給定背包容量，裝滿背包有多少個物品 [474. Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/description/)
