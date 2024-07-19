# [977. Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/description/)
題目

<img width="761" alt="image" src="https://github.com/user-attachments/assets/5f6b3e37-42ca-4898-9a50-b30c123c1d00">


思路
- 題目有說數組是排序過後的，因此，代表數組的最左或最右一定平方後一定是最大值
- 因此可以用雙指針法，初始時指向數組兩側，並初始化一個 res 當作回傳變數
- 每次迭代時，比較兩側，並將其放置於 res[i] 處，重複直到遍歷 nums 完成
```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        l, r = 0, len(nums) - 1
        res = [0] * len(nums)

        for i in range(r,-1,-1):
            #print(i,l,r)
            if nums[l] * nums[l] >= nums[r] * nums[r]:
                res[i] = nums[l] * nums[l]
                l += 1
            elif nums[l] * nums[l] < nums[r] * nums[r]:
                res[i] = nums[r] * nums[r]
                r -= 1

        return res

```

# [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/description/)
題目

<img width="773" alt="image" src="https://github.com/user-attachments/assets/c84ff2a8-035f-4a85-ab79-74e1f7b1a4af">
<<<<<<< HEAD
=======


思路
- todo
```python
```
>>>>>>> 3f27433d815a7962f5550501069e43ab64bc09be


思路
- Slide window 核心思想是雙指針
- 本題關鍵在於，迴圈的操作變數是窗口的左邊還是右邊 (若左邊則跟暴力解無異)
- 判斷內圈是要 while or if 也是此題核心觀念
```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        sum, l = 0, 0
        res = sys.maxsize

        for r in range(0, len(nums)):
            sum += nums[r]
            while sum >= target:
                res = min(res, (r - l + 1))
                sum -= nums[l]
                l+=1

        return 0 if res == sys.maxsize else res
```

# [59. Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/description/)
題目

<img width="770" alt="image" src="https://github.com/user-attachments/assets/570722e2-3fae-4561-9c0d-4bea4274be25">
<<<<<<< HEAD
=======


思路
- todo
```python
```
>>>>>>> 3f27433d815a7962f5550501069e43ab64bc09be


思路
- 此題無任何演算法與資結，單純考驗對操作元素的理解程度
- 此題的核心思想是要讓每條邊的邊界設定一致(左閉右開)
```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        x,y = 0,0
        count, offset = 1, 1
        res = [[0] * n for _ in range(n)]
        loop = n // 2

        while loop > 0:
            for j in range(y, n - offset):
                res[y][j] = count
                count += 1
            for i in range(x, n - offset):
                res[i][n-offset] = count
                count += 1
            for j in range(n - offset, y, -1):
                res[n - offset][j] = count
                count += 1
            for i in range(n - offset, x, -1):
                res[i][y] = count
                count += 1 
            x += 1
            y += 1
            offset += 1
            loop -= 1

        if n % 2 > 0:
            res[x][y] = count
        
        return res

```

## Ref
- https://docs.qq.com/doc/DUGRwWXNOVEpyaVpG
