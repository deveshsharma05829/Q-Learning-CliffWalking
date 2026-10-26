# Q-Learning on CliffWalking-v1

A tabular implementation of the Q-learning algorithm, applied to the CliffWalking-v1 environment from [Gymnasium](https://gymnasium.farama.org/).

## Environment

CliffWalking is a 4×12 grid world. The agent starts in the bottom-left corner and must reach the bottom-right goal. The bottom row between start and goal is a cliff: stepping into it gives a reward of -100 and sends the agent back to the start. Every other step costs -1.

- States: 48 (one per grid cell)
- Actions: 4 (up, right, down, left)
- Optimal return: -13 (walks along the cliff edge)

## Algorithm

Q-learning is an off-policy TD control method. It updates the Q-table using the maximum Q-value of the next state, regardless of which action the agent actually takes:

$$Q(s, a) \leftarrow Q(s, a) + \alpha \big[ r + \gamma \max_{a'} Q(s', a') - Q(s, a) \big]$$

Because the update assumes greedy behavior at the next step, Q-learning learns the optimal policy even while exploring. In CliffWalking, this means it learns the shortest path along the cliff edge.

## Hyperparameters

| Parameter  | Value | Description                       |
|------------|-------|-----------------------------------|
| gamma    | 0.99  | Discount factor                   |
| alpha    | 0.5   | Learning rate                     |
| epsilon  | 0.1   | Exploration rate (epsilon-greedy) |
| episodes | 500   | Training episodes                 |

## Installation

pip install "gymnasium[toy-text]" numpy## Usage

1. Open Q_Learning.ipynb in Jupyter.
2. Run all cells from top to bottom.
3. Every 50 episodes, the environment renders in human mode so you can watch the agent's progress.

A final greedy run (argmax over the learned Q-table) prints the total reward and episode length of the learned policy.

## Results

| Metric                   | Value           |
|--------------------------|-----------------|
| Greedy policy evaluation | -13 (length 13) |

The greedy policy reaches the optimal return of -13 by following the cliff edge. Training returns are noisy early on because epsilon-greedy exploration sometimes steps into the cliff, but they converge as the Q-table fills in.

## Project Structure

.
├── Q_Learning.ipynb   # Q-learning implementation and experiments
└── README.md## Comparison with SARSA

| Aspect        | SARSA (on-policy)                   | Q-learning (off-policy)             |
|---------------|-------------------------------------|-------------------------------------|
| Update target | Q(s', a') (actual next action)    | max Q(s', ·) (greedy next action) |
| Learned path  | Safer, farther from cliff           | Optimal, along cliff edge           |
| Greedy return | -17                                 | -13                                 |

## References

- Sutton, R. S., & Barto, A. G. *Reinforcement Learning: An Introduction* (2nd ed.), Section 6.5 (Q-learning) and Example 6.6 (Cliff Walking).
- [Gymnasium CliffWalking documentation](https://gymnasium.farama.org/environments/toy_text/cliff_walking/)
