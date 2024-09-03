# [322. Coin Change](https://leetcode.com/problems/coin-change/description/)
題目

<img width="529" alt="image" src="https://github.com/user-attachments/assets/c59d5b49-89ea-4a3a-a9aa-6af23e1452ba">

- 思路
  - 與 [518. Coin Change II](https://leetcode.com/problems/coin-change-ii/description/) 類似，但本題是求最小少的組合個數
- 五部曲
  1. dp[j]: 在 amount = j 時，最少的硬幣個數為 dp[j]
  2. 遞推公式：
     - 湊足 j-coin[i] 的最少硬幣個數為 amount[j-coin[i]], 那只需加上一個 coins[i] (即，`dp[j - coins[i]] + 1`), 就是 dp[j]
     - 因此公式為：`dp[j] = min(dp[j], dp[j-coins[i]] + 1)`
  3. dp 初始化： dp[0] 一定為 0, 但避免後面被 dp[0] 覆蓋變成 0，因此其他非 0 元素全部初始化為 `float("int")`
  4. 遞推順序：因為不是求組合數量，也不是求排列數量，所以本題不管是先背包再容量，還是先容量再背包都可以！
  5. dry run
```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [float("inf")] * (amount + 1)
        dp[0] = 0

        for coin in coins:
            for j in range(amount + 1):
                if (j - coin >= 0):
                    dp[j] = min(dp[j], dp[j - coin] + 1)
        
        return dp[amount] if dp[amount] != float("inf") else -1
```

# [279. Perfect Squares](https://leetcode.com/problems/perfect-squares/description/)
題目

<img width="535" alt="image" src="https://github.com/user-attachments/assets/6ed1831c-3779-46ed-9e43-ec8b44e90cad">

- 思路
  - 把數字看成是物品，可以無限重複使用，湊正整數 n（容量），這就與與前一題 [322. Coin Change](https://leetcode.com/problems/coin-change/description/) 是一模一樣的題目了！
- 五部曲
  1. dp[j]: 和為 j 的完全平方正整數的最少數量為 dp[j]
  2. 遞推公式：
     - dp[j] 可以由 dp[j - i * i] 推出，dp[j - i * i] + 1 便可湊成 dp[j]
     - 因此，公式為： `min(dp[j], dp[j - i * i] + 1)`
  3. dp 初始化： dp[0] 一定為 0, 但避免後面被 dp[0] 覆蓋變成 0，因此其他非 0 元素全部初始化為 `float("int")`
  4. 遞推順序：因為不是求組合數量，也不是求排列數量，所以本題不管是先背包再容量，還是先容量再背包都可以！
  5. dry run
```python
class Solution:
    def numSquares(self, n: int) -> int:
        dp = [float("inf")] * (n + 1)
        dp[0] = 0

        for i in range(1, int(n ** 0.5) + 1):
            for j in range(i*i, n + 1):
                dp[j] = min(dp[j], dp[j - i * i] + 1)
        
        return dp[n]
```

# [139. Word Break](https://leetcode.com/problems/word-break/description/)
題目

<img width="534" alt="image" src="https://github.com/user-attachments/assets/afa36d73-4f8f-4bff-b062-fdb9484f9fd6">

- 思路
  - 看到分割就想到用回溯，但本主題主要是要用 dp 求解，因此兩種都寫上!

- 回溯: 會 TLE！
  #1. 遍歷所有可能的拆分位置 
  #2. 如果被截取的子字串在字典中，且後續部分也可以分割成單字，傳回True
  #3. 轉換為哈希集合，提高查找效率
```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        wordSet = set(wordDict) #3

        def backtracking(startIdx):
            nonlocal wordSet

            if startIdx >= len(s): #1
                return True
            
            for i in range(startIdx, len(s)):
                word = s[startIdx:i + 1]
                if word in wordSet and backtracking(i + 1): #2
                    return True
                
            return False
        
        return backtracking(0)
```
- dp: 單字就是物品，字串s就是背包，單字能組成字串s，就是問物品能不能把背包裝滿，拆分時可以重複使用字典中的單詞，說明此題就是一個完全背包！
  1. dp[i]: 字符串長度為 i 的話，若 dp[i] = ture, 說明可以拆分為一個或多個在字典裡面出現的單字
  2. dp[j] 為 true 的話，且子串 [j:i] 區間的出現在字典裡, 那麼 dp[i] 一定為 true, j < i，所以遞推公式 --> if 子串[j:i]出現在字典裡，且dp[j]==true
  3. dp[0] = True, 其餘 false
  4. 求組合數是先遍歷物品再遍歷背包，求排列數是先遍歷背包再遍歷物品，而本題以 "applepenapple" 為例，要求一定的順序關係，因此本題是排列問題，要先遍歷背包再遍歷物品
  5. dry run
```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        dp = [False] * (len(s) + 1)
        dp[0] = True

        for i in range(1, len(s) + 1):
            for j in range(i):
                subStr = s[j:i]
                if subStr in wordDict and dp[j]:
                    dp[i] = True
        
        return dp[len(s)]
```

# [56. 携带矿石资源（第八期模拟笔试）](https://kamacoder.com/problempage.php?pid=1066)
題目

<img width="592" alt="image" src="https://github.com/user-attachments/assets/a9cd9b1b-984b-4057-b4fe-abf01faaff29">

- 多重背包問題
  - 每件物品最多有 Mi 件可用，把 Mi 件攤開，其實就是一個 01 背包問題了
```python
C, N = map(int, input().split())
weight = list(map(int, input().split()))
value = list(map(int, input().split()))
capacity = list(map(int, input().split()))

dp = [0] * (C + 1)

# 先遍歷物品再遍歷背包，作為01背包處理
for i in range(N):
    for j in range(C, weight[i] - 1, -1):
        # 遍歷每種物品的數量
        for k in range(1, capacity[i] + 1):
            # 遍歷 k，如果已經大於背包容量直接跳出循環
            if k * weight[i] > j:
                break
            dp[j] = max(dp[j], dp[j - k * weight[i]] + k * value[i])

print(dp[C])
```

# 背包總結
