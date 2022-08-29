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
f(y) \geq f(x) + \bigtriangledown f(x)^T (y-x)
$$

4. 第四种定义：二阶条件，假设f二阶可微，即Hessian矩阵或者二阶导数$\bigtriangledown ^2f$​存在，则f是凸函数的充要条件是

$$
\forall x \in \bold{dom}f, \bigtriangledown ^2 f(x) \succeq 0
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
   f^*(y) = x^{*T} \bigtriangledown f(x^*) - f^*(x) 
   $$

3. 共轭的共轭：
   f是凸函数，$\bold{epi}f$是闭集，则$f(x) = f^{**}(x)$ 
   f不非凸，$f^{**}(x)$仍然是一个凸函数，而且包住了原有的 $f(x)$



## Section 4：拟凸函数——有全局最小值

1. 函数$f:R^n \rightarrow R$称为拟凸函数，（或者单峰函数），如果其定义域及所有下水平集$S_\alpha = \{ x \in \bold{dom}f \mid f(x) \leq \alpha \}, \alpha \in R$ 都是凸集
2. 第二种定义（Jensen不等式）：$f(\theta x + ( 1 - \theta) y) \leq \max\{f(x), f(y)\}, \forall x,y \in \bold{dom}f, \theta \in [0, 1]$
3. 第三种定义（高维到低维）：$f是拟凸函数 \Leftrightarrow g(t) = f(x + tv)是拟凸函数$
4. 第四种定义（拟凸函数一阶条件）：设函数$f:R^n \rightarrow R$可微，则函数f是拟凸的充要条件是，$\bold{dom}f$是凸集且$f(y) \leq f(x) \Rightarrow \bigtriangledown f(x)^T(y - x) \leq 0, \forall x,y \in \bold{dom}f $。
   几何意义：$\bigtriangledown f(x)$ 在点x处定义了水平集$\{y \mid f(y) \leq f(x)\}$的一个支撑超平面

辨析：

- 当f是凸函数，$\bigtriangledown f(x) = 0$表明(x, f(x))为最小值点
- 当f是拟凸函数，$\bigtriangledown f(x) = 0$无意义

5. 第五种定义（二阶条件）：
   （1）假设函数f二次可微，如果函数f是拟凸函数，则
   $$
   y^T \bigtriangledown f(x) = 0 \Rightarrow y^T \bigtriangledown ^2 f(x) y \geq 0, \forall x \in \bold{dom}f, y \in R^n
   $$
   这就意味着当$\bigtriangledown f(x) = 0 $时，$\bigtriangledown ^2 f(x) \succeq 0$；当$\bigtriangledown f(x) \neq 0 $时，$\bigtriangledown ^2 f(x)$在$\bigtriangledown f(x)$的正交补空间（n - 1维）半正定，那么黑塞矩阵最多只有一个负特征值。

   （2）反之，如果下面的成立，则f是拟凸函数
   $$
   y^T \bigtriangledown f(x) = 0 \Rightarrow y^T \bigtriangledown ^2 f(x) y > 0, \forall x \in \bold{dom}f, y \in R^n
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
   logf(x)为凸 \Leftrightarrow \bigtriangledown ^2 logf(x) \succeq 0
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
   x是最优解 \Leftrightarrow \bigtriangledown f_0(x)^T(y - x) \geq 0, \forall y \in X
   $$
   几何解释： 点x是最优解，那么$-\bigtriangledown f_0(x)$定义了X在x处的一个支撑超平面

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

1. 凸函数

2. 凸函数的典型例子：仿射函数(affine function)、范数
   $$
   f(x) = a'x + b
   $$

3. 优化问题
   $$
   minimize \ f_0(x) \\
   subject \ to \ f_i(x) \leq bi, i = 1, ..., m
   $$
   线性规划：目标函数和约束函数都为线性函数——单纯形法，内点法

   凸优化：目标函数和约束函数都为凸函数——内点法（10-100步）

   最小二乘问题：没有约束条件，目标函数是若干项的平方和

4. 符号定义：domf 表示f的定义域



$$
y = a*x_1 + (1-a) * x_2 = a * (x_1 - x_2) + x_2k
$$





















## class 4



透视函数，反透视函数是保凸的

线性分式函数（相当于一个仿射函数复合一个透视函数）是保凸的







