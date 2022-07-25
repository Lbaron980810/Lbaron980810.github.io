---
title: 计算几何学习笔记
# tags: 
category: CourseNotes
---

# Chapter 1: Convex Hull

convex combination 是 affine combination的子集，因为前者要求所有的元素都是非负的。

凸相关——三点共线，中间的点可以由端点所组合而成，则是凸相关的。



**Extremity 极性与Extreme point极点**

​	对于凸多边形的顶点，可以找到一条直线使得凸多边形内的所有点都在直线的同一侧，这些点被称为极点，extreme point。

**In-Triangle Test**

时间复杂度O(n^4)

​	给定一个点集，所有能够有某三个点组合而成的点都不是极点。（也就是在某三个点组成的三角形内部的点）

![Screenshot from 2022-04-06 23-21-02](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-06%2023-21-02.png)

**To-Left Test**

时间复杂度O(n^4)

当一个点对三个边都在left的时候，就是in triangle.

![Screenshot from 2022-04-06 23-29-06](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-06%2023-29-06.png)

具体计算方法：依赖行列式

三角形的pqs的面积为S，则
$$
2*S = det
\left[\begin{matrix}
p.x & p.y & 1 \\
q.x & q.y & 1 \\
s.x & s.y & 1 \\
\end{matrix}\right]
$$
当S在直线pq左侧时，面积值为正，在右侧时为负

**Extreme Edges 极边**

对凸包有贡献的边叫做极边，或者说是相邻极点的连线

**利用极边来寻找凸包的算法**

时间复杂度O(n^3)

![Screenshot from 2022-04-07 18-48-28](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-07%2018-48-28.png)

## Incremental Construction

Decrease and Conquer减而治之

先得到一个小凸包，再不断引入新的点来扩大这个凸包。

![Screenshot from 2022-04-07 19-25-15](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-07%2019-25-15.png)

Step1：判断新引入的点是否是一个新的极点

**In-Convex-Polygon Test**

![Screenshot from 2022-04-07 19-08-46](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-07%2019-08-46.png)

利用上图中的二分查找算法似乎可以将判断点是否在凸包内降到log(n)复杂度。但实际是不可行的，因为就类似于插入排序，就算这样可以加速判断，但这要求利用一个更复杂的数据结构来存储信息，从而导致插入操作变慢，整体而言并不能降低算法时间复杂度。

这里只需要利用常规的O(n)方法，逆时针遍历每一条现有的极边，判断点是否都在极边左侧，如果不是则点在极边外。这样整体算法的复杂度就降低为O(n^2)

Step2：如果是一个新极点的话，如何插入到凸包中

找到切点，然后分成近处的一半和远处的一半

![Screenshot from 2022-04-07 19-24-34](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-07%2019-24-34.png)

Step3： 如何找到切点

![Screenshot from 2022-04-07 19-31-22](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-07%2019-31-22.png)

对于切点而言，所有其他的极点都位于新引入的点和切点的连线同侧，并且可以根据左右关系判别出具体是哪一个切点。

![Screenshot from 2022-04-07 19-35-55](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-07%2019-35-55.png)

上面所述的三步本质上可以通过一个循环的判别解决：

只需逆时针判断每一个极点与新引入点的连线。如果是内部点则不会存在ts段(L+R)，是外部点则可以直接找到s和t以及st段。于是一次插入的成本变成O(n)，整体算法变为O(n^2)

## Jarvis March提出构造凸包的方法

Inspired by 选择排序：首先得到一个极点，然后在剩余的点中选择一条能够与当前点连接成为极边的点。

这样每一次查找的复杂度就是O(n)，整体的复杂度就是O(n^2)

![Screenshot from 2022-04-08 16-22-40](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-08%2016-22-40.png)

如何找到下一个极点？根据当前所在的极点，与剩余的点相连，然后做to-left test

![Screenshot from 2022-04-08 16-30-02](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-08%2016-30-02.png)

如何得到第一个极点？--> lowest then leftmost point(LTL)

先找最低的点，如果最低点不唯一，就再找其中最左侧的点

算法整体结构：

![Screenshot from 2022-04-08 16-49-21](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-08%2016-49-21.png)

![Screenshot from 2022-04-08 16-53-31](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-08%2016-53-31.png)

同样是O(n^2)，该方法的优势是什么？ -> output seneitivity 输出敏感性

上一种方法必须遍历所有点，因此最好和最坏情况的复杂度都是O(n^2)。

而该方法取决于凸包的形状。最坏的情况是所有点都是极点，则复杂度就是O(n^2)，最好的情况就是常数个点是极点，复杂度就是O(n)

![Screenshot from 2022-04-08 16-59-31](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-08%2016-59-31.png)



## Lower Bound: Reduction规约

这个问题最好的方案的时间复杂度是多少？或者说最低时间复杂度是多少？

![Screenshot from 2022-04-08 17-08-37](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-08%2017-08-37.png)

A问题是研究透彻了的，B问题是待研究的。

如果A的输入可以在线性时间复杂度内化成B的输入，B的输出也可以在线性时间复杂度内化成A的输出。

称A问题可以规约到B问题。

因此对于B问题的解决算法，可以用于解决A问题，而A问题的复杂度的下界必然小于等于B问题的复杂度



排序问题 可以规约成 2D-Convex Hull 问题

