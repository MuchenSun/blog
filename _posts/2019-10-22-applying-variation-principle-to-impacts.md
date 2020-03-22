---
layout: post
title: 'Northwestern ME314 Notes: Applying the Variational Principle to Impacts'
categories: [Robotics]
mathjax: true
---

The focus in this lecture is to calculate the trajectory (equations of motion) of a system when impacts are considered. Without impact, everything will be smooth, like a point mass falling down or double pendulum moving freely. However, impacts mean discontinuity, which indicates that the state of the system will not change smoothly like before. Figuring out what happened during the impact and how to model this process become the emphasis of this lecture.

Now, since we, again, are on the road of deriving the trajectory (equations of motion) of a system, we will, again, use the variational principle, which is stated at the end of lecture 3 as:

> The action principle states that the trajectory of the particle is the one that extremizes the action with respect to perturbations in the trajectory that are zero at $t_0$ and $t_f$.

So, we should expect something like $\delta A = 0$. But this time, because of the existance of discontinuity during the process, the action integral $A$ will have two parts(before and after impact) and three variables($q$, $\dot{q}$, and time $\tau$ when impact happens):

$$
\begin{aligned}
A(q, \dot{q}, \tau) & = \int_{0}^{T} L(q, \dot{q}) dt \\
                    & = \int_{0}^{\tau} L(q, \dot{q}) dt + \int_{\tau}^{T} L(q, \dot{q}) dt
\end{aligned}
$$

Similarly, we will have two perturbations(one for $q$ and $\dot{q}$, another for $\tau$) when we set $\delta A$ to 0 for all possible variations in the trajectory:

$$
\begin{aligned}
A(\underbrace{q+\epsilon \eta}_{q_{\epsilon}}, \underbrace{\dot{q}+\epsilon \eta}_{\dot{q}_{\epsilon}}, \underbrace{\tau+\epsilon \theta}_{\tau_{\epsilon}}) & = \int_{0}^{\tau_{\epsilon}} L(q_{\epsilon}, \dot{q}_{\epsilon}) dt + \int_{\tau_{\epsilon}}^{T} L(q_{\epsilon}, \dot{q}_{\epsilon}) dt
\end{aligned}
$$

In the course note, we have:

$$
\begin{aligned}
\delta A \cdot \left[\begin{matrix} \eta \\ \theta \end{matrix}\right] & = \frac{d}{d\epsilon} \left[ \int_{0}^{\tau_{\epsilon}} L(q_{\epsilon}, \dot{q}_{\epsilon})dt + \int_{\tau_{\epsilon}}^{T} L(q_{\epsilon}, \dot{q}_{\epsilon})dt \right]_{\epsilon=0}
\end{aligned}
$$

Let's expand it in a more detailed way:

$$
\begin{aligned}
\delta A \cdot \left[\begin{matrix} \eta \\ \theta \end{matrix}\right] & = \frac{d}{d\epsilon} \left[ \int_{0}^{\tau_{\epsilon}} L(q_{\epsilon}, \dot{q}_{\epsilon})dt + \int_{\tau_{\epsilon}}^{T} L(q_{\epsilon}, \dot{q}_{\epsilon})dt \right]_{\epsilon=0} \\
    & = \underbrace{\frac{d}{d\epsilon} \left[ \int_{0}^{\tau_{\epsilon}} L(q_{\epsilon}, \dot{q}_{\epsilon})dt \right]_{\epsilon=0}}_{I_1} + \underbrace{\frac{d}{d\epsilon} \left[ \int_{\tau_{\epsilon}}^{T} L(q_{\epsilon}, \dot{q}_{\epsilon})dt \right]_{\epsilon=0}}_{I_2}
\end{aligned}
$$

Then, we derive $I_1$ and $I_2$ seperately:

