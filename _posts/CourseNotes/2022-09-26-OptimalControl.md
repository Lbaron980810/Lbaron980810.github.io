---
title: Optimal Control and Reinforcement Learning
# tags: 
category: CourseNotes


---

# Lecture 1: Dynamics Review

## 1. Continuous-Time Dynamics

- Most general/generic form for smooth system:

$$
\dot x = f(x, u), x\in \R^n, u \in \R^m
$$

- For a mechanical system:

$$
x = \left [ \begin{array}{lcl}
q \\ v
\end{array}
\right ],
q \text{ is configuration}, v \text{ is velocity}
$$

Example(Pendulum倒立摆): 
$$
ml^2 \ddot \theta +mgl \sin(\theta) = \tau \\
q = \theta, v = \dot \theta, u = \tau \\
x = \left [ \begin{array}{lcl}
\theta \\ \dot \theta
\end{array}
\right ] \Rightarrow 
\dot x = 
\left [ \begin{array}{cc}
\dot \theta \\ -\frac{q}{l}\sin(\theta) + \frac{1}{ml^2}u
\end{array}
\right ] \\
q \in S^1, x \in S^1 \times \R (\text{cylinder})
$$


## 2. Control-Affine Systems

$$
\dot x = f_0(x) + B(x) u \\
f_0(x) \text{ is call drift}, B(x) \text{ is input Jacobian}
$$

- Most systems can be put in this form
- Pendulum:

$$
f_0(x) = 
\left [
\begin{array}{cc}
\dot \theta \\ 
-\frac{q}{l} \sin(\theta)
\end{array}
\right ],
B(x) = 
\left [
\begin{array}{cc}
\dot \theta \\ 
\frac{1}{ml^2}
\end{array}
\right ]
$$

- Manipulator Dynamics:

$$
M(q) \dot v + C(q, v) = B(q) u \\
M(q) \text{ is Mass Matrix}\\
C(q, v) \text{ is Dynamic Bias}\\
B(q) \text{ is Input Jacobian} \\
\dot q = G(q) v \text{ is called velocity kinematics} \\
\Rightarrow
\dot x = f(x, u) =
\left [
\begin{array}{cc}
G(q) v \\ 
-M(q)^{-1} (B(q) u - C)
\end{array}
\right ]
$$

- Pendulum:

$$
M(q) = ml^2, C(q, v) = mgl \sin(\theta), B = I
$$

- All mechanical systems can be written in this form，因为这其实就是重写了Euler-Lagrange equation for $L = \frac{1}{2} v^TM(q) v - V(q)$





# HW/Julia使用教程

## HW0:

for $f(x): \R \to \R$

- 用derivative求导

for $f(x): \R^N \to \R$ 

- 用jacobian求导
- jacobian 是一个行向量；gradient 是jacobian的转置，是列向量

for $f(x): \R^N \to \R^M$ 

- 用gradient求导

```julia
∂foo1_∂x = ForwardDiff.derivative(foo1, x1);
∇foo2 = ForwardDiff.gradient(foo2, x2); 
∇²foo2 = ForwardDiff.hessian(foo2, x2);
∂foo3_∂x = ForwardDiff.jacobian(foo3,x2);
```



Derivatives for functions with multiple input arguments:

```julia
function dynamics(x,a,b,c)
    return [x[1]*a; b*c*x[2]*x[1]]
end
ForwardDiff.jacobian(dx -> dynamics(dx, a, b, c), x1)

function eulers(x,u,J)
    # dynamics when x is angular velocity and u is an input torque
    ẋ = J\(u - cross(x,J*x))
    return ẋ
end
# ∂ẋ/∂x
A = ForwardDiff.jacobian(dx -> eulers(dx, u, J), x);
# ∂ẋ/∂u
B = ForwardDiff.jacobian(du -> eulers(x, du, J), u);
```



Derivatives of composite functions:复合函数求导，两种方法都可以

```julia
der = ForwardDiff.derivative(dx -> g2(f2(dx)), x);
d_f2 = ForwardDiff.derivative(f2, x);
der = d_f2'* ForwardDiff.derivative(g2, f2(x));
```



问题：





