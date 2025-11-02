# Report: Algorithms for Smart City Task Scheduling

## 1. Introduction

This project involves implementing and analyzing algorithms used to schedule tasks in a smart city context. The tasks are represented as dependencies in a directed graph, where some tasks are dependent on others. The graph can have both cyclic and acyclic dependencies, and we apply several graph algorithms to optimize how these tasks are scheduled and executed. Specifically, we will look at algorithms for **Strongly Connected Components (SCC)**, **Topological Sorting**, and **Shortest Paths in Directed Acyclic Graphs (DAGs)**.

## 2. Theoretical Part

### 2.1 Strongly Connected Components (SCC)

**What is SCC?**  
A **Strongly Connected Component (SCC)** is a subset of a directed graph in which there is a path from every vertex to every other vertex within the subset. In the context of scheduling tasks, SCCs represent tasks that have circular dependencies. These tasks need to be handled together before moving on to others.

**Algorithm:**  
We use two well-known algorithms to detect SCCs:
- **Tarjan’s Algorithm**: It uses Depth-First Search (DFS) and a stack to find SCCs.
- **Kosaraju’s Algorithm**: This algorithm performs two DFS traversals — the first on the original graph, and the second on the transposed graph (where all edges are reversed).

**Time Complexity:**  
Both Tarjan’s and Kosaraju’s algorithms work in **O(V + E)** time, where **V** is the number of vertices and **E** is the number of edges in the graph.

### 2.2 Topological Sorting

**What is Topological Sorting?**  
Topological sorting is a linear ordering of the vertices in a Directed Acyclic Graph (DAG), where for every directed edge **(u, v)**, vertex **u** comes before vertex **v** in the ordering. In the context of our task scheduling, topological sorting helps us determine in which order tasks should be executed.

**Algorithm:**
- **Kahn’s Algorithm**: This algorithm works by repeatedly removing nodes with no incoming edges (i.e., tasks that don’t depend on any other tasks).
- **DFS-Based Algorithm**: This algorithm sorts the graph by performing a DFS and adding nodes to the list as we finish exploring them.

**Time Complexity:**  
Both algorithms have a time complexity of **O(V + E)**, since they visit every vertex and edge once.

### 2.3 Shortest Path in a DAG

**What is the Shortest Path Problem?**  
In a DAG, the shortest path from a starting node to other nodes is the path that minimizes the total weight of the edges. For task scheduling, this could represent the shortest time required to complete a sequence of tasks, where each task takes a certain amount of time or has a certain duration.

We also implement the **Longest Path** problem by reversing the sign of edge weights and then applying the shortest path algorithm, or by using a dynamic programming approach over the topological order.

**Algorithm:**
- **Single-Source Shortest Path**: This algorithm calculates the shortest path from one starting vertex to all other vertices in the DAG.
- **Longest Path**: This is done by first converting the DAG to a problem of finding the shortest path on a graph where edge weights are inverted (i.e., negative weights).

**Time Complexity:**  
Both algorithms take **O(V + E)** time for DAGs, as they visit each vertex and edge once.

---

## 3. Dataset

The datasets used in this project represent task scheduling problems with varying graph structures. We have three categories of datasets:
- **Small (6-10 vertices)**: Simple cases, 1-2 cycles or pure DAGs.
- **Medium (10-20 vertices)**: Mixed structures with several SCCs.
- **Large (20-50 vertices)**: Performance and timing tests.

Each dataset includes:
- Number of vertices (nodes) and edges.
- A description of whether the graph is cyclic or a DAG.
- The edge weights or node durations.

We use these datasets to test the performance and correctness of the algorithms.

---

## 4. Analysis

### 4.1 Performance Metrics

We track the following performance metrics for each algorithm:
- **SCC Algorithm**: We count the number of DFS visits and edges traversed.
- **Topological Sort**: We measure the number of pops and pushes in the queue (for Kahn’s algorithm) or DFS traversals.
- **Shortest Path**: We count the number of relaxations performed in the shortest path algorithm.

### 4.2 Results

The results for each algorithm are stored in the following CSV files:

#### SCC Results

