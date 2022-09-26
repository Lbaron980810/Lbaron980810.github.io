---
title: 凸优化理论阅读笔记
# tags: 
category: CourseNotes
---

# Chapter 1：引言



# Chapter 2：凸集

## Section 1：仿射集合与凸集

1. 直线与线段

2. 仿射集：直线是仿射集，线段不是
   $$
   C \subseteq R^n \\
   \theta x_1 + (1-\theta) x_2 \in C \quad \forall x_1, x_2 \in C
   $$

3. 仿射集合中任意点的仿射组合仍然在仿射集中
   $$
   x_1, ..., x_k \in C \\
   \forall \theta_i \in R \quad  \sum_i \theta_i = 1 \\
   \theta_1 x_1 + ... + \theta_k x_k \in C
   $$

4. 与仿射集合C相关联的**子空间**
   $$
   V= C- x_o= \{x - x_0 \mid x_0 \in C \}
   $$
   意义：对于任意一条直线而言，是仿射集，但是必须要求系数theta的和为1；但如果这条直线过原点，则不需要这个限制，也可以满足仿射组合的点在原直线上。子空间相当于将原来的集合平移，因此可以得到一个**过原点的直线（子空间必须包括原点）**。

5. 仿射包：由集合C中的点的所有仿射组合组成的集合为C的仿射包
   $$
   \bold{aff}C = \{\theta_1 x_1 + ... + \theta_k x_k \mid x_1, ..., x_k \in C, \sum_i \theta_i = 1 \}
   $$
   意义：仿射包是包含C的最小的仿射集合（如何理解？见下，C是两个点，显然不是仿射集合；但C的仿射包是这两个点的连线，就是一个仿射集了，并且任意包含C的仿射集合必然包含affC）
   $$
   C = \{x_1, x_2\} \\
   \bold{aff}C = \{\theta_1 x_1 + \theta_2 x_2 \mid x_1, x_2 \in C, \theta_1 + \theta_2 = 1 \}
   $$
   
6. 仿射维数与相对内部
   **仿射维数**：其仿射包的维数（即上面的两个点构成的集合C的维数是1，因为他的仿射包是一条直线；三个点构成的集合C的维数是2，因为其仿射包是一个平面；书上的例子，R2上的单位圆的仿射维数是2，其仿射包为R2全空间，也是平面）
   **相对内部与相对边界**：对于R3上z=0平面上(-1, 1)的一个正方形C，C的内部为空，相对内部为C的内部；边界为C本身，相对边界为其边框（书上的例2.2）
   $$
   \bold{relint}C \subseteq C \subseteq \bold{aff}C\\
   \bold{relint}C = \{x \in C \mid B(x, r) \cap \bold{aff}C \subseteq C 对某些r > 0 \} \\
   B(x, r) = \{y \mid \Vert y - x \Vert \leq r \} \\
   $$

7. 凸集：类似于仿射集 ->仿射组合->仿射包，也有凸集->凸组合->凸包，凸包是包含C的最小凸集。
   集合C被成为凸集，当且仅当
   $$
   \theta x_1 + (1 - \theta) x_2 \in C \quad  \forall x_1, x_2 \in C, \theta \in [0, 1] \\
   $$
   凸组合：x_1, ..., x_k的凸组合为 
   $$
   \theta_1 x_1 + ... + \theta_k x_k \quad  \forall \sum_i \theta_i = 1 \\
   $$
   凸包：
   $$
   \bold{conv}C = \{\theta_1 x_1 + ... + \theta_k x_k \mid x_i \in C, \theta_i \geq 0, \sum_i \theta_i = 1\}
   $$
   **凸\*与仿射\*的区别：凸\*要求**
   $$
   \theta_i \in [0, 1]
   $$
   
8. 锥（非负齐次）：可以理解为一个**过原点的**射线（仿射集为直线，凸集为线段，锥为射线）
   $$
   \theta x \in C \quad \forall x \in C, \theta \geq 0
   $$
   如何理解非负齐次？
   $$
   Ax = 0 \\
   齐次方程组的解空间x即为锥
   $$
   凸锥：
   $$
   \theta_1 x_1 + \theta_2 x_2 \in C \quad \forall x_1, x_2 \in C \& \ \theta_1, \theta_2 \geq 0
   $$
   锥组合（非负线性组合）：
   $$
   \theta_1 x_1 + ... + \theta_k x_k \quad  \forall \theta_i \geq 0
   $$
   锥包：集合C的锥包是C中元素的所有锥组合的集合，是包含C的最小的凸锥
   $$
   \{\theta_1 x_1 + ... + \theta_k x_k \mid x_i \in C, \theta_i \geq 0\}
   $$

**对比与总结：**

- 仿射集：直线
  $$
  \forall x_1, ..., x_k \in C, \theta_1 + ... + \theta_k = 1
  $$

- 凸集：线段，包含了仿射集和凸锥
  $$
  \forall x_1, ..., x_k \in C, \theta_1 + ... + \theta_k = 1 \&\& \theta_i \geq 0
  $$
  
- 凸锥：过原点的射线
  $$
  \forall x_1, ..., x_k \in C, \theta_i \geq 0
  $$
  

## Section 2：重要的例子

- 空集，任意点（单点集），全空间Rn都是Rn的仿射子集
- 任意直线是仿射的。如果通过零点，则是子空间，也是凸锥
- 一条线段是凸的，但不是仿射的（除非退化为一个点）
- 一条射线是凸的，但不是仿射的；如果基点是0，则他是凸锥
- 任意子空间是仿射的，凸锥



1. 超平面(hyperplane)：是仿射集，是凸集，过原点才是凸锥
   $$
   \{x \mid a^Tx = b\}, a, x \in R^n
   $$

2. 半空间(half space)：是凸集，不是仿射集，过原点才是凸锥
   $$
   \{x \mid a^Tx \leq b\}, x \in R^n
   $$

3. Euclid球和椭球
   Euclid球：
   $$
   B(x_c, r) = \{x \mid \Vert x-x_c\Vert_2 \leq r\} = \{x_c + ru \mid \Vert u \Vert_2 \leq 1\}
   $$
   椭球：其中P是对称正定矩阵，决定了椭球从圆心向各个方向扩展的幅度。椭球的半轴长度由$\sqrt{\lambda_i}$给出，其中$\lambda_i$为P的特征值；A是非奇异的方阵，假设A对称正定，取$A = P^{1/2}$，当A为对称半正定矩阵但奇异时，变为退化的椭球，其仿射维数等于A的秩
   $$
   \varepsilon = \{x \mid (x-x_c)^TP^{-1}(x-x_c) \leq 1\} = \{x_c + Au \mid \Vert u \Vert_2 \leq 1\}
   $$
   
3. 范数球与范数锥
   范数球：
   $$
   C = \{x \mid \Vert x-x_c\Vert \leq r\}
   $$
   范数锥：
   $$
   C = \{(x,t) \mid \Vert x \Vert \leq t\} \subseteq R^{n + 1}
   $$
   
3. 多面体(polyhedral)：有限个半平面和超平面的交集
   $$
   P = \{x \mid a_j^Tx \leq b_j, j = 1, ..., m, c_j^Tx = d_j, j = 1,...,p\} \\
    = \{x \mid Ax \prec x, Cx = d\} \\
   P =\{\theta_1 x_1 + ... + \theta_k x_k \mid \theta_1 + ... + \theta_m = 1,\theta_i \geq 0, i = 1, ... k \}
   $$
   最下的表示方式说明前m个组合形成一个凸集，另外k-m个构成一个凸锥，因此多面体是凸集和凸锥的结合体
   
   举例：非负象限既是多面体也是锥，被称为多面体锥
   
