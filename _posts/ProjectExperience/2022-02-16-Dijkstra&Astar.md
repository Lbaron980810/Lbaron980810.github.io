---
title: 基于搜索的轨迹规划算法
# tags: Trajectory Planning
category: ProjectExperience
---

## Dijkstra and A*

如果图的边的权重都为1，那么BFS可以找到最优的一条路径，但如果边的权重不都相同，怎么尽快找到一条cost最小的路径呢？

### Dijkstra

Strategy: visit the node with cheapest accumulated cost g(n)

当所有边的代价都为1的时候就相当于BFS

### A*: Dijkstra with a Heuristic

f(n) = g(n)+h(n)

g(n): cost from node n to start state.

Heuristic h(n): estimated least cost from node n to goal state.

要保证A *的最优性，就要使得A *在每个点Heuristic的值是小于真实的到终点的cost的

因此，h越小就越好，当h=0，就退化成了Dijkstra算法。

当然，因此也就出现了Sub-optimal Solution，追求求解速度而牺牲最优性

Weighted A * : f = g + εh  ε>1 等于说bias towards states that are closer to goal.

#### 工程实现上的经验

<img src="/home/liuboyuu/1_PersonalFile/Lbaron980810.github.io/_posts/Pics_PathPlanning/Screenshot from 2021-09-22 15-04-02.png" alt="Screenshot from 2021-09-22 15-04-02" style="zoom:50%;" />

#### 如何评价一个Heuristic的好与坏呢

采用不同的Heuristic，最后扩展 的节点数量是完全不一样的，因此扩展的节点数量越少，则占用的储存空间越少，Heuristic就越好。对于二维的路径搜索而言，存在一个可计算的值，如下图右侧所示。将这个式子作为Heuristic，可以有效地减少需要扩展的节点数量。

<img src="/home/liuboyuu/1_PersonalFile/Lbaron980810.github.io/_posts/Pics_PathPlanning/Screenshot from 2021-09-22 15-25-23.png" alt="Screenshot from 2021-09-22 15-25-23" style="zoom: 50%;" />

#### Tie Breaker

打破路径中的对称性。最短的路径长度可能有很多的实现方式，但我们可能更希望其中一种方式，或者有打破对称性之后需要扩展的节点更少，因此可以采用修改h函数的方式打破对称性，让每个点的h都不一样，需要扩展的节点量就会改变。



# D\* (Dynamic A\*) 

G表示终点

X表示当前节点

b(X) = Y 表示Y是X的下一个节点，称为后向指针

c(X, Y)表示从X到Y的cost

t(X)表示X的状态，t(X) = NEW表示未添加到open list，t(x) = NEW / OPEN / CLOSED









