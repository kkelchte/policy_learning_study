---
title: Output Layer: Discrete vs Continuous
layout: default
---
## Output layer

### Intro

A policy maps from state to action space. A neural network can predict both discrete and continuous outputs. Success stories in computer vision are visible in both classification tasks (semantic segmentation, scene recognition, action recognition) as regression tasks (Optical Flow or Depth prediction). 

A continuous action space has infinite range of control allowing a smoother control. This can be desirable, especially in the case of developing a navigation policy. In our setting, the expert or demonstrator is a continuous function as well. It takes as input a horizontal line of ground truth depth from a simulated kinect and predicts with Behavior Arbitration the yaw velocity. DNNs (Deep Neural Networks) are known to be universal function approximators. These are all reasons to prefer a continuous action space. In our setting this results in a final node that predicts one yaw velocity ranging from -1 to 1. The node is not restricted with a `tan` activation function as you sometimes see in RL policies.

On the other hand, most DNN policies applied in the real world are using discrete action spaces. A discrete output layer has many advantages as well. In this setting, the final layer has a node for each discretized output. It allows a policy to indicate that multiple directions are possible when for instance turning left and right are equally good at a T-junction or close to a frontal collision. Moreover the activation of each output node represents a certainty of the network about this direction. If all activations are low, it indicates the network is rather unsure. The uncertainty could then be used to influence for instance the speed. This intuition is not visible with a continuous action space. In other words, using a discrete action space results in more interpretable results.

Moreover, as a continuous actions space obtains infinitely more options, one can guess that it will require much more training time and data.

_Claims:_
1. _Discrete ‘fires’ of a discrete action space can be interpreted as how certain a policy is._
2. _More discrete actions (up to infinite with continuous action space) requires more trainingtime and -data before convergence._
3. _Continuous action-space results in a smoother policy allowing higher robustness at different speeds._

### Experiments

1. **Uncertainty**: Train a discrete policy with 17 control options from -1 to 1 and see if they fire according to probabilistic distribution on the canyon, forest and sandbox task. Capture example situations where the discrete control defines both far left and far right as a potential direction.
2. **Discretization speeds up training**: Train policies at different levels of discretization (3, 9, 17, 65, inf) and see the influence on training time, data and stability on the task of canyon following in simulation.
3. **Discretization comes with Quantization Noise**: Test policies of different levels of discretization (3, 9, 17, 65, inf) on 3 sets of speeds (0.5m/s, 1.0m/s and 1.5m/s). Compare the performances with higher levels of quantization noise. _Remark: Smoother control is hard to define experimentally. With this experiment we can only measure the adaptability of a network to different speeds. The distribution of predicted controls will differ from the training setting, and the more robust the network the better it can adjust its prediction. In order to detect the different speed, the model requires a sense of time which is possible in an n_fc network (n features concatenated before the fc-control-layers). This does not represent the impact of quantization noise. The quantization noise might be visible at higher speeds in smaller canyons in which case a small error has large impact. The forest might be a better setting to test this as the example control is more distributed around 0._


**As long as target distribution is bad, not intermediate nodes will learn properly resulting in bad models, better to start off with RL signal and training.**

Network    | Training Steps  | Distance @ 0.5 (#successes)   | Distance @ 1.0 (#successes) | Distance @ 1.5 (#successes)
-----------|-----------------|-------------------------------|-----------------------------|----------------------------
Mobile-3   |        					|								|							  |
Mobile-9   |        					|								|							  |
Mobile-17  |        					|								|							  |
Mobile-65  |        					|								|							  |
Mobile-inf |        					|								|							  |
Resnet-3   |        					|								|							  |
Resnet-9   |        					|								|							  |
Resnet-17  |        					|								|							  |
Resnet-65  |        					|								|							  |
Resnet-inf |        					|								|							  |
Squeeze-3  |                  |               |               |
Squeeze-9  |                  |               |               |
Squeeze-17 |                  |               |               |
Squeeze-65 |                  |               |               |
Squeeze-inf|                  |               |               |

Todo:
- find proper hyperparameters for the discrete setting
- train 10 mobilenet-inf nets with different seeds 


To expect:
- mobile 3 shows some influence on quantization noise though in our basic experiments this influence is alreay negligible from mobile-9
- mobile 17 and 65 have some nodes that aren't trained properly as they don't have any examples in the training data this makes the nodes trained to say 0 all the time. This is undesirable as they become unpredictable. This issue is not the case for the regression setting.
- mobile 3 and 9 are learned with less data and have a higher success rate than mobile inf thanks to the simplicity of the model