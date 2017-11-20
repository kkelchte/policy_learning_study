---
title: Training Deep Neural Network Policies
layout: default
---

## Why this post?

This blog came to live in the prospect of a paper. As I'm not such an experienced writer, I found that writing ideas down on a place that others can see helps to clearify them. This post is about **training deep neural network policies** for the task of **autonomous navigation** based on high dimensional rgb images. Many success stories are out there but a proper comparison of different training settings seems to lack. Possible decisions are: what kind of output should my network predict, what is the influence of recurrency,  what is the most (data/time/stability) efficient way of providing the data, … . The influence of these decisions is evaluated in task-specific performance (average collision-free distance travelled, success rate), absolute training time, data efficiency, stability over different seeding, stability over different hyperparameters, generalization in new test performances, … . 
The tasks are trained and evaluated in simulation. There are different simulated tasks: canyon following, obstacle avoidance in a cylinder forest and obstacle avoidance in a sandbox of objects. 
The drone flies (for now) at fixed height and speed. The navigation is controlled by turning in the yaw directions with -1:1. There is an example policy based on the groundtruth kinect that demonstrates a good behavior, further referred to as the expert.

``` You can redo the experiments within a docker image kkelchte/ros_gazebo_tensorflow with the following packages: [drone simulator](https://github.com/kkelchte/hector_quadrotor), [simulation-supervised](https://github.com/kkelchte/simulation_supervised) package and [pilot](https://github.com/kkelchte/pilot) package. This is also explained on the [try-it](https://kkelchte.github.io/doshico/try) page of the Doshico challenge. ```

## Output layer
In general a policy maps from state to action space. A neural network can predict both discrete as continuous outputs. Most success stories in computer vision are visible in both classification tasks (semantic segmentation, scene recognition, action recognition) as regression tasks (Optical Flow or Depth prediction). 

A continuous action space knows infinite possibilities which allows for a smoother control. This can be especially desirable when thinking about a navigation policy. The expert is a continuous function as well. Besides, DNNs are known to be universal function approximators. These are all reasons to decide for a continuous action space. In our setting this results in a final node that predicts one yaw velocity.

Most DNN policies from the last years are using discrete action spaces. A discrete output layer has many advantages. It allows a policy to indicate that different directions are possible when for instance turning left and right are equally good at a T-junction or close to a frontal collision. On the other hand represents the activation of each output node a certainty of the network about this node. If all activations are low, it indicates the network is pretty unsure. This intuition is not visible with a continuous action space. In other words, using a discrete action space results in better interpretable results (1).

Moreover, as a continuous actions space obtains infinitely more options, one can guess that it will require much more training time and data (2).

> Claims:
- Discrete ‘fires’ of a discrete action space can be interpreted as how certain a policy is.
- The more discrete actions as well as continuous action space requires more training time and data before convergence.
- Continuous action-space results in a smoother policy as well as higher robustness for different speeds.


### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/kkelchte/policy_learning_study/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