Sorting <=_N 2d-CH，而排序问题的复杂度是O(nlogn)由此可知2D-Convex Hull 问题的复杂度也是O(nlogn)

![Screenshot from 2022-04-08 17-28-39](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-08%2017-28-39.png)



## Graham Scan 算法 

### Algorithm

该算法保证时间复杂度在最坏的情况下也不超过O(nlogn)

**预排序**：得到最下方的一个极边，并对其他点按照极角进行排序。将得到的点放入右侧所示的两个开口相对的栈中。

![Screenshot from 2022-04-12 23-50-28](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-12%2023-50-28.png)

**Graham Scan**：关注S栈中的栈顶、次栈顶及T栈的栈顶

更新当前极边或得到一条新的极边

![Screenshot from 2022-04-12 23-54-46](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-12%2023-54-46.png)

### Correctness

利用数学归纳法证明

### 复杂度分析

**Bactracks**

在新的点出现时，可能会需要回溯，而在最坏的情况下，甚至会回退到只剩两个点。

**Planarity**

![Screenshot from 2022-04-13 22-41-27](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-13%2022-41-27.png)

**Amortization**

证明整个过程的复杂度是O(n)

关注s.size() + 2 * T.size()变量，每次都会减少1

从最初的2n - 2到最后的至少为3，一定是线性时间复杂度

![Screenshot from 2022-04-13 22-45-57](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-13%2022-45-57.png)

**Simpification**

如何将算法简化？不用之前的极角presorting而选择常用的坐标排序

![Screenshot from 2022-04-13 22-52-35](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-13%2022-52-35.png)



## Divide and Conquer 分而治之

启发自归并排序，将极点分组得到对应的凸包，然后再合并成一个大凸包

下一步就是找到多个小凸包的公共的核，这样就可以用Graham Scan法了

![Screenshot from 2022-04-14 20-38-38](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-14%2020-38-38.png)

对于两个小凸包的例子，首先取一个小凸包的重心作为核

于是存在两种情况：

1. 核在另一个凸包的内部

这种情况只需要直接归并即可

![Screenshot from 2022-04-14 22-01-34](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-14%2022-01-34.png)



2. 核在另一个凸包的外部

这种情况从核向另一个凸包射出两条切线，取有用的一半，对这1.5个凸包做归并

![Screenshot from 2022-04-14 21-59-55](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-14%2021-59-55.png)

### 另一种Divide and Conquer算法

1. 预处理：按照x方向平均分为两个凸包

2. 求取common tangents公切线

![Screenshot from 2022-04-14 22-50-46](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-14%2022-50-46.png)

3. zig-zag方法

先从左侧凸包的最右点和右侧凸包的最左点开始，二者相连，得到最下方的绿线。

先判断该线右侧端点，其前驱点和后继点是否都在连线同侧，是则判断左侧端点，否则前进一步，变为上方一条绿线。

交错而行，最终找到upper common tangent 和 lower common tangent

![Screenshot from 2022-04-14 22-51-41](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-14%2022-51-41.png)



# Chapter 2: Geometric Intersection

- determine if they have a non-empty common intersection
- count the cardinality of their intersection set
- enumerate the set of their intersections
- construct the region of their intersection

## Preliminary

### Element Uniqueness

找寻一个几何中是否存在相同的元素？

排序即可，因此时间复杂度是O(nlogn)的 

### Min Gap

给出n个点，找出差距最小的两个点

![Screenshot from 2022-04-15 20-27-10](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-15%2020-27-10.png)

### Max Gap

给出n个点，找出差距最大的两个连续点

### ![Screenshot from 2022-04-15 20-27-45](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-15%2020-27-45.png)

### IEU （Integer Element Uniqueness）

难度仍然是O(nlogn)



## Interval Intersection Detection Algorithm

从线段相交检测算法开始

算法步骤如下，将所有线段的端点分为左端点L和又端点R并放在数轴上，当存在LL RR的情况时表示有相交。

该算法的时间复杂度是O(nlogn)

![Screenshot from 2022-04-15 20-35-28](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-15%2020-35-28.png)

时间复杂度的证明：归约

![Screenshot from 2022-04-15 20-38-44](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-15%2020-38-44.png)



## Segment Intersection Reporting

任务1：detect possible intersection among them判断是否存在

任务2：identify all pairs of intersecting segments找出所有相交

算法lower bound：O(nlogn + I) I指交点的数量

![Screenshot from 2022-04-16 23-18-06](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-16%2023-18-06.png)



![Screenshot from 2022-04-16 23-23-02](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-16%2023-23-02.png)

![Screenshot from 2022-04-16 23-23-31](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-16%2023-23-31.png)



### BO Algorithm

1. 算法的形成：如果有一条竖直的线从左到右扫描，那么相交的线的交点必定出现在竖直线上

![Screenshot from 2022-04-16 23-41-45](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-16%2023-41-45.png)

2. 如何比较？根据线段与竖直扫描线交点的高度作为对比项目确定顺序

![Screenshot from 2022-04-16 23-42-47](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-16%2023-42-47.png)

3. 更新状态：如果两个线存在交点，则两个先的顺序变化一定是a > b -> a = b -> a < b

![Screenshot from 2022-04-16 23-43-06](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-16%2023-43-06.png)

4. 数据结构：

   - 状态数据结构：在某一时刻，与竖直扫描线相交的有序序列

   - 事件队列：包含左右端点和交点的优先级队列（按照时间顺序）

