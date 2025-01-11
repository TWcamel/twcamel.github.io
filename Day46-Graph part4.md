# [107. 寻找存在的路径](https://kamacoder.com/problempage.php?pid=1179)

```python
class UnionFind:
    def __init__(self, size):
        self.parrent = list(range(size+1))
    
    def isSame(self, u, v):
        return self.find(u) == self.find(v)
    
    def find(self, u):
        if self.parrent[u] != u:
            self.parrent[u] = self.find(self.parrent[u])
        return self.parrent[u]
    
    def union(self, u, v):
        rootU = self.find(u)
        rootV = self.find(v)
        if rootU != rootV:
            self.parrent[rootV] = rootU

def solution():
    n, m = map(int, input().strip().split())
    
    uf = UnionFind(n)
    
    for _ in range(m):
        s, t = map(int, input().strip().split())
        uf.union(s, t)
    
    source, target = map(int, input().strip().split())
    
    if uf.isSame(source, target):
        print(1)
    else:
        print(0)
        
solution()
```

# (108. 冗余连接)[https://kamacoder.com/problempage.php?pid=1181]

```python
class UnionFind:
    def __init__(self, size):
        self.parrent = list(range(size+1))
    
    def isSame(self, u, v):
        return self.find(u) == self.find(v)
    
    def find(self, u):
        if self.parrent[u] != u:
            self.parrent[u] = self.find(self.parrent[u])
        return self.parrent[u]
    
    def union(self, u, v):
        rootu = self.find(u)
        rootv = self.find(v)
        if rootu != rootv:
            self.parrent[v] = u
        
def solution():
    n = int(input().strip())
    uf = UnionFind(n)
    
    for _  in range(n):
        s, t = map(int, input().strip().split())
        if uf.isSame(s, t):
            print(f"{s} {t}")
            return
        else:
            uf.union(s, t)
        
solution()
```
# (109. 冗余连接II)[https://kamacoder.com/problempage.php?pid=1182]

```python
from collections import defaultdict

class UnionFind:
    def __init__(self, size):
        self.parent = list(range(size + 1))
    
    def isSame(self, u, v):
        return self.find(u) == self.find(v)
    
    def find(self, u):
        if self.parent[u] != u:
            self.parent[u] = self.find(self.parent[u])
        return self.parent[u]
    
    def union(self, u, v):
        rootU = self.find(u)
        rootV = self.find(v)
        if rootU != rootV:
            self.parent[rootU] = rootV
    
def solution():
    n = int(input())
    edges = list()
    inDegree = defaultdict(int)
    
    for i in range(n):
        s, t = map(int, input().split())
        inDegree[t] += 1
        edges.append([s, t])
    
    vec = list()
    for i in range(n - 1, -1, -1):
        if inDegree[edges[i][1]] == 2:
            vec.append(i)
    
    if len(vec) > 0:
        if isTreeAfterRemoveEdge(edges, vec[0], n):
            print(edges[vec[0]][0], edges[vec[0]][1])
        else:
            print(edges[vec[1]][0], edges[vec[1]][1])
    else:
        getRemoveEdge(edges, n)

def isTreeAfterRemoveEdge(edges, deleteEdge, n):
    uf = UnionFind(n)
    
    for i in range(len(edges)):
        if i == deleteEdge:
            continue
        s, t = edges[i]
        if uf.isSame(s, t):
            return False
        else:
            uf.union(s, t)
            
    return True
    
def getRemoveEdge(edges, n):
    uf = UnionFind(n)
    
    for s, t in edges:
        if uf.isSame(s, t):
            print(s, t)
            return
        else:
            uf.union(s, t)
    
solution()
```