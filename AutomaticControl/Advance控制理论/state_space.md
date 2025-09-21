# Advanced控制理论
状态-空间表达 State-Space Representation
1. 状态方程（描述状态变量的变化率）：
   $$\dot{\mathbf{x}} = \mathbf{A}\mathbf{x} + \mathbf{B}u$$
   其中 $\dot{\mathbf{x}} = \begin{bmatrix} \dot{x}_1 \\ \dot{x}_2 \end{bmatrix}$，$\mathbf{x} = \begin{bmatrix} x_1 \\ x_2 \end{bmatrix}$


2. 输出方程（描述输出与状态的关系）：
   $$y = \mathbf{C}\mathbf{x} + \mathbf{D}u$$
![弹簧阻尼实例](AutomaticControl/Advance控制理论/images/1.png)
```angular2html
输入:U(x) = f(t)
```