![Screenshot from 2022-04-16 23-43-46](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-16%2023-43-46.png)

5. 算法流程：如下图左侧所示

![Screenshot from 2022-04-16 23-44-16](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-16%2023-44-16.png)

### Degeneracy退化情况

上述的是一般情况的解法，但还有如下一些退化情况（特殊情况）需要处理

![Screenshot from 2022-04-17 17-46-16](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-17%2017-46-16.png)



### Event Queue的生成

期望的结果如下图，在扫描线的一个状态时，事件队列中应该包括每一条线段的左右端点和已经检测到的交点。储存在优先级队列中

![Screenshot from 2022-04-17 17-47-47](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-17%2017-47-47.png)

在一开始，只有排列好的左右端点。在扫描线顺序变化之后才有交点进入队列。因为顺序变化之后需要检测变化后的新的相邻线段是否有交点，这个交点是在当前状态之后的。

![Screenshot from 2022-04-17 17-55-57](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-17%2017-55-57.png)

扫描线状态用BBST（平衡二叉搜索树）储存

![Screenshot from 2022-04-17 20-40-42](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-17%2020-40-42.png)

### Corrrectness的证明

### Retest

下图情况上下两条平行线会被重复测试

![Screenshot from 2022-04-17 20-51-18](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-17%2020-51-18.png)



### Complexity of Event Queue

对于优先级队列，操作的复杂度是O(logS)，S表示事件数量。

![Screenshot from 2022-04-17 20-54-25](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-17%2020-54-25.png)

事件数量最多到2n+I，I表示交点数量，最多可能为n^2^。但log之后仍然是O(logn)的复杂度。

另外，初始化队列中的2n个点的时间复杂度其实是O(n)，不需要2n的时间。（这是为啥？）

![Screenshot from 2022-04-17 20-56-46](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-17%2020-56-46.png)



### Complexity of Status Structure

单次扫描线的点不多于n，因此单次操作复杂度为O(logn)

![Screenshot from 2022-04-17 21-00-28](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-17%2021-00-28.png)

总的时间复杂度是O((2n + I) * logn) = O((n + I) * logn)

由于I可以大到n^2^，整体复杂度有可能变为O(n^2^logn)，但其实在绝大多数情况下还是可以的。

![Screenshot from 2022-04-17 21-01-09](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-17%2021-01-09.png)



## Detection Intersection Between Convex Polygons

### Monotone Partitioning单调划分

取多边形的最高和最低点，将多边形的边化成两条多边形链，并在两端水平方向延长成射线

![Screenshot from 2022-04-18 20-06-54](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-18%2020-06-54.png)



#### Criterion

判定是否相交的原则如下：

- 多边形P与Q相交，当且仅当P_Left与Q_Right相交或P_Right与Q_Left相交
- 多边形P与Q不相交，当且仅当P_Left与Q_Right不相交且P_Right与Q_Left不相交

![Screenshot from 2022-04-18 20-07-25](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-18%2020-07-25.png)



### Decrease And Conquer

类似于二分法，将每一个多边形链储存成一个线段的向量的形式，二分测试是否有交叉。具体情况如后面图示

![Screenshot from 2022-04-18 21-17-43](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-18%2021-17-43.png)

情况1：所选两条线段交叉，算法结束

![Screenshot from 2022-04-18 21-18-21](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-18%2021-18-21.png)

情况2：可以设去其中一条多边形链的一半。

![Screenshot from 2022-04-18 21-18-28](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-18%2021-18-28.png)

情况3：可以消去两条多边形链的一半。

![Screenshot from 2022-04-18 21-19-18](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-18%2021-19-18.png)



### Complexity

算法的时间复杂度是O(log(n + m))，n+m表示边的数量

![Screenshot from 2022-04-18 21-22-24](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-18%2021-22-24.png)



## Edge Chasing算法  (构建相交的部分)

算法直观理解：两个多边形的边交替遍历，当出现交点的时候交换。如下图所示，先是红色边前进，出现交点；蓝色前进，出现交点；当没出现交点时就要一直前进。

![Screenshot from 2022-04-18 22-06-40](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-18%2022-06-40.png)

如何确定那条边先前进呢？这里没细讲，让看参考文献了

![Screenshot from 2022-04-18 22-08-10](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-18%2022-08-10.png)

复杂度：相当于对两个多边形都遍历一圈，O(n + m)

![Screenshot from 2022-04-18 22-18-35](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-18%2022-18-35.png)

特殊情况：当一个多边形完全包含于另一个时，如何判定呢？——>让自己想

![Screenshot from 2022-04-18 22-20-29](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-18%2022-20-29.png)



## Plane Sweeping算法

平面扫描方法：

每个扫描线与一个多边形最多交于两个点，因此每次扫描最多只与四条线相交。每次操作都是常数时间。

![Screenshot from 2022-04-19 11-11-00](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-19%2011-11-00.png)

事件开始：到达两个多边形最左侧的点

每次判定都只需要考虑四条边，因此也是常数时间

![Screenshot from 2022-04-19 11-12-11](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-19%2011-12-11.png)

整体复杂度为O(n + m)

![Screenshot from 2022-04-19 11-13-00](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-19%2011-13-00.png)



## Half-plane Intersection Construction算法

Unbounded Convex Polygons & Half-planes

将算法延伸到无界的凸多边形（广义凸多边形），这里考虑由一条直线分割得到的半平面组成的多边形。

