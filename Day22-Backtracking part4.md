# [491. Non-decreasing Subsequences](https://leetcode.com/problems/non-decreasing-subsequences/description/)
題目

- 本題與 [90. Subsets II](https://leetcode.com/problems/subsets-ii/description/) 類似，重點在於去重，尤其是對樹層進行去重
```python
class Solution:
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        res = []

        def backtracking(path, startIdx):
            if len(path) >= 2:
                res.append(path)
            
            uset = set() # 對樹層進行去重
            for i in range(startIdx, len(nums)):
                if (path and nums[i] < path[-1]) or nums[i] in uset: continue
                uset.add(nums[i])
                path.append(nums[i])
                backtracking(path[:], i+1)
                path.pop()
        
        backtracking([], 0)

        return res
```

# [491. Non-decreasing Subsequences](https://leetcode.com/problems/non-decreasing-subsequences/description/)
題目

- 排列是有序的，例如 `{1,2}`, `{2,1}`，這是兩個排序集合，但是同一個組合
```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res, used = [], [False] * len(nums)

        def backtracking(path, used):
            if len(path) == len(nums):
                res.append(path)
                return
            
            for i in range(len(nums)):
                if used[i]: continue
                used[i] = True
                path.append(nums[i])
                backtracking(path[:], used)
                path.pop()
                used[i] = False

        backtracking([], used)

        return res
```

# [47. Permutations II](https://leetcode.com/problems/permutations-ii/description/)
題目

- 與 [491. Non-decreasing Subsequences](https://leetcode.com/problems/non-decreasing-subsequences/description/) 差異在於，題目給定的數組元素不唯一，因此要回溯時要多加上樹層去重
```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        res, used = [], [False] * len(nums)

        def backtracking(path, used):
            if len(path) == len(nums):
                res.append(path)
                return
            
            uset = set() #樹層去重
            for i in range(len(nums)):
                if used[i] or nums[i] in uset: continue 
                used[i] = True
                uset.add(nums[i])
                path.append(nums[i])
                backtracking(path[:], used)
                path.pop()
                used[i] = False

        backtracking([], used)

        return res
```

# [332. Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/description/)
題目

- 在一個行程中，如果航班處理不好容易變成一個圈，成為死循環
- 有多種解法，字母序靠前排在前面，讓很多同學望而退步，如何該記錄映射關係呢?
- 使用回溯法（也可以說深搜） 的話，那麼終止條件是什麼呢?
- 在搜尋的過程中，如何遍歷一個機場所對應的所有機場
```python
class Solution:
    def findItinerary(self, tickets):
        # Build a graph to represent the flight connections
        self.graph = self._build_graph(tickets)
        # Initialize an empty list to store the final itinerary
        self.result = []
        # Start the backtracking process from the "JFK" airport
        self._backtrack("JFK")
        # Return the final itinerary in reverse order
        return self.result[::-1]

    def _build_graph(self, tickets):
        # Create a default dictionary to store the graph
        graph = defaultdict(list)
        # Iterate over each ticket and add the connection to the graph
        for departure, arrival in tickets:
            graph[departure].append(arrival)
        # Sort the neighbors of each airport in reverse alphabetical order to ensure backtrack do the correct order
        '''
        tickets = [["JFK", "LHR"], ["JFK", "ORD"], ["ORD", "SFO"], ["LHR", "SFO"]]
        When we build the graph, we get:
        graph = {
            "JFK": ["LHR", "ORD"],
            "LHR": ["SFO"],
            "ORD": ["SFO"]
        }
        Now, let's say we start our journey from "JFK". We have two options: "LHR" and "ORD". 
        If we don't sort the neighbors, we might visit "LHR" first, and then we'll be stuck because we can't visit "ORD" anymore 
        (since we've already visited "LHR" and we can't go back).
        However, if we sort the neighbors of "JFK" in reverse alphabetical order, we get:
        graph = {
            "JFK": ["ORD", "LHR"],
            "LHR": ["SFO"],
            "ORD": ["SFO"]
        }
        '''
        for neighbors in graph.values():
            neighbors.sort(reverse=True)
        # Return the constructed graph
        return graph

    def _backtrack(self, airport):
        # While there are still unvisited neighbors of the current airport
        while self.graph[airport]:
            # Get the next unvisited neighbor
            next_airport = self.graph[airport].pop()
            # Recursively visit the next airport
            self._backtrack(next_airport)
        # Add the current airport to the result list
        self.result.append(airport)
```

# [51. N-Queens](https://leetcode.com/problems/n-queens/description/)
題目

- 題目條件: 皇后兩兩大米字不能對到
- 針對這個限制條件，我們在每次遞回時確認有無符合題目條件，最後判斷是否到了葉節點即可完成此題
```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        res, chessboard = [], ['.' * n for _ in range(n)] 

        def isValid(c, r):
            for i in range(r):
                if chessboard[i][c] == 'Q':
                    return False
            
            i, j = r - 1, c - 1
            while i >= 0 and j >= 0:
                if chessboard[i][j] == 'Q':
                    return False
                i -= 1
                j -= 1
            
            i, j = r - 1, c + 1
            while i >= 0 and j < n:
                if chessboard[i][j] == 'Q':
                    return False
                i -= 1
                j += 1
                
            return True
            
        def backtracking(r):
            if r == n:
                res.append(chessboard[:])
                return
            
            for c in range(n):
                if isValid(c, r):
                    chessboard[r] = chessboard[r][:c] + 'Q' + chessboard[r][c+1:]
                    backtracking(r+1)
                    chessboard[r] = chessboard[r][:c] + '.' + chessboard[r][c+1:]
        
        backtracking(0)

        return res
```

# [37. Sudoku Solver](https://leetcode.com/problems/sudoku-solver/description/)
題目

- 太累了，明天補上 ＝ ＝
```python
```