$$
\begin{aligned}
I_1 & = \frac{d}{d\epsilon} \left[ \int_{0}^{\tau_{\epsilon}} L(q_{\epsilon}, \dot{q}_{\epsilon})dt \right]_{\epsilon=0} \\
    & = \frac{d}{d\epsilon} \left[ \int_{0}^{\tau_{\epsilon}} L\bigg(q(t)+\epsilon \eta(t), \dot{q}(t)+\epsilon \dot{\eta}(t)\bigg) dt \right]_{\epsilon=0} \\
    \xrightarrow{\text{Leibniz Rule}} & = \left[ L\bigg( q(\tau_{\epsilon})+\epsilon\eta(\tau_{\epsilon}) , \dot{q}(\tau_{\epsilon})+\epsilon\dot{\eta}(\tau_{\epsilon}) \bigg) \frac{d}{d\epsilon}(\tau_{\epsilon}) - 0 + \int_{0}^{\tau_{\epsilon}} \frac{d}{d\epsilon} L\bigg(q_{\epsilon}(t), \dot{q}_{\epsilon}(t)\bigg)dt \right]_{\epsilon=0} \\
    & = \left[ L\bigg( q_{\epsilon}(\tau_{\epsilon}) , \dot{q}_{\epsilon}(\tau_{\epsilon}) \bigg) \theta + \int_{0}^{\tau_{\epsilon}} \frac{d}{d\epsilon} L(q_{\epsilon}, \dot{q}_{\epsilon})dt \right]_{\epsilon=0} \\
    \xrightarrow{\tau_{\epsilon}=\tau^{-}} & = \left[ L(q_{\epsilon} , \dot{q}_{\epsilon}) \theta \bigg\rvert_{\tau^{-}} + \int_{0}^{\tau^{-}} \frac{d}{d\epsilon} L(q_{\epsilon}, \dot{q}_{\epsilon})dt \right]_{\epsilon=0} \\
    \xrightarrow{\text{chain rule}} & = \left[ L(q_{\epsilon} , \dot{q}_{\epsilon}) \theta \bigg\rvert_{\tau^{-}} + \int_{0}^{\tau^{-}} \left( \frac{\partial L}{\partial q_{\epsilon}} \frac{dq_{\epsilon}}{d\epsilon} + \frac{\partial L}{\partial \dot{q}_{\epsilon}} \frac{d\dot{q}_{\epsilon}}{d\epsilon} \right) dt \right]_{\epsilon=0} \\
    & = \left[ L(q_{\epsilon} , \dot{q}_{\epsilon}) \theta \bigg\rvert_{\tau^{-}} + \int_{0}^{\tau^{-}} \left( \frac{\partial L}{\partial q_{\epsilon}} \cdot \eta \right) dt + \underbrace{\int_{0}^{\tau^{-}} \left( \frac{\partial L}{\partial \dot{q}_{\epsilon}} \cdot \dot{\eta} \right) dt}_{\text{integration by part}} \right]_{\epsilon=0} \\
    & = \left[ L(q_{\epsilon} , \dot{q}_{\epsilon}) \theta \bigg\rvert_{\tau^{-}} + \int_{0}^{\tau^{-}} \left( \frac{\partial L}{\partial q_{\epsilon}} \cdot \eta \right) dt + \left( \frac{\partial L}{\partial \dot{q}_{\epsilon}} \cdot \eta \right) \bigg\vert^{\tau^{-}}_{0} - \int_{0}^{\tau^{-}} \left( \frac{d}{dt} \frac{\partial L}{\partial \dot{q}_{\epsilon}} \cdot \eta \right) dt \right]_{\epsilon=0} \\
    \xrightarrow{\eta(0)=0} & = \left[ L(q_{\epsilon} , \dot{q}_{\epsilon}) \theta \bigg\rvert_{\tau^{-}} + \int_{0}^{\tau^{-}} \left( \frac{\partial L}{\partial q_{\epsilon}} \cdot \eta \right) dt + \left( \frac{\partial L}{\partial \dot{q}_{\epsilon}} \cdot \eta \right) \bigg\vert_{\tau^{-}} - \int_{0}^{\tau^{-}} \left( \frac{d}{dt} \frac{\partial L}{\partial \dot{q}_{\epsilon}} \cdot \eta \right) dt \right]_{\epsilon=0} \\
    & = \left[ \int_{0}^{\tau^{-}} \left( \frac{\partial L}{\partial q_{\epsilon}} \cdot \eta - \frac{d}{dt} \frac{\partial L}{\partial \dot{q}_{\epsilon}} \cdot \eta \right) dt + \left( \frac{\partial L}{\partial \dot{q}_{\epsilon}} \cdot \eta + L(q_{\epsilon} , \dot{q}_{\epsilon}) \theta \right)_{\tau^{-}} \right]_{\epsilon=0} \\
    & = \int_{0}^{\tau^{-}} \left( \frac{\partial L}{\partial q} \cdot \eta - \frac{d}{dt} \frac{\partial L}{\partial \dot{q}} \cdot \eta \right) dt + \left( \frac{\partial L}{\partial \dot{q}} \cdot \eta + L(q , \dot{q}) \theta \right)_{\tau^{-}} 