![Screenshot from 2022-04-19 11-19-17](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-19%2011-19-17.png)

算法的下界：通过规约的方式证明算法的下界为O(nlogn)

将其规约成排序问题。

规约方式：排序->HIC，由横轴上的a与纵轴上的点1/a连接就可以转化成一条直线；反之类似，沿着多边形的边遍历

![Screenshot from 2022-04-19 11-41-10](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-19%2011-41-10.png)

整体算法思路：分而治之

![Screenshot from 2022-04-19 11-41-43](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-19%2011-41-43.png)

算法复杂度为O(nlogn)，是最优算法

![Screenshot from 2022-04-19 11-43-40](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-19%2011-43-40.png)



# Chapter 3: Triangulation

## Methodology

将多边形分成三角形（多边形的三角剖分）

![Screenshot from 2022-04-20 17-32-09](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-20%2017-32-09.png)



## Art Gallery Problem

对一个多边形的画廊，至少要布置多少个哨兵才能覆盖整个画廊？

![Screenshot from 2022-04-20 17-42-48](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-20%2017-42-48.png)

### Lower & Upper bound

对于凸多边形或星形多边形，只需要一个哨兵

![Screenshot from 2022-04-20 17-44-05](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-20%2017-44-05.png)

对于任意多边形，最多需要n个哨兵，即在每一个顶点都放置哨兵。但对于三维情况有例外。

![Screenshot from 2022-04-20 17-50-02](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-20%2017-50-02.png)



### Complexity

这个问题是一个NP-Hard问题（无法用多项式表达复杂度的问题）

即便是做出如下简化，仍然是NP-Hard问题

1. vertex guard：只布置在顶点（绿色点）
2. point guard：多边形内部点（红色点）
3. edge guard：可以在一条边上巡回
4. orthogonal polygon：正交多边形，所有的边都是垂直或水平的。

![Screenshot from 2022-04-20 17-48-50](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-20%2017-48-50.png)

### Approximation & Classification

Approximation：有学者提出解决近似问题的方法，但超出了本课范围内

Classification：将多边形分类，再解决。

![Screenshot from 2022-04-20 17-53-27](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-20%2017-53-27.png)



## Art Gallery Theorem

最多只需要n/3个哨兵

必要性：下图所示的情况就需要n/3个哨兵

![Screenshot from 2022-04-20 18-13-52](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-20%2018-13-52.png)

充分性：任何一个多边形都可以分成不超过n/3把像扇子一样的多边形，因此只需要n/3个哨兵

![Screenshot from 2022-04-20 19-18-44](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-20%2019-18-44.png)



## Fisk's Proof

证明一个多边形确实只需要n/3个哨兵

首先将多边形用内对角线进行三角划分，不考虑corner case

![Screenshot from 2022-04-21 16-48-14](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-21%2016-48-14.png)

对多边形顶点用**三种颜色**上色，要求由边或内对角线相连的两个顶点的颜色互异

![Screenshot from 2022-04-21 16-50-06](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-21%2016-50-06.png)

上色过程需要用到多边形三角化后的对偶图。

对偶图：将三角形都视作一个点，有相邻边的两个三角形所代表的点相连。最后可以得到一个树状图。

根据这一方法可知，对偶图上相连的两点所代表的三角形的顶点有两个是公共的，另外两个是同色的。而整个多边形的顶点必然能用三个颜色覆盖。

![Screenshot from 2022-04-21 16-54-40](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-21%2016-54-40.png)

用任何一种颜色的点作为哨兵，必然能够覆盖整个多边形

![Screenshot from 2022-04-21 16-59-35](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-21%2016-59-35.png)

由鸽巢原理，必然存在一种颜色的点的数量小于等于n/3。

![Screenshot from 2022-04-21 17-01-36](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-21%2017-01-36.png)

但是这一方法并没有考虑到有洞的情况，有洞的话对偶图就不是一个树状图，而必然存在环路。

另外，最重要的前提，如何保证多边形一定能够三角划分，也没有证明

![Screenshot from 2022-04-21 17-03-37](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-21%2017-03-37.png)

## Orthogonal Polygons

一种画廊问题的变体：如果画廊都是正交多边形

首先类似于凸多边形的证明，我们可以知道一定存在需要n//4个哨兵的情况。

![Screenshot from 2022-04-21 17-26-57](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-21%2017-26-57.png)

那么如何证明最多只需要n/4个哨兵呢？

类似的，将正交多边形用凸四边形进行划分，这种划分即使在正交多边形中存在空洞也依然是成立的（后续需要证明）。

![Screenshot from 2022-04-21 17-27-31](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-21%2017-27-31.png)

## Triangulation

### Existence

简单多边形：非相邻的边不会相交

![Screenshot from 2022-04-22 16-17-06](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-22%2016-17-06.png)

带洞的多边形：无论沿着外边界还是内边界前进，多边形的内部总是在左手一侧

- outer boundary：逆时针描述
- inner boundary：顺时针描述

![Screenshot from 2022-04-22 16-20-00](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-22%2016-20-00.png)







任何简单多边形，无论是否有洞，都是可以被三角化的。

### Ear-cutting 

- ear: 前后相邻的三个点构成的三角形完全落在多边形**内部**，且该三角形内**不会**有多边形的顶点(empty)
  - the tip of the ear: ear的三个点的中间点，这点对于多边形而言是凸的（convex）

