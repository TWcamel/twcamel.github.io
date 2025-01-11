# [110. 字符串接龙](https://kamacoder.com/problempage.php?pid=1183)

```python
from collections import deque

def solution():
    n = int(input().strip())
    beginStr, endStr = input().split()
    if begainStr == endStr:
        print(0)
        return

    strList = []
    for _ in range(n):
        strList.append(input().strip())

    visited = [False for _ in range(n)]
    que = deque()
    que.append([begainStr, 1])

    while que:
        temp, step = que.popleft()

        if judge(temp, endStr):
            print(step+1)
            return

        for i in range(n):
            if not visited[i] and judge(strList[i], temp):
                visited[i] = True
                que.append([strList[i], step+1])

def judge(str1, str2):
    diff = 0
    for i in range(len(str1)):
        if str1[i] != str2[i]:
            diff += 1
    return diff == 1

solution()
```

# [105. 有向图的完全可达性](https://kamacoder.com/problempage.php?pid=1177)

##  解法一： dfs
```python
import collections

def solution():
    n,k = map(int, input().strip().split())

    graph = collections.defaultdict(list)
    for _ in range(k):
        src,dest = map(int, input().strip().split())
        graph[src].append(dest)

    visited = [False] * (n+1)
    dfs(graph, 1, visited)

    print(judge(graph, n, visited))

def dfs(graph, node, visited):
    if visited[node]:
        return

    visited[node] = True

    for neighber in graph[node]:
        dfs(graph, n, visited)

def judge(graph, n, visited):
    for i in range(1, 1+n):
        if not visited[i]:
            return -1
    return 1

solution()
```

##  解法二： bfs
```python
import collections

def solution():
    n,k = map(int, input().strip().split())

    graph = collections.defaultdict(list)
    for _ in range(k):
        src,dest = map(int, input().strip().split())
        graph[src].append(dest)

    path = set()

    bfs(garph, path, 1)

    print(judge(path, n))

def bfs(graph, path, root):
    que = collections.deque()
    que.append(root)

    while que:
        curr = que.popleft()
        path.add(curr)

        for neighbor in graph[curr]:
            que.append(neighbor)

        graph[curr] = []

    return

def judge(path, n):
    if path == {_ for _ in range(1, 1+n)}:
        return 1
    else:
        return -1

solution()
```

# [106. 岛屿的周长](https://kamacoder.com/problempage.php?pid=1178)

## 解法一: 考慮島嶼相鄰陸地，利用邊界與判斷是否旁邊是海水計算周長

```python
directions = [[1,0], [0,1], [-1,0], [0,-1]]

def solution():
    n,m = map(int, input().strip().split())
    
    graph = []
    for _ in range(n):
        graph.append(list(map(int, input().strip().split())))
    
    print(edgeLengthCalculator(graph, n, m))

def edgeLengthCalculator(graph, n, m):
    res = 0
    
    for i in range(n):
        for j in range(m):
            if not graph[i][j]:
                continue
            for dx, dy in directions:
                nx, ny = i + dx, j + dy
                if nx < 0 or nx >= n or ny < 0 or ny >= m or not graph[nx][ny]:
                    res += 1
        
    return res
    
solution()
```

## 解法二: 考慮島嶼相鄰陸地，相鄰島嶼數量計算周長時，減去兩個邊長

```python
directions = [[1,0], [0,1], [-1,0], [0,-1]]

def solution():
    n,m = map(int, input().strip().split())
    
    graph = []
    for _ in range(n):
        graph.append(list(map(int, input().strip().split())))
    
    print(edgeLengthCalculator(graph, n, m))

def edgeLengthCalculator(graph, n, m):
    landsCnt = 0
    adjcentCnt = 0
    
    for i in range(n):
        for j in range(m):
            if not graph[i][j]:
                continue
            landsCnt += 1
            if i - 1 >= 0 and graph[i-1][j]: adjcentCnt += 1
            if j - 1 >= 0 and graph[i][j-1]: adjcentCnt += 1
        
    return landsCnt * 4 - adjcentCnt * 2
    
solution()
```
