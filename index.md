## Intermediate Results on the Policy Learning Study

### Why this post?

This blog is about **training deep neural network policies** for the task of **autonomous navigation** based on high dimensional rgb images. Many success stories are out there but a proper comparison of different training settings seems to lack. Possible decisions are: what kind of output should my network predict, what is the influence of recurrency, what is the most (data/time/stability) efficient way of providing the data, … . The influence of these decisions is evaluated in task-specific performance (average collision-free distance travelled, success rate), absolute training time, data efficiency, stability over different seeding, stability over different hyperparameters, generalization in new test performances, … . 
The tasks are trained and evaluated in simulation. There are different simulated tasks: canyon following, obstacle avoidance in a cylinder forest and obstacle avoidance in a sandbox of objects. 
The drone flies (for now) at fixed height and speed. The navigation is controlled by turning in the yaw directions with a range of -1 to 1 as angular velocity. There is an example policy based on the groundtruth kinect that demonstrates a good behavior, further referred to as the expert or behavior arbitration (BA).

>You can redo the experiments within a docker/singularity image kkelchte/ros_gazebo_tensorflow with the following packages: [drone simulator](https://github.com/kkelchte/hector_quadrotor), [simulation-supervised](https://github.com/kkelchte/simulation_supervised) package and [pilot](https://github.com/kkelchte/pilot) package. This is also explained on the [try-it](https://kkelchte.github.io/doshico/try) page of the Doshico challenge.

#### [Changing the Output Layer](./docs/output.md)
#### [Changing the Labels: Behavioral Cloning vs Reinforcement Learning](./docs/labels.md)
#### [Adding Recurrency in the Architecture](./docs/recurrency.md)
#### [Feeding the Data](./docs/data.md)