| Dataset | Nodes | Edges | Density | Structure     | SCCs | Duration Type | Time (ms) | DFS Visits | Edges Traversed |
| ------- | ----- | ----- | ------- | ------------- | ---- | ------------- | --------- | ---------- | --------------- |
| 1       | 6     | 10    | Sparse  | Pure DAG      | 0    | Node          | 6         | 32         | 17000           |
| 2       | 8     | 13    | Sparse  | One Cycle     | 2    | Node          | 1         | 42         | 12000           |
| 3       | 10    | 22    | Sparse  | Two Cycles    | 5    | Node          | 3         | 64         | 28100           |
| 4       | 6     | 11    | Dense   | Pure DAG      | 0    | Node          | 6         | 34         | 9200            |
| 5       | 8     | 29    | Dense   | One Cycle     | 4    | Node          | 1         | 74         | 10300           |
| 6       | 10    | 51    | Dense   | Two Cycles    | 1    | Node          | 1         | 122        | 23200           |
| 7       | 14    | 27    | Sparse  | One Cycle     | 5    | Node          | 1         | 82         | 21800           |
| 8       | 16    | 38    | Sparse  | Two Cycles    | 6    | Node          | 2         | 108        | 23500           |
| 9       | 20    | 55    | Sparse  | Multiple SCCs | 10   | Node          | 4         | 150        | 31500           |
| 10      | 14    | 93    | Dense   | One Cycle     | 6    | Node          | 1         | 214        | 49900           |
| 11      | 16    | 113   | Dense   | Two Cycles    | 4    | Node          | 1         | 258        | 44500           |
| 12      | 20    | 120   | Dense   | Multiple SCCs | 7    | Node          | 3         | 280        | 36200           |
| 13      | 30    | 122   | Sparse  | Pure DAG      | 0    | Node          | 30        | 304        | 54500           |
| 14      | 40    | 175   | Sparse  | Multiple SCCs | 2    | Node          | 3         | 430        | 69600           |
| 15      | 50    | 557   | Sparse  | Two Cycles    | 21   | Node          | 1         | 1214       | 156800          |
| 16      | 30    | 311   | Dense   | Pure DAG      | 0    | Node          | 30        | 682        | 278900          |
| 17      | 40    | 459   | Dense   | Multiple SCCs | 13   | Node          | 4         | 998        | 192700          |
| 18      | 50    | 1402  | Dense   | Two Cycles    | 13   | Node          | 1         | 2904       | 350900          |


#### Topological Sort Results

| Dataset | Nodes | Edges | Density | Structure        | SCCs | Duration Type | Time (ms) | DFS Visits | Edges Traversed |  
|---------|-------|-------|---------|------------------|------|---------------|-----------|------------|-----------------|
| 1       | 6     | 10    | Sparse  | Pure DAG         | 0    | Node          | 16        | 8          | 8000            |
| 2       | 8     | 13    | Sparse  | One Cycle        | 2    | Node          | 21        | 4          | 4800            |
| 3       | 10    | 22    | Sparse  | Two Cycles       | 5    | Node          | 32        | 21         | 21600           |
| 4       | 6     | 11    | Dense   | Pure DAG         | 0    | Node          | 17        | 3          | 3500            |
| 5       | 8     | 29    | Dense   | One Cycle        | 4    | Node          | 37        | 5          | 5800            |
| 6       | 10    | 51    | Dense   | Two Cycles       | 1    | Node          | 61        | 6          | 6500            |
| 7       | 14    | 27    | Sparse  | One Cycle        | 5    | Node          | 41        | 7          | 6300            |
| 8       | 16    | 38    | Sparse  | Two Cycles       | 6    | Node          | 54        | 8          | 8400            |
| 9       | 20    | 55    | Sparse  | Multiple SCCs    | 10   | Node          | 75        | 10         | 9500            |
| 10      | 14    | 93    | Dense   | One Cycle        | 6    | Node          | 107       | 7          | 7000            |
| 11      | 16    | 113   | Dense   | Two Cycles       | 4    | Node          | 129       | 12         | 10100           |
| 12      | 20    | 120   | Dense   | Multiple SCCs    | 7    | Node          | 140       | 14         | 10600           |
| 13      | 30    | 122   | Sparse  | Pure DAG         | 0    | Node          | 152       | 15         | 13100           |
| 14      | 40    | 175   | Sparse  | Multiple SCCs    | 2    | Node          | 215       | 21         | 17800           |
| 15      | 50    | 557   | Sparse  | Two Cycles       | 21   | Node          | 607       | 50         | 38000           |
| 16      | 30    | 311   | Dense   | Pure DAG         | 0    | Node          | 341       | 30         | 37200           |
| 17      | 40    | 459   | Dense   | Multiple SCCs    | 13   | Node          | 499       | 40         | 66100           |
| 18      | 50    | 1402  | Dense   | Two Cycles       | 13   | Node          | 1452      | 100        | 197800          |