\end{aligned}
$$

Similarly, we will have $I_2$ as:

$$
\begin{aligned}
I_2 & = \frac{d}{d\epsilon} \left[ \int_{\tau_{\epsilon}}^{T} L(q_{\epsilon}, \dot{q}_{\epsilon})dt \right]_{\epsilon=0} \\
    & = \frac{d}{d\epsilon} \left[ \int_{\tau_{\epsilon}}^{T} L\bigg(q(t)+\epsilon \eta(t), \dot{q}(t)+\epsilon \dot{\eta}(t)\bigg) dt \right]_{\epsilon=0} \\
    \xrightarrow{\text{Leibniz Rule}} & = \left[ 0 - L\bigg( q(\tau_{\epsilon})+\epsilon\eta(\tau_{\epsilon}) , \dot{q}(\tau_{\epsilon})+\epsilon\dot{\eta}(\tau_{\epsilon}) \bigg) \frac{d}{d\epsilon}(\tau_{\epsilon}) + \int_{\tau_{\epsilon}}^{T} \frac{d}{d\epsilon} L\bigg(q_{\epsilon}(t), \dot{q}_{\epsilon}(t)\bigg)dt \right]_{\epsilon=0} \\
    & = \left[ -L\bigg( q_{\epsilon}(\tau_{\epsilon}) , \dot{q}_{\epsilon}(\tau_{\epsilon}) \bigg) \theta + \int_{\tau_{\epsilon}}^{T} \frac{d}{d\epsilon} L(q_{\epsilon}, \dot{q}_{\epsilon})dt \right]_{\epsilon=0} \\
    \xrightarrow{\tau_{\epsilon}=\tau^{+}} & = \left[ -L(q_{\epsilon} , \dot{q}_{\epsilon}) \theta \bigg\rvert_{\tau^{+}} + \int_{\tau^{+}}^{T} \frac{d}{d\epsilon} L(q_{\epsilon}, \dot{q}_{\epsilon})dt \right]_{\epsilon=0} \\
    \xrightarrow{\text{chain rule}} & = \left[ -L(q_{\epsilon} , \dot{q}_{\epsilon}) \theta \bigg\rvert_{\tau^{+}} + \int_{\tau^{+}}^{T} \left( \frac{\partial L}{\partial q_{\epsilon}} \frac{dq_{\epsilon}}{d\epsilon} + \frac{\partial L}{\partial \dot{q}_{\epsilon}} \frac{d\dot{q}_{\epsilon}}{d\epsilon} \right) dt \right]_{\epsilon=0} \\
    & = \left[ -L(q_{\epsilon} , \dot{q}_{\epsilon}) \theta \bigg\rvert_{\tau^{+}} + \int_{\tau^{+}}^{T} \left( \frac{\partial L}{\partial q_{\epsilon}} \cdot \eta \right) dt + \underbrace{\int_{\tau^{+}}^{T} \left( \frac{\partial L}{\partial \dot{q}_{\epsilon}} \cdot \dot{\eta} \right) dt}_{\text{integration by part}} \right]_{\epsilon=0} \\
    & = \left[ -L(q_{\epsilon} , \dot{q}_{\epsilon}) \theta \bigg\rvert_{\tau^{+}} + \int_{\tau^{+}}^{T} \left( \frac{\partial L}{\partial q_{\epsilon}} \cdot \eta \right) dt + \left( \frac{\partial L}{\partial \dot{q}_{\epsilon}} \cdot \eta \right) \bigg\vert^{T}_{\tau^{+}} - \int_{\tau^{+}}^{T} \left( \frac{d}{dt} \frac{\partial L}{\partial \dot{q}_{\epsilon}} \cdot \eta \right) dt \right]_{\epsilon=0} \\
    \xrightarrow{\eta(T)=0} & = \left[ -L(q_{\epsilon} , \dot{q}_{\epsilon}) \theta \bigg\rvert_{\tau^{+}} + \int_{\tau^{+}}^{T} \left( \frac{\partial L}{\partial q_{\epsilon}} \cdot \eta \right) dt - \left( \frac{\partial L}{\partial \dot{q}_{\epsilon}} \cdot \eta \right) \bigg\vert_{\tau^{+}} - \int_{\tau^{+}}^{T} \left( \frac{d}{dt} \frac{\partial L}{\partial \dot{q}_{\epsilon}} \cdot \eta \right) dt \right]_{\epsilon=0} \\
    & = \left[ \int_{\tau^{+}}^{T} \left( \frac{\partial L}{\partial q_{\epsilon}} \cdot \eta - \frac{d}{dt} \frac{\partial L}{\partial \dot{q}_{\epsilon}} \cdot \eta \right) dt - \left( \frac{\partial L}{\partial \dot{q}_{\epsilon}} \cdot \eta + L(q_{\epsilon} , \dot{q}_{\epsilon}) \theta \right)_{\tau^{+}} \right]_{\epsilon=0} \\
    & = \int_{\tau^{+}}^{T} \left( \frac{\partial L}{\partial q} \cdot \eta - \frac{d}{dt} \frac{\partial L}{\partial \dot{q}} \cdot \eta \right) dt - \left( \frac{\partial L}{\partial \dot{q}} \cdot \eta + L(q , \dot{q}) \theta \right)_{\tau^{+}}
