# Graph Algorithms Template

## Table of Contents
1. [Graph Representation](#graph-representation)
   - [Adjacency List](#adjacency-list)
   - [Adjacency Matrix](#adjacency-matrix)
   - [Edge List](#edge-list)
2. [Graph Traversals](#graph-traversals)
   - [Breadth-First Search (BFS)](#breadth-first-search-bfs)
   - [Depth-First Search (DFS)](#depth-first-search-dfs)
3. [Shortest Path Algorithms](#shortest-path-algorithms)
   - [Dijkstra's Algorithm](#dijkstras-algorithm)
   - [Bellman-Ford Algorithm](#bellman-ford-algorithm)
   - [Floyd-Warshall Algorithm](#floyd-warshall-algorithm)
4. [Minimum Spanning Tree (MST)](#minimum-spanning-tree-mst)
   - [Kruskal's Algorithm](#kruskals-algorithm)
   - [Prim's Algorithm](#prims-algorithm)
5. [Topological Sorting](#topological-sorting)
6. [Union-Find (Disjoint Set)](#union-find-disjoint-set)
7. [Complexity Analysis](#complexity-analysis)

---

## Graph Representation

### Adjacency List
```java
// For unweighted graph
List<List<Integer>> graph = new ArrayList<>();
int n = 5; // number of nodes
for (int i = 0; i < n; i++) {
    graph.add(new ArrayList<>());
}
// Add edge a -> b
graph.get(a).add(b);

// For weighted graph
class Edge {
    int to;
    int weight;
    Edge(int t, int w) { to = t; weight = w; }
}
List<List<Edge>> weightedGraph = new ArrayList<>();
```

### Adjacency Matrix
```java
// For unweighted graph
int[][] graph = new int[n][n];
// Add edge a -> b
graph[a][b] = 1;

// For weighted graph
int[][] weightedGraph = new int[n][n];
// Initialize with infinity
for (int[] row : weightedGraph) Arrays.fill(row, Integer.MAX_VALUE);
// Add edge a -> b with weight w
weightedGraph[a][b] = w;
```

### Edge List
```java
// For unweighted graph
List<int[]> edges = new ArrayList<>();
// Add edge a -> b
edges.add(new int[]{a, b});

// For weighted graph
class Edge {
    int from, to, weight;
    Edge(int f, int t, int w) { from = f; to = t; weight = w; }
}
List<Edge> weightedEdges = new ArrayList<>();
```

---

## Graph Traversals

### Breadth-First Search (BFS)

#### Iterative BFS
```java
public void bfsIterative(List<List<Integer>> graph, int start) {
    boolean[] visited = new boolean[graph.size()];
    Queue<Integer> queue = new LinkedList<>();
    queue.offer(start);
    visited[start] = true;

    while (!queue.isEmpty()) {
        int node = queue.poll();
        System.out.print(node + " ");
        
        for (int neighbor : graph.get(node)) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                queue.offer(neighbor);
            }
        }
    }
}
```

#### Recursive BFS
```java
public void bfsRecursive(Queue<Integer> queue, boolean[] visited, List<List<Integer>> graph) {
    if (queue.isEmpty()) return;
    
    int node = queue.poll();
    System.out.print(node + " ");
    
    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) {
            visited[neighbor] = true;
            queue.offer(neighbor);
        }
    }
    
    bfsRecursive(queue, visited, graph);
}

// Wrapper method
public void bfsRecursiveStart(List<List<Integer>> graph, int start) {
    boolean[] visited = new boolean[graph.size()];
    Queue<Integer> queue = new LinkedList<>();
    queue.offer(start);
    visited[start] = true;
    bfsRecursive(queue, visited, graph);
}
```

### Generic Graph Templates

#### Adjacency List Representation
```java
import java.util.*;

class Graph {
    private int V; // Number of vertices
    private List<List<Integer>> adj; // Adjacency list

    // Constructor
    public Graph(int v) {
        V = v;
        adj = new ArrayList<>();
        for (int i = 0; i < v; i++) {
            adj.add(new LinkedList<>());
        }
    }

    // Add edge to the graph
    public void addEdge(int v, int w) {
        adj.get(v).add(w);
        // For undirected graph, add the reverse edge as well
        // adj.get(w).add(v);
    }

    // BFS traversal from a given source node
    public void bfs(int s) {
        boolean[] visited = new boolean[V];
        Queue<Integer> queue = new LinkedList<>();
        visited[s] = true;
        queue.offer(s);

        while (!queue.isEmpty()) {
            int node = queue.poll();
            System.out.print(node + " ");

            for (int neighbor : adj.get(node)) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.offer(neighbor);
                }
            }
        }
    }

    // DFS traversal from a given source node
    public void dfs(int v, boolean[] visited) {
        visited[v] = true;
        System.out.print(v + " ");

        for (int neighbor : adj.get(v)) {
            if (!visited[neighbor]) {
                dfs(neighbor, visited);
            }
        }
    }
}
```

#### Adjacency Matrix Representation
```java
import java.util.*;

class GraphMatrix {
    private int V; // Number of vertices
    private int[][] adjMatrix; // Adjacency matrix

    // Constructor
    public GraphMatrix(int v) {
        V = v;
        adjMatrix = new int[V][V];
    }

    // Add edge to the graph
    public void addEdge(int v, int w, int weight) {
        adjMatrix[v][w] = weight;
        // For undirected graph, add the reverse edge as well
        // adjMatrix[w][v] = weight;
    }

    // BFS traversal from a given source node
    public void bfs(int s) {
        boolean[] visited = new boolean[V];
        Queue<Integer> queue = new LinkedList<>();
        visited[s] = true;
        queue.offer(s);

        while (!queue.isEmpty()) {
            int node = queue.poll();
            System.out.print(node + " ");

            for (int i = 0; i < V; i++) {
                if (adjMatrix[node][i] != 0 && !visited[i]) {
                    visited[i] = true;
                    queue.offer(i);
                }
            }
        }
    }

    // DFS traversal from a given source node
    public void dfs(int v, boolean[] visited) {
        visited[v] = true;
        System.out.print(v + " ");

        for (int i = 0; i < V; i++) {
            if (adjMatrix[v][i] != 0 && !visited[i]) {
                dfs(i, visited);
            }
        }
    }
}
```

### Depth-First Search (DFS)
```java
// Recursive DFS
public void dfsRecursive(List<List<Integer>> graph, int node, boolean[] visited) {
    visited[node] = true;
    System.out.print(node + " ");
    
    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) {
            dfsRecursive(graph, neighbor, visited);
        }
    }
}

// Iterative DFS
public void dfsIterative(List<List<Integer>> graph, int start) {
    boolean[] visited = new boolean[graph.size()];
    Stack<Integer> stack = new Stack<>();
    stack.push(start);
    
    while (!stack.isEmpty()) {
        int node = stack.pop();
        if (!visited[node]) {
            visited[node] = true;
            System.out.print(node + " ");
            
            // Push in reverse order to process left-to-right
            for (int i = graph.get(node).size() - 1; i >= 0; i--) {
                int neighbor = graph.get(node).get(i);
                if (!visited[neighbor]) {
                    stack.push(neighbor);
                }
            }
        }
    }
}
```

---

## Shortest Path Algorithms

### Dijkstra's Algorithm
```java
public int[] dijkstra(List<List<int[]>> graph, int start) {
    int n = graph.size();
    int[] dist = new int[n];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[start] = 0;
    
    // [distance, node]
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
    pq.offer(new int[]{0, start});
    
    while (!pq.isEmpty()) {
        int[] curr = pq.poll();
        int d = curr[0], u = curr[1];
        
        if (d > dist[u]) continue;
        
        for (int[] edge : graph.get(u)) {
            int v = edge[0], w = edge[1];
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.offer(new int[]{dist[v], v});
            }
        }
    }
    
    return dist;
}
```

### Bellman-Ford Algorithm
```java
public int[] bellmanFord(int n, int[][] edges, int start) {
    int[] dist = new int[n];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[start] = 0;
    
    // Relax all edges V-1 times
    for (int i = 0; i < n - 1; i++) {
        for (int[] edge : edges) {
            int u = edge[0], v = edge[1], w = edge[2];
            if (dist[u] != Integer.MAX_VALUE && dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
            }
        }
    }
    
    // Check for negative weight cycles
    for (int[] edge : edges) {
        int u = edge[0], v = edge[1], w = edge[2];
        if (dist[u] != Integer.MAX_VALUE && dist[u] + w < dist[v]) {
            System.out.println("Graph contains negative weight cycle");
            return null;
        }
    }
    
    return dist;
}
```

### Floyd-Warshall Algorithm
```java
public int[][] floydWarshall(int[][] graph) {
    int n = graph.length;
    int[][] dist = new int[n][n];
    
    // Initialize distance matrix
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (i == j) dist[i][j] = 0;
            else if (graph[i][j] != 0) dist[i][j] = graph[i][j];
            else dist[i][j] = Integer.MAX_VALUE / 2; // Avoid overflow
        }
    }
    
    // Floyd-Warshall
    for (int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (dist[i][j] > dist[i][k] + dist[k][j]) {
                    dist[i][j] = dist[i][k] + dist[k][j];
                }
            }
        }
    }
    
    return dist;
}
```

---

## Minimum Spanning Tree (MST)

### Kruskal's Algorithm
```java
class UnionFind {
    int[] parent, rank;
    
    public UnionFind(int n) {
        parent = new int[n];
        rank = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;
    }
    
    public int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }
    
    public boolean union(int x, int y) {
        int xr = find(x), yr = find(y);
        if (xr == yr) return false;
        if (rank[xr] < rank[yr]) {
            parent[xr] = yr;
        } else if (rank[xr] > rank[yr]) {
            parent[yr] = xr;
        } else {
            parent[yr] = xr;
            rank[xr]++;
        }
        return true;
    }
}

public int kruskalMST(int n, int[][] edges) {
    // Sort edges by weight
    Arrays.sort(edges, (a, b) -> a[2] - b[2]);
    
    UnionFind uf = new UnionFind(n);
    int mstWeight = 0, edgesUsed = 0;
    
    for (int[] edge : edges) {
        int u = edge[0], v = edge[1], w = edge[2];
        if (uf.union(u, v)) {
            mstWeight += w;
            edgesUsed++;
            if (edgesUsed == n - 1) break;
        }
    }
    
    return mstWeight;
}
```

### Prim's Algorithm
```java
public int primMST(List<List<int[]>> graph) {
    int n = graph.size();
    boolean[] inMST = new boolean[n];
    int[] minEdge = new int[n];
    Arrays.fill(minEdge, Integer.MAX_VALUE);
    minEdge[0] = 0; // Start from node 0
    
    // [weight, node]
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
    pq.offer(new int[]{0, 0});
    
    int mstWeight = 0;
    
    while (!pq.isEmpty()) {
        int[] curr = pq.poll();
        int w = curr[0], u = curr[1];
        
        if (inMST[u]) continue;
        inMST[u] = true;
        mstWeight += w;
        
        for (int[] edge : graph.get(u)) {
            int v = edge[0], weight = edge[1];
            if (!inMST[v] && weight < minEdge[v]) {
                minEdge[v] = weight;
                pq.offer(new int[]{weight, v});
            }
        }
    }
    
    return mstWeight;
}
```

---

## Topological Sorting

### Kahn's Algorithm (BFS)
```java
public List<Integer> topologicalSort(int n, int[][] edges) {
    // Build graph and in-degree array
    List<List<Integer>> graph = new ArrayList<>();
    int[] inDegree = new int[n];
    for (int i = 0; i < n; i++) graph.add(new ArrayList<>());
    
    for (int[] edge : edges) {
        graph.get(edge[0]).add(edge[1]);
        inDegree[edge[1]]++;
    }
    
    // Initialize queue with nodes having 0 in-degree
    Queue<Integer> queue = new LinkedList<>();
    for (int i = 0; i < n; i++) {
        if (inDegree[i] == 0) queue.offer(i);
    }
    
    List<Integer> result = new ArrayList<>();
    
    while (!queue.isEmpty()) {
        int u = queue.poll();
        result.add(u);
        
        for (int v : graph.get(u)) {
            if (--inDegree[v] == 0) {
                queue.offer(v);
            }
        }
    }
    
    // Check for cycles
    if (result.size() != n) {
        return new ArrayList<>(); // Cycle exists
    }
    
    return result;
}
```

---

## Union-Find (Disjoint Set)
```java
class UnionFind {
    int[] parent;
    int[] rank;
    int count;
    
    public UnionFind(int n) {
        parent = new int[n];
        rank = new int[n];
        count = n;
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            rank[i] = 1;
        }
    }
    
    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    }
    
    public boolean union(int x, int y) {
        int xRoot = find(x);
        int yRoot = find(y);
        
        if (xRoot == yRoot) return false; // Already in same set
        
        // Union by rank
        if (rank[xRoot] < rank[yRoot]) {
            parent[xRoot] = yRoot;
        } else if (rank[xRoot] > rank[yRoot]) {
            parent[yRoot] = xRoot;
        } else {
            parent[yRoot] = xRoot;
            rank[xRoot]++;
        }
        
        count--;
        return true;
    }
    
    public int getCount() {
        return count;
    }
}
```

---

## Complexity Analysis

| Algorithm | Time Complexity | Space Complexity |
|-----------|-----------------|------------------|
| BFS | O(V + E) | O(V) |
| DFS | O(V + E) | O(V) |
| Dijkstra's | O((V + E) log V) | O(V) |
| Bellman-Ford | O(V * E) | O(V) |
| Floyd-Warshall | O(V³) | O(V²) |
| Kruskal's | O(E log E) | O(V) |
| Prim's | O(E log V) | O(V) |
| Topological Sort (Kahn's) | O(V + E) | O(V) |
| Union-Find | O(α(n)) per operation | O(V) |

Where:
- V = number of vertices
- E = number of edges
- α = Inverse Ackermann function (effectively a small constant for all practical values of n)

---

## When to Use Which Algorithm

1. **BFS vs DFS**:
   - Use BFS for shortest path in unweighted graphs
   - Use DFS for topological sort, cycle detection, or when you need to explore all paths

2. **Shortest Path**:
   - Dijkstra's: Non-negative weights, single source
   - Bellman-Ford: Can handle negative weights, detects negative cycles
   - Floyd-Warshall: All pairs shortest paths

3. **Minimum Spanning Tree**:
   - Kruskal's: Sparse graphs (E << V²)
   - Prim's: Dense graphs (E ≈ V²)

4. **Topological Sort**:
   - Use for dependency resolution, course scheduling
   - Kahn's algorithm is generally preferred for its simplicity

5. **Union-Find**:
   - Use for dynamic connectivity problems
   - Kruskal's algorithm for MST uses Union-Find