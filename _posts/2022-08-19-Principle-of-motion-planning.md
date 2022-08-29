---
title: Principlkes of Robot Motion
tags: C++
category: ReadingNotes

---

# Principlkes of Robot Motion 阅读笔记



# Chap 1 Introduction

1. Complete:  An algorithm A is complete if in a finite amount of time, A always finds a solution if a solution exists or otherwise A determines that a solution does not exist

完备性：如果在起始点和目标点之间有路径解存在，那么一定可以得到解，如果得不到解那么一定说明没有解存在

概率完备性：如果在起始点和目标点之间有路径解存在，只要规划或搜索的时间足够长，就一定能确保找到一条路径解

最优性：规划得到的路径在某个评价指标上是最优的

渐进最优性：经过有限次规划迭代后得到的路径是接近最优的次优路径，且每次迭代后都与最优路径更加接近，是一个逐渐收敛的过程