#### Shortest Path Results

| Dataset | Nodes | Edges | Density | Structure        | SCCs | Duration Type | Time (ms) | DFS Visits | Edges Traversed |  
|---------|-------|-------|---------|------------------|------|---------------|-----------|------------|-----------------|
| 1       | 6     | 10    | Sparse  | Pure DAG         | 0    | Node          | 26        | 22100      | 22100           |
| 2       | 8     | 13    | Sparse  | One Cycle        | 2    | Node          | 0         | 200        | 200             |
| 3       | 10    | 22    | Sparse  | Two Cycles       | 5    | Node          | 0         | 200        | 200             |
| 4       | 6     | 11    | Dense   | Pure DAG         | 0    | Node          | 28        | 14800      | 14800           |
| 5       | 8     | 29    | Dense   | One Cycle        | 4    | Node          | 0         | 200        | 200             |
| 6       | 10    | 51    | Dense   | Two Cycles       | 1    | Node          | 0         | 100        | 100             |
| 7       | 14    | 27    | Sparse  | One Cycle        | 5    | Node          | 0         | 200        | 200             |
| 8       | 16    | 38    | Sparse  | Two Cycles       | 6    | Node          | 0         | 300        | 300             |
| 9       | 20    | 55    | Sparse  | Multiple SCCs    | 10   | Node          | 0         | 100        | 100             |
| 10      | 14    | 93    | Dense   | One Cycle        | 6    | Node          | 0         | 2000       | 2000            |
| 11      | 16    | 113   | Dense   | Two Cycles       | 4    | Node          | 0         | 200        | 200             |
| 12      | 20    | 120   | Dense   | Multiple SCCs    | 7    | Node          | 0         | 200        | 200             |
| 13      | 30    | 122   | Sparse  | Pure DAG         | 0    | Node          | 274       | 57700      | 57700           |
| 14      | 40    | 175   | Sparse  | Multiple SCCs    | 2    | Node          | 0         | 200        | 200             |
| 15      | 50    | 557   | Sparse  | Two Cycles       | 21   | Node          | 0         | 200        | 200             |
| 16      | 30    | 311   | Dense   | Pure DAG         | 0    | Node          | 652       | 221000     | 221000          |
| 17      | 40    | 459   | Dense   | Multiple SCCs    | 13   | Node          | 0         | 200        | 200             |
| 18      | 50    | 1402  | Dense   | Two Cycles       | 13   | Node          | 0         | 100        | 100             |


### 4.3 Bottlenecks

- The **SCC algorithms** perform similarly on small graphs, but the time increases significantly as the number of vertices and cycles grows.
- **Topological sorting** is fast for small to medium-sized graphs but takes more time for larger graphs with complex dependencies.
- **Shortest path calculations** are efficient on DAGs, but performance drops as the graph size and edge weights increase.

### 4.4 Practical Recommendations

- **SCC algorithms** are essential when dealing with cyclic dependencies. If the graph has only a few cycles, Tarjan’s algorithm is recommended due to its stack-based approach. For larger, more complex graphs, Kosaraju’s algorithm can be more efficient due to its two-pass structure.
- **Topological sorting** is suitable when we need to schedule tasks without circular dependencies. It is fast for DAGs but can become slower with graphs that have multiple SCCs.
- **Shortest path** algorithms are ideal for graphs where task durations or edge weights are important. For performance-critical applications, we should aim to keep the graph sparse and minimize the number of relaxations.

---

## 5. Conclusion

This project successfully demonstrates how graph algorithms can be applied to real-world task scheduling problems. By analyzing SCCs, topological sorting, and shortest paths in DAGs, we can optimize task scheduling in smart cities. The algorithms perform well on small to medium-sized graphs, but care must be taken when dealing with large, dense graphs or graphs with many cycles. The results and analysis provide valuable insights into when and how to use each algorithm in practice.
