# [344. Reverse String](https://leetcode.com/problems/reverse-string/description/)
題目

思路
- 題目關鍵是兩兩交換，所以使用雙指針法如下
```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        sz = len(s)

        if sz == 1:
            return s
        
        l, r = 0, sz - 1 
        
        while l < r:
            tmp = s[l]
            s[l] = s[r]
            s[r] = tmp

            l += 1
            r -= 1
        
        return s
```

# [541. Reverse String II](https://leetcode.com/problems/reverse-string-ii/description/)
題目

思路
- 承接上題，照題目給的資訊讓每次迭代時, 依照題目規定的規則反轉就可以了
- 注意要左閉右開
```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        sz = len(s)
        for i in range(0, sz, 2*k):
            if i + k <= sz:
                s = self.reversFun(list(s), i, i + k - 1)
            else: 
                s = self.reversFun(list(s), i, sz - 1)
        return s
        
    def reversFun(self, s: list, l: int, r: int) -> str:

        while l < r:
            tmp = s[l]
            s[l] = s[r]
            s[r] = tmp

            l += 1
            r -= 1

        return ''.join(s)
```

# [54. 替換數字](https://kamacoder.com/problempage.php?pid=1064)
題目

思路
- 字串在 python 裡面屬於 primitive, 不可直接修改，因此要進行拓展，或是新增另一個空字串對其進行黏合
```python
quiz = input()
res = ""
for s in quiz:
    if ord("0") <= ord(s) <= ord("9"):
        res += "number"
    else:
        res += s
print(res)
```