# Linear Algebra & System Dynamics: The Power of Eigenvalues

> **Course Source:** [Steve Brunton - Control Bootcamp](https://www.youtube.com/playlist?list=PLMrJAkhIeNNR20Mz-VpzgfQs5zrYi085m)
> **Topic:** Solving Linear Differential Equations using Eigenvalue Decomposition.

---

## Part 1: The Full Mathematical Derivation

This section demonstrates the complete logical chain from the differential equation $\dot{x}=Ax$ to the solution formula $e^{At} = Te^{Dt}T^{-1}$.

### Step 1: Problem Definition
Given a linear differential equation:

$$
\dot{x} = Ax, \quad x(0) = x_0
$$

The solution is given by the matrix exponential:

$$
x(t) = e^{At} x_0
$$

By the definition of the Taylor series, $e^{At}$ is:

$$
e^{At} = I + At + \frac{A^2 t^2}{2!} + \frac{A^3 t^3}{3!} + \dots + \frac{A^k t^k}{k!} + \dots
$$

> **Challenge:** Computing this infinite series directly is extremely difficult because $A$ is a coupled matrix, making high powers $A^k$ hard to calculate.

### Step 2: Eigenvalue Decomposition (Diagonalization)
Assume $A$ has $n$ linearly independent eigenvectors.

$$
A \xi_i = \lambda_i \xi_i
$$

We assemble them into matrices:
* $T = [\xi_1, \xi_2, \dots, \xi_n]$ (Eigenvector Matrix)
* $D = \text{diag}(\lambda_1, \dots, \lambda_n)$ (Eigenvalue Matrix)

According to the relationship $AT = TD$, we get the decomposition formula:

$$
A = T D T^{-1}
$$

### Step 3: The Power Trick
This is the crucial step. We need to calculate powers of $A$ efficiently.

$$
A^2 = (T D T^{-1}) (T D T^{-1}) = T D \underbrace{(T^{-1} T)}_{I} D T^{-1} = T D^2 T^{-1}
$$

$$
A^3 = A \cdot A^2 = (T D T^{-1}) (T D^2 T^{-1}) = T D^3 T^{-1}
$$

By induction, we obtain:

$$
A^k = T D^k T^{-1}
$$

> **Insight:** The $T$ and $T^{-1}$ terms cancel out in the middle, leaving only the diagonal matrix $D$ to be raised to the power $k$.

### Step 4: Substitution into the Series
Substitute $A^k = T D^k T^{-1}$ back into the Taylor series from Step 1:

$$
\begin{aligned}
e^{At} &= I + (T D T^{-1})t + \frac{(T D^2 T^{-1})t^2}{2!} + \dots \\
&= T I T^{-1} + T (Dt) T^{-1} + T \frac{(D^2 t^2)}{2!} T^{-1} + \dots \\
&= T \left[ I + Dt + \frac{D^2 t^2}{2!} + \dots \right] T^{-1}
\end{aligned}
$$

### Step 5: Conclusion
The series inside the brackets is exactly the definition of $e^{Dt}$. Since $D$ is a diagonal matrix, its exponential is simply the exponential of its diagonal elements:

$$
e^{Dt} = \begin{bmatrix} e^{\lambda_1 t} & 0 \\ 0 & e^{\lambda_n t} \end{bmatrix}
$$

Thus, we arrive at the famous formula:

$$
\boxed{e^{At} = T e^{Dt} T^{-1}}
$$

---

[## Part 2: Step-by-Step Calculation Example

To make the theory concrete, we will solve a specific $2 \times 2$ linear differential equation $\dot{x} = Ax$ manually.

### 1. The Problem Setup

**System Matrix:**

$$
A = \begin{bmatrix} 4 & -2 \\ 1 & 1 \end{bmatrix}
$$

**Initial Condition:**

$$
x(0) = \begin{bmatrix} 1 \\ 0 \end{bmatrix}
$$

**Goal:** Find the state vector $x(t)$.

### 2. Calculation Workflow

#### Step 1: Find Eigenvalues ($\lambda$)
Solve the characteristic equation $\det(A - \lambda I) = 0$:

$$
\det \begin{bmatrix} 4-\lambda & -2 \\ 1 & 1-\lambda \end{bmatrix} = 0
$$

$$
\begin{aligned}
(4-\lambda)(1-\lambda) - (-2)(1) &= 0 \\
(\lambda^2 - 5\lambda + 4) + 2 &= 0 \\
\lambda^2 - 5\lambda + 6 &= 0 \\
(\lambda - 2)(\lambda - 3) &= 0
\end{aligned}
$$

**Result:**
* $\lambda_1 = 2$
* $\lambda_2 = 3$

#### Step 2: Find Eigenvectors ($\xi$)
Solve $(A - \lambda I)\xi = 0$ for each $\lambda$.

**For $\lambda_1 = 2$:**

$$
\begin{bmatrix} 4-2 & -2 \\ 1 & 1-2 \end{bmatrix} \begin{bmatrix} \xi_{1a} \\ \xi_{1b} \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \end{bmatrix} \Rightarrow \begin{bmatrix} 2 & -2 \\ 1 & -1 \end{bmatrix} \begin{bmatrix} \xi_{1a} \\ \xi_{1b} \end{bmatrix} = 0
$$

Equation: $1\xi_{1a} - 1\xi_{1b} = 0 \Rightarrow \xi_{1a} = \xi_{1b}$.
Let $\xi_1 = \begin{bmatrix} 1 \\ 1 \end{bmatrix}$.

**For $\lambda_2 = 3$:**

$$
\begin{bmatrix} 4-3 & -2 \\ 1 & 1-3 \end{bmatrix} \begin{bmatrix} \xi_{2a} \\ \xi_{2b} \end{bmatrix} = 0 \Rightarrow \begin{bmatrix} 1 & -2 \\ 1 & -2 \end{bmatrix} \begin{bmatrix} \xi_{2a} \\ \xi_{2b} \end{bmatrix} = 0
$$

Equation: $1\xi_{2a} - 2\xi_{2b} = 0 \Rightarrow \xi_{2a} = 2\xi_{2b}$.
Let $\xi_2 = \begin{bmatrix} 2 \\ 1 \end{bmatrix}$.

#### Step 3: Construct Matrices $T$ and $D$

$$
T = [\xi_1, \xi_2] = \begin{bmatrix} 1 & 2 \\ 1 & 1 \end{bmatrix}
$$

$$
D = \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix} = \begin{bmatrix} 2 & 0 \\ 0 & 3 \end{bmatrix}
$$

#### Step 4: Calculate $T^{-1}$
For a generic $2 \times 2$ matrix:

$$
\begin{bmatrix} a & b \\ c & d \end{bmatrix}^{-1} = \frac{1}{ad-bc}\begin{bmatrix} d & -b \\ -c & a \end{bmatrix}
$$

Determinant of $T$: $(1)(1) - (2)(1) = -1$.

$$
T^{-1} = \frac{1}{-1} \begin{bmatrix} 1 & -2 \\ -1 & 1 \end{bmatrix} = \begin{bmatrix} -1 & 2 \\ 1 & -1 \end{bmatrix}
$$

#### Step 5: Calculate $e^{At} = T e^{Dt} T^{-1}$

**1. Compute $e^{Dt}$ (Easy part):**

$$
e^{Dt} = \begin{bmatrix} e^{2t} & 0 \\ 0 & e^{3t} \end{bmatrix}
$$

**2. Multiply $T e^{Dt}$:**

$$
T e^{Dt} = \begin{bmatrix} 1 & 2 \\ 1 & 1 \end{bmatrix} \begin{bmatrix} e^{2t} & 0 \\ 0 & e^{3t} \end{bmatrix} = \begin{bmatrix} e^{2t} & 2e^{3t} \\ e^{2t} & e^{3t} \end{bmatrix}
$$

**3. Multiply $(T e^{Dt}) T^{-1}$:**

$$
\begin{aligned}
e^{At} &= \begin{bmatrix} e^{2t} & 2e^{3t} \\ e^{2t} & e^{3t} \end{bmatrix} \begin{bmatrix} -1 & 2 \\ 1 & -1 \end{bmatrix} \\
&= \begin{bmatrix} -e^{2t} + 2e^{3t} & 2e^{2t} - 2e^{3t} \\ -e^{2t} + e^{3t} & 2e^{2t} - e^{3t} \end{bmatrix}
\end{aligned}
$$

#### Step 6: Final Solution $x(t) = e^{At}x(0)$

$$
x(t) = \begin{bmatrix} -e^{2t} + 2e^{3t} & 2e^{2t} - 2e^{3t} \\ -e^{2t} + e^{3t} & 2e^{2t} - e^{3t} \end{bmatrix} \begin{bmatrix} 1 \\ 0 \end{bmatrix}
$$

Perform matrix-vector multiplication:

$$
x(t) = \begin{bmatrix} (-e^{2t} + 2e^{3t})(1) + (2e^{2t} - 2e^{3t})(0) \\ (-e^{2t} + e^{3t})(1) + (2e^{2t} - e^{3t})(0) \end{bmatrix}
$$

**Final Result:**

$$
x(t) = \begin{bmatrix} 2e^{3t} - e^{2t} \\ e^{3t} - e^{2t} \end{bmatrix}
$$
