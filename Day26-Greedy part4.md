# [452. Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/)
題目

<img width="530" alt="image" src="https://github.com/user-attachments/assets/6cde2a50-b8cd-4247-ac69-3799c55e2515">

- 本題考的是重疊區間，區域解是遇到重疊區間就更新範圍，沒有重疊則將 `res += 1`
```python
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        if len(points) == 0: return 0
        points, res = sorted(points, key=lambda x : x[1]), 1

        for i in range(1, len(points)):
            l, r = points[i]
            prevL, prevR = points[i - 1]
            if l > prevR:
                res += 1
            else:
                r = min(prevR, r)
                points[i] = l, r
        
        return res
```
- 錯誤寫法: 
  - #1. 遇到重複區間直接將索引加上2. 這樣可以避免重複處理到重複區間
  - #2. 這裡是出錯的地方，因為預設是若數組飛零則必有至少一隻弓箭，但這邊在第一次處理會重複加一次
```python
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        if len(points) == 0: return 0
        points, res = sorted(points, key=lambda x : x[1]), 1
        print(points)
        i = 0
        while i < len(points) - 1:
            l, r = points[i]
            nextL, nextR = points[i + 1]
            print(f"nextL: {nextL}, nextR: {nextR}, l: {l}, r: {r}")
            if r >= nextL:
                i += 2 #1.
            else:
                i += 1 #2.
            res += 1

        return res
```

- [435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/description/)
題目

<img width="536" alt="image" src="https://github.com/user-attachments/assets/e44cc6fb-b6af-4306-9c46-95ad8e601807">

- 用 [452. Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/) 稍微修改，讓所有元素扣除不重疊區間就是答案
- 用右邊排序: 計算不重疊區間數
```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        if len(intervals) == 0: return 0
        intervals, cnt = sorted(intervals, key=lambda x: x[1]), 1

        for i in range(1, len(intervals)):
            l, r = intervals[i]
            lastL, lastR = intervals[i - 1]
            if lastR <= l: # 不重疊區間
                cnt += 1
            else:
                intervals[i] = l, min(lastR, r)

        return len(intervals) - cnt
```
- 用左邊排序: 計算重疊區間數
```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        if len(intervals) == 0: return 0
        intervals, cnt = sorted(intervals, key=lambda x: x[0]), 0

        for i in range(1, len(intervals)):
            l, r = intervals[i]
            lastL, lastR = intervals[i - 1]
            if l < lastR: #重疊區間
                intervals[i] = l, min(lastR, r)
                cnt += 1

        return cnt
```

# [763. Partition Labels](https://leetcode.com/problems/partition-labels/description/)
題目

<img width="544" alt="image" src="https://github.com/user-attachments/assets/3df3bce0-534f-43cf-a4be-4cc59e8f089e">

- 想到分割就想到回溯，但其實這題不需要使用暴力解，而可以紀錄每個字母的最後一個出現位置，當目前字母與最後字母出現位置相同，代表這是一個分割點
- #1.紀錄每個字母的最後一次出現位置
- #2.當前字母下標與最後一次出位置相等，代表找到了分割點
```python
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        lastOccurence = {}
        for i, c in enumerate(s): #1
            lastOccurence[c] = i
        
        res = []
        start, end = 0, 0
        for i, c in enumerate(s):
            end = max(end, lastOccurence[c])
            if i == end: #2
                res.append(end - start + 1)
                start = i + 1
        
        return res
                

```
