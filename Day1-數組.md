# [704. Binary Search](https://leetcode.com/problems/binary-search/description/)
題目

<img width="816" alt="image" src="https://github.com/user-attachments/assets/17415d4c-df63-48b1-a0d1-6493b09ac1fd">

思路
- 使用迴圈法，首先需要確認終止條件，迴圈裡面則要確認每次迭代時指針該怎麼作動
- python 中，處理整數問題建議都向下取整 (`floor` or `//`)
```
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        if len(nums) == 1 and nums[0] == target:
            return 0

        l = 0
        r = len(nums) - 1
        while l < r:
            mid = (r + l) // 2
            if nums[mid] == target:
                return mid
            elif nums[l] == target:
                return l
            elif nums[r] == target:
                return r
            
            if target < nums[mid]:
                r = mid - 1 
            elif target > nums[mid]:
                l = mid + 1
            
        return -1
  ```

# [27. Remove Element](https://leetcode.com/problems/remove-element/description/)
題目

<img width="806" alt="image" src="https://github.com/user-attachments/assets/041e388a-0d71-443f-a3be-d3edab517988">
<img width="811" alt="image" src="https://github.com/user-attachments/assets/e9d1bb11-52c8-4915-9fb9-f88b18b0ac6f">

思路
- 雙指針題目，一個指針指向操作元素，另一個指向被替換元素，並進行反面表列
- 終止條件為遍歷所有需操作元素
```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:

        slow = 0

        for fast in range(len(nums)):
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow+=1

        return slow
```

思考 & 總結
- 一開始有想到這題是雙指針類型的題目，但在決定指針擔任角色時還是有小卡關
- 卡關的地方在於，對於指針角色定義不明確，自評是要在多練習類似題目

## Ref
- https://docs.qq.com/doc/DUG9UR2ZUc3BjRUdY
