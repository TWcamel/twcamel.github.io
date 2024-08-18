# [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)
題目

<img width="531" alt="image" src="https://github.com/user-attachments/assets/f7bb3a64-927b-469d-bedb-6ab9f2ed2edc">

- 從全局的觀點出發:
  - #1. 若 cost > gas, 說明油量不夠繞完一圈
  - #2. gas[i] - cost[i], 為一天剩下的油量，若累加後都無小於零的情況出現，說明可以從原點出發
  - #3. 累加最小值是負數，則從後往前看，找到能把負數填平的加油站就是出發點
```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        minNum, rest, sm = float("inf"), [], 0
        for i in range(len(gas)):
            rest.append(gas[i] - cost[i])
            sm += rest[i]
            if sm < minNum:
                minNum = sm
        
        if sm < 0: return -1 #1
        if minNum >= 0: return 0 #2

        for i in range(len(gas) - 1, 0, -1): #3
            minNum += rest[i]
            if minNum >= 0:
                return i

        return -1
```

# [135. Candy](https://leetcode.com/problems/candy/description/)
題目

<img width="536" alt="image" src="https://github.com/user-attachments/assets/43abc967-4478-4f36-867e-78ebabde0f4b">

- 這題列為困難的原因在於，要從局部最優推敲出全局最優
  - #1. 先鎖定一邊考慮左孩子大於右孩子的情況
  - #2. 接著再考慮右邊孩子大於左邊孩子的情況
  - #3. 因為全局組優解需讓每個相鄰分數最高的孩子拿到最多的糖果，因此 #2 結果需要比較 #1 的結果，由局部最優推出全局最優
```python
class Solution:
    def candy(self, ratings: List[int]) -> int:
        res, arr = 0, [1] * len(ratings)

        for i in range(1, len(ratings)): #1
            if ratings[i] > ratings[i - 1]:
                arr[i] = arr[i - 1] + 1
        
        for i in range(len(ratings) - 2, -1, -1): #2
            if ratings[i] > ratings[i + 1]:
                arr[i] = max(arr[i + 1] + 1, arr[i]) #3
            
        return sum(arr)
```

# [860. Lemonade Change](https://leetcode.com/problems/lemonade-change/description/)
題目

<img width="540" alt="image" src="https://github.com/user-attachments/assets/5683a084-72a7-4fd9-9ad1-fb075faf8942">

- 此題可以拆開三種情況來看
  - #1. 收到五刀，當作後面可以找零的來源 (Count + 1)
  - #2. 收到 10 刀，檢查五刀夠不夠找
  - #3. 收到 20 刀，先檢查夠不夠 1 個五刀與 1 個 10 刀，若不夠就扣掉三個五刀，否則不夠找
```python
class Solution:
    def lemonadeChange(self, bills: List[int]) -> bool:
        coinMap = Counter()

        arr = [5,10,20]
        for ele in arr:
            coinMap[ele] = 0

        for i in range(len(bills)):
            if bills[i] == 10: #2
                if coinMap[5] <= 0:
                    return False
                coinMap[5] -= 1
            elif bills[i] == 20: #3
                if coinMap[5] > 0 and coinMap[10] > 0:
                    coinMap[5] -= 1
                    coinMap[10] -= 1
                elif coinMap[5] >= 3:
                    coinMap[5] -= 3
                else:
                    return False
            coinMap[bills[i]] += 1 #1
        return True
```

- [406. Queue Reconstruction by Height](https://leetcode.com/problems/queue-reconstruction-by-height/description/)
題目

<img width="536" alt="image" src="https://github.com/user-attachments/assets/d2e1d5fb-9e44-4ef9-8bc4-98ddece6d4d9">

- 此題與 [1005. Maximize Sum Of Array After K Negations](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/description/), [135. Candy](https://leetcode.com/problems/candy/description/) 非常類似
- 此題也是先找出區域解，接著再用區域解找到全域最佳解
  - #1. 此題目有兩個維度，我們需要先鎖定其中一個維度，然後再想辦法解決另一個維度，***否則就會顧己失彼*** 
  - #2. 先鎖定第一個維度 (身高), 並按照身高大到小排序 (區域)
  - #3. 接著鎖定位置進行插入，因為插入位置已經按照身高大小排序過了，因此可以直接插入而不會影響最終答案 (全域)
```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people = sorted(people, key=lambda x: (-x[0], x[1])) #2
        res = deque()
        for i in range(len(people)):
            position = people[i][1]
            res.insert(position, people[i]) #3
        return res
```
