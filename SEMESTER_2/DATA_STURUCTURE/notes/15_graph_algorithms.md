# Topic 15: Graph Algorithms

## 1. Overview

A **Graph** $G = (V, E)$ consists of a set of **vertices** (nodes) $V$ and a set of **edges** (connections) $E$. Graphs model relationships between objects — social networks, maps, web pages, dependencies, etc.

---

## 2. Key Concepts

### 2.1 Graph Terminology

| Term | Definition |
|------|------------|
| **Vertex (Node)** | A fundamental unit |
| **Edge** | Connection between two vertices |
| **Directed graph (Digraph)** | Edges have direction (A → B) |
| **Undirected graph** | Edges have no direction (A — B) |
| **Weighted graph** | Edges have weights/costs |
| **Degree** | Number of edges connected to a vertex |
| **In-degree / Out-degree** | For directed graphs: incoming / outgoing edges |
| **Path** | Sequence of vertices connected by edges |
| **Cycle** | A path that starts and ends at the same vertex |
| **Connected** | Every pair of vertices has a path between them |
| **DAG** | Directed Acyclic Graph (no cycles) |

### 2.2 Graph Representations

#### Adjacency Matrix
$n \times n$ matrix where `matrix[i][j] = 1` if edge exists (or weight).

| | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
| **0** | 0 | 1 | 1 | 0 |
| **1** | 1 | 0 | 0 | 1 |
| **2** | 1 | 0 | 0 | 1 |
| **3** | 0 | 1 | 1 | 0 |

**Space:** $O(V^2)$ | **Edge check:** $O(1)$ | **Find neighbors:** $O(V)$

#### Adjacency List
Array of lists — each vertex stores its list of neighbors.

```
0: [1, 2]
1: [0, 3]
2: [0, 3]
3: [1, 2]
```

**Space:** $O(V + E)$ | **Edge check:** $O(\text{degree})$ | **Find neighbors:** $O(\text{degree})$

### 2.3 BFS vs DFS

| Feature | BFS | DFS |
|---------|-----|-----|
| Data structure | Queue | Stack (or recursion) |
| Order | Level by level | Depth first |
| Shortest path (unweighted) | Yes | No |
| Space | $O(V)$ | $O(V)$ |
| Time | $O(V + E)$ | $O(V + E)$ |
| Use cases | Shortest path, level-order | Cycle detection, topological sort, connected components |

---

## 3. Code Examples

### 3.1 Graph Using Adjacency List

```python
class Graph:
    def __init__(self):
        self.graph = {}

    def add_vertex(self, v):
        if v not in self.graph:
            self.graph[v] = []

    def add_edge(self, u, v, directed=False):
        self.add_vertex(u)
        self.add_vertex(v)
        self.graph[u].append(v)
        if not directed:
            self.graph[v].append(u)

    def display(self):
        for vertex in self.graph:
            print(f"{vertex}: {self.graph[vertex]}")

g = Graph()
g.add_edge(0, 1)
g.add_edge(0, 2)
g.add_edge(1, 3)
g.add_edge(2, 3)
g.display()
# 0: [1, 2]
# 1: [0, 3]
# 2: [0, 3]
# 3: [1, 2]
```

### 3.2 Breadth-First Search (BFS)

```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)
    order = []

    while queue:
        vertex = queue.popleft()
        order.append(vertex)
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    return order

graph = {0: [1, 2], 1: [0, 3], 2: [0, 3], 3: [1, 2]}
print(bfs(graph, 0))  # [0, 1, 2, 3]
```

### 3.3 Depth-First Search (DFS)

**Recursive:**
```python
def dfs_recursive(graph, vertex, visited=None):
    if visited is None:
        visited = set()
    visited.add(vertex)
    print(vertex, end=" ")
    for neighbor in graph[vertex]:
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited)

dfs_recursive(graph, 0)  # 0 1 3 2
```

**Iterative (using stack):**
```python
def dfs_iterative(graph, start):
    visited = set()
    stack = [start]
    order = []

    while stack:
        vertex = stack.pop()
        if vertex not in visited:
            visited.add(vertex)
            order.append(vertex)
            for neighbor in reversed(graph[vertex]):
                if neighbor not in visited:
                    stack.append(neighbor)
    return order

print(dfs_iterative(graph, 0))  # [0, 1, 3, 2]
```

### 3.4 Shortest Path (BFS — Unweighted)

```python
from collections import deque

def bfs_shortest_path(graph, start, end):
    visited = {start}
    queue = deque([(start, [start])])

    while queue:
        vertex, path = queue.popleft()
        if vertex == end:
            return path
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, path + [neighbor]))
    return None  # No path found

graph = {0: [1, 2], 1: [0, 3, 4], 2: [0, 4], 3: [1], 4: [1, 2, 5], 5: [4]}
print(bfs_shortest_path(graph, 0, 5))  # [0, 1, 4, 5] or [0, 2, 4, 5]
```

### 3.5 Dijkstra's Algorithm (Weighted Shortest Path)

