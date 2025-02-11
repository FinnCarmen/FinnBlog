+++
date = '2025-02-01T16:02:15+08:00'
draft = false
title = 'Dijkstra'
+++

这段代码实现了 Dijkstra 算法，用于计算从一个起点到图中其他所有节点的最短路径。下面我们一步一步来讲解这段代码和 Dijkstra 算法的原理。

---

## 1. 算法概述

Dijkstra 算法是一种贪心算法，用于求解**单源最短路径**问题，即给定一个图和一个起点，求出从起点到其他所有节点的最短距离。该算法适用于所有边权均为非负值的图。

### 算法核心思想：

- **初始化**：将起点的距离设为 0，其余节点的距离设为无穷大（在代码中用 `INT_MAX` 表示）。
- **贪心选择**：每次从未确定最短路径的节点中选择距离起点最近的节点（这一步通过优先队列实现）。
- **松弛操作**：对于当前节点的每个邻接节点，检查是否可以通过当前节点走一条更短的路径，如果可以则更新距离。
- **重复操作**：重复选择和松弛操作直到所有节点都被处理完毕。

---

## 2. 代码讲解

### 2.1 图的表示

```cpp
typedef pair<int, int> Edge;
typedef unordered_map<int, vector<Edge>> Graph;
```

- **Edge**：使用 `pair<int, int>` 表示一条边，其中 `first` 表示邻接节点编号，`second` 表示边的权重。
- **Graph**：图使用 `unordered_map` 表示，键为节点编号，值为从该节点出发的所有边的列表。

例如：

```cpp
g[0] = {{1, 2}, {2, 4}};
```

表示从节点 0 出发，有两条边：一条到节点 1 权重为 2，一条到节点 2 权重为 4。

### 2.2 Dijkstra 算法函数

函数原型：

```cpp
vector<int> dijkstra(const Graph &g, int start, int n)
```

- **参数说明**：
  - `g`：图的邻接表表示。
  - `start`：起点编号。
  - `n`：图中节点的总数量。
- **返回值**：返回一个整数向量，其中第 `i` 个元素表示从起点到节点 `i` 的最短距离。

#### 2.2.1 初始化

```cpp
vector<int> dist(n, INT_MAX);
dist[start] = 0;
```

- 使用 `dist` 数组保存从起点到各节点的当前最短距离，初始时除了起点外都设为无穷大（`INT_MAX`）。
- 起点的距离为 0。

#### 2.2.2 优先队列

```cpp
priority_queue<Edge, vector<Edge>, greater<Edge>> pq;
pq.push({0, start});
```

- 使用一个优先队列（小顶堆）来存储待处理的节点。队列中的元素是 `(距离, 节点编号)` 对。
- 初始时，将起点（距离为 0）加入队列。
- `greater<Edge>` 用于保证队列顶端元素是距离最小的。

#### 2.2.3 主循环

```cpp
while (!pq.empty())
{
    int cur_dist = pq.top().first;
    int u = pq.top().second;
    pq.pop();

    if (cur_dist > dist[u])
        continue;

    for (const auto &edge : g.at(u))
    {
        int v = edge.first;
        int weight = edge.second;
        int new_dist = cur_dist + weight;

        if (new_dist < dist[v])
        {
            dist[v] = new_dist;
            pq.push({new_dist, v});
        }
    }
}
```

**解析**：

1. **选择当前最短路径的节点**：

   - 取出队列中距离最小的节点 `(cur_dist, u)`。

2. **路径检查**：

   - 如果 `cur_dist > dist[u]`，说明这个节点已经通过其他路径找到了更短的距离，因此跳过当前处理。

3. **松弛操作**：
   - 遍历当前节点 `u` 的所有邻接边。
   - 对于每个边 `(v, weight)`，计算新的可能路径距离 `new_dist = cur_dist + weight`。
   - 如果 `new_dist` 比当前记录的 `dist[v]` 小，则更新 `dist[v]`，并将 `(new_dist, v)` 加入优先队列以便后续处理。

