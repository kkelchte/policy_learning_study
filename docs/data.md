---
title: Feeding the Data
layout: default
---
## Feeding the Data
The training data can be applied online and on-policy which is slow as you need to run a simulated environment for the creation of each frame and you through a lot of data away after generating. Keeping this data in a replay buffer allows… but introduces a state space shift.

Offline
Off-policy
Random
Policy Mixing

All task together
Curriculum learning

Behavioral cloning introduces a state-space shift that introduces an extra bias in both the states as well as the actions. Using online simulation-supervised learning can overcome the bias on the side of the state space. Mixing the two policies gives a smoother range in state space shift. On-policy RL learning overcomes the shift in both state and action space. > Check distribution of states/features and actions in different training settings + the influence of noise + over different training environments including ESAT. 
_Claims:_
1. _TODO_



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
