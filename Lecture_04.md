# 深度解析：非线性动力系统的线性化与稳定性分析
# In-Depth Analysis: Linearization and Stability of Nonlinear Dynamical Systems

> **Note**: 本文档包含关于如何将复杂的非线性物理系统转化为线性系统并分析其稳定性的详细数学推导。

---

## Part 1: 理论推导 (Theoretical Derivation)

我们的目标是分析非线性系统 $\dot{x} = f(x)$ 在某个状态附近的行为。

### 1. 泰勒级数展开 (The Taylor Series Expansion)

假设系统有一个**不动点（Fixed Point）**，记为 $\bar{x}$。
我们将系统的真实状态 $x(t)$ 看作是不动点加上一个微小的扰动（Perturbation）$\Delta x(t)$：

$$
x(t) = \bar{x} + \Delta x(t)
$$

我们需要找到扰动随时间的变化率 $\Delta \dot{x}$。
由于 $\bar{x}$ 是常数，其导数为 0，因此：

$$
\dot{x} = \frac{d}{dt}(\bar{x} + \Delta x) = 0 + \Delta \dot{x} = \Delta \dot{x}
$$

现在，我们将非线性函数 $f(x)$ 在 $\bar{x}$ 处进行**多维泰勒展开**：

$$
\underbrace{\dot{x}}_{\Delta \dot{x}} = f(x) = f(\bar{x} + \Delta x)
$$

根据泰勒公式（保留一阶项，忽略高阶项 H.O.T.）：

$$
f(\bar{x} + \Delta x) = \underbrace{f(\bar{x})}_{\text{项 1}} + \underbrace{\frac{\partial f}{\partial x}\bigg|_{\bar{x}} \cdot \Delta x}_{\text{项 2}} + \underbrace{\mathcal{O}(\Delta x^2)}_{\text{项 3}}
$$

#### 逐项分析：

* **项 1: $f(\bar{x})$**
    根据不动点的定义，系统在 $\bar{x}$ 处停止运动，即 $\dot{x}=0$。
    所以 $f(\bar{x}) = 0$。这一项消失了。

* **项 3: $\mathcal{O}(\Delta x^2)$**
    这是高阶无穷小量（包含 $\Delta x^2, \Delta x^3 \dots$）。
    因为我们假设扰动 $\Delta x$ 极其微小（接近于 0），它的平方项就更小了，可以安全地忽略。

* **项 2: $\frac{\partial f}{\partial x}\big|_{\bar{x}} \Delta x$**
    这是剩下的核心项。$\frac{\partial f}{\partial x}$ 就是**雅可比矩阵 (Jacobian Matrix)**，我们记为 $A$。

#### 最终结论

经过上述推导，原本复杂的非线性方程被简化为：

$$
\Delta \dot{x} \approx A \Delta x
$$

> **这意味着**：在微观尺度下，任何光滑的曲线看起来都是直的。

---

## Part 2: 雅可比矩阵详解 (The Jacobian Matrix)

雅可比矩阵 $A$ 本质上是多维空间中的“斜率”或“导数”。如果系统有 $n$ 个状态变量，它就是一个 $n \times n$ 的矩阵。

$$
A = \frac{\partial f}{\partial x} = \begin{bmatrix}
\nabla f_1 \\
\nabla f_2 \\
\vdots \\
\nabla f_n
\end{bmatrix} = 
\begin{bmatrix} 
\frac{\partial f_1}{\partial x_1} & \frac{\partial f_1}{\partial x_2} & \cdots & \frac{\partial f_1}{\partial x_n} \\
\frac{\partial f_2}{\partial x_1} & \frac{\partial f_2}{\partial x_2} & \cdots & \frac{\partial f_2}{\partial x_n} \\
\vdots & \vdots & \ddots & \vdots \\
\frac{\partial f_n}{\partial x_1} & \frac{\partial f_n}{\partial x_2} & \cdots & \frac{\partial f_n}{\partial x_n} 
\end{bmatrix}
$$

* **第 $i$ 行**代表第 $i$ 个状态方程 $f_i$ 对所有状态变量的敏感度。
* **$A_{ij}$ 元素**的具体含义是：当第 $j$ 个状态 $x_j$ 发生微小变化时，第 $i$ 个速度 $\dot{x}_i$ 会怎么变。

---

## Part 3: 案例研究——阻尼单摆 (Detailed Walkthrough)

我们将上述理论应用到一个具体的物理模型中：**有摩擦阻力的单摆**。

<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/24/Oscillating_pendulum.gif/220px-Oscillating_pendulum.gif" alt="Pendulum Animation">
</div>

### 1. 物理建模 (Modeling)

根据牛顿第二定律（转动）：力矩 = 转动惯量 $\times$ 角加速度。

$$
I \ddot{\theta} = -mgL \sin\theta - b \dot{\theta}
$$

简化系数后，得到标准形式：

$$
\ddot{\theta} = -\sin\theta - \delta \dot{\theta}
$$

* $-\sin\theta$：重力产生的回复力（非线性源头）。
* $-\delta \dot{\theta}$：空气阻力或摩擦力（$\delta > 0$）。