- mouth: 前后相邻的三个点构成的三角形完全落在多边形**外部**，且该三角形内**不会**有多边形的顶点(empty)
  - mouth的三个点的中间点，这点对于多边形而言是凹的（reflex）
- dirty: 该三角形内有多边形的顶点

![Screenshot from 2022-04-22 16-29-35](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-22%2016-29-35.png)

**对一个多边形而言，将存在的“耳朵”切掉，就可以简化这个多边形。如果能一直切下去，就可以最终完成多边形的三角化。**

那么是否能够一直切下去呢？

### Two-Ear Theorem

任何一个简单多边形至少有两个耳朵

![Screenshot from 2022-04-22 16-34-13](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-22%2016-34-13.png)

 

### 利用数学归纳法证明可以三角剖分

1. 对多边形的顺序的定义

- 洞多的多边形更大
- 洞数相等时，顶点多的多边形更大
- 洞数一样，顶点数一样的两个多边形一样大。

![Screenshot from 2022-04-22 16-41-07](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-22%2016-41-07.png)

要利用数学归纳法，首先要有一个归纳基。这里因为所有的三角形的三角剖分就是他自己，因此可以作为归纳基

归纳假设：所有的小于多边形P的多边形可以被三角剖分。

![Screenshot from 2022-04-22 19-40-24](Pics_ComputerNet/Screenshot from 2022-04-22 19-40-24.png)

凸定点：首先找lowest-then-leftmost点J ，这个点一定是凸的

![Screenshot from 2022-04-22 19-42-17](Pics_ComputerNet/Screenshot from 2022-04-22 19-42-17.png)

Induction：

考虑找到的凸点的前后邻居所连成的边IK

- 这个边可能是内对角线--->找到了耳朵
- 也可能不是内对角线--->内部有另一个顶点M

对于第二种情况，图中的JM一定是内对角线。

由此，任意多边形都可以被分成一个更小的多边形。而由归纳假设——所有小于当前多边形的多边形都可以被三角剖分，那么该三角形也就一定能被三角剖分

![Screenshot from 2022-04-22 20-05-16](Pics_ComputerNet/Screenshot from 2022-04-22 20-05-16.png)

如果是有洞的情况呢？

按照下图右侧所示的方式裁切，虽然连通分量数没有变化，但洞的数量减少了，因此多边形仍然被分成了更小的多边形

![Screenshot from 2022-04-22 20-08-40](Pics_ComputerNet/Screenshot from 2022-04-22 20-08-40.png)

### Properties

1. Uniqueness

![Screenshot from 2022-04-22 20-09-49](Pics_ComputerNet/Screenshot from 2022-04-22 20-09-49.png)

2. Complexity

![Screenshot from 2022-04-22 20-11-15](Pics_ComputerNet/Screenshot from 2022-04-22 20-11-15.png)

3. Dual Graph

![Screenshot from 2022-04-22 20-12-40](Pics_ComputerNet/Screenshot from 2022-04-22 20-12-40.png)



## Triangulating Monotone Polygons

单调多边形三角化

### Monotone Polygon

单调链：投影到一条直线上，所有的顶点仍然按照原来的顺序排列

![Screenshot from 2022-04-22 20-30-33](Pics_ComputerNet/Screenshot from 2022-04-22 20-30-33.png)

单调多边形：可以分解成两条单调链的多边形

![Screenshot from 2022-04-22 20-32-44](Pics_ComputerNet/Screenshot from 2022-04-22 20-32-44.png)

### Monotonicity Testing

单调性测试：给定一个多边形，如何判断是否单调？如何找到他的单调方向？如果不单调，如何切割？out of scope

![Screenshot from 2022-04-22 20-36-06](Pics_ComputerNet/Screenshot from 2022-04-22 20-36-06.png)

### Strategy

扫描线策略：一个单调多边形可以在线性时间内被三角剖分

- 三角剖分所有当前扫面线上方的可以剖分的部分
- 去掉剖分后的部分

![Screenshot from 2022-04-22 20-48-18](Pics_ComputerNet/Screenshot from 2022-04-22 20-48-18.png)

首先对所有顶点排序，因为是单调的，所以只需要讲两条单调链归并，这在线性时间内是可以实现的。

![Screenshot from 2022-04-22 20-40-37](Pics_ComputerNet/Screenshot from 2022-04-22 20-40-37.png)



### Stack-Chain Consistency

用一个栈来储存所有的顶点，入栈出栈的规则和顺序如图所示。

![Screenshot from 2022-04-22 20-50-53](Pics_ComputerNet/Screenshot from 2022-04-22 20-50-53.png)

要验证整个栈变化的过程，规则是一致的。或者说栈中的点有如下共同点。

![Screenshot from 2022-04-22 20-51-19](Pics_ComputerNet/Screenshot from 2022-04-22 20-51-19.png)

两大类三小类情况分析：

1. 点位于同侧链且是凹的

![Screenshot from 2022-04-22 20-55-35](Pics_ComputerNet/Screenshot from 2022-04-22 20-55-35.png)

2. 点位于同侧链但是凸的

![Screenshot from 2022-04-22 20-57-02](Pics_ComputerNet/Screenshot from 2022-04-22 20-57-02.png)

3. 点位于异侧链

![Screenshot from 2022-04-22 21-01-20](Pics_ComputerNet/Screenshot from 2022-04-22 21-01-20.png)

