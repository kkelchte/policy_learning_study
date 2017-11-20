## Why this post?

This blog came to live in the prospect of a paper. As I'm not such an experienced writer, I found that writing ideas down on a place that others can see helps to clearify them. This post is about **training deep neural network policies** for the task of **autonomous navigation** based on high dimensional rgb images. Many success stories are out there but a proper comparison of different training settings seems to lack. Possible decisions are: what kind of output should my network predict, what is the influence of recurrency,  what is the most (data/time/stability) efficient way of providing the data, … . The influence of these decisions is evaluated in task-specific performance (average collision-free distance travelled, success rate), absolute training time, data efficiency, stability over different seeding, stability over different hyperparameters, generalization in new test performances, … . 
The tasks are trained and evaluated in simulation. There are different simulated tasks: canyon following, obstacle avoidance in a cylinder forest and obstacle avoidance in a sandbox of objects. 
The drone flies (for now) at fixed height and speed. The navigation is controlled by turning in the yaw directions with -1:1. There is an example policy based on the groundtruth kinect that demonstrates a good behavior, further referred to as the expert.

>You can redo the experiments within a docker image kkelchte/ros_gazebo_tensorflow with the following packages: [drone simulator](https://github.com/kkelchte/hector_quadrotor), [simulation-supervised](https://github.com/kkelchte/simulation_supervised) package and [pilot](https://github.com/kkelchte/pilot) package. This is also explained on the [try-it](https://kkelchte.github.io/doshico/try) page of the Doshico challenge.

## Output layer
In general a policy maps from state to action space. A neural network can predict both discrete as continuous outputs. Success stories in computer vision are visible in both classification tasks (semantic segmentation, scene recognition, action recognition) as regression tasks (Optical Flow or Depth prediction). 

A continuous action space knows infinite possibilities which allows for a smoother control. This can be especially desirable when developing a navigation policy. In our setting, the expert is a continuous function as well. It takes as input a horizontal line of ground truth depth from a simulated kinect and predicts with Behavior Arbitration the yaw velocity. DNNs (Deep Neural Networks) are known to be universal function approximators. These are all reasons to prefer a continuous action space. In our setting this results in a final node that predicts one yaw velocity ranging from -1 to 1.

On the other hand, most DNN policies from the last years are using discrete action spaces. A discrete output layer has many advantages as well. In this setting, the final layer has a node for each discretized output. It allows a policy to indicate that multiple directions are possible when for instance turning left and right are equally good at a T-junction or close to a frontal collision. Moreover the activation of each output node represents a certainty of the network about this direction. If all activations are low, it indicates the network is rather unsure. The uncertainty could then be used to influence for instance the speed. This intuition is not visible with a continuous action space. In other words, using a discrete action space results in more interpretable results (1).

Moreover, as a continuous actions space obtains infinitely more options, one can guess that it will require much more training time and data (2).

_Claims:_
1. _Discrete ‘fires’ of a discrete action space can be interpreted as how certain a policy is._
2. _More discrete actions (infinitely a continuous action space) requires more trainingtime and -data before convergence._
3. _Continuous action-space results in a smoother policy as well as higher robustness for different speeds._

### Experiments

1. [ ] **Uncertainty**: Train a discrete policy with 17 control options from -1 to 1 and see if they fire according to probabilities. 
2. [ ] **Discretization speeds up training**: Train policies at different levels of discretization (3, 9, 17, 65, inf) and see the influence on training time, data and stability. The task is canyon following in simulation.
3. [ ] **Quantization noise can lead to fatal crashes**: Train policies at different levels of discretization (3, 9, 17, 65, inf) for 3 sets of speeds (0.5m/s, 1.0m/s and 1.5m/s). Compare the performances with higher levels of quantization noise.



