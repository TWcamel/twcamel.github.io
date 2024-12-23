# [98. 所有可达路径](https://kamacoder.com/problempage.php?pid=1170)

- 思路
圖的核心觀念為 dfs & bfs, 記得 dfs 主要是使用 backtracking 來解，而 bfs 則用迴圈, 而資料存儲的方式又分為鄰接矩陣 以及 鄰接表，兩種需分別用二維矩陣 以及 鏈表 來進行資存儲的操作

## 1. 鄰接矩陣
```python
def dfs(graph, x, n, path, result):
    if x == n:
        result.append(path.copy()) # do result
        return
    for i in range(1, n+1):
        if graph[x][i] == 1:
            path.append(i)
            dfs(graph=graph, x=i, n=n, path=path, result=result)
            path.pop() # find path, do backtracking
    
def main():
    n,m = map(int, input().split())
    graph = [[0] * (n+1) for _ in range(n+1)]

    for i in range(m):
        s,t = map(int, input().split())
        graph[s][t] = 1
    
    result = []
    dfs(graph=graph, x=1, n=n, path=[1], result=result)

    if not result:
        print(-1)
    else:
        for path in result:
            print(' '.join(map(str, path)))

if __name__ == "__main__":
    main()
```
## 2. 鄰接表
```python
from collections import defaultdict

result = []
path = []

def dfs(graph, x, n):
    if x == n:
        result.append(path.copy()) # do result
        return
    
    for i in graph[x]:
        path.append(i)
        dfs(graph, i, n)
        path.pop() # backtacking
    
    
def main():
    n,m = map(int, input().split())

    graph = defaultdict(list)
    for i in range(m):
        s,t = map(int, input().split())
        graph[s].append(t)
    
    path.append(1)
    dfs(graph, 1, n)

    if not result:
        print(-1)

    for p in result:
        print(' '.join(map(str, p)))
    
if __name__ == "__main__":
    main()
```

# [99. 島嶼数量](https://kamacoder.com/problempage.php?pid=1171)

```python
from collections import deque

dir = [[0, 1], [1, 0], [-1, 0], [0, -1]]

def bfs(graph, visited, x, y, maxX, maxY):
    que = deque([])
    que.append([x, y])
    visited[x][y] = 1
    while que:
        curX, curY = que.popleft()
        for i, j in dir:
            nextX, nextY = curX + i, curY + j
            if (nextX < 0 or nextY < 0 or nextX >= maxX or nextY >= maxY):
                continue
            if not visited[nextX][nextY] and graph[nextX][nextY]:
                visited[nextX][nextY] = 1
                que.append([nextX, nextY])

def dfs(graph, visited, x, y, maxX, maxY):
    if x < 0 or y < 0 or x >= maxX or y >= maxY or visited[x][y] or graph[x][y] == 0:
        return

    visited[x][y] = 1
    for i in range(4):
        nextX, nextY = x + dir[i][0], y + dir[i][1]
        dfs(graph, visited, nextX, nextY, maxX, maxY)

def main():
    n, m = map(int, input().split())

    graph = []
    visited = []
    for i in range(n):
        graph.append(list(map(int, input().split())))
        visited.append([0 for _ in range(m)]) 

    res = 0

    for i in range(n):
        for j in range(m):
            if graph[i][j] == 1 and not visited[i][j]: 
                res += 1
                # dfs(graph, visited, i, j, n, m) 
                bfs(graph, visited, i, j, n, m)

    print(res)

if __name__ == "__main__":
    main()
```

# [100. 島嶼的最大面積](https://kamacoder.com/problempage.php?pid=1172)

