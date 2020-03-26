---
layout: post
title: Dynamic Mode Decomposition and Koopman Operator Theory (Episode I)
categories: [Robotics, Dynamical System]
mathjax: true
---

In this post I will talk about dynamic mode decomposition and Koopman operator theory, which, in contrast to classical dynamical system analysis methods, aim to describe the system with a linear operator in a data-driven manner. 

## Part I

Dynamic mode decomposition (DMD) has its root in the Koopman operator theory, as its target is to approximate a linear transformation, denoted as $A$, that could project the current system state to next time step as:
$$
x_{t+1} = A x_t
$$

Assume this unknown non-linear dynamical system has $N$ state variables, so at each time step we have $x_t = [x_t^1, \cdots, x_t^N]$. Then, we collect $M-1$ consecutive system states into a N-by-M matrix X denoted as:
$$
X_1^{M-1} = [x_1, x_2, \cdots, x_{M-1}]
$$

In this case, the operator (for now still unknown) can project each element in $X_1^{M-1}$ into the next time step:
$$
\begin{align}
A X_1^{M-1} = X_2^M
\end{align}
$$

In the meantime, we can write $x_M$ in terms of Krylov basis (here is where the DMD method connects to Krylov subspaces and the Arnoldi algorithm, but for now this discussion can keep going without more details about those) as:

$$
x_M = \sum_{m=1}^{M-1} b_m x_m + r
$$ 

where $b_m$ is the coefficient of the Krylov space vectors and $r$ is the residual (or error) that lies outside (orthogonal to) the Krylov space. Then, we can reformulate the relation between $X_1^{M-1}$ and $X_2^M$ as:

$$
\begin{align}
X_2^M = X_1^{M-1} S + r e^H_{M-1}
\end{align}
$$ 

where $e_{M-1}$ is the $(M-1)$th unit vector and $(\cdot)^H$ denotes conjugate transpose. $S$ contains those Krylov coefficients:
$$
\begin{align}
    S = \begin{bmatrix}
        0 & \cdots & \quad & 0 & b_1 \\
        1 & \ddots & \quad & 0 & b_2 \\
        0 & \ddots & \ddots & \quad & \vdots \\
        \quad & \ddots & \ddots & 0 & b_{M-2} \\
        0 & \cdots & 0 & 1 & b_{M-1} 
        \end{bmatrix}
\end{align}
$$

## Part II

Now, recall that our goal in DMD is to approximate the linear operator $A$, **more precisely, what we will actually do is to approximate the eigenvalues of $A$**. If we ignore the residual terms in (2), we can see that $S$ can also transfer $X_1^{M-1}$ to $X_2^M$ as a linear transformation. So, we can also approximate the eigenvalues of $S$ to reach the same goal.

To do so, we apply singular value decomposition (SVD) to $X_1^{M-1}$ and have 
$$
\begin{align}
X_1^{M-1} = U \sigma V^H
\end{align}
$$

Substituting this into (1) gives us:
$$
\begin{align}
    X_2^{M} & = A U \Sigma V^H \\
    A & = X_2^M (U \Sigma V^H)^{\dagger} = X_2^M V \Sigma^{-1} U^H
\end{align}
$$

Then we apply similarity transformation to $A$, which would give us another matrix $\tilde{A}$ having same set of eigenvalues with $A$:
$$
\begin{align}
    \tilde{A} = U^H A U = U^H (X_2^M V \Sigma^{-1}U^H) U = U^H X_2^M V \Sigma^{-1}
\end{align} 
$$

Thus, we can approximate a matrix $\tilde{A}$ from SVD of $X_1^{M-1}$ and $X_2^M$ that has a same set of eigenvalues with $A$. 

Now, let's see if we could get the same result from $S$. Again, we can use $S$ to project $X_1^{m-1}$ to $X_2^M$:
$$
\begin{align}
    X_2^M = X_1^{M-1} S
\end{align} 
$$

Similarly, by applying SVD to $X_1^{M-1}$ and substituting into the equation above, we have
$$
\begin{align}
    X_2^M & = U \Sigma V^H S \\ 
    S & = V \Sigma^{-1} U^H X_2^M
\end{align} 
$$

This time, we apply similarity transformation twice to $S$ to get the same result as we derive from $A$:
$$
\begin{align}
    \tilde{S} & = \Sigma (V^H S V) \Sigma^{-1} = U^H X_2^M V \Sigma^{-1}
\end{align}
$$

To prove that the eigenvalues of $\tilde{S}$ and $\tilde{A}$ are also the eigenvalues of $A$, we first consider the eigenvalue problem associated with $\tilde{S}$ as (since $\tilde{S}$ and $\tilde{A}$ are same, we just need to consider one of them):
$$
\begin{align}
    \tilde{S} y_k = \mu_k y_k \quad k = 1, 2, \dots, K
\end{align} 
$$

$K$ is the rank of the approximation we are choosing to make. According to Kutz's book, "the eigenvalues $\mu_k$ capture the time dynamics of the discrete Koopman map $A$ as a $\nabla t$ step is taken forward in time". And we can see that these eigenvalues are also the eigenvalues of $A$ by writing down:
$$
\begin{align}
    \tilde{S} & = U^H A U \\
    \tilde{S} y_k & = (U^H A U) y_k = \mu_k y_k \\
    A (U y_k) & = \mu_k (U y_k) \\
    A y_k & = \mu_k y_k
\end{align}
$$

Now, with those low-rank approximation of eigenvalues and eigenvectors of $A$, it is possible accurately predict future states of the system, which will be discussed in the next post. See you then!

## References

[1] Kutz, J. Nathan. Data-driven modeling & scientific computation: methods for complex systems & big data. Oxford University Press, 2013.

[2] [Nathan Kutz Lecture (Youtube): Dynamic Mode Decomposition (Theory)](https://www.youtube.com/watch?v=bYfGVQ1Sg98)

[3] [Nathan Kutz Lecture (Youtube): Dynamic Mode Decomposition (Code)](https://www.youtube.com/watch?v=KAau5TBU0Sc)

[4] [Steven Brunton Lecture (Youtube): Dynamic Mode Decomposition (Overview)](https://www.youtube.com/watch?v=sQvrK8AGCAo)

***