### Example

整体流程举例

![Screenshot from 2022-04-22 21-13-31](Pics_ComputerNet/Screenshot from 2022-04-22 21-13-31.png)



### Analysis

正确性：

![Screenshot from 2022-04-22 21-15-39](Pics_ComputerNet/Screenshot from 2022-04-22 21-15-39.png)

Complexity：

![Screenshot from 2022-04-22 21-17-36](Pics_ComputerNet/Screenshot from 2022-04-22 21-17-36.png)



## Monotone Decomposition

多边形不单调的原因是因为存在如下图所示的两种点，分别称为钟乳石型点(stalactite, T)和石笋型点(stalagmite, M)

因此单调分解的关键变成了去除这些点。

接下来以M型点为例讲解。

![Screenshot from 2022-04-23 19-03-06](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-23%2019-03-06.png)

仍然选择用扫面线的策略： 

先按照高度顺序排列放进事件队列

水平线自上而下扫描。

遇到M型点时，与之前储存的某一个点相连，从而消去这一个M型点。这个点 b被称为**Helper**

![Screenshot from 2022-04-23 19-05-26](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-23%2019-05-26.png)

### Helper

helper和M型点及边界可以形成一个梯形（trapezoid），算法不断为现有的梯形储存helper，当M型点出现时与helper相连。

helper的存在可能有三种情况

- 左侧边界的上顶点
- 右侧边界的上顶点
- M型点上方的一个T型点



![Screenshot from 2022-04-23 19-19-43](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-23%2019-19-43.png)

![Screenshot from 2022-04-23 19-19-51](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-23%2019-19-51.png)

![Screenshot from 2022-04-23 19-20-04](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-23%2019-20-04.png)

因此扫描线状态就会变成下图的样子

![Screenshot from 2022-04-23 19-25-04](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-23%2019-25-04.png)

### Possible Cases

1. Start Vertex：对于第一个顶点，会形成一个退化的梯形——三角形，要将其放入存储结构中，顶点V也要放入helper中

![Screenshot from 2022-04-23 19-39-11](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-23%2019-39-11.png)

2. End Vertex：最后一个顶点，类似于开始顶点。需要在存储结构中删去最后一个三角形

![Screenshot from 2022-04-23 19-39-56](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-23%2019-39-56.png)

3. Left Adjacency：左侧顶点使图中出现上下两个梯形，新的helper变成V

![Screenshot from 2022-04-23 19-41-11](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-23%2019-41-11.png)

4. Right Adjacency：同上

![Screenshot from 2022-04-23 19-41-32](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-23%2019-41-32.png)

5. M型点：原本的T变成了T_L, T_R，新的helper是V

![Screenshot from 2022-04-23 19-42-39](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-23%2019-42-39.png)

6. T型点：与M型点对偶，T_L和T_R变成T，新的helper是V

![Screenshot from 2022-04-23 19-43-28](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-23%2019-43-28.png)

### Example

[j, a : 0]表示这个梯形的侧边是j和a，helper是0

![Screenshot from 2022-04-23 20-07-18](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-23%2020-07-18.png)

### Analysis

该算法的复杂度是O(nlogn)，有人给出了O(nloglogn)复杂度的算法，最终有学者给出了O(n)的算法，但太复杂了，目前还没有实现。

实际应用中该方法较常用

![Screenshot from 2022-04-23 20-11-50](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-23%2020-11-50.png)

## Tetrahedralization 四面体

 三维空间的单纯形：四面体，因此会想把三维空间的复杂多面体剖分成四面体。

但三维空间更复杂，一个正方体，既能分成奇数个四面体，也能分成偶数个四面体。甚至有的多面体无法剖分出四面体。

在二维的画廊问题中，在所有顶点都放置一个哨兵，一定能覆盖整个平面；但是对于三维的情况，并不一定。



# Chapter 4: Voronoi Diagram

相关的经典综述文章

Voronoi diagrams: a survey of a fundamental data structure

## Terminologies:

- site：红色的点
- cell：分割后每个site所占据的区域，是n-1个半平面的交集，由于convex的计算封闭性，一定是convex
- Voronoi edge：两个site的平分线
- Voronoi vertex：voronoi edge的交点
- Voronoi Diagram, VD(S)：由edge所构成的图，只储存edge的信息

![Screenshot from 2022-04-24 20-25-57](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-24%2020-25-57.png)

![Screenshot from 2022-04-24 20-28-31](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-24%2020-28-31.png)

![Screenshot from 2022-04-24 20-31-18](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-24%2020-31-18.png)

![Screenshot from 2022-04-24 21-33-31](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-24%2021-33-31.png)

## Properties

非空性：每一个site的cell都是非空的

![Screenshot from 2022-04-25 19-55-28](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-25%2019-55-28.png)

空圆性：以任意点x为圆心，以与它最近的site的距离为半径做圆，这个圆内一定不包含任何site。

- x在cell内：圆上只有一个site
- x在edge上：圆上有两个site
- x在vertex上：圆上有三个site

![Screenshot from 2022-04-25 20-04-17](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-25%2020-04-17.png)



最近site同圆：同时距离点x最近的site一定在同一个圆上（这不废话么？）

![Screenshot from 2022-04-25 20-03-03](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-25%2020-03-03.png)

vertex的度(degree)：距离它最近的site的数量，正常情况下degree不会超过3

