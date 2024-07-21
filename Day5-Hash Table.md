# [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/description/)
題目

思路
- 此題關鍵為英文字母小寫 26 個, 預計算兩個字串是否每個 char 出現的頻率皆相同
  1. 法一: array 
    ```python
        class Solution:
            def isAnagram(self, s: str, t: str) -> bool:
                arr = [0] * 26

                for i in s:
                    arr[ord(i) - ord("a")] += 1

                for i in t:
                    arr[ord(i) - ord("a")] -= 1

                for i in arr:
                    if i != 0:
                        return False

                return True
    ```
  2. 法二: hash
    ```python
        class Solution:
            def isAnagram(self, s: str, t: str) -> bool:
                import collections.Counter

                c1 = Counter(s)
                c2 = Counter(t)

                for k, v in c1.items():
                    if k not in c2:
                        return False
                    elif c2[k] != v:
                        return False

                for k, v in c2.items():
                    if k not in c1:
                        return False
                    elif c1[k] != v:
                        return False

                return True
    ```


# [349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/description/)
題目

思路
- 透過 "不得重複"，第一時間可以想到是 Hash table
- 此外，需要交叉查找兩組 Array 才能夠算得相交數字為何，因此最後的複雜度為 O(N+M)
```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:

        cnt, res = Counter(), []

        for w in nums1:
            cnt[w] += 1
        
        for w in nums2:
            if (w in cnt) and (w not in res):
                res.append(w)
    
        return res
```

# [202. Happy Number](https://leetcode.com/problems/happy-number/description/)
題目

思路
- 此題關鍵在於，"若數字重複，則永遠不會到達 1"，而想到重複，第一個就會想到 Hash Table
```python
class Solution:
    def isHappy(self, n: int) -> bool:
        cnt = Counter()

        while n != 1:
            if n in cnt:
                return False
            cnt[n] += 1
            lst = list(str(n))
            sm = 0
            for i in lst:
                sm += int(i)*int(i)
            n = sm

        return True
```

# [1. Two Sum](https://leetcode.com/problems/two-sum/description/)
題目

- Leetcode 的第一個題目，這題是暴力解的改良版，即，讓暴力解有了記憶性，並使用 Hash Map 加速查找效率
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        dic = dict()

        for i, num in enumerate(nums):
            tmp = target - num
            if tmp in dic:
                return [dic[tmp], i]
            dic[num] = i

        return []
```