---
title: Adding Recurrency in the Architecture
layout: default
---
## Adding Recurrency in the Architecture
Intuitively it makes sense that a policy should have a sense of memory. In this way the policy could incorporate the speed at which the environment around changes. It could as well remember obstacles that are just out of side. Training recurrent neural networks on the other hand is known to be much harder than general FNN. Your input space becomes much more complex as there is always an intrinsic state from which your network starts. In simulation this initial state could be saved during online performance or estimated by running feeding several (10 or 20) through the RNN before feeding your batch. 

Another difficulty with training RNNs (LSTMs) is the correlation between the frames, resulting in gradients that are overconfident. Decorrelating these frames with WW-TBPTT might be key. > check the distribution of the gradients with or without WW-TBPTT.

Taking a turn at different speeds requires different (more aggressive) controls. The difference in a single image policy is not visible and will certainly lead to fatal crashes. Concatenating frames might overcome this issue as correlations between different frames can give a sense of speed to the network. Though instead of concatenating frames at a fixed rate and learning relations between these fixed rates, it might be better to let the network during learning decide which information should be kept over longer frames leading to better control estimation. LSTMs can learn to remember information in order to get optimal state representations.

This results in a different distribution of required actions. We can train policies at different velocities by applying different velocities in the dataset / during online-learning. 
With a continuous policy this leads to more extreme yaw-velocities. Discrete policies will endure harder from the quantization of the actionspace. 

_Claims:_
1. _Temporal information is required for adjusting the control for different speeds._
1. _LSTMs are more robust in speed variation than concatenated CNNs due to the optimal memory is learned instead of a fixed set of concatenated frames._
1. _Recurrent policies can use the temporal correlation to adjust to more aggressive controls if required due to for instance higher speed._
1. _WW-TBPTT leads to more stable and faster training of recurrent policies than S-TBPTT._
1. _Quantization noise will be more visible at different (higher and lower) speeds than at constant speed, making the continuous action space more suitable?_



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
