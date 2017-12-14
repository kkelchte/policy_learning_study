---
title: Output Layer: Discrete vs Continuous
layout: default
---
## Output layer
A policy maps from state to action space. A neural network can predict both discrete as continuous outputs. Success stories in computer vision are visible in both classification tasks (semantic segmentation, scene recognition, action recognition) as regression tasks (Optical Flow or Depth prediction). 

A continuous action space knows infinite possibilities which allows for a smoother control. This can be desirable especially when developing a navigation policy. In our setting, the expert is a continuous function as well. It takes as input a horizontal line of ground truth depth from a simulated kinect and predicts with Behavior Arbitration the yaw velocity. DNNs (Deep Neural Networks) are known to be universal function approximators. These are all reasons to prefer a continuous action space. In our setting this results in a final node that predicts one yaw velocity ranging from -1 to 1.

On the other hand, most DNN policies from the last years are using discrete action spaces. A discrete output layer has many advantages as well. In this setting, the final layer has a node for each discretized output. It allows a policy to indicate that multiple directions are possible when for instance turning left and right are equally good at a T-junction or close to a frontal collision. Moreover the activation of each output node represents a certainty of the network about this direction. If all activations are low, it indicates the network is rather unsure. The uncertainty could then be used to influence for instance the speed. This intuition is not visible with a continuous action space. In other words, using a discrete action space results in more interpretable results.

Moreover, as a continuous actions space obtains infinitely more options, one can guess that it will require much more training time and data.

_Claims:_
1. _Discrete ‘fires’ of a discrete action space can be interpreted as how certain a policy is._
2. _More discrete actions (up to infinite with continuous action space) requires more trainingtime and -data before convergence._
3. _Continuous action-space results in a smoother policy allowing higher robustness at different speeds._

### Experiments

1. **Uncertainty**: Train a discrete policy with 17 control options from -1 to 1 and see if they fire according to probabilistic distribution on the canyon, forest and sandbox task. 
2. **Discretization speeds up training**: Train policies at different levels of discretization (3, 9, 17, 65, inf) and see the influence on training time, data and stability on the task of canyon following in simulation.
3. **Discretization comes with Quantization Noise**: Train policies at different levels of discretization (3, 9, 17, 65, inf) for 3 sets of speeds (0.5m/s, 1.0m/s and 1.5m/s). Compare the performances with higher levels of quantization noise.


Network    | Training Steps  | Distance @ 0.5 (#successes)   | Distance @ 1.0 (#successes) | Distance @ 1.5 (#successes)
-----------|-----------------|-------------------------------|-----------------------------|----------------------------
Mobile-3   |					|								|							  |
Mobile-9   |					|								|							  |
Mobile-17  |					|								|							  |
Mobile-65  |					|								|							  |
Mobile-inf |					|								|							  |
Resnet-3   |					|								|							  |
Resnet-9   |					|								|							  |
Resnet-17  |					|								|							  |
Resnet-65  |					|								|							  |
Resnet-inf |					|								|							  |
Squeeze-3  |          |               |               |
Squeeze-9  |          |               |               |
Squeeze-17 |          |               |               |
Squeeze-65 |          |               |               |
Squeeze-inf|          |               |               |
