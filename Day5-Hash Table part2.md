# [454. 4Sum II](https://leetcode.com/problems/4sum-ii/description/)
題目

思路
- 此題為 [1. Two Sum](https://leetcode.com/problems/two-sum/description/) 的延伸
- 這題題目最基本的解法是將四個數組內的每個數字排列出來進行加總
- 因為最後要得其實是組合的加總結果，且組合的加總結果有可能重複
- 因此可以進行 Hash Table 進行優化，結果如下
```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        nums, cnt = defaultdict(int), 0

        for i in nums1:
            for j in nums2:
                nums[i + j] += 1

        for i in nums3:
            for j in nums4:
                cnt += nums.get(-(i + j), 0)

        return cnt
```

# [383. Ransom Note](https://leetcode.com/problems/ransom-note/description/)
題目

思路
- 此題目需要判斷 ransomNote 內的元素數量，是否被 magazine 所包含住
- 有看到數量第一優先就是使用 Hash Table
```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        cnt = Counter()

        for i in ransomNote:
            cnt[i] += 1
        
        for i in magazine:
            cnt[i] -= 1

        for k, v in cnt.items():
            if v > 0:
                return False
        
        return True

```

# [15. 3Sum](https://leetcode.com/problems/3sum/description/)
題目

思路
- 一開始做題時想不到解法，所以有看了提示
- 此題目最難解的地方在於，題目說不可以包含重複的三元組
- 有了不重複關鍵字，一開始想到的是 Hash Table，Hash Table 要處裡的邊界條件較多
- 因此使用雙指針來完成這道題
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        res, sz = [], len(nums)

        for i in range(sz):
            if nums[i] > 0:
                return res
            
            if i > 0 and nums[i-1] == nums[i]:
                continue
            
            l = i + 1
            r = sz - 1
        
            while l < r:
                sm = nums[i] + nums[l] + nums[r]
                if sm < 0:
                    l += 1
                elif sm > 0:
                    r -= 1
                else:
                    res.append([nums[i], nums[l], nums[r]])

                    while l < r and nums[l] == nums[l+1]:
                        l += 1
                    while l < r and nums[r] == nums[r-1]:
                        r -= 1

                    l += 1
                    r -= 1

        return res
```

# [18. 4Sum](https://leetcode.com/problems/4sum/description/)
題目

- 此題目為 [15. 3Sum](https://leetcode.com/problems/3sum/description/) 的延伸題
- 想法是固定兩個數字，讓剩下的給雙指針去處理
- 要注意的是這裡題目要求 target 可以有負數，若多做一、二級剪枝可以讓整體算法再快一些
```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        sz, res = len(nums), []
        if sz < 4:
            return res

        nums.sort()
        
        firstNum = nums[0]

        for i in range(0, sz):

            # 這條剪枝沒有考慮到的 一級剪枝
            # if nums[i] > target and nums[i] > 0 and target > 0: 
            #     break

            if i > 0 and nums[i - 1] == nums[i]:
                continue

            for j in range(i+1,sz):

                # 這條剪枝沒有考慮到的 二級剪枝
                # if nums[i] + nums[j] > target and target > 0:
                    # break
                
                if j > i + 1 and nums[j - 1] == nums[j]:
                    continue

                l, r = j+1, sz -1

                while l < r:
                    sm = nums[i] + nums[j] + nums[l] + nums[r]

                    if sm > target:
                        r -= 1
                    elif sm < target:
                        l += 1
                    else:
                        res.append([nums[i], nums[j], nums[l], nums[r]])

                        while l < r and nums[l] == nums[l + 1]:
                            l += 1
                        
                        while l < r and nums[r] == nums[r - 1]:
                            r -= 1
                        
                        l += 1
                        r -= 1

        return res
```