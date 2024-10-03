# [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/description/)
題目

<img width="552" alt="image" src="https://github.com/user-attachments/assets/f3bcd4d7-f5c4-4975-bfb7-38a98afea39c">

- 思路: 本題面試高頻題，下面用三個版本來解題
1. 暴力解法 (雙指針)：會 TLE
```python
class Solution:
    def trap(self, height: List[int]) -> int:
        res = 0

        for i in range(1, len(height)-1):
            lh, rh = height[i], height[i]

            for r in range(i+1, len(height)):
                rh = max(rh, height[r])

            for l in range(i-1, -1, -1):
                lh = max(lh, height[l])
            
            h = min(lh, rh) - height[i]

            if h > 0:
                res += h

        
        return res
```
2. 雙指針優化: 為了得到兩邊的最高高度，使用了雙指針來遍歷，每到一個柱子都向兩邊遍歷一遍，這其實是有重複計算的。我們把每個位置的左邊最高高度記錄在一個陣列上（maxLeft），右邊最高高度記錄在一個陣列上（maxRight），這樣就避免了重複計算。
```python
class Solution:
    def trap(self, height: List[int]) -> int:

        lhm, rhm = [0] * len(height), [0] * len(height)
        lhm[0], rhm[len(height)-1] = height[0], height[len(height)-1]

        for i in range(1, len(height)):
            lhm[i] = max(height[i], lhm[i-1])

        for i in range(len(height)-2, -1, -1):
            rhm[i] = max(height[i], rhm[i+1])


        res = 0
        for i in range(1, len(height)-1):
            h = min(lhm[i], rhm[i]) - height[i]
            if h > 0:
                res += h
        
        return res
```
3. 單調棧: 本題是要求下標 j 元素的左邊和右邊一個大於自己的元素，因此這題可以使用單調棧來解
   1. 單調棧是使用橫向比較
   2. 單調棧內存的元素：下標 j 代表的是柱子的位置
   3. 單調棧內，從棧頂到棧底，是小到大，還是大到小？ 從題目來思考，由於我們只要知道大於 j 的一個元素即可，因此，小到大排列可以達到此目的
   4. 盤點遇到的情況：
      1. height[st[-1]] == height[j] (e.g., 5, 5,1): 此時將舊有的元素 pop, 並存入新的元素
      2. height[st[-1]] > height[j] (e.g., 6,2, 1): 此時直接存入新元素 
      2. height[st[-1]] < height[j] (e.g., 6,2,1, 3): 我們需要比較三個元素: 下標j, 下標 j 的左邊大於 j 的第一個元素，下標 j 的右邊大於 j 的第一個元素, 因此，我們需要重複找到凹槽並計算寬高
```python
class Solution:
    def trap(self, height: List[int]) -> int:

        st, res = deque(), 0
        st.append(0)

        for i in range(1, len(height)):
            if height[st[-1]] == height[i]:
                st.pop()
                st.append(i)
            elif height[st[-1]] > height[i]:
                st.append(i)
            else:
                while len(st) > 0 and height[st[-1]] < height[i]:
                    mid = st.pop()
                    if len(st) > 0:
                        l, r = height[st[-1]], height[i]
                        h = min(l, r) - height[mid]
                        w = i - st[-1] - 1
                        res += h * w
                st.append(i)

        return res
# or 
# class Solution:
#     def trap(self, height: List[int]) -> int:

#         st, res = deque(), 0
#         st.append(0)

#         for i in range(1, len(height)):
#             while len(st) > 0 and height[st[-1]] < height[i]:
#                 mid = st.pop()
#                 if len(st) > 0:
#                     l, r = height[st[-1]], height[i]
#                     h = min(l, r) - height[mid]
#                     w = i - st[-1] - 1
#                     res += h * w
#             st.append(i)

#         return res
```

# [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/description/)
題目

<img width="554" alt="image" src="https://github.com/user-attachments/assets/8c32f9e3-8a8e-4f78-8190-3d1810de8f8c">

- 思路: 本題與 [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/description/) 非常類似，因此，本題也會使用三種解法來解題
1. 暴力解法 (雙指針)：會 TLE
```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        res = 0

        for i in range(len(heights)):
            l, r = i, i 

            while l >= 0:
                if heights[i] > heights[l]:
                    break
                l -= 1
            while r < len(heights):
                if heights[i] > heights[r]:
                    break
                r += 1

            h, w = heights[i], r - l - 1

            res = max(res, h*w)


        return res
```
2. 雙指針優化: 為了得到兩邊的第一個低於自己的高度，使用了雙指針來遍歷，每到一個柱子都向兩邊遍歷一遍，這其實是有重複計算的。我們把每個位置的左邊低於自己的第一個元素的高度記錄在一個陣列上（leftLowerOne），右邊也記錄在一個陣列上（rightLowerOne），這樣就避免了重複計算。
```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        res = 0
        llo, rlo = [0] * len(heights), [0] * len(heights)
        llo[0], rlo[-1] = -1, len(heights)

        for i in range(1, len(heights)):
            l = i - 1
            while l >= 0 and heights[l] >= heights[i]:
                l = llo[l]
            llo[i] = l

        for i in range(len(heights)-2, -1, -1):
            r = i + 1
            while r < len(heights) and heights[r] >= heights[i]:
                r = rlo[r]
            rlo[i] = r

        for i in range(len(heights)):
            area = heights[i] * (rlo[i] - llo[i] - 1)
            res = max(res, area)

        return res
```
3. 單調棧: 本題是要求下標 j 元素的左邊和右邊一個小於自己的元素，因此這題可以使用單調棧來解
   1. 單調棧是使用橫向比較
   2. 單調棧內存的元素：下標 j 代表的是柱子的位置
   3. 單調棧內，從棧頂到棧底，是小到大，還是大到小？ 從題目來思考，由於我們只要知道小於 j 的一個元素即可，因此，大到小排列可以達到此目的
   4. 盤點遇到的情況：
      1. height[st[-1]] == height[j] (e.g., 5, 5,1): 此時將舊有的元素 pop, 並存入新的元素
      2. height[st[-1]] < height[j] (e.g., 6,2, 1): 此時直接存入新元素 
      3. height[st[-1]] > height[j] (e.g., 6,2,1, 3): 我們需要比較三個元素: 下標j, 下標 j 的左邊大於 j 的第一個元素，下標 j 的右邊大於 j 的第一個元素, 因此，我們需要重複找到凹槽並計算寬高
```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        res, st = 0, deque()
        arr = [0] + heights[::] + [0]
        st.append(0)
        
        for i in range(1, len(arr)):
            if arr[i] > arr[st[-1]]:
                st.append(i)
            elif arr[i] == arr[st[-1]]:
                st.pop()
                st.append(i)
            else:
                while st and arr[i] < arr[st[-1]]:
                    mid = st[-1]
                    st.pop()
                    if st:
                        l, r = st[-1], i
                        w, h = r-l-1, arr[mid]
                        res = max(res, w*h)
                st.append(i)

        return res
    
# or
# class Solution:
#     def largestRectangleArea(self, heights: List[int]) -> int:
#         res, st = 0, deque()
#         arr = [0] + heights[::] + [0]
#         st.append(0)
        
#         for i in range(1, len(arr)):
#             while st and arr[i] < arr[st[-1]]:
#                 mid = st[-1]
#                 st.pop()
#                 if st:
#                     l, r = st[-1], i
#                     w, h = r-l-1, arr[mid]
#                     res = max(res, w*h)
#             st.append(i)

#         return res
```
