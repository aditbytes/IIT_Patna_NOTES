# Graph Algorithms — Basic to Advanced

---

## Table of Contents

1. [What is a Graph?](#1-what-is-a-graph)
2. [Graph Terminology](#2-graph-terminology)
3. [Graph Representations](#3-graph-representations)
4. [Graph Implementation in Python](#4-graph-implementation-in-python)
5. [Graph Traversals — BFS & DFS](#5-graph-traversals--bfs--dfs)
6. [Shortest Path Algorithms](#6-shortest-path-algorithms)
7. [Cycle Detection](#7-cycle-detection)
8. [Topological Sort](#8-topological-sort)
9. [Minimum Spanning Tree (Prim's & Kruskal's)](#9-minimum-spanning-tree-prims--kruskals)
10. [Union-Find (Disjoint Set Union)](#10-union-find-disjoint-set-union)
11. [Strongly Connected Components](#11-strongly-connected-components)
12. [Floyd-Warshall (All-Pairs Shortest Path)](#12-floyd-warshall-all-pairs-shortest-path)
13. [Bellman-Ford Algorithm](#13-bellman-ford-algorithm)
14. [Bridges & Articulation Points](#14-bridges--articulation-points)
15. [Bipartite Graph Check](#15-bipartite-graph-check)
16. [Complexity Summary](#16-complexity-summary)

---

## 1. What is a Graph?

A **Graph** $G = (V, E)$ is a pair of:
- $V$ — a set of **vertices** (nodes)
- $E$ — a set of **edges** (connections between nodes)

Graphs model real-world relationships: road networks, social connections, web links, task dependencies, circuit layouts, and more.

---

## 2. Graph Terminology

| Term | Definition |
|------|------------|
| **Vertex / Node** | A fundamental element of a graph |
| **Edge** | A connection between two vertices |
| **Directed (Digraph)** | Edges have a direction: $A \to B$ |
| **Undirected** | Edges have no direction: $A - B$ |
| **Weighted** | Edges carry a numerical cost/weight |
| **Degree** | Number of edges at a vertex |
| **In-degree** | Edges *coming into* a vertex (directed) |
| **Out-degree** | Edges *going out of* a vertex (directed) |
| **Path** | Sequence of vertices connected by edges |
| **Simple Path** | A path with no repeated vertices |
| **Cycle** | A path that starts and ends at the same vertex |
| **Simple Cycle** | A cycle with no repeated vertices (except start/end) |
| **Connected** | There exists a path between every pair of vertices (undirected) |
| **Strongly Connected** | Every vertex is reachable from every other vertex (directed) |
| **DAG** | Directed Acyclic Graph — directed with no cycles |
| **Sparse Graph** | $E \ll V^2$ — few edges |
| **Dense Graph** | $E \approx V^2$ — many edges |
| **Complete Graph** | Every pair of vertices is connected; $E = \frac{V(V-1)}{2}$ |
| **Bipartite Graph** | Vertices can be split into two sets; edges only go *between* sets |
| **Tree** | Connected, undirected graph with no cycles; $E = V - 1$ |
| **Spanning Tree** | A subgraph that is a tree and includes all vertices |
| **MST** | Spanning tree with minimum total edge weight |

---

## 3. Graph Representations

### 3.1 Adjacency Matrix

An $n \times n$ matrix. `matrix[i][j] = 1` (or weight) if edge $(i, j)$ exists.

```
Graph:  0—1, 0—2, 1—3, 2—3

    0  1  2  3
0 [ 0, 1, 1, 0 ]
1 [ 1, 0, 0, 1 ]
2 [ 1, 0, 0, 1 ]
3 [ 0, 1, 1, 0 ]
```

| Operation | Complexity |
|-----------|-----------|
| Space | $O(V^2)$ |
| Edge check `(u, v)` | $O(1)$ |
| All neighbors of `u` | $O(V)$ |
| Add edge | $O(1)$ |

**Best for:** Dense graphs or when fast edge lookup is needed.

---

### 3.2 Adjacency List

Each vertex stores a list of its neighbors (and optionally weights).

```
0: [1, 2]
1: [0, 3]
2: [0, 3]
3: [1, 2]
```

| Operation | Complexity |
|-----------|-----------|
| Space | $O(V + E)$ |
| Edge check `(u, v)` | $O(\deg(u))$ |
| All neighbors of `u` | $O(\deg(u))$ |
| Add edge | $O(1)$ |

**Best for:** Sparse graphs (most real-world graphs).

---

### 3.3 Edge List

A flat list of all edges — simplest form, used in Kruskal's algorithm.

```python
edges = [(0,1), (0,2), (1,3), (2,3)]
# weighted
edges = [(2, 0, 1), (3, 0, 2), (1, 1, 3), (4, 2, 3)]  # (weight, u, v)
```

| Operation | Complexity |
|-----------|-----------|
| Space | $O(E)$ |
| Edge check | $O(E)$ |
| All neighbors | $O(E)$ |

---

### 3.4 Comparison

| | Adjacency Matrix | Adjacency List | Edge List |
|--|---|---|---|
| Space | $O(V^2)$ | $O(V+E)$ | $O(E)$ |
| Edge check | $O(1)$ | $O(\deg)$ | $O(E)$ |
| Iterate neighbors | $O(V)$ | $O(\deg)$ | $O(E)$ |
| Best for | Dense, fast lookup | Sparse (default) | Sorting edges |

---

## 4. Graph Implementation in Python

### 4.1 Basic Graph Class

```python
class Graph:
    def __init__(self, directed=False):
        self.graph = {}          # adjacency list
        self.directed = directed

    def add_vertex(self, v):
        if v not in self.graph:
            self.graph[v] = []

    def add_edge(self, u, v, weight=None):
        self.add_vertex(u)
        self.add_vertex(v)
        entry = (v, weight) if weight is not None else v
        self.graph[u].append(entry)
        if not self.directed:
            entry2 = (u, weight) if weight is not None else u
            self.graph[v].append(entry2)

    def neighbors(self, v):
        return self.graph.get(v, [])

    def vertices(self):
        return list(self.graph.keys())

    def display(self):
        for v, neighbors in self.graph.items():
            print(f"  {v}: {neighbors}")

# Usage
g = Graph()
g.add_edge(0, 1)
g.add_edge(0, 2)
g.add_edge(1, 3)
g.add_edge(2, 3)
g.display()
```

### 4.2 Weighted Graph

```python
wg = Graph()
wg.add_edge('A', 'B', weight=2)
wg.add_edge('A', 'C', weight=4)
wg.add_edge('B', 'C', weight=1)
wg.add_edge('B', 'D', weight=5)
wg.display()
# A: [('B', 2), ('C', 4)]
# B: [('A', 2), ('C', 1), ('D', 5)]
# C: [('A', 4), ('B', 1)]
# D: [('B', 5)]
```

---

## 5. Graph Traversals — BFS & DFS

### 5.1 Breadth-First Search (BFS)

Explores level by level using a **queue**. Visits all vertices at distance $k$ before distance $k+1$.

```
      0
     / \
    1   2
    |   |
    3   4
```

BFS from 0: `0 → 1 → 2 → 3 → 4`

```python
from collections import deque

def bfs(graph, start):
    visited = set([start])
    queue   = deque([start])
    order   = []

    while queue:
        v = queue.popleft()
        order.append(v)
        for neighbor in graph[v]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    return order

graph = {0: [1, 2], 1: [0, 3], 2: [0, 4], 3: [1], 4: [2]}
print(bfs(graph, 0))  # [0, 1, 2, 3, 4]
```

**Time:** $O(V + E)$ | **Space:** $O(V)$

---

### 5.2 Depth-First Search (DFS)

Explores as deep as possible before backtracking. Uses a **stack** (or recursion).

DFS from 0: `0 → 1 → 3 → 2 → 4` (order depends on neighbor ordering)

**Recursive:**
```python
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    visited.add(start)
    print(start, end=' ')
    for neighbor in graph[start]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
```

**Iterative:**
```python
def dfs_iterative(graph, start):
    visited = set()
    stack   = [start]
    order   = []

    while stack:
        v = stack.pop()
        if v not in visited:
            visited.add(v)
            order.append(v)
            for neighbor in reversed(graph[v]):
                if neighbor not in visited:
                    stack.append(neighbor)
    return order
```

**Time:** $O(V + E)$ | **Space:** $O(V)$

---

### 5.3 BFS vs DFS

| Feature | BFS | DFS |
|---------|-----|-----|
| Data structure | Queue | Stack / Recursion |
| Traversal order | Level-by-level | Deep path first |
| Shortest path (unweighted) | ✅ Yes | ❌ No |
| Cycle detection | ✅ | ✅ |
| Topological sort | Kahn's algorithm | ✅ DFS-based |
| Space | $O(V)$ | $O(V)$ |
| Time | $O(V + E)$ | $O(V + E)$ |

---

## 6. Shortest Path Algorithms

### 6.1 BFS Shortest Path (Unweighted)

BFS naturally finds the shortest path in an **unweighted** graph.

```python
from collections import deque

def bfs_shortest_path(graph, start, end):
    visited = {start}
    queue   = deque([(start, [start])])

    while queue:
        v, path = queue.popleft()
        if v == end:
            return path
        for neighbor in graph[v]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, path + [neighbor]))
    return None  # no path

graph = {0: [1,2], 1: [0,3,4], 2: [0,4], 3:[1], 4:[1,2,5], 5:[4]}
print(bfs_shortest_path(graph, 0, 5))  # [0, 1, 4, 5]
```

---

### 6.2 Dijkstra's Algorithm (Weighted, Non-negative)

Uses a **min-heap** to always process the nearest unvisited vertex.  
**Does not work with negative weights.**

```python
import heapq

def dijkstra(graph, start):
    """graph: {node: [(neighbor, weight), ...]}"""
    dist   = {v: float('inf') for v in graph}
    dist[start] = 0
    parent = {start: None}
    pq     = [(0, start)]   # (distance, node)

    while pq:
        d, u = heapq.heappop(pq)
        if d > dist[u]:
            continue           # stale entry
        for v, w in graph[u]:
            if dist[u] + w < dist[v]:
                dist[v]   = dist[u] + w
                parent[v] = u
                heapq.heappush(pq, (dist[v], v))

    return dist, parent

def reconstruct_path(parent, start, end):
    path, node = [], end
    while node is not None:
        path.append(node)
        node = parent[node]
    return path[::-1] if path[-1] == start else []

graph = {
    'A': [('B', 1), ('C', 4)],
    'B': [('A', 1), ('C', 2), ('D', 5)],
    'C': [('A', 4), ('B', 2), ('D', 1)],
    'D': [('B', 5), ('C', 1)]
}
dist, parent = dijkstra(graph, 'A')
print(dist)                              # {'A':0, 'B':1, 'C':3, 'D':4}
print(reconstruct_path(parent, 'A','D')) # ['A', 'B', 'C', 'D']
```

**Time:** $O((V + E) \log V)$ | **Space:** $O(V)$

---

### 6.3 Bellman-Ford Algorithm (Negative Weights)

Relaxes all edges $V-1$ times. Can detect **negative weight cycles**.

```python
def bellman_ford(vertices, edges, start):
    """
    vertices: list of all nodes
    edges: list of (u, v, weight)
    """
    dist = {v: float('inf') for v in vertices}
    dist[start] = 0

    # Relax edges V-1 times
    for _ in range(len(vertices) - 1):
        for u, v, w in edges:
            if dist[u] != float('inf') and dist[u] + w < dist[v]:
                dist[v] = dist[u] + w

    # Check for negative cycles
    for u, v, w in edges:
        if dist[u] != float('inf') and dist[u] + w < dist[v]:
            return None  # Negative cycle detected

    return dist

vertices = ['A','B','C','D','E']
edges = [
    ('A','B', 6), ('A','D', 7),
    ('B','C', 5), ('B','D', 8), ('B','E',-4),
    ('D','E', 9), ('D','C',-3),
    ('E','C', 7), ('E','A', 2),
    ('C','B',-2)
]
print(bellman_ford(vertices, edges, 'A'))
# {'A': 0, 'B': 2, 'C': 4, 'D': 7, 'E': -2}
```

**Time:** $O(V \cdot E)$ | **Space:** $O(V)$

---

### 6.4 Floyd-Warshall (All-Pairs Shortest Path)

Finds shortest path between **every pair** of vertices. Works with negative weights (not negative cycles).

```python
def floyd_warshall(graph, vertices):
    """
    graph: dict-of-dict  graph[u][v] = weight
    """
    INF  = float('inf')
    n    = len(vertices)
    idx  = {v: i for i, v in enumerate(vertices)}
    dist = [[INF]*n for _ in range(n)]

    for i in range(n):
        dist[i][i] = 0

    for u in graph:
        for v, w in graph[u].items():
            dist[idx[u]][idx[v]] = w

    for k in range(n):                # intermediate vertex
        for i in range(n):
            for j in range(n):
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]

    return dist

vertices = ['A','B','C','D']
graph = {
    'A': {'B': 3,  'C': 8, 'D': -4},
    'B': {'D': 7,  'E': 1},
    'C': {'B': 4},
    'D': {'C': 2}
}
```

**Time:** $O(V^3)$ | **Space:** $O(V^2)$

---

### 6.5 Shortest Path Comparison

| Algorithm | Works with | Time | Negative weights | Negative cycles |
|-----------|-----------|------|-----------------|-----------------|
| BFS | Unweighted | $O(V+E)$ | N/A | N/A |
| Dijkstra's | Weighted | $O((V+E)\log V)$ | ❌ | ❌ |
| Bellman-Ford | Weighted | $O(V \cdot E)$ | ✅ | Detects |
| Floyd-Warshall | All pairs | $O(V^3)$ | ✅ | Detects |

---

## 7. Cycle Detection

### 7.1 Undirected Graph — BFS / DFS

```python
def has_cycle_undirected(graph):
    visited = set()

    def dfs(node, parent):
        visited.add(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                if dfs(neighbor, node):
                    return True
            elif neighbor != parent:   # back edge found
                return True
        return False

    for node in graph:
        if node not in visited:
            if dfs(node, -1):
                return True
    return False
```

---

### 7.2 Directed Graph — DFS with Color Marking

Color states: **White** (unvisited) → **Gray** (in current DFS path) → **Black** (done).  
A back edge to a **Gray** node = cycle.

```python
def has_cycle_directed(graph):
    WHITE, GRAY, BLACK = 0, 1, 2
    color = {v: WHITE for v in graph}

    def dfs(node):
        color[node] = GRAY
        for neighbor in graph[node]:
            if color[neighbor] == GRAY:
                return True        # back edge → cycle
            if color[neighbor] == WHITE:
                if dfs(neighbor):
                    return True
        color[node] = BLACK
        return False

    for node in graph:
        if color[node] == WHITE:
            if dfs(node):
                return True
    return False

graph = {0: [1], 1: [2], 2: [0], 3: [4], 4: []}  # cycle: 0→1→2→0
print(has_cycle_directed(graph))  # True
```

---

## 8. Topological Sort

A **topological ordering** of a DAG lists vertices such that for every edge $u \to v$, $u$ comes before $v$.  
Only valid for **DAGs** (directed acyclic graphs).

### 8.1 Kahn's Algorithm (BFS-based)

```python
from collections import deque

def topological_sort_kahn(graph):
    in_degree = {v: 0 for v in graph}
    for v in graph:
        for neighbor in graph[v]:
            in_degree[neighbor] += 1

    queue = deque([v for v in in_degree if in_degree[v] == 0])
    order = []

    while queue:
        v = queue.popleft()
        order.append(v)
        for neighbor in graph[v]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    if len(order) != len(graph):
        return None  # cycle detected
    return order

graph = {0:[1,2], 1:[3], 2:[3], 3:[4], 4:[]}
print(topological_sort_kahn(graph))  # [0, 1, 2, 3, 4]  (or [0, 2, 1, 3, 4])
```

### 8.2 DFS-based Topological Sort

```python
def topological_sort_dfs(graph):
    visited = set()
    stack   = []

    def dfs(v):
        visited.add(v)
        for neighbor in graph[v]:
            if neighbor not in visited:
                dfs(neighbor)
        stack.append(v)   # push after all descendants are processed

    for v in graph:
        if v not in visited:
            dfs(v)

    return stack[::-1]

print(topological_sort_dfs(graph))  # [0, 2, 1, 3, 4]
```

**Time:** $O(V + E)$ | **Space:** $O(V)$

---

## 9. Minimum Spanning Tree (Prim's & Kruskal's)

A **Minimum Spanning Tree (MST)** of a connected, undirected, weighted graph is a spanning tree with the **minimum total edge weight**.

Properties:
- Has exactly $V - 1$ edges
- No cycles
- Connects all vertices

---

### 9.1 Prim's Algorithm

Grows one tree from a start vertex by repeatedly adding the **cheapest edge** that connects a new vertex to the current tree.

**Uses:** Min-heap (greedy on cut edges)

```python
import heapq

def prim(graph, start):
    """graph: {node: [(neighbor, weight), ...]}"""
    visited     = set([start])
    mst_edges   = []
    total_weight = 0
    # (weight, to_node, from_node)
    min_heap = [(w, v, start) for v, w in graph[start]]
    heapq.heapify(min_heap)

    while min_heap and len(visited) < len(graph):
        w, v, u = heapq.heappop(min_heap)
        if v in visited:
            continue
        visited.add(v)
        mst_edges.append((u, v, w))
        total_weight += w
        for neighbor, weight in graph[v]:
            if neighbor not in visited:
                heapq.heappush(min_heap, (weight, neighbor, v))

    return mst_edges, total_weight

graph = {
    'A': [('B', 2), ('C', 3)],
    'B': [('A', 2), ('C', 1), ('D', 4)],
    'C': [('A', 3), ('B', 1), ('D', 5)],
    'D': [('B', 4), ('C', 5)]
}
edges, weight = prim(graph, 'A')
print(edges)   # [('A','B',2), ('B','C',1), ('B','D',4)]
print(weight)  # 7
```

**Trace:**

| Step | Edge Added | Total |
|------|-----------|-------|
| 1 | A — B (2) | 2 |
| 2 | B — C (1) | 3 |
| 3 | B — D (4) | 7 |

**Time:** $O((V + E) \log V)$ | Best for dense graphs

---

### 9.2 Kruskal's Algorithm

Sorts all edges by weight and adds them one by one, skipping edges that would create a cycle. Uses **Union-Find** for efficient cycle detection.

```python
class UnionFind:
    def __init__(self, nodes):
        self.parent = {n: n for n in nodes}
        self.rank   = {n: 0 for n in nodes}

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # path compression
        return self.parent[x]

    def union(self, x, y):
        rx, ry = self.find(x), self.find(y)
        if rx == ry:
            return False           # same component → cycle
        if self.rank[rx] < self.rank[ry]:
            rx, ry = ry, rx
        self.parent[ry] = rx
        if self.rank[rx] == self.rank[ry]:
            self.rank[rx] += 1
        return True


def kruskal(nodes, edges):
    """
    nodes: list of vertices
    edges: list of (weight, u, v)
    """
    edges.sort()
    uf           = UnionFind(nodes)
    mst_edges    = []
    total_weight = 0

    for w, u, v in edges:
        if uf.union(u, v):
            mst_edges.append((u, v, w))
            total_weight += w
            if len(mst_edges) == len(nodes) - 1:
                break   # MST complete

    return mst_edges, total_weight

nodes = ['A','B','C','D']
edges = [(2,'A','B'), (3,'A','C'), (1,'B','C'), (4,'B','D'), (5,'C','D')]
mst, weight = kruskal(nodes, edges)
print(mst)    # [('B','C',1), ('A','B',2), ('B','D',4)]
print(weight) # 7
```

**Trace:**

| Step | Edge | Action | Reason |
|------|------|--------|--------|
| 1 | B—C (1) | Add | Different components |
| 2 | A—B (2) | Add | Different components |
| 3 | A—C (3) | Skip | A & C already connected |
| 4 | B—D (4) | Add | Different components — done |

**Time:** $O(E \log E)$ | Best for sparse graphs

---

### 9.3 Prim's vs Kruskal's

| Feature | Prim's | Kruskal's |
|---------|--------|-----------|
| Approach | Grow tree from one vertex | Merge components via sorted edges |
| Data structure | Min-heap | Sorted edge list + Union-Find |
| Time | $O((V+E)\log V)$ | $O(E \log E)$ |
| Best for | Dense graphs | Sparse graphs |
| Disconnected graphs | No | Yes (spanning forest) |
| Cycle detection | Visited set | Union-Find |

---

## 10. Union-Find (Disjoint Set Union)

A data structure that efficiently tracks a set of elements partitioned into non-overlapping subsets.

**Operations:**
- `find(x)` — which component does `x` belong to?
- `union(x, y)` — merge the components of `x` and `y`

**Optimisations:**
- **Path Compression** — flatten the tree during `find`
- **Union by Rank** — attach the smaller tree under the larger

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank   = [0] * n
        self.count  = n         # number of connected components

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # path compression
        return self.parent[x]

    def union(self, x, y):
        rx, ry = self.find(x), self.find(y)
        if rx == ry:
            return False
        if self.rank[rx] < self.rank[ry]:
            rx, ry = ry, rx
        self.parent[ry] = rx
        if self.rank[rx] == self.rank[ry]:
            self.rank[rx] += 1
        self.count -= 1
        return True

    def connected(self, x, y):
        return self.find(x) == self.find(y)

uf = UnionFind(5)
uf.union(0, 1)
uf.union(1, 2)
print(uf.connected(0, 2))  # True
print(uf.connected(0, 3))  # False
print(uf.count)             # 3
```

**Time per operation:** near $O(1)$ amortised (inverse Ackermann $\alpha(n)$)

---

## 11. Strongly Connected Components

An **SCC** is a maximal set of vertices such that every vertex is reachable from every other vertex (directed graph).

### 11.1 Kosaraju's Algorithm

1. Run DFS on the original graph; push vertices to a stack in finish order.
2. Transpose (reverse) the graph.
3. Pop vertices from the stack; run DFS on the transposed graph — each DFS tree is one SCC.

```python
def kosaraju(graph):
    visited = set()
    finish_stack = []

    def dfs1(v):
        visited.add(v)
        for w in graph.get(v, []):
            if w not in visited:
                dfs1(w)
        finish_stack.append(v)

    for v in graph:
        if v not in visited:
            dfs1(v)

    # Build transposed graph
    transposed = {v: [] for v in graph}
    for v in graph:
        for w in graph[v]:
            transposed[w].append(v)

    visited.clear()
    sccs = []

    def dfs2(v, component):
        visited.add(v)
        component.append(v)
        for w in transposed[v]:
            if w not in visited:
                dfs2(w, component)

    while finish_stack:
        v = finish_stack.pop()
        if v not in visited:
            component = []
            dfs2(v, component)
            sccs.append(component)

    return sccs

graph = {0:[1], 1:[2], 2:[0,3], 3:[4], 4:[3]}
print(kosaraju(graph))  # [[0,1,2], [3,4]] (order may vary)
```

**Time:** $O(V + E)$

---

## 12. Floyd-Warshall (All-Pairs Shortest Path)

Finds the shortest path between **every pair** of vertices. Works with negative edge weights (but not negative cycles).

```python
def floyd_warshall(n, edges):
    """
    n: number of vertices (0-indexed)
    edges: list of (u, v, weight)
    Returns: dist matrix
    """
    INF  = float('inf')
    dist = [[INF]*n for _ in range(n)]
    for i in range(n):
        dist[i][i] = 0
    for u, v, w in edges:
        dist[u][v] = w           # directed; add dist[v][u]=w for undirected

    for k in range(n):
        for i in range(n):
            for j in range(n):
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]

    # Detect negative cycle: any dist[i][i] < 0
    for i in range(n):
        if dist[i][i] < 0:
            return None  # negative cycle

    return dist

# 4 vertices, directed weighted edges
edges = [(0,1,3),(0,3,7),(1,0,8),(1,2,2),(2,0,5),(2,3,1),(3,0,2)]
result = floyd_warshall(4, edges)
for row in result:
    print(row)
```

**Time:** $O(V^3)$ | **Space:** $O(V^2)$

---

## 13. Bellman-Ford Algorithm

Finds shortest paths from a **single source**, allowing **negative edge weights**.  
Detects negative weight cycles.

```python
def bellman_ford(n, edges, src):
    """
    n    : number of vertices (0-indexed)
    edges: list of (u, v, weight)
    src  : source vertex
    """
    dist = [float('inf')] * n
    dist[src] = 0

    for _ in range(n - 1):           # relax all edges n-1 times
        updated = False
        for u, v, w in edges:
            if dist[u] != float('inf') and dist[u] + w < dist[v]:
                dist[v]  = dist[u] + w
                updated  = True
        if not updated:
            break                    # early termination

    # Detect negative cycle
    for u, v, w in edges:
        if dist[u] != float('inf') and dist[u] + w < dist[v]:
            return None              # negative cycle exists

    return dist

edges = [(0,1,6),(0,3,7),(1,2,5),(1,3,8),(1,4,-4),
         (2,1,-2),(3,2,-3),(3,4,9),(4,0,2),(4,2,7)]
print(bellman_ford(5, edges, 0))  # [0, 2, 4, 7, -2]
```

**Time:** $O(V \cdot E)$ | **Space:** $O(V)$

---

## 14. Bridges & Articulation Points

### 14.1 Bridge

An edge whose **removal increases the number of connected components** (disconnects the graph).

### 14.2 Articulation Point (Cut Vertex)

A vertex whose **removal increases the number of connected components**.

Both use DFS + discovery timestamps + `low` values.

```python
def find_bridges_and_articulations(graph):
    n         = len(graph)
    visited   = {}
    disc      = {}    # discovery time
    low       = {}    # lowest discovery reachable
    parent    = {}
    timer     = [0]
    bridges   = []
    art_points = set()

    def dfs(u):
        visited[u]  = True
        disc[u] = low[u] = timer[0]
        timer[0] += 1
        child_count = 0

        for v in graph[u]:
            if not visited.get(v):
                child_count += 1
                parent[v] = u
                dfs(v)
                low[u] = min(low[u], low[v])

                # Bridge condition
                if low[v] > disc[u]:
                    bridges.append((u, v))

                # Articulation point condition
                if parent.get(u) is None and child_count > 1:
                    art_points.add(u)
                if parent.get(u) is not None and low[v] >= disc[u]:
                    art_points.add(u)

            elif v != parent.get(u):
                low[u] = min(low[u], disc[v])

    for node in graph:
        if not visited.get(node):
            parent[node] = None
            dfs(node)

    return bridges, art_points

graph = {0:[1,2], 1:[0,2,3], 2:[0,1], 3:[1,4], 4:[3]}
bridges, aps = find_bridges_and_articulations(graph)
print("Bridges:", bridges)       # [(1,3), (3,4)]
print("Art. Points:", aps)       # {1, 3}
```

**Time:** $O(V + E)$

---

## 15. Bipartite Graph Check

A graph is **bipartite** if its vertices can be coloured with 2 colours such that no two adjacent vertices share the same colour. Equivalently, a graph is bipartite if and only if it has **no odd-length cycles**.

```python
from collections import deque

def is_bipartite(graph):
    color = {}

    for start in graph:
        if start in color:
            continue
        color[start] = 0
        queue = deque([start])

        while queue:
            v = queue.popleft()
            for neighbor in graph[v]:
                if neighbor not in color:
                    color[neighbor] = 1 - color[v]   # flip color
                    queue.append(neighbor)
                elif color[neighbor] == color[v]:
                    return False                       # same color → odd cycle
    return True

graph1 = {0:[1,3], 1:[0,2], 2:[1,3], 3:[0,2]}   # cycle of length 4 → bipartite
graph2 = {0:[1,2,3], 1:[0,2], 2:[0,1,3], 3:[0,2]}  # triangle → not bipartite

print(is_bipartite(graph1))  # True
print(is_bipartite(graph2))  # False
```

**Time:** $O(V + E)$

---

## 16. Complexity Summary

| Algorithm | Time | Space | Graph Type |
|-----------|------|-------|------------|
| BFS | $O(V+E)$ | $O(V)$ | Unweighted |
| DFS | $O(V+E)$ | $O(V)$ | Any |
| BFS Shortest Path | $O(V+E)$ | $O(V)$ | Unweighted |
| Dijkstra's | $O((V+E)\log V)$ | $O(V)$ | Weighted, non-negative |
| Bellman-Ford | $O(V \cdot E)$ | $O(V)$ | Weighted (neg. ok) |
| Floyd-Warshall | $O(V^3)$ | $O(V^2)$ | All-pairs |
| Topological Sort | $O(V+E)$ | $O(V)$ | DAG |
| Prim's MST | $O((V+E)\log V)$ | $O(V)$ | Undirected, weighted |
| Kruskal's MST | $O(E \log E)$ | $O(V+E)$ | Undirected, weighted |
| Union-Find ops | $O(\alpha(n)) \approx O(1)$ | $O(V)$ | Any |
| Kosaraju's SCC | $O(V+E)$ | $O(V)$ | Directed |
| Bridges/Cut Vertices | $O(V+E)$ | $O(V)$ | Undirected |
| Bipartite Check | $O(V+E)$ | $O(V)$ | Any |

---

*End of Graph Algorithms notes*