```python
import heapq

def dijkstra(graph, start):
    """graph: {node: [(neighbor, weight), ...]}"""
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    pq = [(0, start)]  # (distance, node)
    parent = {start: None}

    while pq:
        current_dist, current = heapq.heappop(pq)
        if current_dist > distances[current]:
            continue
        for neighbor, weight in graph[current]:
            distance = current_dist + weight
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                parent[neighbor] = current
                heapq.heappush(pq, (distance, neighbor))

    return distances, parent

# Weighted graph
wgraph = {
    'A': [('B', 1), ('C', 4)],
    'B': [('A', 1), ('C', 2), ('D', 5)],
    'C': [('A', 4), ('B', 2), ('D', 1)],
    'D': [('B', 5), ('C', 1)]
}
distances, parent = dijkstra(wgraph, 'A')
print(distances)  # {'A': 0, 'B': 1, 'C': 3, 'D': 4}
```

### 3.6 Topological Sort (DAG)

```python
from collections import deque

def topological_sort(graph, in_degree):
    """Kahn's algorithm using BFS."""
    queue = deque([v for v in graph if in_degree[v] == 0])
    order = []

    while queue:
        vertex = queue.popleft()
        order.append(vertex)
        for neighbor in graph[vertex]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    if len(order) != len(graph):
        return None  # Cycle detected
    return order

# DAG
graph = {0: [1, 2], 1: [3], 2: [3], 3: [4], 4: []}
in_degree = {0: 0, 1: 1, 2: 1, 3: 2, 4: 1}
print(topological_sort(graph, in_degree))  # [0, 1, 2, 3, 4]
```

### 3.7 Detect Cycle (Undirected — DFS)

```python
def has_cycle(graph):
    visited = set()

    def dfs(node, parent):
        visited.add(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                if dfs(neighbor, node):
                    return True
            elif neighbor != parent:
                return True
        return False

    for node in graph:
        if node not in visited:
            if dfs(node, -1):
                return True
    return False
```

---

## 4. MCQs (15 Questions)

**Q1.** A graph with directed edges is called:
- A) Weighted graph
- B) Digraph
- C) Complete graph
- D) Bipartite graph

**Answer:** B) Digraph

---

**Q2.** BFS uses which data structure?
- A) Stack
- B) Queue
- C) Priority queue
- D) Array

**Answer:** B) Queue

---

**Q3.** DFS uses which data structure?
- A) Queue
- B) Priority queue
- C) Stack (or recursion)
- D) Hash table

**Answer:** C) Stack (or recursion)

---

**Q4.** Time complexity of BFS and DFS:
- A) $O(V)$
- B) $O(E)$
- C) $O(V + E)$
- D) $O(V \times E)$

**Answer:** C) $O(V + E)$

---

**Q5.** Which algorithm finds the shortest path in an unweighted graph?
- A) DFS
- B) BFS
- C) Dijkstra's
- D) Floyd-Warshall

**Answer:** B) BFS

---

**Q6.** Dijkstra's algorithm does NOT work with:
- A) Directed graphs
- B) Weighted graphs
- C) Graphs with negative edge weights
- D) Undirected graphs

**Answer:** C) Graphs with negative edge weights

---

**Q7.** Space complexity of an adjacency matrix for $V$ vertices:
- A) $O(V)$
- B) $O(V + E)$
- C) $O(V^2)$
- D) $O(E)$

**Answer:** C) $O(V^2)$

---

**Q8.** Topological sort is possible only for:
- A) Any graph
- B) Directed Acyclic Graphs (DAGs)
- C) Undirected graphs
- D) Complete graphs

**Answer:** B) DAGs

---

**Q9.** An adjacency list is more space-efficient than an adjacency matrix when:
- A) The graph is dense
- B) The graph is sparse
- C) The graph is complete
- D) Always

**Answer:** B) The graph is sparse — $O(V + E)$ vs $O(V^2)$.

---

**Q10.** A connected undirected graph with $V$ vertices has at least:
- A) $V$ edges
- B) $V - 1$ edges
- C) $V + 1$ edges
- D) $2V$ edges

**Answer:** B) $V - 1$ edges (a spanning tree)

---

**Q11.** Dijkstra's algorithm time complexity with a binary heap:
- A) $O(V^2)$
- B) $O((V + E) \log V)$
- C) $O(V \cdot E)$
- D) $O(E^2)$

**Answer:** B) $O((V + E) \log V)$

---

**Q12.** Which can detect a cycle in a directed graph?
- A) BFS only
- B) DFS with coloring (white/gray/black)
- C) Dijkstra's algorithm
- D) Topological sort only

**Answer:** B) DFS with coloring — A back edge to a gray node indicates a cycle.

---

**Q13.** The degree of a vertex in an undirected graph is:
- A) The number of vertices
- B) The number of edges connected to it
- C) The number of paths from it
- D) The weight of its edges

**Answer:** B) The number of edges connected to it

---

**Q14.** In a DAG with 5 vertices, which is a valid topological ordering?
```
Edges: A→B, A→C, B→D, C→D, D→E
```
- A) A, B, C, D, E
- B) E, D, C, B, A
- C) A, C, B, D, E
- D) Both A and C

**Answer:** D) Both A and C — Multiple valid orderings exist.

---

**Q15.** BFS starting from vertex 0 on graph `{0: [1,2], 1: [3], 2: [3], 3: []}` visits in order:
- A) 0, 1, 3, 2
- B) 0, 1, 2, 3
- C) 0, 2, 1, 3
- D) 3, 1, 2, 0

**Answer:** B) 0, 1, 2, 3