![Screenshot from 2022-04-25 20-05-04](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-25%2020-05-04.png)

当点x沿着一条edge移动到一个vertex上：

可以认为是一个圆不断扩张，然后split成两个圆；

也可以认为是edge出现了分叉

![Screenshot from 2022-04-25 20-10-49](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-25%2020-10-49.png)



## Complexity

### Linearity

The VD of n sites has :

- O(2n - 4) vertices
- O(3n - 6) edges

### Proof

D：所有face的周长的edge数的和

f：face数，也可以是cell或site 的数目（注意：外面无界的部分也算一个face）

e：edge数

v：vertex数

c：连通域数（对voronio图而言总是1）

planrity和Euler's formula一起组成方程就可以证明

![Screenshot from 2022-04-25 20-28-00](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-25%2020-28-00.png)



## Representation

如何存储voronoi图？---> subdivision

### Subdivision

需要有一个无穷远点作为最外边界的终点，这样就会导致边界有曲线。

Fary Theorem证明了所有的曲线平面图都可以转换为直线平面图，因此无穷远点不会影响division的结果，结果仍然是一个平面图

![Screenshot from 2022-04-26 19-49-03](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-26%2019-49-03.png)

![Screenshot from 2022-04-26 19-48-43](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-26%2019-48-43.png)

## DCEL双向链接链表

用于表示VD的数据结构

首先给出对偶边的概念：一条线段对应的两个有向线段互相对偶

![Screenshot from 2022-04-26 19-56-34](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-26%2019-56-34.png)

DCEL数据结构中储存Half edge的如下指标

- id, twin
- origin:起点
- inc：对应的面（左侧的面）
- pred：前驱
- succ：后继

![Screenshot from 2022-04-26 19-57-18](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-26%2019-57-18.png)

DCEL数据结构中储存Vertex的如下指标

- id
- x&y：坐标
- inc：点的第一个half-edge

![Screenshot from 2022-04-26 20-10-24](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-26%2020-10-24.png)

DCEL数据结构中储存face的如下指标

- id
- inc：面的第一个half-edge

![Screenshot from 2022-04-26 20-11-48](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-26%2020-11-48.png)

因此可以在图中方便的遍历Vertex和Face，下面还有一个计算与某一直线相交的face的例子

![Screenshot from 2022-04-26 20-18-11](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-26%2020-18-11.png)

![Screenshot from 2022-04-26 20-22-03](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-26%2020-22-03.png)



## Hardness

1维和二维的VD构建都可以规约成sorting问题，因此复杂度下届是O(nlogn)

![Screenshot from 2022-04-27 21-38-24](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-27%2021-38-24.png)

![Screenshot from 2022-04-27 21-41-39](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-27%2021-41-39.png)

## Sorted Sets

对于凸包构建，如果顶点按照高度是有序的，那么凸包的构建复杂度就会变为O(n)。

但对于VD而言，这并不能帮助减小复杂度，复杂度仍然是O(nlogn)。

因为凸包是Combinatorial Structure，而VD是Geometric Structure。区别是：

- 对于前者而言，仿射变换（拉伸，剪切等）后对应的结构不会变化
- 对于后者而言，仿射变换后结构会发生变化

![Screenshot from 2022-04-27 21-56-40](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-27%2021-56-40.png)

 

![Screenshot from 2022-04-27 21-58-59](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-27%2021-58-59.png)

## VD_sorted

根据按照某一方向（如y方向）排序好的数组来构建VD图，得到的算法时间复杂度仍然是O(nlogn)

归约问题：VD的构建可以归约成如下一个“求无序数组中是否存在两个数的绝对值小于epsilon”的问题，而这个问题的复杂度仍然是O(nlogn)，因此可知原问题的复杂度下界也是O(nlogn)

![Screenshot from 2022-04-28 19-43-56](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-28%2019-43-56.png)

输入的转换：从无序数组输入转换到VD图的输入，公式如下图左侧所示。（圈内的数字是数据在无序数组中的下标）

![Screenshot from 2022-04-28 19-45-30](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-28%2019-45-30.png)

利用VD构建的算法得到了VD图，注意，构建的含义是得到对应的数据结构等全部信息

![Screenshot from 2022-04-28 19-56-42](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-28%2019-56-42.png)

输出的转换——VD的输出会有两种情况

- 第一种是x轴穿过了所有的face，那么就相当于的到了一个排序的结果，可以在O(n)的时间内得到归约问题的解了。
- 第二种是x轴穿过了部分的face，那么根据图示的几何关系，必然有两个点的距离是小于epsilon的，也可以在O(n)时间内解决epsilon-closness问题。

![Screenshot from 2022-04-28 19-57-52](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-28%2019-57-52.png)

![Screenshot from 2022-04-28 19-58-04](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-28%2019-58-04.png)

![Screenshot from 2022-04-28 19-59-36](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-28%2019-59-36.png)

## Naive Construction

最简单的构造VD的算法：使用裁剪的方法进行最后cell的生成，复杂度是O(n^3)



## Incremental Construction

从n-1个site增加一个site：新的site与每一个旧site生成边界，每个新site最多要与每个旧site都生成边界，一共要新增n个site。

![Screenshot from 2022-04-29 20-19-48](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-29%2020-19-48.png)

复杂度：O(n^2logn)：新的site与每一个旧site生成边界，本质是与凸多边形求交，时间复杂度是O(logn)。

