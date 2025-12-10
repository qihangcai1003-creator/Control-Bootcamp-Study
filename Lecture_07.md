你在提供的笔记草稿中，**Section 2 (实例演示)** 的计算部分排版非常混乱，且 **Case A** 和 **Case B** 的详细程度不一致（Case A 过于冗杂，Case B 过于简略），导致阅读体验割裂。

此外，**Section 3 (凯莱-哈密顿)** 中关于特征多项式的定义有一个常见的符号严谨性问题（$\det(A-\lambda I)$ 展开后的最高次项系数通常是 $(-1)^n$，除非写成 $\det(\lambda I - A)$）。

以下是修正后的版本。主要的改进点：
1.  **统一计算格式**：将 Case A 和 Case B 的计算步骤标准化，使其清晰易读。
2.  **视觉辅助**：在矩阵中高亮关键的“零行”或“非零元素”，直观展示秩的变化。
3.  **严谨性修正**：修正了特征多项式的定义符号，使其数学上更严谨。

***

# 线性控制理论笔记：可控性与凯莱-哈密顿定理

本笔记基于 Steve Brunton 的控制理论课程板书整理，涵盖了 **PBH 测试 (Popov-Belevitch-Hautus Test)** 与 **凯莱-哈密顿定理 (Cayley-Hamilton Theorem)** 的核心概念及其在控制系统中的应用。

---

## 1. PBH 测试 (Popov-Belevitch-Hautus Test)

PBH 测试是判断线性时不变 (LTI) 系统可控性的重要理论工具，特别适用于理论分析和数学证明。

### 1.1 核心定义
对于线性系统 $\dot{x} = Ax + Bu$，系统 $(A, B)$ 是 **可控的 (Controllable)**，当且仅当对于所有的复数 $\lambda \in \mathbb{C}$，以下秩条件成立：

$$
\text{rank} \left[ (A - \lambda I) \quad B \right] = n
$$

> **💡 关键点**：实际上只需要检查矩阵 $A$ 的 **特征值 (Eigenvalues)**。因为如果 $\lambda$ 不是特征值，$(A - \lambda I)$ 已经是满秩矩阵（秩为 $n$），无论 $B$ 如何，条件都自然满足。

### 1.2 直观理解 (Intuition)
1.  **特征值的秩缺失**：
    当 $\lambda$ 是特征值时，$(A - \lambda I)$ 变得奇异，其秩下降（$<n$），失去了某些方向的满秩性。
2.  **$B$ 的补位作用**：
    为了保持组合矩阵 $[(A - \lambda I) \quad B]$ 的满秩，$B$ 的列向量必须“填补”上 $(A - \lambda I)$ 失去的那个方向。
    * **物理意义**：控制输入 $B$ 不能与系统的特征向量正交。如果正交，输入就无法影响该特征模态。

---

## 2. 实例演示：PBH 测试如何工作

为了直观验证理论，我们使用一个简单的 2D 对角系统。由于是对角矩阵，特征值和特征向量非常直观。

**系统设定：**
$$A = \begin{bmatrix} 1 & 0 \\ 0 & 2 \end{bmatrix}$$
* **特征值**：$\lambda_1 = 1, \quad \lambda_2 = 2$
* **系统维度**：$n = 2$

我们将针对特征值 **$\lambda = 2$** 进行 PBH 测试。此时：
$$A - 2I = \begin{bmatrix} 1-2 & 0 \\ 0 & 2-2 \end{bmatrix} = \begin{bmatrix} -1 & 0 \\ 0 & \mathbf{0} \end{bmatrix}$$
*(注意：第二行全为 0，导致秩亏缺)*

### 案例 A：不可控情况

假设输入矩阵 $B = \begin{bmatrix} 1 \\ 0 \end{bmatrix}$（输入仅作用于 $x_1$）。

构建 PBH 矩阵：
$$
\begin{aligned}
\mathcal{M} = [(A - 2I) \quad B] &= \left[ \begin{array}{cc|c} -1 & 0 & 1 \\ 0 & \mathbf{0} & \mathbf{0} \end{array} \right]
\end{aligned}
$$

* **观察**：矩阵的 **第二行全为 0**。
* **结果**：$\text{rank}(\mathcal{M}) = 1$ （小于 $n=2$）。
* **结论**：秩条件不满足，系统在 $\lambda=2$ 处 **不可控**。
    *(物理阐释：输入 $u$ 无法影响状态 $x_2$ 的动态)*

### 案例 B：可控情况

假设输入矩阵 $B = \begin{bmatrix} 1 \\ 1 \end{bmatrix}$（输入同时作用于 $x_1$ 和 $x_2$）。

构建 PBH 矩阵：
$$
\begin{aligned}
\mathcal{M} = [(A - 2I) \quad B] &= \left[ \begin{array}{cc|c} -1 & 0 & 1 \\ 0 & \mathbf{0} & \mathbf{1} \end{array} \right]
\end{aligned}
$$

* **观察**：虽然 $(A-2I)$ 的第二行为 0，但 $B$ 在该行提供了非零元素 $\mathbf{1}$。第一列 $[-1, 0]^T$ 和第三列 $[1, 1]^T$ 线性无关。
* **结果**：$\text{rank}(\mathcal{M}) = 2$ （等于 $n$）。
* **结论**：秩条件满足，系统 **可控**。

---

## 3. 凯莱-哈密顿定理 (Cayley-Hamilton Theorem)

这是一个连接**线性代数**与**控制理论**的桥梁，它解释了计算矩阵函数（如 $e^{At}$）时为何只需要有限项。

### 3.1 定理陈述

> **Theorem: Every matrix $A$ satisfies its own characteristic equation.**
> (每一个矩阵 $A$ 都满足它自己的特征方程。)

特征多项式定义为 $p(\lambda) = \det(\lambda I - A)$。设其展开形式为：
$$p(\lambda) = \lambda^n + a_{n-1}\lambda^{n-1} + \dots + a_1\lambda + a_0 = 0$$

那么矩阵 $A$ 满足同构的矩阵方程：
$$A^n + a_{n-1}A^{n-1} + \dots + a_1 A + a_0 I = \mathbf{0}$$

### 3.2 重要推论：幂的降阶

根据上述公式，我们可以将最高次幂 $A^n$ 用低次幂表示：
$$A^n = -a_{n-1}A^{n-1} - \dots - a_1 A - a_0 I$$

这意味着：**任何 $n$ 次及以上的幂 ($A^k, k \ge n$)，最终都可以表示为前 $n$ 项 ($I, A, \dots, A^{n-1}$) 的线性组合。**

### 3.3 在控制理论中的应用

#### A. 矩阵指数 (Matrix Exponential)
状态方程的解 $e^{At}$ 原本是无穷级数：
$$e^{At} = \sum_{k=0}^{\infty} \frac{A^k t^k}{k!}$$
由凯莱-哈密顿定理，所有高次项 $A^k (k \ge n)$ 均可被降阶，因此无穷级数坍缩为有限项求和：
$$e^{At} = \alpha_0(t)I + \alpha_1(t)A + \dots + \alpha_{n-1}(t)A^{n-1}$$

#### B. 可控性矩阵的维度
可控性矩阵 $\mathcal{C} = [B \quad AB \quad \dots \quad A^{n-1}B]$ 为什么只需要写到 $A^{n-1}$？
因为 $A^n B$ 已经是前面各项的线性组合，继续增加 $A^n B, A^{n+1} B$ 等项只会增加线性相关的列，**不会增加矩阵的秩**。包含前 $n$ 项已包含了系统所有的独立动态方向。
