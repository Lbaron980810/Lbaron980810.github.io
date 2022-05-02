---
title: 李宏毅2021 概述Reinforcement Learning
# tags: 
category: CourseNotes
---
Outline:

1. What is RL?
2. Policy Gradient
3. Actor-Critic
4. Reward Shaping
5. No Reward Learning from Demonstration

## What is RL?

已知Machine Learning = Looking for a Function，实际上RL也是在寻找一个function

![Screenshot from 2022-02-05 19-40-07](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/websitePics/Screenshot%20from%202022-02-05%2019-40-07.png)

希望找到一个function，使得environment给actor的reward总和最大

![Screenshot from 2022-02-05 19-40-21](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/websitePics/Screenshot%20from%202022-02-05%2019-40-21.png)

RL的真正难点就是如何求解这一Optimization问题，如何找到一组network参数使R最大

## Policy Gradient

 policy gradient 问题的引出：

Version 0: 在设计ai玩alien invade游戏的时候，如果只是在消灭外星人的时候才给到正的反馈，那么就会导致设计出的ai会疯狂开火，而不左右移动，这是效果很差的。这样的ai被称为version 0。因此我们需要看一下真正的RL是如何操作的？Policy Gradient又是什么

Version 1: 刚才是用r1评估a1是一个好的action还是不好的action，而现在我们用a1之后所有的结果总和G1来评价。这种reward被称为culmulated reward。那version 1又有什么问题呢？如果游戏过程非常长，那么aN的reward归功于a1又不太合理了。

Version 2：设计一个小于1的Discount factor，让前面步骤对后面步骤的影响逐渐减弱。







# Deep Learning Theory

## 1-1 Can shallow network fit any function















