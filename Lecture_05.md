# 线性控制系统的核心：可控性 (Controllability)

> **核心问题**：我们是否可以通过施加控制输入 $u$，将系统从任意初始状态引导到任意目标状态？
>
> 之前讨论的 $\dot{x} = Ax$ 仅描述了系统的自然演化。现在，引入控制输入 $u$，目的是**改变**系统的动态行为。

---

## 1. 控制系统的架构 (The Architecture)

### 1.1 核心方程
标准的线性控制系统方程如下：

$$
\dot{x} = \underbrace{A}_{\text{系统矩阵}} x + \underbrace{B}_{\text{输入矩阵}} u
$$

* **$x$ (状态)**：系统的内部变量（如飞机的位置和速度）。
* **$u$ (输入)**：可以施加的外部干预（如推拉操纵杆的力）。
* **$B$ (输入矩阵)**：决定了输入 $u$ 如何直接作用于系统状态。

### 1.2 闭环控制 (Closed-Loop Control)与全状态反馈
闭环控制的核心策略是 **$u = -Kx$**（全状态反馈 Full State Feedback）。

* **思路**：根据当前的状态 $x$ 实时计算出控制量 $u$。
* **数学推导**：
    将 $u = -Kx$ 代入原方程：
    
    $$
    \dot{x} = Ax + B(-Kx) \implies \dot{x} = (A - BK)x
    $$

> **数学原理**
> * 原本系统的稳定性由 **$A$** 的特征值决定（可能是正实部，导致不稳定）。
> * 闭环系统的稳定性由 **$(A - BK)$** 的特征值决定。
> * **结论**：只要系统是“可控”的，就能通过设计矩阵 $K$，将特征值配置到复平面的任意位置（例如将不稳定的特征值移至左半平面）。

---

## 2. 直觉上的可控性：直观案例

并非所有的系统都能通过反馈控制被稳定。以下通过两个对比案例说明。

### 案例 1：不可控系统 (Uncontrollable)

$$
\begin{bmatrix} 
\dot{x}_1 \\ 
\dot{x}_2 
\end{bmatrix} = 
\begin{bmatrix} 
1 & 0 \\ 
0 & 2 
\end{bmatrix} 
\begin{bmatrix} 
x_1 \\ 
x_2 
\end{bmatrix} + 
\begin{bmatrix} 
0 \\ 
1 
\end{bmatrix} u
$$

* **展开方程**：
    1.  $\dot{x}_1 = 1 \cdot x_1 + 0 \cdot u$
    2.  $\dot{x}_2 = 2 \cdot x_2 + 1 \cdot u$
* **分析**：
    * 方程 1 表明，$x_1$ 的变化只与它自己有关。
    * 输入 $u$ 前面的系数是 **0**，且方程中没有 $x_2$ 的参与。
    * 这意味着：**无论如何改变 $u$，都无法影响 $x_1$**。如果 $x_1$ 不稳定（特征值为1），无法通过控制使其稳定。此即**不可控**。

### 案例 2：可控系统 (Controllable)

$$
\begin{bmatrix} 
\dot{x}_1 \\ 
\dot{x}_2 
\end{bmatrix} = 
\begin{bmatrix} 
1 & \mathbf{1} \\ 
0 & 2 
\end{bmatrix} 
\begin{bmatrix} 
x_1 \\ 
x_2 
\end{bmatrix} + 
\begin{bmatrix} 
0 \\ 
1 
\end{bmatrix} u
$$

* **展开方程**：
    1.  $\dot{x}_1 = 1 \cdot x_1 + \mathbf{1 \cdot x_2}$  <-- **关键耦合项**
    2.  $\dot{x}_2 = 2 \cdot x_2 + 1 \cdot u$
* **分析**：
    * 虽然 $u$ 依然无法直接作用于 $x_1$（系数为0）。
    * 但是，$u$ 可以控制 $x_2$，而 $x_2$ 直接出现在了 $x_1$ 的方程里。
    * **策略**：通过控制 $x_2$，利用系统内部的**耦合 (Coupling)**，间接控制 $x_1$。此即**可控**。

---

## 3. 数学判据：卡尔曼秩条件 (Rank Condition)

对于高维系统，需要依靠数学工具进行判断。

**可控性矩阵 (Controllability Matrix)** 定义为 $\mathcal{C}$：

$$
\mathcal{C} = \begin{bmatrix} B & AB & A^2B & \dots & A^{n-1}B \end{bmatrix}
$$

