Day 1: Simulating the motion of a mass connected by a spring-damper

# Precautions
This course will proceed based on this document.
Please program according to this material and solve the tasks and questions. (For advanced tasks, only those who have completed all the tasks should solve. Basically, if you solve the tasks, you will pass.)
The answers to the tasks should be compiled into a report. For details, please read the [ReadMe](https://github.com/amby-1/sogoenshu_2023/blob/main/README.md) thoroughly.

# Overview
The explanation will be provided using slides.

# Simulation of motion in the spring-damper system
As an exercise, we will numerically simulate the motion of a mass suspended by a spring while reviewing the numerical solution methods for differential equations we have learned so far.

## Problem setting
Consider a system where, under gravity (gravitational acceleration: $g$), a mass (mass: $m$) is suspended from the ceiling by a linear spring (spring constant: $k$) and a damper (damper constant: $c$).
At this time, define the coordinate system with the equilibrium position of the spring as the origin and the upward vertical direction as positive, and represent the position of the mass as $x$. The position of the tip of the spring when no weight was attached (at its natural length) is represented as $x_n = mg/k$.
Furthermore, an arbitrary external force $u$ is also applied to the mass (in reality, applying an external force without contact is difficult, but it is assumed that an external force can be controlled non-contact using magnetism, etc.).

![Spring Mass Model](Figs/model1.png)

At this time, when an initial position and velocity $(x_0, \dot{x}_0)$ are given to the mass at time $t=0$, verify how the motion of the mass develops over time by numerical calculation.

## Derivation of the equation of motion
The equation of motion for this mass can be easily derived using Newton's equations of motion, but here we will review the derivation method using Euler-Lagrange's equation.
First, it is necessary to define the generalized coordinates $q$ of the system. Here, the position $x$ of the mass is defined as the generalized coordinate $q$. At this time, the Lagrangian $L = T - V$ ( $T$: kinetic energy, $V$: potential energy) can be written as:
$$L = \frac{1}{2} m \dot{q}^2 - m g q - \frac{1}{2} k {(x_n - q)}^2$$
Also, the dissipation function $D$ of the system, considering the resistance force by the damper, can be written as:
$$D = \frac{1}{2} c \dot{q}^2$$
The dissipation function might not have been covered in the course. The dissipation function (scalar) is defined for a resistance force $f_i$ that depends only on velocity as $f_i = - \frac{\partial D}{\partial \dot{q_i}}$ (where $i$ corresponds to the elements of the generalized coordinate $q$).
If the system does not have a generalized force other than the potential and dissipation functions, the time development of the total energy $E$ can be written as $\frac{dE}{dt} = - D$. That is, the physical meaning of the dissipation function represents the energy lost per unit time due to the resistance force.

Next, the generalized force $Q$ of this system can be represented as the external force $u$ applied to the mass.
From the above, using the Euler-Lagrange equation for $q_i$:
$$\frac{d}{dt} \frac{\partial L}{\partial \dot{q}_i} - \frac{\partial L}{\partial q_i} = - \frac{\partial L}{\partial \dot{q}_i} + Q_i $$
The motion equation of this system can be derived as:
$$m \ddot x + c \dot{x} + k {x} = u$$

```
Question 1: Verify the above by actual calculation.
```

## Numerical Solution for the Equation of Motion
In many cases, motion simulations boil down to numerically solving differential equations (equations of motion). In the current problem, when $u$ is a specific function (e.g., when $u$ is not dependent on time), it can be solved analytically (an exact solution can be derived). However, when $u$ becomes an arbitrary time function, it is difficult to obtain an analytical solution. Here, we will calculate the time evolution of this equation of motion (differential equation) using two types of numerical solutions.

### Euler Method
For the differential equation:
$$\dot{x} = f(t, x) $$
By setting a small time step $\Delta t$, the state $x$ at time $t+\Delta t$ is approximated by:
$$x(t+\Delta t) = x(t) + f(t, x) \Delta t $$
This is a calculation where the value of the previous step is added to the product of the slope $f(t, x)$ for the time change and the time step $\Delta t$. It is straightforward to implement, but there is a drawback that errors can easily accumulate.

### Runge-Kutta (4th Order) Method
The Euler method linearly approximated the system's behavior at the point $(x(t); t)$ and determined the value of $x(t + \Delta t)$. Using Taylor's expansion, a more detailed approximation becomes possible. The Runge-Kutta (4th order) method matches the results of the Taylor expansion up to the fourth term of $x(t+\Delta t) $. To perform Taylor expansion, higher-order time derivatives of $x $ (higher-order time derivatives of $f(t,x) $) are needed, but the Runge-Kutta method allows for 4th-order approximation using only $f(t,x)$. Without delving into details, the calculation formula is as follows:
```math
x(t + \Delta t) = x(t) + \frac{k_1 + 2*k_2 + 2*k_3 + k_4}{6} \Delta t
```
Where,
$$k_1 = f(t, \, x(t)), \:   k_2 = f(t + \Delta t /2, \, x(t) + k_1 \Delta t /2) , \: k_3 = f(t + \Delta t /2, \, x(t) + k_2 \Delta t /2) ,  \: k_4 = f(t + \Delta t, \, x(t) + k_3 \Delta t) $$
With this not so complicated calculation, you can achieve 4th order accuracy. Thus, it is a numerical calculation method frequently used in robot simulations, among others.

## Implementation of the Program
We will determine the time evolution of the motion of the mass using the two numerical solutions mentioned above. However, the system's equation of motion:
$$m \ddot x + c \dot{x} + k {x} = u $$ 
is a second-order differential equation, and it needs to be converted into the first-order differential equation discussed above.

As learned in control engineering, the second-order differential equation can be converted to a first-order differential equation by introducing a new variable. In the case of this system, by introducing the variable $ y = \dot x $, it can be reduced to the following first-order differential equation:
```math
\frac{d}{dt}
\left[
\begin{matrix}
x \\ 
y
\end{matrix}
\right]
= 
\left[
\begin{matrix}
y \\ 
(- c \dot{x} - k {x} + u ) /  m 
\end{matrix}
\right]
```
Note that in control, the differentiated variables in the differential equation (in this case, $ (x, y) $) were called the system's state variables.

```
Task 1: Use the Euler and Runge-Kutta methods to write a program that numerically calculates the equation of motion. Define the external force $u$ as a function depending only on time within the program. Also, solve the equation of motion using the numerical examples specified below.
```



