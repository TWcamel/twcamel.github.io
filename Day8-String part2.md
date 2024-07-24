# [151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/description/)
題目

思路
- 整個算法最核心的概念是"反轉"
- 但這邊有個問題是，如何翻轉單字? -> 讓整個字串翻轉完成後，再針對單字做翻轉，即可達到效果
```python
class Solution:
    def reverseWords(self, s: str) -> str:
        s = s.strip()
        rec = defaultdict(int)

        lst = list(s)

        # remove blanks between the words
        for i, e in enumerate(lst):
            if lst[i] == " " and lst[i - 1] == " ":
                j = i
                while j < len(lst):
                    if lst[j] != " ":
                        break
                    j += 1
                lst[i::] = lst[j::]
        
        # reverse entire string
        lst = lst[::-1]

        # revers in each words
        res = ""
        l = 0
        for i in range(len(lst)):
            if lst[i] == " ":
                # reverse
                r = i
                res += self.reverse(lst[l:r])
                res += " "
                l = i + 1

        res +=  self.reverse(lst[l:len(lst)])

        return res
    
    def reverse(self, lst):
        l, r = 0, len(lst) - 1
        while l < r:
            tmp = lst[l]
            lst[l] = lst[r]
            lst[r] = tmp

            l += 1
            r -= 1
        
        return ''.join(lst)
```

# [右旋字符串](https://kamacoder.com/problempage.php?pid=1065)
題目

思路
- python 字串操作要注意 `:` 左右邊的用法
```python
n = int(input())
s = input()

print(s[len(s)-n:] + s[:(len(s)-n)])
```

# [28. Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)
題目

## 暴力解
- 思路
    - 當遇到不匹配的情況，將整個移動窗口向右移動一位，若整個窗口與模式串相同則代表找到匹配字串
    - 這邊的時間複雜度為 `O(N)`，若要進一步優化，則需要利用已經匹配過的字串來產生記憶，從而達到剪枝效果，其中最具代表性的演算法是 `KMP`
    ```python
    class Solution:
        def strStr(self, haystack: str, needle: str) -> int:
            lenH, lenN = len(haystack), len(needle)

            if lenN > lenH:
                return -1

            for i in range(lenH):
                if haystack[i:i+lenN] == needle:
                    return i

            return -1
    ```
## `KMP`
- 思路
    - `KMP` 精隨在於利用前綴表來完成算法
    - 前綴表資訊中包含了 "最大長度的前後綴", 也是利用此來找到下個迴圈的查找下標
    ```python
    class Solution:
        def strStr(self, haystack: str, needle: str) -> int:
            lenH, lenN = len(haystack), len(needle)
    
            if lenH < lenN: return -1
    
            # init
            next = [0] * lenN
            self.getNext(next, needle)
            j = 0
            for i in range(lenH):
                # while char is not the same
                while j > 0 and haystack[i] != needle[j]:
                    j = next[j - 1]
                # if chat is the same
                if haystack[i] == needle[j]:
                    j += 1
                # if the search is complete
                if j == lenN:
                    return i - lenN + 1
    
            return -1
        
        def getNext(self, next: List[int], s: str):
            # init, j = prefix, i = suffix
            j = 0
            next[0] = 0
            for i in range(1, len(s)):
                # while i and j is not the same
                while j > 0 and s[i] != s[j]:
                    j = next[j - 1]
                # if i and j is the same
                if s[i] == s[j]:
                    j += 1
                # update the next array
                # print(i, j, next)
                next[i] = j
    ```

# [459. Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern/description/)
題目

思路
- 本題目的解題邏輯是，若將原字串去頭去尾後 concat，後面的子串做前串，前面的子串作後串，若還能組出字串 `s`，就代表有重複字串
    - 暴力解
    ```python
    class Solution:
        def repeatedSubstringPattern(self, s: str) -> bool:
            ext = s[1:] + s[:-1]
            return ext.find(s) != -1
    ```

    - `KMP`
    ```python
    class Solution:
        def repeatedSubstringPattern(self, s: str) -> bool:
            n = len(s)
            if n <= 1:
                return False
            ext = s[1:] + s[:-1]               
    
            j = 0
            next = [0] * len(s)
            self.strStr(next, s)
    
            for i in range(len(ext)):
                while j > 0 and ext[i] != s[j]:
                    j = next[j - 1]
                if ext[i] == s[j]:
                    j += 1
                if j == len(s):
                    return True
            
            return False
    
        
        def strStr(self, next: List[int], s: str):
            j = 0
            next[0] = 0
            for i in range(1, len(s)):
                while j > 0 and s[j] != s[i]:
                    j = next[j - 1]
                if s[j] == s[i]:
                    j += 1
                next[i] = j
                
    ```