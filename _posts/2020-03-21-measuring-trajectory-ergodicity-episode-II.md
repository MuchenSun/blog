---
layout: post
title: Measuring Trajectory Ergodicity (Episode II)
categories: [Robotics]
mathjax: true
---

In this post, we will derive one of the metrics used to measure how far one trajectory is from being ergodic with respect to a spatial information distribution.

First, define a trajectory as $x_v(t)$, which can considered as function of time $t$. At each time point $t_i$, the trajectory $x_v(t_i)$ is a point $x_v$ in the search space $\mathcal{X}^v$, which is defined as $\mathcal{X}^v \triangleq [0,L_1]\times[0,L_2]\times\cdots\times[0,L_v]$. Our target is to develop a metric that takes in one trajectory in a time period $[t_0, t_0+T]$ and quantify how far this trajectory is from being ergodic with respect to a given spatial distribution.

The spatial distribution $\Phi(s): \mathcal{X}^v \rightarrow \mathbb{R}$ is defined as a function project from $\mathcal{X}^v$ to $\mathbb{R}$. Then, the essential idea here is that, similar as the spatial distribution, spatial statistics of the trajectory can also be considered as a function that project a $s\in\mathcal{X}^v$ point in search space to a scalar, which means for any region/point in the search space, we want to know average time the trajectory intersecting with it. Therefore, we define spatial statistics of the trajectory in time period $[t_0, t_0+T]$ as:

$$
C(s) = \frac{1}{T} \int_{t_0}^{t_0+T} \delta[s - x_v(t)] dt
$$ 

Now, given these two distributions, we want a quantity to measure the difference between the two as the metric. Here, we choose to use the weighted squared distance of Fourier coefficients calculated from the two distributions.

For the spatial distribution, we define its corresponding Fourier coefficients as

$$
\phi_k = \int_{\mathcal{X}^v} \Phi(x_v) F_k(x_v) dx_v
$$

Similarly, the Fourier coefficients of the trajectory spatial statistics are defined as

$$
c_k = \frac{1}{T} \int_{t_0}^{t_0+T} F_k(x_v(t)) dt
$$

Notice that there are multiple choices for the Fourier basis function $F_k(s)$, for example it can be 

$$
F_k(s) = \frac{1}{h_k} \prod_{i=1}^{v} \cos(\frac{k_i \pi}{L_i}s_i)
$$

Last, with the Fourier coefficients of each distribution, the ergodic metric of a trajectory in a time period with respect to a spatial distribution can be formualted as below:

$$
\begin{align}
\mathcal{E}(x_v(t)) & = \sum_{k\in\mathcal{K}} \Lambda_k [ c_k(x_v(t)) - \phi_k ]^2 \\
\Lambda_k & = \frac{1}{(1+\Vert k \Vert^2)^s}, \quad s = \frac{v+1}{2}
\end{align}
$$

In the next blog, we will start to implement this metric in Python for given trajectory and spatial distribution data. See you then!