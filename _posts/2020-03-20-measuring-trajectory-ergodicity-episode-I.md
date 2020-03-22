---
layout: post
title: Measuring Trajectory Ergodicity (Episode I)
categories: [Robotics]
mathjax: true
---

Today I will start a series of blogs about how to measure ergodicity of a trajectory with respect to a given information distribution. In this blog (maybe also the next few blogs), I will go through the math about the metric for measuring trajectory ergodicity. In the end of the series, I want to implement this metric to calculate ergodicity from given trajectory and information distribution.

First, why we need to talk about this? And what is ergodicity? In robotics, in order to enable robots to finish different tasks, people usually setup different task objectives. For example, in search-and-rescue scenarios, the objective maybe setup as "finding the location where the target is most likely to be", and then some algorithms will compute the next commands for robots to achieve this goal. This kind of objectives is called information maximization, which is widely used in robotics. However, information maximization strategies have a drawback which is actually kind of intuitive: imagine you're chasing a bus, you need to focus the bus since it's the target, but meanwhile you also need to take care of other vehicles on the road to avoid being hit. In such case, information maximization would lead us to a very dangerous situation, thus we need a metric to "teach" robots to take care of other things while chasing its target. Ergodicity is such a metric.  

From Wikipedia we have the definition of ergodicity as:

> In probability theory, an ergodic system is a stochastic process which proceeds in time and which has the same statistical behavior averaged over time as over the system's entire possible state space.

It sounds a little bit vague, so let's be more specific under the scenario of robotics. Suppose we want a cleaning robot to clean the dust in living room, where the densities of dust are different and can be considered as some kind of distribution. We are in a hurry since we will have guests in five minutes, but it will take the robot twenty minutes to cover the whole living room. Thus, instead of ordering the robot to cover the whole living room, we want to the robot to clean the dirties parts first and clean as much as it can in five minutes. In this case, given an information distribution, we say the robot's trajectory is ergodic when the time robot spent in a region is proportional to the information density of that region. Now, it should kind of clear that setting ergodicity as the objective might be a good choice in this cleaning robot example: since we have limited time, being ergodic means the robot will spend more time in the dirtier areas and thus make the living room as clean as possible in five minute.

Similarly, in our bus chasing scenario, being ergodic will enable us to notice other things while chasing the target: the bus can be treated as an information distribution with high density, and other vehicles on the road are information distributions with lower densities. High information density will make the agent focus on the target in most of the time, while being ergodic will prevent the agent from being hit.

So now, we know why this idea of ergodicity might be helpful for robots, in the next blog we will start to discuss how to measure ergodicity of a trajectory. See you then!

## References

[1] [CMU RI Seminar: Todd Murphey : Active Learning in Robot Motion Control](https://www.youtube.com/watch?v=CKAC4E7GcuU)

***