```python
from collections import deque

dir = [[0,1], [1,0], [-1, 0], [0,-1]]

def bfs(graph, visited, x, y, maxX, maxY):
    que = deque([])
    que.append([x,y])
    visited[x][y] = 1
    area = 1
    while que:
        curX, curY = que.popleft()
        for i, j in dir:
            nextX, nextY = curX + i, curY + j
            if nextX < 0 or nextY < 0 or nextX >= maxX or nextY >= maxY or visited[nextX][nextY] or not graph[nextX][nextY]:
                continue
            visited[nextX][nextY] = 1
            que.append([nextX, nextY])
            area += 1
    return area

def dfs(graph, visited, x, y, maxX, maxY):
    if x < 0 or y < 0 or x >= maxX or y >= maxY or visited[x][y] or graph[x][y] == 0:
        return 0 
    
    visited[x][y] = 1
    area = 1

    for i in range(4):
        nextX, nextY = x + dir[i][0], y + dir[i][1]
        area += dfs(graph, visited, nextX, nextY, maxX, maxY)
    
    return area

def main():
    n, m = map(int, input().split())

    graph = []
    visited = []

    for i in range(n):
        graph.append(list(map(int, input().split())))
        visited.append([0] * m)

    res = 0
    
    for i in range(n):
        for j in range(m):
            if not visited[i][j] and graph[i][j]:
                curr = bfs(graph, visited, i, j, n, m)
                res = max(curr, res)

    print(res)

if __name__ == "__main__":
    main()
```

# [101. 孤島的總面積](https://kamacoder.com/problempage.php?pid=1173)

```python
from collections import deque

directions = [[0,1], [1,0], [-1,0], [0,-1]]

def dfs(graph, visited, x, y, maxX, maxY):
    stack = [(x, y)]
    visited[x][y] = True
    area = 0
    isIsolated = True
    
    while stack:
        cx, cy = stack.pop()
        area += 1
        if cx == 0 or cx == maxX - 1 or cy == 0 or cy == maxY -1:
            isIsolated = False
        for dx, dy in directions:
            nx, ny = cx + dx, cy + dy
            if 0 <= nx < maxX and 0 <= ny < maxY:
                if not visited[nx][ny] and graph[nx][ny] == 1:
                    visited[nx][ny] = True
                    stack.append((nx, ny))
    if isIsolated:
        return area
    else:
        return 0

def bfs(graph, visited, x, y, maxX, maxY):
    queue = deque()
    queue.append((x, y))
    visited[x][y] = True
    area = 0
    isIsolated = True
    
    while queue:
        cx, cy = queue.popleft()
        area += 1
        if cx == 0 or cx == maxX - 1 or cy == 0 or cy == maxY -1:
            isIsolated = False
        for dx, dy in directions:
            nx, ny = cx + dx, cy + dy
            if 0 <= nx < maxX and 0 <= ny < maxY:
                if not visited[nx][ny] and graph[nx][ny] == 1:
                    visited[nx][ny] = True
                    queue.append((nx, ny))
    if isIsolated:
        return area
    else:
        return 0

def main():
    n, m = map(int, input().split())
    graph = []
    for i in range(n):
       graph.append(list(map(int, input().split())))
    visited = [[False] * m for i in range(n)]
    
    res = 0
    for i in range(n):
        for j in range(m):
            if not visited[i][j] and graph[i][j] == 1:
                res += bfs(graph, visited, i, j, n, m)

    print(res)

if __name__ == "__main__":
    main()
```

# [102. 沉没孤岛](https://kamacoder.com/problempage.php?pid=1174)

```python
direction = [[1,0],[0,1],[-1,0],[0,-1]]

def main():
    n,m = map(int, input().split())
    graph = []
    
    for i in range(n):
        graph.append(list(map(int, input.split())))
    
    res = graph.copy()
    
    for i in range(n):
        if graph[i][0] == 1: dfs(graph, i, 0)
        if graph[i][m-1] == 1: dfs(graph, i, m-1)

    for j in range(m):
        if graph[0][j] == 1: dfs(graph, 0, j)
        if graph[n-1][j] == 1: dfs(graph, n-1, j)

    for i in range(n):
        for j in range(m):
            if graph[i][j]:
                res[i][j] = 0
    
    print(res)

def dfs(graph, x, y):
    graph[x][y] = 0
    
    for cx, cy in direction:
        nx, ny = x + cx, y + cy
        if nx < 0 or ny < 0 or nx >= len(graph) or ny >= len(graph[0]):
            continue
        if graph[nx][ny] == 0:
            continue
        
        dfs(graph, nx, ny)
        

if __name__ == "__main__":
    main()
```