### 2. 状态空间转换 (State-Space Formulation)

定义状态向量 $x = [x_1, x_2]^T = [\theta, \dot{\theta}]^T$。

$$
\begin{cases}
\dot{x}_1 = \dot{\theta} = x_2 \\
\dot{x}_2 = \ddot{\theta} = -\sin(x_1) - \delta x_2
\end{cases}
$$

写成向量形式 $\dot{x} = f(x)$：

$$
f(x) = \begin{bmatrix} f_1(x_1, x_2) \\ f_2(x_1, x_2) \end{bmatrix} = \begin{bmatrix} x_2 \\ -\sin(x_1) - \delta x_2 \end{bmatrix}
$$

### 3. Step 1: 寻找不动点 (Fixed Points)

令 $\dot{x} = 0$：

$$
\begin{bmatrix} x_2 \\ -\sin(x_1) - \delta x_2 \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \end{bmatrix}
$$

解得：
* $x_2 = 0$ (必须静止)。
* $\sin(x_1) = 0 \Rightarrow x_1 = 0, \pi, 2\pi \dots$

主要关注两个点：
1.  **垂直向下 (Down)**: $\bar{x}_{down} = [0, 0]^T$
2.  **垂直向上 (Up)**: $\bar{x}_{up} = [\pi, 0]^T$

### 4. Step 2: 计算雅可比矩阵 (Calculating Jacobian)

这是最关键的求导步骤。我们需要对矩阵中的每个元素求偏导。

$$
A(x) = \begin{bmatrix} \frac{\partial f_1}{\partial x_1} & \frac{\partial f_1}{\partial x_2} \\ \frac{\partial f_2}{\partial x_1} & \frac{\partial f_2}{\partial x_2} \end{bmatrix}
$$

**推导细节：**
* 对于 $f_1 = x_2$：
    * 对 $x_1$ 求导: 0
    * 对 $x_2$ 求导: 1
* 对于 $f_2 = -\sin(x_1) - \delta x_2$：
    * 对 $x_1$ 求导: $\frac{d}{dx_1}(-\sin x_1) = -\cos x_1$
    * 对 $x_2$ 求导: $\frac{d}{dx_2}(-\delta x_2) = -\delta$

**结果矩阵：**

$$
A(x) = \begin{bmatrix} 0 & 1 \\ -\cos(x_1) & -\delta \end{bmatrix}
$$

### 5. Step 3 & 4: 稳定性判据 (Stability Analysis)

现在我们将具体的 $\bar{x}$ 代入 $A(x)$ 并计算特征值 $\lambda$。
特征方程公式为：$|\lambda I - A| = \lambda^2 - \text{tr}(A)\lambda + \det(A) = 0$。

#### 场景 A: 向下垂 ($\bar{x} = [0, 0]^T$)

代入 $x_1 = 0 \implies \cos(0) = 1$。

$$
A_{down} = \begin{bmatrix} 0 & 1 \\ -1 & -\delta \end{bmatrix}
$$

**特征值计算：**
特征多项式：$\lambda(\lambda + \delta) - (-1)(1) = \lambda^2 + \delta\lambda + 1 = 0$。
使用求根公式：

$$
\lambda_{1,2} = \frac{-\delta \pm \sqrt{\delta^2 - 4}}{2}
$$

**分析：**
由于 $\delta > 0$（有摩擦），实部 $-\delta/2$ 是负数。
即使 $\delta$ 很小导致根号下是负数（出现虚部，产生震荡），实部依然由 $-\delta$ 决定。

> **结论**： $Re(\lambda) < 0 \rightarrow$ **稳定 (Stable)**。物理上表现为振荡衰减，最终停在最低点。

#### 场景 B: 倒立摆 ($\bar{x} = [\pi, 0]^T$)

代入 $x_1 = \pi \implies \cos(\pi) = -1$。
**注意矩阵左下角的变化**：$-(-1) = 1$。

$$
A_{up} = \begin{bmatrix} 0 & 1 \\ 1 & -\delta \end{bmatrix}
$$

**特征值计算：**
特征多项式：$\lambda(\lambda + \delta) - (1)(1) = \lambda^2 + \delta\lambda - 1 = 0$。
使用求根公式：

$$
\lambda_{1,2} = \frac{-\delta \pm \sqrt{\delta^2 - 4(-1)}}{2} = \frac{-\delta \pm \sqrt{\delta^2 + 4}}{2}
$$

**分析：**
我们关注 "+" 号的那个根：
$$\lambda_1 = \frac{-\delta + \sqrt{\delta^2 + 4}}{2}$$
因为 $\sqrt{\delta^2 + 4} > \sqrt{\delta^2} = \delta$，所以 $\sqrt{\delta^2 + 4}$ 一定比 $\delta$ 大。
这意味着 $(-\delta + \text{大正数}) > 0$。

> **结论**： 存在一个 $Re(\lambda) > 0 \rightarrow$ **不稳定 (Unstable)**。
>
> **物理意义**： 哪怕是一粒灰尘落在倒立的摆上（微小扰动 $\Delta x$），这个正特征值也会像“指数爆炸”一样把摆推离平衡点。

---
