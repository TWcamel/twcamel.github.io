# Monotonic stack (單調棧)
- 什麼時候適合使用 Monotonic stack?
  - 通常是一維數組，要尋找任一個元素的右邊或是左邊第一個比自己大或小的元素的位置，此時我們就要想到可以用單調棧了
- Monotonic stack 需要注意的地方？
  1. 棧裡存放的元素是什麼？
  2. 棧裡元素是遞增呢？ 還是遞減呢?
  3. 主要判断條件?
    - 目前遍歷的元素T[i]小於棧頂元素T[st.top()]的情況
    - 目前遍歷的元素T[i]等於棧頂元素T[st.top()]的情況
    - 目前遍歷的元素T[i]大於棧頂元素T[st.top()]的情況

## [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/description/)
題目

<img width="547" alt="image" src="https://github.com/user-attachments/assets/f1b9f0f6-8547-4891-9769-a9a2a8259339">

- 思路: 根據上面定義,看到本題可以直接聯想到 Monotoic Stack
    1. 存放元素：裡面存放的元素為 T 內元素的下標 i
    2. 遞增 or 遞減？ 因為本題為求下標 i 元素後的第一個大於元素，因此從棧頂向棧底應該為遞增排序，才能符合題意
    3. 主要判断條件? 三種情況都要考慮，詳細可以參考下面連結
       - [卡哥詳細解說](https://programmercarl.com/0739.%E6%AF%8F%E6%97%A5%E6%B8%A9%E5%BA%A6.html#%E6%80%9D%E8%B7%AF)
- 詳細版本：
```python class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        st, res = deque(), []

        for i in range(len(temperatures)):
            res.append(0)
        st.append(0)

        for i in range(1, len(temperatures)):
            if temperatures[i] < temperatures[st[-1]]:
                st.append(i)
            elif temperatures[i] == temperatures[st[-1]]:
                st.append(i)
            elif temperatures[i] > temperatures[st[-1]]:
                while st and temperatures[i] > temperatures[st[-1]]:
                    res[st[-1]] = i - st[-1]
                    st.pop()
                st.append(i)

        return res
```
- 精簡版：
```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        st, res = deque(), []

        for i in range(len(temperatures)):
            res.append(0)
        st.append(0)

        for i in range(1, len(temperatures)):
            while st and temperatures[i] > temperatures[st[-1]]:
                res[st[-1]] = i - st[-1]
                st.pop()
            st.append(i)

        return res
```

## [496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/description/)
題目

<img width="551" alt="image" src="https://github.com/user-attachments/assets/795e48c8-9ffa-454b-9c74-742febc8538d">

- 思路: 在 [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/description/) 中是求每個元素下一個比目前元素大的元素的位置。 本題則是說 nums1 是 nums2 的子集，找 nums1 中的元素在 nums2 中下一個比目前元素大的元素。注意，題目有說到沒有重複數組，因此可以用 map 開紀錄走過的元素
```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        st, mp, res = deque(), {}, [-1] * len(nums1)

        st.append(0)
        
        for i in range(1, len(nums2)):
            if nums2[st[-1]] >= nums2[i]:
                st.append(i)
            else:
                while len(st) > 0 and nums2[st[-1]] < nums2[i]:
                    if nums2[st[-1]] in nums1:
                        idx = nums1.index(nums2[st[-1]])
                        res[idx] = nums2[i]
                    st.pop()
                st.append(i)

        return res
```

## [503. Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/description/)
題目

<img width="551" alt="image" src="https://github.com/user-attachments/assets/3df44bcc-9705-4e5a-84f8-3818e5700793">

- 思路: 題目給出的範例有提示 circularly loop, 因此可以把此題目視作 [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/description/)，將數組 double 並迭代兩次的版本
```python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        st, res = deque(), [-1] * len(nums)

        for i in range(len(nums)*2):
            while len(st) > 0 and nums[st[-1]] < nums[i % len(nums)]:
                res[st[-1]] = nums[i%len(nums)]
                st.pop()
            st.append(i%len(nums))

        return res
```