> **定理 (Kalman Rank Condition)**
> 一个 $n$ 维系统是可控的，**当且仅当** 矩阵 $\mathcal{C}$ 是**满秩的 (Full Rank)**。
> 即：$\text{rank}(\mathcal{C}) = n$

* **满秩**：意味着可以将特征值配置到任意位置。
* **不满秩**：意味着系统存在不可控的子空间，输入无法触及某些状态。

---

# 线性控制系统的核心：可控性 (Controllability)

> **核心问题**：我们是否可以通过施加控制输入 $u$，将系统从任意初始状态引导到任意目标状态？

---

## 4. 物理案例详解：两辆小车模型

为了理解数学矩阵背后的物理意义，构建“两辆小车”模型：
* **1号车 ($x_1$)**：自带动力，运动独立。
* **2号车 ($x_2$)**：受控制输入 ($u$) 作用。

### 场景一：无连接 (不可控)

**物理情景**：两车中间无连接。推动 2号车，1号车不受影响。

**数学模型**：

$$
\begin{bmatrix} 
\dot{x}_1 \\ 
\dot{x}_2 
\end{bmatrix} = 
\underbrace{\begin{bmatrix} 1 & 0 \\ 0 & 2 \end{bmatrix}}_{A} 
\begin{bmatrix} x_1 \\ x_2 \end{bmatrix} + 
\underbrace{\begin{bmatrix} 0 \\ 1 \end{bmatrix}}_{B} u
$$

**计算可控性矩阵 $\mathcal{C}$**：

第一步，提取矩阵 $B$：

$$
B = \begin{bmatrix} 
0 \\ 
1 
\end{bmatrix}
$$

第二步，计算 $AB$：

$$
AB = \begin{bmatrix} 
1 & 0 \\ 
0 & 2 
\end{bmatrix} 
\begin{bmatrix} 
0 \\ 
1 
\end{bmatrix} = 
\begin{bmatrix} 
0 \\ 
2 
\end{bmatrix}
$$

第三步，合并得到 $\mathcal{C}$：

$$
\mathcal{C} = [B, AB] = 
\begin{bmatrix} 
0 & 0 \\ 
1 & 2 
\end{bmatrix}
$$

* **秩分析**：行列式 $0\times2 - 0\times1 = 0$。秩为 1 (小于维度 2)。
* **结论**：**不可控**。这符合物理直觉——无物理连接，无法通过控制一辆车影响另一辆车。

---

### 场景二：刚性连接 (可控)

**物理情景**：两车之间系有一根刚性**拖车绳**。推动 2号车，绳子会拉动 1号车。

**数学模型**：

$$
\begin{bmatrix} 
\dot{x}_1 \\ 
\dot{x}_2 
\end{bmatrix} = 
\underbrace{\begin{bmatrix} 1 & \mathbf{1} \\ 0 & 2 \end{bmatrix}}_{A} 
\begin{bmatrix} x_1 \\ x_2 \end{bmatrix} + 
\underbrace{\begin{bmatrix} 0 \\ 1 \end{bmatrix}}_{B} u
$$

* **注意**：矩阵 $A$ 右上角的 **1** 代表了“绳子”（物理耦合）。

**计算可控性矩阵 $\mathcal{C}$**：

第一步，提取矩阵 $B$：

$$
B = \begin{bmatrix} 
0 \\ 
1 
\end{bmatrix}
$$

第二步，计算 $AB$：

$$
AB = \begin{bmatrix} 
1 & 1 \\ 
0 & 2 
\end{bmatrix} 
\begin{bmatrix} 
0 \\ 
1 
\end{bmatrix} = 
\begin{bmatrix} 
1 \\ 
2 
\end{bmatrix}
$$

*(注意：由于耦合项的存在，结果的第一行变成了 1)*

> **物理意义**：$A \times B$ 模拟了能量传递路径：输入 $u \to$ 2号车 $\to$ 绳子($A$) $\to$ 1号车。

第三步，合并得到 $\mathcal{C}$：

$$
\mathcal{C} = [B, AB] = 
\begin{bmatrix} 
0 & 1 \\ 
1 & 2 
\end{bmatrix}
$$

* **秩分析**：行列式 $0\times2 - 1\times1 = -1 \neq 0$。矩阵**满秩**。
* **结论**：**可控**。虽然输入无法直接作用于1号车，但通过耦合关系，可以实现间接控制。
