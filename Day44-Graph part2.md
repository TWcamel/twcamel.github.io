# [103. 水流問題](https://kamacoder.com/problempage.php?pid=1175)

- 思路
1. 暴力搜索所有點，看這個點是否能同時到達邊界一 & 邊界二，但這個解法會 TLE

```python
direction = [[1,0], [0,1], [-1,0], [0,-1]]

def main():
    n, m = map(int, input().split())
    graph = []

    for i in range(n):
        graph.append(list(map(int, input().strip().split())))

    for i in range(n):
        for j in range(m):
            if isresult(graph, i, j, n, m):
                print(f"{i} {j}")

def isresult(graph, x, y, n, m):
    visited = [[False for _ in range(m)] for _ in range(n)]

    dfs(graph, visited, x, y, n, m)

    isfirst, issecond = False, False

    for j in range(m):
        if visited[0][j]:
            isfirst = True
            break

    for i in range(n):
        if visited[i][n-1]:
            issecond = True
            break

    for j in range(m):
        if visited[n-1][j]:
            issecond = True
            break

    for i in range(n):
        if visited[i][0]:
            isfirst = True
            break

    return isfirst and issecond

def dfs(graph, visited, x, y, n, m):
    if x < 0 or y < 0 or x >= n or y >= m or visited[x][y]:
        return

    visited[x][y] = True

    for dx, dy in direction:
        nx, ny = x + dx, y + dy
        if 0 <= nx < n and 0 <= ny < m and graph[nx][ny] < graph[x][y]:
            dfs(graph, visited, nx, ny, n, m)

if __name__ == "__main__":
    mai()
```

2. 反過來想，水往高處流，分別從邊界一 & 邊界二兩邊逆流而上，找到兩邊相交的點
```python
direction = [[1,0], [0,1], [-1,0], [0,-1]]

def main():
    n, m = map(int, input().strip().split())
    graph = []

    for _ in range(n):
        graph.append(list(map(int, input().strip().split())))

    printResult(graph, n, m)

def printResult(graph, n, m):
    visitedFirst, visitedSecond = [[False for _ in range(m)] for _ in range(n)], [[False for _ in range(m)] for _ in range(n)]

    for i in range(n):
        dfs(graph, visitedFirst, i, 0, n, m)
        dfs(graph, visitedSecond, i, m - 1, n, m)

    for j in range(m):
        dfs(graph, visitedFirst, 0, j, n, m)
        dfs(graph, visitedSecond, n - 1, j, n, m)

    for i in range(n):
        for j in range(m):
            if visitedFirst[i][j] and visitedSecond[i][j]:
                print(f"{i} {j}")

def dfs(graph, visited, x, y, n, m):
    if visited[x][y]:
        return

    visited[x][y] = True

    for dx, dy in direction:
        nx, ny = x + dx, y + dy
        if 0 <= nx < n and 0 <= ny < m and graph[nx][ny] >= graph[x][y]:
            dfs(graph, visited, nx, ny, n, m)

if __name__ == "__main__":
    main()
```

# [104. 建造最大岛屿](https://kamacoder.com/problempage.php?pid=1176)

```python
def solve():
    import sys

    n, m = map(int, sys.stdin.readline().strip().split())
    graph = [list(map(int, sys.stdin.readline().strip().split())) for _ in range(n)]

    island_id = [[0]*m for _ in range(n)]
    island_area = {}

    mark = 2

    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    def dfs(x, y, mark):
        stack = [(x, y)]
        island_id[x][y] = mark
        area = 0
        while stack:
            cx, cy = stack.pop()
            area += 1
            for dx, dy in directions:
                nx, ny = cx + dx, cy + dy
                # 范围检查，且是陆地，且还没被标记
                if 0 <= nx < n and 0 <= ny < m and graph[nx][ny] == 1 and island_id[nx][ny] == 0:
                    island_id[nx][ny] = mark
                    stack.append((nx, ny))
        return area

    for i in range(n):
        for j in range(m):
            if graph[i][j] == 1 and island_id[i][j] == 0:
                area = dfs(i, j, mark)
                island_area[mark] = area
                mark += 1

    if len(island_area) == 0:
        print(1)
        return

    max_area = max(island_area.values())

    for i in range(n):
        for j in range(m):
            if graph[i][j] == 0:
                neighbors = set()
                for dx, dy in directions:
                    nx, ny = i + dx, j + dy
                    # 范围内，且是陆地
                    if 0 <= nx < n and 0 <= ny < m and graph[nx][ny] == 1:
                        neighbors.add(island_id[nx][ny])

                new_area = 1
                for island_mark in neighbors:
                    new_area += island_area[island_mark]

                max_area = max(max_area, new_area)

    print(max_area)

solve()
```
