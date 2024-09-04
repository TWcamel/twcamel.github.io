# [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)
題目

- 暴力解: 最直接的想法
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        profit = 0
        for i in range(len(prices)):
            for j in range(i+1, len(prices)):
                profit = max(profit, prices[j] - prices[i])
        return profit
```
- 貪心: 如果找不到反例，那就試試貪心
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        lo = float("inf")
        profit = 0

        for i in range(len(prices)):
            lo = min(lo, prices[i])
            profit = max(prices[i] - lo, profit)
        return profit
```
- dp: 由前面例子可以得知，要知道第 i 天的最大利潤，可以由前幾天的結果推算得知, 因此可以用 dp
  1. dp[i][0]: 第 i 天手上持有**最多現金數**; dp[i][1]: 第 i 天手上**不**持有**最多現金數**
  2. 如下：
      - 第 i - 1 天就 **持有** 股票，那麼手上的現金就是保持 i-1 天買入股票後的水位: dp[i][0] = max(dp[i-1][0], -price[i])
      - 第 i 天手上的現金，就是按照今日價格賣出後減去之前持有股票的價格: dp[i][1] = max(price[i] - dp[i-1][0], dp[i-1][1])
  3. 由上面公式可以得知，dp狀態需要由 dp[i][0] 得知，因此 dp[0][0] -= price[0], dp[0][1] = 0
  4. 由第一天開始遍歷至最後一天
  5. dry run
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [[0, 0] for _ in range((len(prices)))]
        dp[0][0] -= prices[0]
        
        for i in range(1, len(prices)):
            dp[i][0] = max(dp[i-1][0], -prices[i])
            dp[i][1] = max(prices[i] + dp[i-1][0], dp[i-1][1])

        return dp[len(prices) - 1][1]
```

# [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)
題目

- 貪心: 如果找不到反例，那就試試貪心
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        result = 0
        for i in range(1, len(prices)):
            result += max(prices[i] - prices[i - 1], 0)
        return result
```
- dp: 
  - 題目部分與 [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/) 一樣
  - 差別在於:
    - 在第 i 天不持有股票的最大現金 dp[i][0]，因為題目定義是一擋股票可以買賣多次，因此第 i 天買入股票時，手上的現金包含之前的利潤 (dp[i][0] = max(dp[i-1][1] - prices[i]), dp[i-1][0])
    - 第 i 天賣出股票，所得現金就是今天股價賣出後所得 (dp[i][1] = max(prices[i] + dp[i-1][0] , dp[i-1][1]))
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [[0, 0] for _ in range(len(prices))]
        dp[0][0] -= prices[0]

        for i in range(1, len(prices)):
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] - prices[i])
            dp[i][1] = max(dp[i-1][1], prices[i] + dp[i-1][0])
        
        return dp[len(prices) - 1][1]
```

# [123. Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/description/)
題目

- dp
  1. dp[i][j]: 第 i 天時, 可以選擇的操作項目為 j (j=0,1,2,3,4 表 不操作,第一次買股票,第一次賣股票,第二次買股票,第二次賣股票), dp[i][j] 表示第 i 天在狀態 j 下的最大剩餘現金
  2. 這裡很多細節，我們逐一分析
    1. dp[i][1] = max(-prices[i] + dp[i-1][0], dp[i-1][1])
      - 第 i 天就買入股票了，那麼 dp[i][1] = -prices[i] + dp[i-1][0]
      - 第 i 沒操作，那麼就沿用之前狀態 dp[i][1] = dp[i-1][1]
    2. dp[i][2] = max(prices[i] + dp[i-1][1], dp[i][2])
      - 第 i 天賣出股票，手上剩餘現金為 dp[i][2] = prices[i] + dp[i-1][1]
      - 第 i 天不操作，那麼 dp[i][2] = dp[i-1][2]
    3. dp[i][3] = max(- prices[i] + dp[i-1][2], dp[i-1][3])
      - 第 i 天買入第二次股票，代表之前已經買過一次股票了，因此手上剩餘現金為 dp[i][3] = -prices[i] + dp[i-1][2]
      - 第 i 天不操作，那麼 dp[i][3] = dp[i-1][3]
    4. 同理 dp[i][4] = max(prices[i] + dp[i-1][3], dp[i-1][4])
  3. 初始化：
    - 第 0 天沒有操作： dp[0][0] = 0
    - 第 0 天買入第一次股票: dp[0][1] -= prices[0]
    - 第 0 天賣出第一次股票 (不可能): dp[0][2] = 0
    - 第 0 天買入第二次股票 (等於第 0 天買入第一次又賣出，才買了第二次股票): dp[0][3] -= prices[0]
    - 第 0 天賣出第二次股票 (不可能): dp[0][4] = 0
  4. 前向後遍歷
  5. dry run
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = [[0, 0, 0, 0, 0] for _ in range(len(prices))]
        dp[0][1] -= prices[0]
        dp[0][3] -= prices[0]

        for i in range(1, len(prices)):
            dp[i][1] = max(dp[i-1][1], -prices[i] + dp[i-1][0])
            dp[i][2] = max(dp[i-1][2], prices[i] + dp[i-1][1])
            dp[i][3] = max(dp[i-1][3], -prices[i] + dp[i-1][2])
            dp[i][4] = max(dp[i-1][4], prices[i] + dp[i-1][3])
        
        return dp[len(prices) - 1][4]
```