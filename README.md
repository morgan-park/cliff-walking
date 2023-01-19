# Cliff Walking: Comparison of Sarsa and Q-Learning

Cliff walking is an environment of a grid world with the cliff. The task is to find an optimal path from starting state to the goal state without falling off the cliff (The following figure is from Sutton & Barton (2015) p.132. 

<p align="center">
  <img src="https://user-images.githubusercontent.com/94096127/213368192-7c44d0dd-46ae-4cb6-a159-08cacc270428.png">
</p>

This project compares the performance of Sarsa and Q-Learning in cliff walking. It consists of the experiments:
1) Comparing the path and reward per episode of the two (replicating the example 6.6 from Sutton & Barto (2015) [Code of experiment #1](https://github.com/morgan-park/cliff-walking/blob/main/path_reward_sum.ipynb)
2) The impact of epsilon on the performance of each method [Code of experiment #2](https://github.com/morgan-park/cliff-walking/blob/main/impact_of_epsilon.ipynb)
3) The influence of changing cliff reward on the performance of the two [Code of experiment #3](https://github.com/morgan-park/cliff-walking/blob/main/impact_of_cliff_rewards.ipynb)

## 1. Comparison of Sarsa and Q-Learning: Path & Rewards
### Experiment Setting
* The main difference between Sarsa and Q-Learning is that Sarsa is on-policy whereas Q-Learning is off-policy. Sarsa uses a ε-greedy policy both for prediction and control algorithms. By contrast, Q-Learning uses a ε-greedy policy for prediction but a maximization policy for its control algorithm. The pseudocodes are as follows:

<p align="center">
 <img src="https://user-images.githubusercontent.com/94096127/213369360-bd74d0c5-d987-4cc0-8cf7-3ac4304f0fdd.png">
</p>
<p align="center">
  Sarsa Pseudocode
</p>

<p align="center">
 <img src="https://user-images.githubusercontent.com/94096127/213369380-94d71658-a79d-4453-8439-42a33a6eadc0.png">
</p>
<p align="center">
  Q-Learning Pseudocode
</p>

* ε for both Sarsa and Q-learning : 0.1 
* α for both Sarsa and Q-learning : 0.5 
* γ for both Sarsa and Q-learning: 1 (this is a standard undiscounted, episodic task, with starting and goal states). 
* The number of episodes: 500
* The sum of rewards per episode: Each learning (Sarsa and Q-learning) is repeated 100 times with the above-mentioned setting and averaged. 

### Result
<p align="center">
  <img src="https://user-images.githubusercontent.com/94096127/213371198-9ddb5d44-238a-44b7-88e5-0ad799da1b93.png">
</p>
<p align="center">
  Path: Sarsa
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/94096127/213371211-0b823f01-31bb-490c-b688-9452859d4899.png">
</p>
<p align="center">
  Path: Q-Learning
</p>

Sarsa takes a safe path. Since the policy that drives the learning of action-value function of Sarsa is ε-greedy, there is always a risk to fall off the cliff in action selection (the ε value indicates the percentage of the time to choose an action randomly and Sarsa uses ε > 0 for both action selection and policy update). Thus, Sarsa chooses a safe path to avoid the big penalty of falling off the cliff by choosing a long traveling path instead. By contrast, Q-learning uses the maximization policy to update the policy. Therefore, Q-learning chooses the optimal path right above the cliff. As shown below, however, the sum of rewards per episode during Sarsa’s learning process (approximately -30) is greater than that of Q-learning (approximately -50), since Sarsa tends to avoid the cliff which gives a huge penalty.

<p align="center">
  <img src="https://user-images.githubusercontent.com/94096127/213372417-83a7b437-34c4-4040-b10d-be3dd6f05da4.png" width="500" height="400">
</p>

## 2. Impact of Epsilon

### Experiment Setting
* ε for action selection: 0.1 
* ε values for policy update compared: 0.1, 0.01, 0.001, 0 
* α: 0.5 
* γ: 1 
* The number of episodes: 500
* The sum of rewards per episode: Each learning (Sarsa and Q-learning) is repeated 100 times with the above-mentioned setting and averaged.

### Result

<p align="center">
  <img src="https://user-images.githubusercontent.com/94096127/213375081-bc71daaa-cfc6-479a-9754-bd9123dc155e.png">
  <div align="center"> Path: ε = 0.1</div>
</p>


<p align="center">
  <img src="https://user-images.githubusercontent.com/94096127/213375082-56e07f48-2c8e-4399-a441-2aecd000990e.png">
  <div align="center"> Path: ε = 0.01</div>
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/94096127/213375084-bfd46062-f23f-4f8d-b770-cee3c1c34896.png">
  <div align="center"> Path: ε = 0.001</div>
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/94096127/213375077-bab85da0-fa16-4b2f-b188-b2a662a01260.png">
  <div align="center"> Path: ε = 0</div>
</p>

As shown above, the smaller the ε value for policy update is, the closer it is to the optimal policy. By contrast, the greater the ε value for policy update is, the the sum of rewards per episode during learning process is greater. The ε-greedy policy for action selection (ε1: 0.1) chooses actions randomly with a 1 in 10 chance. When ε value for policy update (ε2) is smaller, the learning algorithm has more chances to choose a path closer to the cliff since it becomes more greedy. However, during the learning process, actions are still selected randomly with the 1 in 10 chance. As a result, this causes the agent often falls off the cliff then they are on the state right next to the cliff. Thus, the smaller ε2 value is, the smaller the sum of rewards per episode during learning process it earns.

<p align="center">
  <img src="https://user-images.githubusercontent.com/94096127/213375598-19c1f0a8-547d-47a5-8ce7-152831fee39e.png" width="600" height="500">
</p>

## 3. Impact of Cliff Reward
### Experiment Setting
* ε for both Sarsa and Q-learning: 0.1 
* α: 0.5 
* γ: 1 (this is a standard undiscounted, episodic task, with starting and goal states). 
* The reward for falling off the cliff compared: -20, -50, -100 
* The number of episodes: 500. 
* The sum of rewards per episode: Each learning is repeated 100 times with the above-mentioned setting and averaged. 

### Result

<p align="center">
  <img src="https://user-images.githubusercontent.com/94096127/213378003-4da5f313-3c86-40ef-ac68-bad43dd29b72.png">
</p>
<p align="center">
  Path: Q-Learning (same for all the different cliff rewards)
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/94096127/213377923-92dd2a31-332c-49d5-b8ff-667cd1b55786.png">
</p>
<p align="center">
  Path: Path: Sarsa With Cliff Reward -20
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/94096127/213377939-1506ff7b-4ada-46b3-ad04-2282d965c6b1.png">
</p>
<p align="center">
  Path: Path: Sarsa With Cliff Reward -50
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/94096127/213377960-3871c0f6-b6e5-4056-add2-64a1d05f4712.png">
</p>
<p align="center">
  Path: Path: Sarsa With Cliff Reward -100
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/94096127/213378021-d5e8a454-9c86-4f89-a4fa-88d661b2f8f2.png">
</p>

When the cliff reward is relatively smaller, the penalty from falling into the cliff for Q-learning becomes lighter, whereas Sarsa is less afraid of falling off the cliff and taking bolder moves closer to the cliff. Thus, when the cliff reward is -50, while Sarsa ended up with a path in-between the safest and optimal paths, Q-learning not only chooses the optimal path but also achieved the similar level of the rewards with Sarsa during the learning process. The cliff reward -50 can be considered a “sweet point” for Q-learning. 

## Reference
* Sutton, R., & Barto, A. (2015). *Reinforcement Learning: An Introduction* (2nd ed.). MIT Press.

