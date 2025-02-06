# [53. 寻宝](https://kamacoder.com/problempage.php?pid=1053)

## 解法ㄧ： Pirme ```python
def solution():
    v,e = map(int, input().strip().split())
    graph = [[10001] * (v+1) for _ in range(v+1)]
    for _ in range(e):
        x,y,k = map(int, input().strip().split())
        graph[x][y] = k
        graph[y][x] = k

    minDist = [10001] * (v+1)
    isInTree = [False] * (v+1)

    for i in range(1, v):
        # choose the node from the graoh of the min gen tree
        curr = -1
        minVal = float("inf")
        for j in range(1,v+1):
            if not isInTree[j] and minDist[j] < minVal:
                minVal = minDist[j]
                curr = j

        # step 2: add the node into the min gen tree from the graph
        isInTree[curr] = True
        
        # step 3: update the minDist
        for j in range(1, v+1):
            if not isInTree[j] and graph[curr][j] < minDist[j]:
                minDist[j] = graph[curr][j]

    return sum(minDist[2:])

print(solution())
```
## 解法二： Kruskal
```python
class UnionFind:
    def __init__(self, size):
        self.parent = list(range(size+1))

    def isSame(self, u, v):
        return self.find(u) == self.find(v)

    def find(self, u):
        if self.parent[u] != u:
            self.parent[u] = self.find(self.parent[u])
        return self.parent[u]
    
    def union(self, u, v):
        rootU = u
        rootV = v
        if rootU != rootV:
            self.parent[rootV] = rootU

def solution():
    v,e = map(int, input().strip().split())
    edges = []

    for _ in range(e):
        s, t, v = map(int, input().strip().split())
        edges.append([v, s, t])

    edges.sort()
    
    graph = UnionFind(v)
    res = 0

    for v, s, t in edges:
        rootS = graph.find(s)
        rootT = graph.find(t)

        if rootS != rootT:
            res += v
            graph.union(rootS, rootT)

    return res


print(solution())
```

# [117. 软件构建](https://kamacoder.com/problempage.php?pid=1191)

```python
from collections import deque, defaultdict

def solution():
    n,m = map(int, input().strip().split())
    umap = defaultdict(list)
    inDegree = [0] * n
    
    for i in range(m):
        s,t = map(int, input().strip().split())
        inDegree[t] += 1
        umap[s].append(t)
        
    que = deque()
    result = []

    for i in range(n):
        if not inDegree[i]:
            que.append(i)

    while que:
        curr = que.popleft()
        result.append(curr)
        for file in umap[curr]:
            inDegree[file] -= 1
            if not inDegree[file]:
                que.append(file)

    if len(result) == n:
        print(" ".join(map(str, result)))
    else:
        print(-1)

solution()
```

# [47. 参加科学大会](https://kamacoder.com/problempage.php?pid=1047)

## 解法一: Dijkstra 樸素版
```python
def solution():
    n,m = map(int, input().strip().split())

    graph =  [[float("inf")] * (n+1) for _ in range(n+1)]]
    
    for i in range(m):
        s,t,v = map(int, input().strip().split())
        graph[s][t] = v

    start,end = 1,n
    minDist = [float("inf") * (n+1)]
    visited = [False * (n+1)]
    
    minDist[start] = 0
    for _ in range(1, n+1):
        minVal = float("inf")
        curr = -1

      for v in range(1, n+1):
            if not visited[v] and minDist[v] < minVal:
                minVal = minDist[v]
                curr = v 

        if curr == -1:
            break

        visited[curr] = True

        for v in range(1, n+1):
            if not visited[v] and graph[curr][v] != float("inf") and minDist[curr] + graph[curr][v] < minDist[v]:
                minDist[v] = minDist[curr] + graph[curr][v]
        
    if minDist[end] == float("inf"):
        print(-1)
    else:
        print(minDist[end])

            
solution()
```

## 解法二: Dijkstra 堆優化版
```python
import heapq

class Edge:
    def __init__(self, to, val):
        self.to = to
        self.val = val
        
def solution():
    n,m = map(int, input().strip().split())
    edges = [tuple(map(int, input().strip().split()), for _ in range(m))]
    start,end = 1,n
    print(dijkstra(n,m,start,end,edges))

def dijkstra(n,m,start,end,edges):
    graph = [[] for _ in range(n+1)]
    
    for s,t,v in edges:
        graph[s].append(Edge(t, v))

    minDist = [float("inf") * (n+1)]
    visited = [False] * (n+1)

    pq = []
    heapq.heappush(pq, (0, start))
    minDist[start] = 0

    while pq:
        currDist, currNode = heapq.heappop(pq)
        
        if visited[currNode]:
            continue
        
        visited[currNode] = True
        
        for edge in graph[currNode]:
            if not visited[edge.to] and currDist + edge.val < minDist[edge.to]:
                minDist[edge.to] = currDist + edge.val
                heapq.heappush(pq, (minDist[edge.to], edge.to))
        
    return -1 if minDist[end] == float("inf") else minDist[end]


solution()
```