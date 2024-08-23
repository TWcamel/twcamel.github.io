# [46. 攜帶研究資料（第六期模擬筆試）](https://kamacoder.com/problempage.php?pid=1046)
題目

- 五部曲
  1. dp[i][j] 含義： dp[i][j] 代表物品 i 在重量 j 的最大價值
  2. 遞推公式 (拿紙筆舉例畫一下)： 重量夠放多樣物品：`dp[i][j] = max(dp[i-1][j], dp[i-1][j-weight] + value[i])`, 重量不夠放物品i: `dp[i][j] = dp[i-1][j]`
  3. 初始化： 重量 = 0 時，價值為零，物品 0 不管重量多少，價值都等於 value[0] (因為題目規定每項物品只能放一次)
  4. 決定遞推順序： 由 2. 可以看出，dp 是由 dp[i-1] 而來，因此正序左到右遞推
  5. Dry run: 拿紙筆從頭到尾走一遍試看看

- 二維 DP
```python
m,n = map(int,input().split())
slst = list(map(int,input().split()))
vlst = list(map(int,input().split()))

dp = [[0] * (n+1) for _ in range(m)]

for j in range(slst[0], n + 1):
    dp[0][j] = vlst[0]

for i in range(1, m):
    for j in range(n + 1):
        if j < slst[i]:
            dp[i][j] = dp[i-1][j]
        else:
            dp[i][j] = max(dp[i-1][j], dp[i-1][j-slst[i]]+ vlst[i])

print(dp[m-1][n])
```

- 一維 DP
  - 思考點：
    1. DP 意義及公式： 將原公式 `dp[i][j] = max(dp[i-1][j], dp[i-1][j-weight] + value[i])` 中，其中的 i 拿掉，意義為重量 `j` 的最大價值為 `dp[j]`,依然可以完成此道題，注意，**所有包問題都可以降維**解題
    2. 初始化： dp[j] = 0, 當 j = 0
    3. 如何遍歷： 注意這裡是先找物品再找重量，那可以反過來嗎？(N), 那重量可以正序嗎？(N),請在白紙上面畫一遍，並舉例即可知
       - ```
         for i=0..n:
             for j=m..cost[i]..-1:
                 dp[j] = max(dp[j], dp[j-cost[i]]+ value[i])
         ```
```python
m,n = map(int,input().split())
cost = list(map(int,input().split()))
value = list(map(int,input().split()))

dp = [0 for _ in range(n+1)]

for i in range(m):
    for j in range(n, cost[i] - 1, -1):
        dp[j] = max(dp[j], dp[j-cost[i]]+ value[i])

print(dp[n])
```

# [416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/description/)
題目

- 本題可以用 backtrack 來解，但會碰到超時問題
- 轉化問題：分析以下四點符合 0-1 背包特徵，那就試試看 (注意，如果 4. 可以重複，那就變成完全背包問題)
  1. 我們將數組加總 (sum / 2)，想成背包體積
  2. 背包內要放入的商品（元素）重量為 元素的數值，價值也為元素的數值
  3. 背包如果裝滿，說明找到 sum/2 的子集合
  4. 背包中每個元素都不可以重複
- 五部曲
  1. dp[j]: 重量為 j 的背包，所背的物品價值最大為 dp[j]
  2. 0-1 背包一維公式為：`dp[j] = max(dp[j], dp[j-weight[i]] + value[i])`, 這裡，我們數組的重量為 nums[i], 價值也為 nums[i], 因此公式為: `dp[j] = max(dp[j], dp[j-nums[i]] + nums[i])`
  3. dp[0] = 0
  4. 物品遍歷順序放在外層(正序)，遍歷背包的順序放在內層(倒序)
  5. 拿紙用 [1,5,11,5] 舉例
```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        _sum = 0

        dp = [0] * 10001
        for num in nums:
            _sum += num

        if _sum % 2 == 1:
            return False
        target = _sum // 2

        for num in nums:
            for j in range(target, num - 1, -1):
                dp[j] = max(dp[j], dp[j - num] + num)
        
        if dp[target] == target:
            return True
        
        return False
```