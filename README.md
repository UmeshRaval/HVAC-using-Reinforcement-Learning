# HVAC-Using-Reinforcement-Learning

It utilizes a defined environment to improve HVAC energy efficiency using Reinforcement Learning Control Algorithms. Several parameters including temperature, humidity, and occupancy were used to train the RL agent.

In this implementation of Reinforcement Learning Control for HVAC (Heating, Ventilation, and Air Conditioning) systems, I have created a versatile framework with **three distinct environments**, each for different considerations like turning off the systems entirely for **energy efficiency** or for exploring the **trade-offs between comfort and energy conservation**.

Within this framework, I have integrated two fundamental reinforcement learning algorithms to tackle the control problem: **Q-Learning** and **Monte Carlo** methods. These approaches enable the system to learn and adapt, optimizing its decision-making process by mapping **state-action pairs** to the expected returns.

In essence, this implementation empowers the HVAC system to become more intelligent and efficient in its operation, taking into account various **energy-saving** strategies while maintaining **occupant comfort**.

## Framework Overview

The environment defines:

- *3 Environment parameters* - Temperature, Humidity, and Occupancy Level
- *3 System modes* - AC_mode, Heating_mode, and Ventilation_mode
- *1 tuning parameter* - Current Watts consumed
- *1 reward calculation* - Absolute difference between the desired and current temperature or high power consumption, encouraging it to minimize both.

The environment calculates the **reward** based on how close the temperature is to the setpoint and **penalizes** high energy consumption. We take 500 steps in the environment initially, and print detailed information about the state every 10 steps.

## The First Algorithm: Q-Learning

### Initialization (`__init__`)

- **Parameters:**
  - `num_actions`: Number of actions available in the environment (e.g., AC, heating, ventilation).
  - `state_space`: The set of possible states in the environment, typically a discretized combination of environmental variables (e.g., temperature, humidity).
  - `learning_rate` (default = 0.1): Determines the weight given to new information when updating the Q-table.
  - `discount_factor` (default = 0.9): Encourages the agent to prioritize long-term rewards over immediate gains.
  - `epsilon` (default = 0.1): Controls the trade-off between exploration (random actions) and exploitation (choosing the best-known action).

- **Q-Table:** A table initialized with zeros to store the expected rewards for each state-action pair.
- **State Mapping:** A mapping from the discrete state to its corresponding index in the Q-table for efficient updates.

### `discretize_state(state)`

- Converts continuous state variables (e.g., temperature, humidity) into discrete bins for use with the Q-table.
- Ensures the resulting state is within the predefined state space.

### `choose_action(state)`

- Selects an action for a given state based on an epsilon-greedy policy:
  - With probability epsilon, the agent explores by selecting a random action.
  - Otherwise, the agent exploits by selecting the action with the highest Q-value for the current state.

### `update_q_table(state, action, reward, next_state)`

Updates the Q-value for a state-action pair using the Q-learning update rule:

![QLearnUpdateEq](https://github.com/user-attachments/assets/4d309e54-8244-4c91-8c1d-257d4c5af558)


- `α` (`learning_rate`): How quickly the Q-values adapt to new information.
- `γ` (`discount_factor`): The importance of future rewards.
- `r`: Immediate reward for taking the action.

## The Second Algorithm: Monte Carlo

- **Action Selection:**
  - Actions for air conditioning (AC), heating, and ventilation are selected using an epsilon-greedy policy, balancing exploration and exploitation.

- **Episode Simulation:**
  - The algorithm interacts with the environment in a loop until the episode terminates.
  - For each time step:
    1. The reward and next state are observed after performing the selected actions.
    2. The return (cumulative discounted reward) is updated iteratively using the discount factor gamma (0.7 in this example).
    3. The Q-value for the current state-action pair is updated by averaging the observed returns using a dictionary (`returns`).

- **Policy Update:**
  - The policy is updated to favor actions with the highest Q-values.
  - For the selected action, the probability is set to `1 - ε + (ε / |A|)` (where `|A|` is the total number of actions).
  - Other actions receive a small probability (`ε / |A|`) to encourage exploration.

- **Iterative Improvement:** The policy and Q-values are refined over 1000 episodes, ensuring the agent improves its decision-making ability over time.

- **State-Action Representation:**
  - States are represented as tuples (temperature, humidity, watts, occupancy).
  - Actions are tuples representing choices for AC, heating, and ventilation modes.

- **Parameters:**
  1. `gamma = 0.7`: Discount factor controlling how future rewards are weighted relative to immediate rewards.
  2. `eps = 0.1`: Epsilon value for the epsilon-greedy policy, determining the balance between exploration and exploitation.
  3. `returns`: A dictionary storing lists of observed returns for each state-action pair.
  4. `Q`: A dictionary representing the Q-table (action-value function).
  5. `policy`: A dictionary mapping states to action probabilities, guiding the agent's behavior.

## Some Outputs:
![MethodsForReinforcementHVAC](https://github.com/user-attachments/assets/9b14860a-3653-4f07-8d9c-45af220e6c59)