4. 单纯形：一类重要的多面体
   $$
   v_0, ..., v_k \in R^n 仿射独立\\
   仿射独立：v_1 - v_0, ..., v_k - v_0 线性独立(无关)\\
   C = \bold{conv} \{v_0, ..., v_k\} = \{\theta_0 v_0 + ... + \theta_k v_k \mid \theta \succeq 0, \bold{1}^T \theta = 1\} \\
   C被称为R^n 空间的k维单纯形 
   $$
   举例：$R^2$空间中{两个点构成的单纯形就是一个线段， 三个点构成的单纯形就是一个三角形；四个以上点在该空间中就不是仿射独立的了}；1维单纯形是一条线段，2维单纯形是一个三角形，3维单纯形是四面体
   单位单纯形：由零向量和单位向量$0, e_1, ..., e_n \in R^n$决定的n维单纯形，可以表示为满足下列条件的向量的集合
   $$
   x \succeq 0, \bold{1}^Tx \leq 1
   $$
   概率单纯形：由单位向量$e_1, ..., e_n \in R^n$​决定的n - 1维单纯形，可以表示为满足下列条件的向量的集合
   $$
   x \succeq 0, \bold{1}^Tx \leq 1
   $$
   
5. 半正定锥(positive semidefinite cone)：
   $$
   S^n = \{ X \in R^{n \times n} \mid X = X^T \} \\
   S_+^n = \{ X \in S^n \mid X \succeq 0 \} \\
   S_{++}^n = \{X \in S^n \mid X \succ 0\} \\
   $$
   其中$S_+^n$是凸锥，成为半正定锥；$S_{++}^n$是凸集，不是凸锥



## Section 3：保凸运算

从凸集构造凸集

1. 交集运算是保凸的：多面体是半空间和超平面的交集，因而是凸的。

2. 仿射函数是保凸的：仿射函数的定义如下，如果$S \subseteq R^n $是凸的，那么$f(S)$和$f^{-1}(S)$是凸的
   $$
   f: R^n \rightarrow R^m \\
   f(x) = Ax +b \quad A \in R^{m \times n}, b \in R^m
   $$
   缩放和移位是凸的；两个凸集的和是凸的；两个凸集的直积也是凸的；线性矩阵不等式(LMI)的解是凸的
   
3. 透视函数是保凸的：
   $$
   P: R^{n+1} \rightarrow R^n \\
   P(z,t) = z/t \\
   \bold{dom}P = R^n \times R_{++}
   $$
   线段经过透视函数后是凸的；凸集的反透视映射仍是凸的；

   线性分式函数是透视函数与仿射函数复合而成
   $$
   设g: R^n \rightarrow R^{m + 1}是仿射的 \\
   g(x) = 
   \left[\begin{matrix}
   A \\
   C^T \\
   \end{matrix}\right]
   x + 
   \left[\begin{matrix}
   b \\
   d \\
   \end{matrix}\right] \\
   A \in R^{m \times n}, b \in R^m, c \in R^n, d \in R \\
   f(x) = P \circ g = P(g(x)) = \frac{Ax+b}{c^Tx + d}, \bold{dom}f = \{x \mid c^T x + d > 0\}
   $$
   线性分式也被称为投射函数：理解为一个点x经过逆透视函数升维成为射线，在乘一个矩阵变换成另一条射线，然后经过透视函数又降维成为了另一个点，因此称为投射函数
   $$
   Q =
   \left[\begin{matrix}
   A & b \\
   C^T & d \\
   \end{matrix}\right] \in R^{(m + 1) \times (n + 1)} \\
   y = \left[\begin{matrix}
   x \\
   1 \\
   \end{matrix}\right] = P^{-1}(x)\\
   g(x) = Qy =QP^{-1}(x) \\
   f(x) = P(QP^{-1}(x))
   $$
   
4. 



## Section 4：广义不等式

1. 正常锥：用于定义广义不等式，即$R^n$上的偏序关系
   称$K \subseteq R^n$ 为正常锥，如果它满足下列条件：
   K是凸的；K是闭的（有界）；K是实的，即具有非空内部；K是尖的（即点集，不包含直线）

2. 广义不等式：$\preceq_K$被称为不严格的广义不等式，$\prec_K$被称为严格的广义不等式；
   $$
   1.\preceq_K \quad x \preceq_K y \Leftrightarrow y - x \in K\\ 
   2.\succeq_K \quad x \succeq_K y \Leftrightarrow x - y \in K\\ 
   3.\prec_K \quad x \prec_K y \Leftrightarrow y - x \in \bold{int}K\\ 
   4.\succ_K \quad x \succ_K y \Leftrightarrow x - y \in \bold{int}K\\
   *\bold{int}K 表示L的内部，即interior \ of \ K
   $$
   不带下标时表示$K = R_+^n$，此时广义不等式就是分量不等式（$x \preceq y$ 等价于 $x_i \leq y_i, i = 1, ..., n$）

3. 最小元（minimum element）与极小元（minimal element）：对应于一维空间的最小值和极小值

   最小元：有$\forall y \in S, x \preceq_K y$，则称x为S的最小元。（相对应的可以定义最大元），一个集合如果有最小（大）元，那么一定是唯一的
   极小元：如果$y \in S, y \preceq_K x \ only \ if = x$，则称x是S上的极小元。一个集合可以有多个极小极大元



## Section 5：分离与支撑超平面

1. 超平面分离定理：对于两个凸集$C \cap D = \varnothing $，那么存在满足如下条件的超平面$\{x \mid a^T x = b\}$，被称为分离超平面

$$
a \neq 0 \ and \ b, such \ that \ a^Tx \leq b \ \forall x \in  C \ and \ a^T x \geq b \ \forall x \in D
$$

2. 严格分离：即$\ a^Tx < b \ \forall x \in  C \ and \ a^T x > b \ \forall x \in D$，不相交的凸集不一定能被严格分离，但在很多特殊情况下，可以构造严格分离。
   举例：点与闭凸集的严格分离。

3. 超平面分离定理的逆定理：如果C, D之间存在分离超平面，则C, D一定不相交——这不成立，除非添加额外的限制。
   例如：C与D中至少有一个是开集

4. 支撑超平面：超平面$\{x \mid a^T x = a^T x_0\}$是集合C在x_0处的支撑超平面，如果满足

$$
Suppose \ C \subseteq R^n, x_o \in \bold{bd}C = \bold{cl}C \backslash \bold{int}C \\
\exist a \neq 0, s.t. a^Tx \leq a^T x_0, \forall x \in C \\
$$

5. 支撑超平面定理：对任意非空凸集C，$\forall x_0 \in \bold{C}$，在x_0处存在C的支撑超平面

6. 支撑超平面逆定理：如果一个集合是闭的，具有非空内部，并且其边界上每个点均存在支撑超平面，那么它是凸的





##  Section 6：对偶锥与广义不等式

1. 对偶锥：令K为一个锥（可以不是凸的），对偶锥为
   $$
   K^* = \{y \mid y^T x \geq 0, \forall x \in K\}
   $$
   对偶锥K*与K不一定有包含关系