#### 2.2.4 返回结果

循环结束后，`dist` 数组中存储的就是从起点到每个节点的最短距离。

---

## 3. 主函数

```cpp
int main()
{
    Graph g;
    g[0] = {{1, 2}, {2, 4}};
    g[1] = {{2, 1}, {3, 7}};
    g[2] = {{3, 3}};
    g[3] = {};

    int n = 4;
    int start = 0;
    vector<int> dist = dijkstra(g, start, n);

    cout << "从起点 " << start << " 到各个节点的最短距离：" << endl;
    for (int i = 0; i < n; i++)
    {
        cout << "到节点 " << i << " 的距离：" << (dist[i] == INT_MAX ? -1 : dist[i]) << endl;
    }

    return 0;
}
```

**解析**：

- 构造了一个包含 4 个节点的图，并定义了各个节点之间的边及其权重。
- 调用 `dijkstra` 函数以节点 0 为起点计算最短路径。
- 最后输出从起点到每个节点的最短距离。如果某个节点不可达，则输出 `-1`（因为对应的距离仍为 `INT_MAX`）。

---

## 4. 小结与注意事项

- **适用条件**：Dijkstra 算法要求所有边权非负。如果图中存在负权边，应使用其他算法（例如 Bellman-Ford 算法）。
- **时间复杂度**：在使用邻接表和优先队列的情况下，算法的时间复杂度为 O(E log V)，其中 E 是边的数量，V 是节点的数量。
- **数据结构选择**：本例中使用 `unordered_map` 表示图，对于稀疏图非常合适。如果节点编号连续且范围较小，也可以使用 `vector<vector<Edge>>`。

---

完整代码

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <unordered_map>
#include <climits>

using namespace std;

// 定义边的类型
typedef pair<int, int> Edge;
typedef unordered_map<int, vector<Edge>> Graph;

// Dijkstra算法实现，返回从起点到其他所有节点的最短路径距离
vector<int> dijkstra(const Graph &g, int start, int n)
{
    // 距离数组，初始化为无穷大（INT_MAX）
    vector<int> dist(n, INT_MAX);
    dist[start] = 0; // 起点的距离为 0

    // 优先队列（小顶堆），用于选择具有最小当前距离的节点
    priority_queue<Edge, vector<Edge>, greater<Edge>> pq;
    pq.push({0, start}); // 将起点放入队列，起点距离为 0

    // 开始遍历
    while (!pq.empty())
    {
        // 取出队列顶端的元素，即距离最小的节点
        int cur_dist = pq.top().first; // 当前节点距离
        int u = pq.top().second;       // 当前节点编号
        pq.pop();

        // 如果当前距离比记录的最短距离更大，说明已经找到更短路径，跳过
        if (cur_dist > dist[u])
            continue;

        // 遍历当前节点的所有邻接节点
        for (const auto &edge : g.at(u))
        {
            int v = edge.first;               // 邻接节点编号
            int weight = edge.second;         // 边的权重
            int new_dist = cur_dist + weight; // 新的路径距离

            // 如果找到更短的路径，则更新距离并加入优先队列
            if (new_dist < dist[v])
            {
                dist[v] = new_dist;
                pq.push({new_dist, v});
            }
        }
    }
    return dist; // 返回最短路径数组
}

int main()
{
    Graph g;
    g[0] = {{1, 2}, {2, 4}};
    g[1] = {{2, 1}, {3, 7}};
    g[2] = {{3, 3}};
    g[3] = {};

    int n = 4;     // 节点数量
    int start = 0; // 起始节点
    vector<int> dist = dijkstra(g, start, n);

    cout << "从起点 " << start << " 到各个节点的最短距离：" << endl;
    for (int i = 0; i < n; i++)
    {
        cout << "到节点 " << i << " 的距离：" << (dist[i] == INT_MAX ? -1 : dist[i]) << endl;
    }

    return 0;
}
```
