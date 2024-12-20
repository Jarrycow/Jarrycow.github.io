---
title: BFS & DFS
author: Jarrycow
img: /medias/featureimages/DFS_BFS.jpg
cover: false
top: false
mathjax: true
categories:
  - 算法
tags:
  - 算法
keywords: BFS & DFS
abbrlink: BFS_DFS
summary: 图和树搜索内容，深搜与宽搜是最常见的方法。
date: 2024-04-17 20:41:29
---



<!--more-->

## DFS

在图中，对于非连通图，DFS 只能访问起点所在连通分量。 DFS 通常可以使用**栈**作为遍历结点的暂存容器。

```java
class Solution {
    private static List<List<Integer>> res;  // 
    private static Deque<Integer> stack;  // 辅助栈
    public List<List<Integer>> dfsTarget(int[][] graph) {
        res = new ArrayList<>(); 
        stack = new ArrayDeque<>();

        int numb = graph.length;  // 结点数量
        stack.add(0);  // 添加首个结点
        dfs(graph, 0,numb - 1);  // 深搜
        return res;
    }
 
    /* DFS */
    private void dfs(int[][] graph, int begin, int end) {
        if (begin == end) {  // 成功搜索，加入路径
            res.add(new ArrayList<>(stack));
            return;
        }

        for (int next : graph[begin]) {  // 深入搜索
            stack.add(next);
            dfs(graph, next,end);
            stack.pollLast();  // 回退
        }
    }
}
```

**时间复杂度：**$O\left(n + edge\right)$

**空间复杂度：**$O\left(n\right)$

## 网格 DFS

网格搜索是比较常见的使用 DFS 的方法，简单说来就是使用 DFS 在二维数组进行检索。

网格中搜索的比较麻烦的就是在边界需要考虑上下左右是否越界，如果上下左右分开分辨比较麻烦，其实可以在 dfs 函数开始分辨结点是否合法。

同时用方法标记访问过的区域，当某区域的上下左右都已访问或不能访问，则 DFS 结束。同时，Java 中 HashSet 无法直接判断 `int[]` 的值，因此无法保证唯一性，所以应当使用 `List` 保存数组并放入集合。

```java
HashSet<List> visited = new HashSet<>();
public void dfs(char[][] grid, int i, int j) {
    /* 边界清除 */
    if (!(i >= 0  &&  i < grid.length  &&  j >= 0  &&  j < grid[0].length))  
        return;

    /* 访问过 */
    if (visited.contains(Arrays.asList(i, j)))
        return;
        
    /* 访问 */
    visited.add(Arrays.asList(i, j));
    if (grid[i][j] == '0')    return;

    dfs(grid, i - 1, j);
    dfs(grid, i + 1, j);
    dfs(grid, i, j - 1);
    dfs(grid, i, j + 1);
}
```

## BFS

从一个点出发求**最短距离**常用的就是 BFS，即把周围这一圈搜索完成之后，再搜索下一圈，是慢慢扩大搜索范围的。

### 网格 BFS（多源最短路径）

```java
public void bfs(int[][] grid) {
    int[][] DRECTIONS = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};  // 四方向 
    List<int[]> queue = new ArrayList<>();  // 访问节点的队列
    boolean[][] seen = new boolean[grid.length][grid[0].length];  // 已访问标记
    
    /* 初始出发点 */ 
    for (int i = 0; i < grid.length; ++i)
    for (int j = 0; j < grid[0].length; ++j) {
        if (grid[i][j] == 2) {
            queue.add(new int[]{i, j});
            seen[i][j] = true;
        }
    }

    /* BFS */
    while (!queue.isEmpty()) {  // 队列
        List<int[]> tmp = queue;  // 当前所在节点
        queue = new ArrayList<>();  // 存储新节点的队列
        for (int[] pos: tmp)  // 访问每个节点的前后左右四个节点
        for (int[] d: DRECTIONS) {
            int i = pos[0] + d[0];
            int j = pos[1] + d[1];
            if (i >= 0 && i < grid.length && 
                 j >= 0 && j < grid[0].length &&  // 位置合法
                 !seen[grid.length][grid[0].length]) {  // 位置可访问
                     grid[i][j] = 2;  // 位置状态更新
                     queue.add(new int[]{i, j});  // 新位置加入队列
                	 seen[grid.length][grid[0].length] = true;
                }
            }
    }
}
```