\end{aligned}
$$

Add $I_1$ and $I_2$ together, we have:

$$
\begin{aligned}
\delta A \cdot \left[\begin{matrix} \eta \\ \theta \end{matrix}\right] & = \int_{0}^{\tau^{-}} \left( \frac{\partial L}{\partial q} \cdot \eta - \frac{d}{dt} \frac{\partial L}{\partial \dot{q}} \cdot \eta \right) dt + \left( \frac{\partial L}{\partial \dot{q}} \cdot \eta + L(q , \dot{q}) \theta \right)_{\tau^{-}} + \int_{\tau^{+}}^{T} \left( \frac{\partial L}{\partial q} \cdot \eta - \frac{d}{dt} \frac{\partial L}{\partial \dot{q}} \cdot \eta \right) dt - \left( \frac{\partial L}{\partial \dot{q}} \cdot \eta + L(q , \dot{q}) \theta \right)_{\tau^{+}} \\
    & = \int_{0}^{\tau^{-}} \left( \frac{\partial L}{\partial q} \cdot \eta - \frac{d}{dt} \frac{\partial L}{\partial \dot{q}} \cdot \eta \right) dt - \left[ \left( \frac{\partial L}{\partial \dot{q}} \cdot \eta + L(q , \dot{q}) \theta \right)_{\tau^{+}} - \left( \frac{\partial L}{\partial \dot{q}} \cdot \eta + L(q , \dot{q}) \theta \right)_{\tau^{-}} \right] + \int_{\tau^{+}}^{T} \left( \frac{\partial L}{\partial q} \cdot \eta - \frac{d}{dt} \frac{\partial L}{\partial \dot{q}} \cdot \eta \right) dt \\
    & = \int_{0}^{\tau^{-}} \left( \frac{\partial L}{\partial q} \cdot \eta - \frac{d}{dt} \frac{\partial L}{\partial \dot{q}} \cdot \eta \right) dt - \left[ \frac{\partial L}{\partial \dot{q}} \cdot \eta + L(q , \dot{q}) \theta \right]^{\tau^{+}}_{\tau^{-}} + \int_{\tau^{+}}^{T} \left( \frac{\partial L}{\partial q} \cdot \eta - \frac{d}{dt} \frac{\partial L}{\partial \dot{q}} \cdot \eta \right) dt
\end{aligned}
$$

During the periods $[0,\tau^{-}]$ and $[\tau^{+},T]$, there is no impact, thus the trajectory is smooth and we can use Euler-Lagrangian Equations to see that the two integral terms in the equation above are zero. Thus, we have:

$$
\begin{aligned}
\delta A \cdot \left[\begin{matrix} \eta \\ \theta \end{matrix}\right] & = \int_{0}^{\tau^{-}} \left( \frac{\partial L}{\partial q} \cdot \eta - \frac{d}{dt} \frac{\partial L}{\partial \dot{q}} \cdot \eta \right) dt - \left[ \frac{\partial L}{\partial \dot{q}} \cdot \eta + L(q , \dot{q}) \theta \right]^{\tau^{+}}_{\tau^{-}} + \int_{\tau^{+}}^{T} \left( \frac{\partial L}{\partial q} \cdot \eta - \frac{d}{dt} \frac{\partial L}{\partial \dot{q}} \cdot \eta \right) dt \\
    & = - \left[ \frac{\partial L}{\partial \dot{q}} \cdot \eta + L(q , \dot{q}) \theta \right]^{\tau^{+}}_{\tau^{-}}
\end{aligned}
$$