# Chapter 3：凸函数

## Section 1：凸函数

1. 第一种定义

$$
f(\theta x + (1 - \theta)y) \leq \theta f(x) + (1 - \theta) f(y)
$$

2. 第二种定义：扩展值延伸

$$
\tilde{f}(x) =
\left\{ 
\begin{array}{rcl} 
f(x) & x \in \bold{dom}f \\
\infty &x \notin \bold{dom}f
\end{array}\right.
$$

3. 第三种定义：一阶条件，假设f可微，则f是凸函数的充要条件是$\bold{dom}f$是凸集且对任意$x,y \in \bold{dom}f$

$$
f(y) \geq f(x) + \grad f(x)^T (y-x)
$$

4. 第四种定义：二阶条件，假设f二阶可微，即Hessian矩阵或者二阶导数$\grad ^2f$​存在，则f是凸函数的充要条件是

$$
\forall x \in \bold{dom}f, \grad ^2 f(x) \succeq 0
$$

67, 68（https://www.bilibili.com/video/BV1CT4y1G739/?spm_id_from=333.788.recommend_more_video.-1&vd_source=782da2433fe73c63132e23c2797e9bd7）-92 看书了，视频就跳过了

5. 几种重要的凸函数：指数函数、幂函数...

6. 下水平集

7. 上境图

8. Jensen不等式

## Section 2：保凸运算

1. 非负加权和
2. 复合仿射映射
3. 逐点最大和逐点上确界
4. 复合
5. 最小化
6. 透视



## Section 3：共轭函数

1. 共轭函数的定义：
   $$
   f^*(y) = \sup_{x \in \bold{dom}f} (y^Tx - f(x))
   $$

2. Legendre变换：可微函数f的共轭函数也被称为函数f的Legendre变换（一般函数的共轭成为Fenchel共轭）
   几何解释：y即在对应x的f(x)的导数 
   $$
   f^*(y) = x^{*T} \grad f(x^*) - f^*(x)
   $$

3. 共轭的共轭：
   f是凸函数，$\bold{epi}f$是闭集，则$f(x) = f^{**}(x)$ 
   f不非凸，$f^{**}(x)$仍然是一个凸函数，而且包住了原有的 $f(x)$



## Section 4：拟凸函数——有全局最小值

1. 函数$f:R^n \rightarrow R$称为拟凸函数，（或者单峰函数），如果其定义域及所有下水平集$S_\alpha = \{ x \in \bold{dom}f \mid f(x) \leq \alpha \}, \alpha \in R$ 都是凸集
2. 第二种定义（Jensen不等式）：$f(\theta x + ( 1 - \theta) y) \leq \max\{f(x), f(y)\}, \forall x,y \in \bold{dom}f, \theta \in [0, 1]$
3. 第三种定义（高维到低维）：$f是拟凸函数 \Leftrightarrow g(t) = f(x + tv)是拟凸函数$
4. 第四种定义（拟凸函数一阶条件）：设函数$f:R^n \rightarrow R$可微，则函数f是拟凸的充要条件是，$\bold{dom}f$是凸集且$f(y) \leq f(x) \Rightarrow \grad f(x)^T(y - x) \leq 0, \forall x,y \in \bold{dom}f $。
   几何意义：$\grad f(x)$ 在点x处定义了水平集$\{y \mid f(y) \leq f(x)\}$的一个支撑超平面

辨析：

- 当f是凸函数，$\grad f(x) = 0$表明(x, f(x))为最小值点
- 当f是拟凸函数，$\grad f(x) = 0$无意义

5. 第五种定义（二阶条件）：
   （1）假设函数f二次可微，如果函数f是拟凸函数，则
   $$
   y^T \grad f(x) = 0 \Rightarrow y^T \grad ^2 f(x) y \geq 0, \forall x \in \bold{dom}f, y \in R^n
   $$
   这就意味着当$\grad f(x) = 0 $时，$\grad ^2 f(x) \succeq 0$；当$\grad f(x) \neq 0 $时，$\grad ^2 f(x)$在$\grad f(x)$的正交补空间（n - 1维）半正定，那么黑塞矩阵最多只有一个负特征值。

   （2）反之，如果下面的成立，则f是拟凸函数
   $$
   y^T \grad f(x) = 0 \Rightarrow y^T \grad ^2 f(x) y > 0, \forall x \in \bold{dom}f, y \in R^n
   $$

6. 保拟凸运算：
   6.1 非负加权最大值
   6.2 复合仿射映射
   6.3 逐点最大和逐点上确界
   6.4 复合
   6.5 最小化





## Section 5：对数-凹函数和对数-凸函数

1. 称函数$f: R^n \rightarrow R$对数凹，如果$f(x) > 0 \ and \ log f 是凹函数, \forall x \in \bold{dom}f$；反之，称函数$f: R^n \rightarrow R$对数凸，如果logf是凸函数

2. 性质：
   $$
   logf(x)为凸 \Leftrightarrow \grad ^2 logf(x) \succeq 0
   $$
   

 

# Chapter 4：凸优化问题

## Section 1：优化问题

1. 标准表示
   $$
   \left.
   \begin{array}{rcl} 
   & minimize & f_0(x) \\
   &  subject \ to & f_i(x) \leq 0, i = 1, ..., m  \\
   & & h_i(x) = 0, i = 1, ..., p
   \end{array}\right.
   $$
   线性规划：目标函数和约束函数都为线性函数——单纯形法，内点法

   凸优化：目标函数和约束函数都为凸函数——内点法（10-100步）

   最小二乘问题：没有约束条件，目标函数是若干项的平方和

2. 等价表示



## Section 2：凸优化

1. 标准形式的凸优化问题

2. 局部最优解与全局最优解：凸优化问题的任意局部最优解也是全局最优解

3. 可微函数$f_0$的最优性准则
   $$
   x是最优解 \Leftrightarrow \grad f_0(x)^T(y - x) \geq 0, \forall y \in X
   $$
   几何解释： 点x是最优解，那么$-\grad f_0(x)$定义了X在x处的一个支撑超平面

   （1）无约束优化问题

   （2）只含等式约束的问题

   （3）非负象限中的极小化

4. 等价的凸问题

5. 拟凸优化：将拟凸优化问题转换为一个凸可行问题，然后用二分法缩小可行区间从而求解







## Section 3：线性规划问题

1. 线性规划的标准形式和不等式形式
   将不等式约束和等式约束转换为
   $$
   \left.
   \begin{array}{rcl} 
   & minimize & c^Tx \\
   &  subject \ to & Ax = b  \\
   & & x \succeq 0
   \end{array}\right.
   $$
   解法：单纯形法、椭圆法、内点法

   - 如果可行解集P非空，P中至少存在一个极点
   - 如果可行解集P非空，并且目标函数有界，则线性规划问题在极点上获得最优值
   - P中存在有限个极点
   - 极点可以被基本可行解表述

2. 单纯形法















1. 代码要先能够跑通(RRT* + 六维优化)
2. 修改优化变量，求解SDP问题
3. 前段生成算法修改（尝试search based ? sampling-based search?）
4. 动态物体躲避？







# 继续看凸优化的书，不要心急， 看完凸优化再看Introduction to SDP，再看parrilo的博士论文，感觉可能就大概也许差不多了？

















# 中科大凌青凸优化网课

## class 1

1. 凸函数

2. 凸函数的典型例子：仿射函数(affine function)、范数
   $$
   f(x) = a'x + b
   $$

3. 优化问题
   $$
   minimize \ f_0(x) \\
   subject \ to \ f_i(x) \leq b_i, i = 1, ..., m
   $$
   线性规划：目标函数和约束函数都为线性函数——单纯形法，内点法

   凸优化：目标函数和约束函数都为凸函数——内点法（10-100步）

   最小二乘问题：没有约束条件，目标函数是若干项的平方和

4. 符号定义：domf 表示f的定义域

$$
y = a*x_1 + (1-a) * x_2 = a * (x_1 - x_2) + x_2k
$$



## class 2 仿射/凸/凸锥 + 集/组合/包

**Chapter 2 Convex Sets**

直线
$$
x_1 \neq x_2 \in R^n \quad \theta \in R \\
y = \theta x_1 + (1 - \theta) x_2 = x_2 + \theta(x_1 - x_2) \\
$$
**仿射集**：一个集合c是仿射集，若$\forall x_1, x_2 \in c$，则连接$x_1, x_2$的**直线**也在集合内

**仿射组合**：仿射集定义的泛化
$$
x_1, ..., x_k \in c, \quad 
\theta_1 + ... + \theta_k = 1\\
\theta_1 x_1 + ... + \theta_k x_k \in c
$$
子空间：v被称为**仿射集**c的子空间，若
$$
v = c - x_0 =\{x - x_0 | x \in c \}
$$
子空间的性质更好：

- 不需要满足系数和为1。
- 子空间一定经过原点。（因为是相对于c内的一个点的平移）

$$
x_1, ..., x_k \in v, \quad 
\theta_1, ..., \theta_k \in R\\
\theta_1 x_1 + ... + \theta_k x_k \in v
$$

例题：证明线性方程组的解集是仿射集

**仿射包**：对任意集合c，构造尽可能小的仿射集
$$
aff\ C = \{\theta_1 x_1 + ... + \theta_k x_k | \forall x_1, ..., x_k \in c, \theta_1 +...+ \theta_k = 1\}
$$


线段
$$
x_1 \neq x_2 \in R^n \quad \theta \in R \\
y = \theta x_1 + (1 - \theta) x_2  \quad \theta \in [0, 1]
$$
**凸集**：一个集合是凸集，当任意两点之间的**线段**仍然在c内

**凸组合**：凸集定义的泛化
$$
x_1, ..., x_k \in C, \\
\theta_1 + ... + \theta_k = 1, \quad
\theta_1, ..., \theta_k \in [0, 1] \\
\theta_1 x_1 + ... + \theta_k x_k \in C
$$
**凸包**：对任意集合c，构造尽可能小的凸集
$$
Conv\ C = \left\{\theta_1 x_1 + ... + \theta_k x_k \mid
\begin{array}{lcl} 
\forall  x_1, ..., x_k \in C \\ 
\forall \theta_1, ..., \theta_k \in [0, 1] \\ 
\theta_1 +...+ \theta_k = 1
\end{array} \right\}
$$
锥Cone与凸锥Convex Cone

锥：$\forall x \in C, \theta \geq 0, 有 \theta x \in C$

**凸锥**：$\forall x_1, x_2 \in C, \theta_1, \theta_2 \geq 0, 有 \theta_1 x_1 + \theta_2 x_2 \in C$

**凸锥组合**：凸锥定义的泛化
$$
x_1, ..., x_k \in C, \\
\theta_1, ..., \theta_k \geq 0\\
\theta_1 x_1 + ... + \theta_k x_k \in C
$$
**凸锥包**：对任意集合c，构造尽可能小的凸集
$$
Conv\ C = \left\{\theta_1 x_1 + ... + \theta_k x_k \mid
\begin{array}{lcl} 
\forall  x_1, ..., x_k \in C \\ 
\forall \theta_1, ..., \theta_k \geq 0
\end{array} \right\}
$$


## class 3 几种重要的凸集

几种重要的凸集：

- $R^n$空间
- $R^n$空间的子空间
- 任意直线

- $\{x_0 + \theta v \mid \theta \geq 0\} \quad x_0 \in R^n, \theta \in R, v \in R^n$
  - 当v=0时为仿射集
  - 一般情况下是凸集
  - 当$x_0 = 0$时为凸锥
- 超平面与半空间：$\{x \mid a^T x = b\} \quad, x, a \in R^n, b \in R, a \neq 0$是一个超平面，超平面将空间分为两个半空间
  - 超平面是仿射集和凸集，当过原点时是凸锥
  - 半空间是凸集但不是仿射集，当超平面过原点时半空间是凸锥

- 球和椭球
  - 球 $B(x_c, r) = \{x \mid \Vert x- x_c \Vert_2 \leq r\}$
  - 椭球 $\varepsilon(x_c, P) = \{x \mid (x- x_c)^T P^{-1}(x- x_c) \leq 1\}, P \in S^n_{++}$

- 多面体Polyhedron 

$$
P = \left\{x \mid
\begin{array}{lcl} 
a^T_j x \leq b_j & j = 1,...,m \\ 
c^T_j x = d_j & j = 1,...,p
\end{array} \right\}
$$

- 单纯形 Simplex：在$R^N$空间中选择$v_0, ..., v_k$共k+1个点，$v_1 - v_0, ..., v_k - v_0$线性无关，则与上述点相关的单纯形为

$$
C = Conv\{v_0, ..., v_k\}
$$

对于n维空间，最多只能找到n+1个点组成单纯形；

例题：证明Simplex是polyhedron的一种

- 对称矩阵集合 $S^n = \{x \in R^{n \times n} \mid x = x^T\}$
  - 是凸集，且是凸锥
- 对称半正定矩阵集合 $S^n_+ = \{x \in R^{n \times n} \mid x = x^T\, x \succeq 0 \}$
  - 是凸集，且是凸锥
- 对称正定矩阵集合 $S^n_{++} = \{x \in R^{n \times n} \mid x = x^T, x \succ 0 \}$
  - 是凸集，不是凸锥
  - 对于一维情况，不包含原点一定不是凸锥
  - 对于高维情况，因为凸锥组合中，$\theta$可以全部为0，所以不能满足$x^TAx + x^TBx \succ 0$要求，因此不满足凸锥的定义





## class 4 保凸运算

哪些变换可以保持集合的凸性：

- 交集：若$S_a$为凸集，$\forall a \in A$，则$\cap_{a \in A} S_2$为凸集
- 仿射函数：$f: R^n \rightarrow R^m$是仿射的，当$f(x) = Ax + b, A \in R^{m \times n}, b \in R^m$，若$S \subset R^n$为凸，则$f(S) = \{f(x) \mid x \in S \}$为凸
- 两个凸集的和是凸的 $S_1 + S_2 = \{x + y \mid x \in S_1, y \in S_2 \}$，相当于对两个集合的笛卡尔积$S_1 \times S_2 = \{(x, y) \mid x \in S_1, y \in S_2 \}$做仿射变换

例题：线性矩阵不等式LMI的解集是凸集

例题：椭球是球的仿射映射

- 透视函数是保凸的

$$
P: R^{n + 1} \rightarrow R^n, dom P = R^n \times R_{++}\\ P(z, t) = \frac{z}{t}, z \in R^n, t \in R_{++}
$$



- 反透视函数是保凸的

$$
P^{-1}(c) = \{(x, t)\in R^{n + 1} \mid \frac{x}{t} \in c, t \gt 0 \}
$$

- 线性分式函数（相当于一个仿射函数复合一个透视函数）是保凸的


$$
g: R^n \rightarrow R^{m+1} \\
g(x) = \left[\begin{matrix}A \\ C^T \end{matrix} \right] x + \left[\begin{matrix}b \\ d \end{matrix} \right]  \\
A \in R^{m \times n} \quad
b \in R^m \quad
C \in R^n \quad
d \in R \\
P: R^{m + 1} \rightarrow R^m \\
f: R^n \rightarrow R^m = P \circ g \\
f(x) = \frac{Ax + b}{C^T x +b} dom f = \{x \mid C^T x +d \gt 0\}
$$



## class 5 凸函数的扩展

凸函数的扩展
$$
f: R^n \rightarrow R 为凸，dom f = C \subseteq R^n
\\
\tilde f = \left\{ \begin{array}{rcl}
 f(x) & x\in domf \\
 \inf & x \notin domf \\
 \end{array}\right.
$$

一阶条件：

设$f: R^n \rightarrow R$可微，即梯度$\grad f$在$domf$上均存在（隐含domf为开集），则f为凸函数等价于

- $dom f$ 为凸
- $f(y) \geq f(x) + \grad f^T(x)(y - x) , \forall x, y \in dom f$（凸函数最重要的性质，与优化问题密切相关）



例题：证明一阶条件



## class 6 凸函数的四个定义，常见函数的凸性

凸函数的定义：

- 定义1

$$
f: R^n \rightarrow R 为凸 \Leftrightarrow 
\left\{ \begin{array} \\
domf为凸 \\
\forall x,y \in domf, 0 \leq \theta \leq 1,
f(\theta x + (1 - \theta) y) \leq \theta f(x) + (1 - \theta) f(y)
\end{array}
\right.
$$

- 定义2

$$
\forall x \in domf, \forall v \\
g(t) = f(x +tv)为凸，domg = \{t \mid x + tv \in domf \}
$$

- 定义3（一阶条件）

$$
若f可微，\forall x, y \in domf \\
f(y) \geq f(x) + \grad f^T(x)(y - x)
$$

- 定义4（二阶条件）

$$
若f:R^n \rightarrow R二阶可微，则f为凸 \Leftrightarrow domf为凸且\grad^2f(x) \succeq 0, \forall x \in domf  
$$



考虑以下函数是否为凸函数：

- 仿射函数 $f(x) = Ax + b$ ，利用二阶条件： $\grad^2 f(x) = 0$ 既是凸函数又是凹函数
- 指数函数 $f(x) = e^{ax}, x \in R$ ，利用二阶条件： $\grad^2 f(x) = a^2 e^{ax} \geq 0$
- 幂函数$f(x) = x^a, x \in R_{++}$，利用二阶条件： $\grad^2 f(x) = a(a-1)x^{a-2}$，当$a \geq 1 || a \leq 0$ 为凸函数，当$0 \leq a \leq 1$ 为凹函数，
- 绝对值函数
- 对数函数
- 负熵函数
- 范数（根据定义可证是凸函数）
- 零范数（不是范数也不是凸函数）
- max函数
- max的解析逼近——log-sum-up函数





## class 7 保持函数凸性的操作

- 非负加权和：

$$
f_1, ..., f_m为凸，w_i \geq 0 \forall i, 则f = \sum_{i=1}^m w_i f_i为凸
$$



- 积分

$$
若对任意 y \in A, f(x,y)均为凸，设w(y) \geq 0, \forall y \in A \\
g(x) = \int_{y \in A} w(y)f(x,y) dy 为凸
$$

- 仿射变换
- 两个函数的极大值函数

$$
f_1, f_2 为凸\\
定义f(x) = max\{f_1(x), f_2(x)\}, domf = domf_1 \cap domf_2 \\
f(x) 也为凸函数
$$

- 无限个凸函数的极大值

$$
f(x,y)对于x为凸，\forall y \in A \\
g = \sup_{y \in A} f(x,y)
$$

- 函数的组合

$$
h: R^k \rightarrow R \\ 
g: R^n \rightarrow R^k \\
f = h \circ g: R^n \rightarrow R \\
f(x) = h(g(x)), domf = \{x \in domg \mid g(x) \in domh \}
$$

当 n = k = 1:
$$
f'(x) = h'(g(x))g'(x) \\
f''(x) = h''(g(x))g'(x)^2 + h'(g(x))g''(x) \\
$$

1. h为凸，且不减，g为凸，则f为凸
2. h为凸，且不增，g为凹，则f为凸
3. h为凹，且不减，g为凹，则f为凹
4. h为凹，且不增，g为凸，则f为凹

放宽条件：
$$
\left\{\begin{array}{lcl}
n, k \geq 1\\
domg, domh, domf \neq R^n, R^k, R^n\\
h,g均不二阶可微
\end{array}\right.
$$
首先扩展h(x)为$\tilde h(x)$，使其定义域为整个空间，则有

1. h为凸，且$\tilde h(x)$不减，g为凸，则f为凸
2. h为凸，且$\tilde h(x)$不增，g为凹，则f为凸
3. h为凹，且$\tilde h(x)$不减，g为凹，则f为凹
4. h为凹，且$\tilde h(x)$不增，g为凸，则f为凹



- 函数的透视

透视函数：
$$
P: R^{n + 1} \rightarrow R^n, domP \in R^n \times R_{++} \\
P(z,t) = \frac{z}{t}
$$
函数的透视：
$$
f: R^n \rightarrow R \\
g: R^n \times R_{++} \rightarrow R \\
g(x,t) = tf(\frac{x}{t}) \\
domg = \{(x,t) \mid t > 0, \frac{x}{t} \in domf \}
$$
 f为凸，g为凸；f为凹，g为凹；



- 函数的共轭（Conjugate）

$$
f: R^n \rightarrow R \\
f^*: R^n \rightarrow R \\
f^*(y) = \sup_{x \in domf} (y^T x - f(x))
$$

函数共轭的性质：

1. f(x)若可微，则$f^*(y)$对应的x必是$f'(x) = y$的一点
2. 函数的共轭一定是凸函数 
2. 若f非凸，$f^{**} \neq f$；若f凸，且为闭函数，则$f^{**} = f$





## class 8 凸集与凸函数的关系，拟凸函数

$\alpha-sublevel \quad set$：

若$f: R^n \rightarrow R$定义其$\alpha - sublevel \ set$为
$$
C_{\alpha} = \{x \in domf \mid f(x) \leq \alpha \}
$$

- 凸函数所有的$\alpha-sublevel \ set$都是凸集
- 若函数f所有的$\alpha-sublevel \ set$都是凸集，则f不一定为凸函数



### Quasi Convex Function 拟凸函数

$f: R^n \rightarrow R$

Quasi Convex: $S_{\alpha} = \{x \in domf \mid f(x) \leq \alpha \}, \forall \alpha 为凸$

Quasi Concave: $S_{\alpha} = \{x \in domf \mid f(x) \geq \alpha \}, \forall \alpha 为凸$

Quasi Linear: $S_{\alpha} = \{x \in domf \mid f(x) = \alpha \}, \forall \alpha 为凸$

拟凸函数也被称为单模态函数(Unimodal function)

回顾凸函数的定义：
$$
f: R^n \rightarrow R 为凸 \Leftrightarrow 
\left\{ \begin{array} \\
domf为凸 \\
\forall x,y \in domf, 0 \leq \theta \leq 1,
f(\theta x + (1 - \theta) y) \leq \theta f(x) + (1 - \theta) f(y)
\end{array}
\right.
$$
对于拟凸而言，则是
$$
f: R^n \rightarrow R 为凸 \Leftrightarrow 
\left\{ \begin{array} \\
domf为凸 \\
\forall x,y \in domf, 0 \leq \theta \leq 1,
f(\theta x + (1 - \theta) y) \leq \max\{f(x), f(y)\}
\end{array}
\right.
$$
例：线性分数函数



## class 9 拟凸函数，拟凸问题

Quiz #1

证明：几何平均函数
$$
f(x) = (x_1* x_2* ... * x_n)^{\frac{1}{n}}, x \in R^n \\
在 domf = R^n_{++} \cap \{\Vert R \Vert_2 \leq 1 \}上为凹
$$

### 如何将非凸问题转换为拟凸问题/凸问题

例：向量的零范数$ x \in R^n, f(x) = \Vert x \Vert_0 $

在一维的时候是一个拟凸函数；在二维以及更高维的时候不是拟凸函数



这里提及了另一个问题：$\min \Vert x \Vert_0 \quad s.t. x \in C$ 因为零范数不是凸的也不是拟凸的，因此这个问题是不好求解的。

于是想把这个问题松弛为一个更易求解的问题：$ \min \Vert x \Vert_1 \quad s.t. x \in C $ 因为1-范数属于范数，因此这个问题是凸的

但是由于零范数和1-范数的函数差别比较大，这样的松弛也不是很好的选择，应该用一个更接近的函数去做松弛。对于零范数而言，$ log(ax^2 + 1) $就是一个更好的选择。对于高维而言，即 $ \min log(x^T x + 1) \quad s.t. x \in C $。同时这个函数是一个拟凸函数





### 可微拟凸函数一阶条件

凸：$domf为凸，f(y) \geq f(x) + \grad^T f(x)(y - x), \forall x,y \in domf$

拟凸：$domf为凸，f(y) \leq f(x) \Rightarrow \grad^Tf(x)(y - x) \leq 0, \forall x,y \in domf$

凸：若$\grad^T f(x) = 0, 则 \forall y, f(y) \geq f(x) $

拟凸：若$\grad^T f(x) = 0, 则 \forall y, f(y) \leq f(x) \Rightarrow 0 \leq 0 $

**课里给出了证明**



### 可微拟凸函数二阶条件

凸：$domf为凸，且\grad^2 f(x) \geq 0, \forall x \in domf $

拟凸：$domf为凸，且y^T \grad f(x) \geq 0 \Rightarrow y^T \grad^2 f(x) y \geq 0$

即对于拟凸函数而言，只要求在$f'(x) = 0 且 y \neq 0$的点满足$f''(x) \geq 0$即可，而不要求在所有点都满足



### 对数凹与对数凸函数

$$
f: R^n \rightarrow R 为log \ concave,若f(x) > 0, \forall x \in domf,且logf为凹函数
$$

$$
f: R^n \rightarrow R 为log \ convex,若f(x) > 0, \forall x \in domf,且logf为凸函数
$$

若f为log convex，则f为convex

若f为concave，f > 0，则f为log concave



chapter2课后习题：2.1, 2.2, 2.5, 2.7, 2.10, 2.16, 2.18, 2.19

chapter3课后习题：3.1, 3.2, 3.5, 3.13, 3.18, 3.21, 3.32, 3.33, 3.36, 3.43

## class 10 凸优化问题 (ep 21开始)

广义：凸目标，凸集约束

- 一般优化问题的描述：

$$
\min f_0(x) \\
s.t. \ f_i(x) \leq 0, i = 1,...,m \\
h_i(x) = 0, i = 1, ..., p
$$

- 优化问题的域domin：

$$
D = \bigcap_{i = 0}^m dom f_i \cap \bigcap_{i=1}^p dom h_i
$$

- 可行解集 feasible set：

$ x \in D $为可行解，若$ f_i(x) \leq 0, i = 1,...,m \quad h_i(x) = 0, i = 1,...,p$

$ x_f = \{x为可行解\} $

- 问题的最优值 optimal value：$p^* = \inf \{f_0(x) \mid x \in x_f \}$

若x_f为空集： $ p^* = +\infty $

- 最优解 optimal point/solution：若$x^*$可行，且$f_0(x^*)= p*$

- 最优解集 optimal set：$ x_{opt} = \{x \mid x \in x_f, f_0(x) = p^* \} $

- $\varepsilon$次优解集($\varepsilon$-suboptimal set)：$ x_{\varepsilon} = \{ x \mid \in x_f, f_0(x) \leq p^* + \varepsilon \} $

- 局部最优解 locally optimal：

$$
f_0(x) = \inf \{f_o(z) \mid 
\begin{array}{lcl} 
f_i(z) \leq 0, i = 1,...,m \\ 
h_i(z) = 0, i = 1,...,p \\
\Vert z - x \Vert \leq R
\end{array}\}
$$

为什么都是小于等于号，而不是小于号？因为只有小于号的不等式约束一定可以转换为小于等于号的不等式约束

- 可行性优化问题feasibility problems

$$
find \ x \\
s.t. f_i(x) \leq 0, i = 1,...,m \\
h_i(x) = 0, i = 1,...,p
$$

狭义凸优化问题：
$$
\min f_0(x) \\
s.t. \ f_i(x) \leq 0, i = 1,...,m \\
a_i^T x = b_i, i = 1, ..., p \\
其中f_i(x), i = 0,...,m为凸函数
$$
对于狭义凸优化问题：

- 可以将等式约束消去，使求解变量降维
- 也可以添加松弛变量，使求解变量升维



Quasi-convex Optimization：与凸优化问题的区别只有目标函数是拟凸函数

其他问题都被称为非凸问题



凸优化问题的性质：

- 局部最优解 = 全局最优解

- 可微目标函数情况下的最优解

$$
X_f = \{x \mid 
\begin{array}{lcl}
f_i(x) \leq 0, i = 1,...,m \\
h_i(x) = 0, i = 1,...,p
\end{array}
\} \cap domf_0 \\

x^* \in X_f 最优 \Leftrightarrow \grad f_0^T(x^*) (y - x^*) \geq 0, \forall y \in X_f
$$



### 典型的凸问题

- 线性规划：目标与约束均为线性

$$
\min C^T x + d  \quad C \in R^n, d \in R \\
s.t. \quad Gx \leq h, G \in R^{m \times n}, h \in R^m \\
Ax = b, A \in R^{k \times n}, b \in R^k
$$



## class 11 (ep 25,26) 线性规划、二次规划问题

### 线性规划问题


$$
\min C^T x + d \\
s.t. Gx \leq h \\
Ax = b
$$
等价变换
$$
\min C^T x + d \\
s.t. Gx + S = h \\
Ax = b \\
S \geq 0
$$
因此，线性规划的一般形式为
$$
\min C^T x + d \\
s.t. Ax = b \\
x \geq 0
$$


### 二次规划 Quadratic Programming

$$
\min \frac{1}{2} x^T Px + q^T x + r , P \in S^n_+ \\
s.t. Gx \leq h \\
Ax = b
$$



### 二次约束二次规划 Quadratically Constrained Quadratic Programming(QCQP)

$$
\min \frac{1}{2} x^T P x+ q^T + r , P \in S_+^n\\
s.t. \frac{1}{2} x^T P_i x + q_i^T x + r_i \leq 0 , P_i \in S_+^n, i = 1,...,m\\
Ax = b \\
$$







## class 12 （ep 27,28）

### Semi-Definite Programming 半正定规划

$$
\min tr(Cx) \\
s.t. tr(A_i x) = b_i, i = 1,...,p \\
x \succeq 0  \\
x \in S_+^n, C \in R^{n \times n}, A_i \in R^{n \times n}, b_i \in R
$$

$$
\min C^T x \\
s.t. x_1 A_1 + ... + x_n A_n \leq B \\
x \in R^n, B \ A_1 ... A_n \in S^k, c \in R^n
$$



### 多目标优化问题

$$
\min f_0(x) \\
s.t. f_i(x) \leq 0, i = 1,...,m \\
h_i(x) = 0, i = 1,...,p \\
f_0: R^n \rightarrow R^q, f_i: R^n \rightarrow R, h_i: R^n \rightarrow R
$$

Pareto optimal front: 这个面上的任一点，若找到另一解使之在某指标上更优，避灾另外某指标上变得更差

Pareto optimal value，Pareto optimal point

若{f_0(x)}在 R^k中为凸，f_i(x)为凸，h_i(x)为仿射，则必可通过下述方法求得Pareto front中一点
$$
\min \sum_{i=1}^q \lambda_i f_{0i}(x), \lambda_i \geq 0 \\
s.t. f_i(x) \leq 0, i = 1,...,m \\
h_i(x) = 0, i = 1,...,p
$$
举例： Ridge Regression（岭回归问题）——一下三种形式是等价的
$$
b = Ax + e, A \in R^{m \times n}, b \in R^m, e \in R^m, x \in R^n
$$
形式1：多目标优化问题
$$
\min \Vert b - Ax \Vert_2^2 \\
\min \Vert x \Vert_2^2
$$
形式2：岭回归问题（将多目标问题转换为单目标问题）
$$
\min \Vert b - Ax \Vert_2^2 + \lambda \Vert x \Vert_2^2
$$
形式3：岭回归问题等价形式（带约束的单目标问题，与形式2通过拉格朗日乘子实现等价）
$$
\min \Vert b - Ax \Vert_2^2 \\
s.t. \Vert x \Vert_2^2 \leq \xi
$$


## class 13 Duality对偶性

Chapter 5 Duality

对于一个一般问题
$$
\min f_0(x) \\
s.t. f_i(x) \leq 0, i = 1,...,m \\
h_i(x) = 0, i = 1,...,p \\
x \in R^n, D = \bigcap_{i = 0}^m domf_i \cap \bigcap_{i = 1}^p dom h_i \\
p^* \text{ optimal value}
$$
 Lagrangian function / Lagrangian:其中$\lambda, v$称为Lagrange Multiplier 	 
$$
L(x, \lambda, u) = f_0(x) + \sum_{i = 1}^m \lambda_i f_i(x) + \sum_{i = 1}^p v_i h_i(x) \\
\lambda \in R^m, v \in R^p
$$
Lagrange Dual Function / Dual Function
$$
g(\lambda, u) = \inf_{x \in D} L(x, \lambda, u)
$$
对偶函数的性质：

- 对偶函数为凹函数
- $\forall \lambda \geq 0, \forall v, g(\lambda, v) \leq p^*$
  - 证明：$\forall \lambda \geq 0, \forall v, \sum_{i = 1}^m \lambda_i f_i(x) + \sum_{i = 1}^p v_i h_i(x) \leq 0$
  - 因此$L(x^*, \lambda, v) = f_0(x^*) + \sum_{i = 1}^m \lambda_i f_i(x^*) + \sum_{i = 1}^p v_i h_i(x^*) \leq f_0(x^*) = p^*$
  - 所以$g(\lambda, v) \leq L(x^*, \lambda, v) \leq p^*$



例1：
$$
\min x^Tx \\
s.t. Ax = b \\
x \in R^n, b \in R^p, A \in R^{p \times n}
$$
于是可以构建拉格朗日函数和对偶函数为
$$
L(x, v) = x^Tx + v^T (Ax - b) \\
g(v) = \inf_{x \in D} L(x, v) = \inf (x^Tx +v^TAx - v^Tb)
$$
对对偶函数求导并求解可得
$$
g'(v) = 2x + A^T v \Rightarrow x = - \frac{A^Tv}{2} \\
g(x^*) = \frac{v^T A A^T v}{4} - \frac{v^T A A^T v}{2} - v^Tb = - \frac{v^T A A^T v}{4} - b^T v
$$
于是变成了一个关于v的凹函数



例2：
$$
\min c^T x \\
s.t. Ax = b \\
-x \leq 0 \\
L(x, \lambda, v) = c^T x - \lambda^T x + v^T(Ax - b) \\
= -b^T c + (c + A^T v - \lambda)^T x \\
g(\lambda, v) = \inf_x L(x, \lambda, v) = \left\{
\begin{array}{lcl}
-b^T v & A^T v - \lambda + c = 0 \\
- \infty & \text{otherwise}
\end{array}\right.
$$



例3：
$$
\min x^T w x \\
s.t. x_i = \pm 1, i = 1,...,m \\
L(x, v) = x^T w x + \sum_{i = 1}^n (x_i^2 - 1) \\
= x^T(w + Diag(v)) x - 1^T v \\
g(v) = \inf_x x^T(w + Diag(v)) x - 1^T v = \left\{
\begin{array}{lcl}
-1^T v & w + Diag(v) \succeq 0 \\
- \infty & \text{otherwise}
\end{array}\right.
$$



### 函数的共轭: ###

$f^*$是$f: R^n \rightarrow R$的共轭，若
$$
f^*(y) = \sup_{x \in domf} (y^Tx - f(x))
$$

例1：本例表现对偶函数与函数共轭之间的转换关系
$$
\min f(x) \\
s.t. x = 0
$$

对偶函数：
$$
L(x, v) = f(x) + v^T x, dom L = domf \times R^n \\
g(v) = \inf_{x \in domf} (f(x) + v^T x) \\
= - \sup_{x \in domf} (-v^T x - f(x)) \\
= -f^*(-v)
$$


例2：
$$
\min f_0(x) \\
s.t. Ax \leq b \\
cx = d \\
L(x, \lambda, v) = f_0(x) + \lambda^T (Ax - b) + v^T (cx - d) \\
= f_0(x) + (\lambda^T A + v^T c) x - \lambda^T b - v^T d \\
g(\lambda, v) = \inf_{x \in domf} L(x, \lambda, v) \\
= - f_0^*(-(\lambda^T A + v^T c) ^T) - \lambda^T b - v^T d \\
$$

因此可以构造这样一个原问题的对偶问题(D) Dual problem / Lagrange problem，最优值为$d^*$
$$
\max g(\lambda, v) \\
s.t. \lambda \geq 0 
$$

原问题(P)：
$$
\min f_0(x) \\
s.t. f_i(x) \leq 0,  i = 1,...,m \\
b_i(x) = 0, i = 1,...,p
$$

对偶问题的性质：

- 对偶问题是一个凸问题

- $d^* \leq p^*$ weak duality弱对偶，当等号成立时为strong duality强对偶
- $p^* - d^*$ Duality gap对偶间隙 
- $\lambda^*, v^*$: optimal lagrange multiplier


例3：
$$
\min c^T x \\
s.t. Ax = b \\
x \geq 0 \\
g(\lambda, v) = \inf_x L(x, \lambda, v) = \left\{
\begin{array}{lcl}
-b^T v & A^T v - \lambda + c = 0 \\
- \infty & \text{otherwise}
\end{array}\right.
$$
因为下界为负无穷没有意义，因此把对偶问题转换为如下问题：
$$
(D) \Rightarrow \max g(\lambda, v) \\
s.t. \lambda \geq 0 \\
\Leftrightarrow \max -b^T v \\
s.t. \lambda \geq 0 \\
A^T v - \lambda + c = 0 \\
\Leftrightarrow \min b^T v \\
s.t. A^T v + c \geq 0
$$


例4：
$$
\min c^T x \\
s.t. Ax \leq b \\
\Rightarrow L(x, \lambda) = c^T x + \lambda^T (Ax - b) \\
= (c + A^T \lambda)^T x - b^T \lambda \\
\Rightarrow g(\lambda) = \inf_x L(x, \lambda) \\
= \left\{
\begin{array}{lcl}
-b^T \lambda & A^T \lambda + c = 0 \\
- \infty & \text{otherwise}
\end{array}\right.
$$
对偶问题：
$$
(D) \Rightarrow \max -b^T \lambda \\
s.t. A^T \lambda + c = 0 \\
\lambda \geq 0
$$
将例3的原问题与例4的对偶问题对比
$$
\left(
\begin{array}{cc}
\min c^T \lambda \\
s.t. A\lambda = b \\
\lambda \geq 0 \\
\end{array}
\right)
\mbox{~and~} 
\left(
\begin{array}{cc}
\min b^T \lambda \\
s.t. -A^T \lambda = c \\
\lambda \geq 0
\end{array}
\right)
$$
将例3的对偶问题与例4的原问题对比
$$
\left(
\begin{array}{cc}
\min b^T x \\
s.t. A^T x + c \geq 0 \\
\end{array}
\right)
\mbox{~and~} 
\left(
\begin{array}{cc}
\min c^T x \\
s.t. -Ax + b \geq 0 \\
\end{array}
\right)
$$
因此可知，例3的对偶问题的对偶问题，就是自身。但并不是所有的问题都有这样的性质，因为对偶问题一定是凸的，因此一个非凸问题的对偶问题的对偶问题就一定不是自身

那么对偶问题的对偶问题等于自身？什么时候原问题的解与对偶问题的解相同？



## class 14 (ep 31-32)

对任意的优化问题(P)
$$
\min f_0(x) \\
s.t. f_i(x) \leq 0, i = 1,...,m \\
h_i(x) = 0, i = 1,...,p \\
D: \text{domin} \quad p^*: \text{optimal value}
$$


D的Realative Interior相对内部：简单翻译就是原集合对应的一个开集（即去掉了原边界）
$$
\text{Relint} D = \{x \in D \mid \exist r > 0, B(x, r) \cap \text{aff} D \leq D\}
$$


### Slater's Condition (什么时候 $p^* = d^*$，充分条件)

若有凸问题
$$
\min f_0(x) \\
s.t. f_i(x) \leq 0, i = 1,...,m \\
A x = b
$$
其中$\forall i, f_i(x) $为凸

slater's condition: 注意下面的是<号，不是等号
$$
\text{if } \exist x \in \text{relint}D, f_i(x) < 0, i = 1,..,m, Ax = b \\
p^* = d^*
$$
wearker slater's condition  :

若不等式约束为仿射时，只要可行域非空，必有$p^* = d^*$



例：
$$
\begin{array}{cc}
\min x^T x \\
s.t. Ax = b
\end{array}
\Leftrightarrow
\begin{array}{lcl}
\max\limits_{v} -\frac{1}{4}v^TAA^Tv - b^Tv
\end{array}
$$
例：QCQP问题
$$
\min \frac{1}{2}x^T P_0 x + q_0^T x + r_0 \\
s.t. \frac{1}{2}x^T P_i x + q_i^T x + r_i, i = 1,...,m \\
P_0 \in S_{++}^n, P_i \in S_+^n \\
\Rightarrow L(x, \lambda) = \frac{1}{2}x^T P_0 x + q_0^T x + r_0 + 
\sum_{i = 1}^m (\frac{1}{2}\lambda_i x^T P_i x + \lambda_i q_i^T x + \lambda_i r_i) \\
= \frac{1}{2} x^T (P_0 +\sum_{i = 1}^m \lambda_i P_i)x + (q_0 + \sum_{i = 1}^m \lambda_i q_i)^T x + (r_0 +\sum_{i = 1}^m \lambda_i r_i) \\
= \frac{1}{2} x^T P x + Q^T x + R
$$
对偶函数
$$
g(\lambda) = \inf_x L(x. \lambda) \\
\Rightarrow \max -\frac{1}{2} Q^T(\lambda) P^{-1}(\lambda)Q(\lambda) + r(\lambda) \\
s.t. \lambda \geq 0 
$$

### 几何解释 $P^* = d^*$

$$
\min f_0(x) \\
s.t. f_i(x) \leq 0, i = 1,...,m
$$



取 i = 1，即
$$
\min f_0(x) \\
s.t. f_1(x) \leq 0 \\
$$
定义
$$
G = \{(f_1(x), f_0(x) \mid x \in D\} \\
p^* = \inf \{t \mid (u, t) \in G, u \leq 0\} \\
g(\lambda) = \inf \{\lambda u + t \mid (u, t) \in G\}
$$
具体几何图示解释见ep32, 5min左右



### 从鞍点(Saddle Point)的角度解释

考虑$L(x, \lambda)$

鞍点：$(x^*, \lambda^*)$
$$
\inf_{x \in D} \sup_{\lambda \geq 0} L(x, \lambda) = 
\sup_{\lambda \geq 0} \inf_{x \in D} L(x, \lambda)
$$
而根据之前对于对偶问题的讨论，有
$$
d^* = \sup_{\lambda \geq 0} \inf_{x \in D} L(x, \lambda)
$$
对于原问题
$$
\min f_0(x) \\
s.t. f_i(x) \leq 0, i = 1,...,m \\
$$
有
$$
\Rightarrow 
\sup_{\lambda \geq 0} L(x, \lambda) = 
\sup_{\lambda \geq 0} (f_0(x) + \sum_i \lambda_i f_i(x)) = \\
\begin{cases}
f_0(x)    & \quad \forall i, f_i(x) \leq 0\\
\infty    & \quad \exist i, f_i(x) > 0
\end{cases} \\
\therefore p^* = \inf_{x \in D} \sup_{\lambda \geq 0} L(x, \lambda)
$$
因此，如果存在$(x^*, \lambda^*)$为鞍点，即可知$d^* = p^*$



### 从多目标优化角度解释

略



### 从经济学角度解释

略，就是举了一个最优价格问题的例子来表述











# Bottom