下图是worst case——即每个新增的site都要与之前的site生成边界

![Screenshot from 2022-04-29 20-24-48](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-29%2020-24-48.png)



## Divide and Conquer

分治算法，现在x方向上presorting，然后划分，合并。关键在于如何合并

![Screenshot from 2022-04-29 21-02-43](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-29%2021-02-43.png)

合并的关键在于处理重合问题，即两个子图有重叠的部分（必然的，因为分治后都是根据分到的site划分整个平面）

![Screenshot from 2022-04-29 21-04-28](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-29%2021-04-28.png)

不断处理重叠的两个cell，最终可以得到一副合格的图，但是这样效率很低。

![Screenshot from 2022-04-29 21-05-42](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-29%2021-05-42.png)

每一次合并的中间线被称为contour，他必定是有voronoi edge构成的，是bisectors

![Screenshot from 2022-04-29 21-23-53](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-29%2021-23-53.png)

Contour的特性：

- Y-Monotonicity 在Y方向上是单调的
- contour的第一段是左右两个凸包的上公切线的bisector，最后一段是下公切线的bisector
- 如果contour左侧有n个site，右侧有m个site，那么contour最多不会超过n+m段



![Screenshot from 2022-04-29 21-29-02](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-29%2021-29-02.png)

具体的合并算法：

![Screenshot from 2022-04-29 21-33-25](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-29%2021-33-25.png)

复杂度：计算上下公切线的复杂度是O(n+m)，转换到相邻cell的复杂度是O(1)，关键的复杂度在于找到端点。

![Screenshot from 2022-04-29 21-37-29](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-29%2021-37-29.png)

在找端点的时候，考虑到最终生成的cell是凸的，因此每次找到一个端点，就可以继续沿着边界往下找，所以复杂度就是遍历一周，线性复杂度（这里其实我也没看懂）

![Screenshot from 2022-04-29 21-41-44](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-29%2021-41-44.png)



![Screenshot from 2022-04-29 21-44-34](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-04-29%2021-44-34.png)



# Chapter 5: Delaunay Triangulation

## Point Set Triangulation

问题定义

![Screenshot from 2022-05-04 09-41-33](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-05-04%2009-41-33.png)

这种三角剖分是不唯一的，但数量是固定的。

如果一个点集的点数为n，凸包的极点数为h，那么一定会用到3(n-1)-h条对角线和2(n-1)-h个三角形

下图为两种三角剖分的方案，这样通过变换一条边实现的被称为**Edge Flipping**。通过Edge Flipping可以得到最优的三角剖分方案。

![Screenshot from 2022-05-04 09-50-28](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-05-04%2009-50-28.png )

三角剖分的Upper bound：类似于前面讲过的将两个凸包连接起来的算法，将点集递归的分为两个部分，然后组合连接即可。这个复杂度是O(nlogn)

![Screenshot from 2022-05-04 09-57-22](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-05-04%2009-57-22.png)

Lower bound: 可以通过将sorting规约到triangulation证明二者的lower bound都是O(nlogn)的



## Delaunay Triangulation

对偶图：如下图所示，对于右侧的voronoi图而言，左侧的图是通过将所有的edge相关的两个site相连接而得到的。

而这就会生成一个三角化之后的凸多边形，这种三角化的方式成为Delaunay Triangulation。

性质：三角剖分后三角形的数目等于Voronoi vertex的数目

![Screenshot from 2022-05-04 19-53-52](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-05-04%2019-53-52.png)

## Properties

划分得到的所有三角形的外接圆内部都没有其他的点，因此是空的，而这些圆的圆心就是一个voronoi vertex



![Screenshot from 2022-05-05 09-10-35](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-05-05%2009-10-35.png)

而得到的三角剖分的结果图中，两个site被连起来，则以他们的连线为弦，一定存在一个空圆。

从右侧的Voronoi图理解就是：以在Voronoi edge上的点为圆心，到相邻的site的距离为半径，得到的圆

![Screenshot from 2022-05-05 09-12-12](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-05-05%2009-12-12.png)

NNG：最近邻图，必然是Delauny Triangulation的子图

对于CG而言，以这条线作为直径的圆是空圆，因此G是C的最近邻。但这是一个有向图，F是G的最近邻

![Screenshot from 2022-05-05 09-12-37](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-05-05%2009-12-37.png)





## Proximity Graph

Delauny Triangulation得到的图是一系列的超图，因此可以通过做Delauny Triangulation进而得到其他的图。这里先介绍这些图中的两个图，这两个图都是反应的元素之间的邻近性，因此被称为Proximity Graphs。

### Gabriel Graph

![Screenshot from 2022-05-05 22-06-02](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-05-05%2022-06-02.png)

构造改图的时间不会超过O(nlogn)，主要时间用于构造Voronoi Graph

![Screenshot from 2022-05-05 22-08-03](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-05-05%2022-08-03.png)



### Relative Neighborhood Graph

这个图是Gabriel Graph的子图，构造时间仍然是O(nlogn)，如果已经有了Voronoi图，则构造时间变成O(n)

![Screenshot from 2022-05-05 22-09-36](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-05-05%2022-09-36.png)



### Lower Bounds

也是用规约到排序的方法来求下界

![Screenshot from 2022-05-05 22-18-11](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputationGeometry/Screenshot%20from%202022-05-05%2022-18-11.png)







# Bottom

