---
title: Behavioral Cloning vs Reinforcement Learning
layout: default
---
## Behavioral Cloning vs Reinforcement Learning
Another aspect linked with the output layer is the labels used for training the network as well as the loss. In the regression setting, an expert could provide the one correct steering velocity. The network in that case tries to clone the expert’s behavior as good as possible. If the behavior is straightforward for instance following a canyon, this can work fine. If the behavior on the other hand is less straightforward like general obstacle avoidance in a sandbox, this might lead to bad learning due to an OA ambiguity. The expert might decide to turn left, while the student prefers to go right. Both behavior successfully avoid the obstacle, though the imitation loss defined by the labels is very big. These losses results in strong gradients that appear in random directions and make the training difficult. This might result in less stable and longer training.

A better setup might be to work with a probability of collision as labels without the need of an expert demonstrating. From a certain starting point the policy can rollout its behavior. The actions of the last frames before collision are then labeled with high probability of collision. In this sense the ambiguity of turning left or right is taken away. 

In order to use these labels with a discrete policy, the policy could predict these probabilities for each action. Using these labels with a continuous policy requires some bigger adjustments. One way to do this is by adding a critic network after the policy (actor). The critic takes as input the state (image) and predicted action and tries to predict the probability of collision. With gradient descent the gradients can flow back from the critics loss to the policy (actor) itself.

_Claims:_
1. _Performance of behavior cloning vs reinforcement learning is not so different in the canyon setting as it is in the sandbox or forest setting._
2. _Training a policy for the sandbox/forest setting is more stable (for different seeds and hyperparameters) than behavioral cloning._


### Experiments




Network   | Training Steps  | Distance @ 0.5 (#successes)   | Distance @ 1.0 (#successes) | Distance @ 1.5 (#successes)
----------|-----------------|-------------------------------|-----------------------------|----------------------------
Mobile-3  |					|								|							  |
Mobile-9  |					|								|							  |
Mobile-17 |					|								|							  |
Mobile-65 |					|								|							  |
Mobile-inf|					|								|							  |
Resnet-3  |					|								|							  |
Resnet-9  |					|								|							  |
Resnet-17 |					|								|							  |
Resnet-65 |					|								|							  |
Resnet-inf|					|								|							  |
