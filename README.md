# Representation-of-a-Real-World-Problem-as-a-Markov-Decision-Process


## Aim

To identify a real-world sequential decision-making problem and represent it as a Markov Decision Process (MDP) by defining its states, actions, transition probabilities, reward function, discount factor, and Python implementation, and to understand how Reinforcement Learning can improve decision-making through experience.

---

## Problem Statement

### Problem Description

College canteens often experience long queues during lunch breaks because many students arrive at the same time. This causes increased waiting time, overcrowding, and delays in food service.

This project represents Smart College Canteen Queue Management as a Markov Decision Process. The intelligent agent observes the current queue condition and selects an action, such as closing an extra counter, maintaining the current arrangement, or opening an additional serving counter.

After an action is performed, the queue condition changes probabilistically. The agent receives a reward based on queue reduction and operating costs. By applying Reinforcement Learning, the system can learn a suitable strategy to reduce waiting time and improve canteen service efficiency.

The probabilities and rewards used in this educational example are illustrative assumptions, not measurements from an actual college canteen.

---

## MDP Components

A Markov Decision Process is represented as:
MDP = (S, A, P, R, γ)

- S — States: Queue conditions (Low, Medium, High).
- A — Actions: Close Extra Counter, Maintain, Open Extra Counter.
- P — Transition probabilities: Probability of moving to another queue condition after taking an action.
- R — Reward function: Feedback based on the resulting queue condition and the cost of the selected action.
- γ — Discount factor: 0.90, giving importance to future rewards as well as immediate rewards.
  
---

## State Space

The state space represents all possible queue conditions in the simplified canteen environment.
\[
S=\{Low,Medium,High\}
\]

```
S = {
    "Low": "0-5 students waiting",
    "Medium": "6-15 students waiting",
    "High": "More than 15 students waiting"
}
```
•Low: A small number of students are waiting.

•Medium: A moderate queue of students is waiting.

•High: A large queue of students is waiting.

---

## Sample State

High is a sample state. It means that more than 15 students are waiting in the canteen queue. The agent should consider opening an additional serving counter to reduce the waiting time.

---

## Action Space

The action space represents all decisions available to the intelligent agent.
\[
A=\{CloseExtraCounter,Maintain,OpenExtraCounter\}
\]

```
A = {
    "Action 1": "Close Extra Counter",
    "Action 2": "Maintain",
    "Action 3": "Open Extra Counter"
}
```

•Close Extra Counter: Close an additional counter when the queue is low.

•Maintain: Keep the current counter arrangement unchanged.

•Open Extra Counter: Open an additional counter when the queue is increasing.

---

## Sample Action

When the current state is High, the agent may select Open Extra Counter to serve more students and reduce overcrowding.

---

## Transition Probability

The transition probability is represented as:
P(s′ | s, a)

It represents the probability of reaching the next state \(s'\) after taking action \(a\) in the current state \(s\).

The following transition probabilities are illustrative.

| Current State | Action               | Next Low | Next Medium | Next High |
|---------------|----------------------|----------|-------------|-----------|
| Low           | Close Extra Counter  | 0.80     | 0.20        | 0.00      |
| Low           | Maintain             | 0.90     | 0.10        | 0.00      |
| Low           | Open Extra Counter   | 0.95     | 0.05        | 0.00      |
| Medium        | Close Extra Counter  | 0.20     | 0.60        | 0.20      |
| Medium        | Maintain             | 0.30     | 0.50        | 0.20      |
| Medium        | Open Extra Counter   | 0.60     | 0.35        | 0.05      |
| High          | Close Extra Counter  | 0.05     | 0.25        | 0.70      |
| High          | Maintain             | 0.10     | 0.40        | 0.50      |
| High          | Open Extra Counter   | 0.35     | 0.55        | 0.10      |

Each row sums to 1.0, representing all possible next queue states for that state-action pair.

For example, if the current state is High and the agent selects Open Extra Counter, there is a 35% probability of reaching Low, a 55% probability of reaching Medium, and a 10% probability of remaining in High.

---

## Reward Function

R(s, a, s′)

The reward depends on the next queue condition and the cost or benefit of the selected action.

### State Rewards

| Next Queue State | Reward |
|------------------|--------|
| Low              | +10    |
| Medium           | +3     |
| High             | -10    |

### Action Rewards

| Action               | Additional Reward |
|----------------------|-------------------|
| Close Extra Counter  | +1                |
| Maintain             | 0                 |
| Open Extra Counter   | -2                |

The total reward is calculated by adding the state reward to the action reward.
R(s, a, s′) = R_state(s′) + R_action(a)

For example, if the agent opens an extra counter and the next state becomes Low, the total reward is:
\[
R=10+(-2)=+8
\]

Positive rewards encourage shorter queues, while negative rewards discourage overcrowding and unnecessary operating costs.

The discount factor is:
γ = 0.90

This encourages the agent to consider future queue conditions in addition to immediate rewards.

---

## Graphical Representation

The following diagram illustrates the MDP states and example transitions.

                 +------------------+
                 |    LOW QUEUE     |
                 |  0-5 students    |
                 +------------------+
                    ^           |
                    |           |
          Maintain  |           | Queue increases
                    |           v
                 +------------------+
                 |   MEDIUM QUEUE   |
                 |  6-15 students   |
                 +------------------+
                    ^           |
                    |           | Queue increases
     Open Extra     |           v
     Counter     +------------------+
                 |    HIGH QUEUE    |
                 | More than 15     |
                 +------------------+
                    |
                    | Open Extra Counter
                    v
                 LOW / MEDIUM
                 
The transition probabilities determine the likelihood of reaching each next state. The rewards evaluate the effectiveness of the chosen action.

---

## Python Representation

The complete implementation is stored in main.py. Python dictionaries represent the states, actions, transition probabilities, rewards, and discount factor. The program also validates the transition probabilities and calculates the expected immediate reward for a sample decision.



## Main.py
```
# Representation of a Real-World Problem as an MDP
# Smart College Canteen Queue Management

print("Name: Mithun S")
print("Register Number: 212224240088")

# State space
states = ["Low", "Medium", "High"]

# Action space
actions = [
    "Close Extra Counter",
    "Maintain",
    "Open Extra Counter"
]

# Transition probabilities
transitions = {
    "Low": {
        "Close Extra Counter": {
            "Low": 0.80, "Medium": 0.20, "High": 0.00
        },
        "Maintain": {
            "Low": 0.90, "Medium": 0.10, "High": 0.00
        },
        "Open Extra Counter": {
            "Low": 0.95, "Medium": 0.05, "High": 0.00
        }
    },
    "Medium": {
        "Close Extra Counter": {
            "Low": 0.20, "Medium": 0.60, "High": 0.20
        },
        "Maintain": {
            "Low": 0.30, "Medium": 0.50, "High": 0.20
        },
        "Open Extra Counter": {
            "Low": 0.60, "Medium": 0.35, "High": 0.05
        }
    },
    "High": {
        "Close Extra Counter": {
            "Low": 0.05, "Medium": 0.25, "High": 0.70
        },
        "Maintain": {
            "Low": 0.10, "Medium": 0.40, "High": 0.50
        },
        "Open Extra Counter": {
            "Low": 0.35, "Medium": 0.55, "High": 0.10
        }
    }
}

# Rewards
state_rewards = {
    "Low": 10,
    "Medium": 3,
    "High": -10
}

action_rewards = {
    "Close Extra Counter": 1,
    "Maintain": 0,
    "Open Extra Counter": -2
}

# Discount factor
gamma = 0.90

print("\nStates:", states)
print("Actions:", actions)
print("Discount Factor:", gamma)

# Validate transition probabilities
print("\nTransition Probability Validation:")

for state in states:
    for action in actions:
        total = sum(transitions[state][action].values())
        print(state, "-", action, "=", round(total, 2))

        assert abs(total - 1.0) < 1e-9

print("All transition probabilities are valid.")

# Sample decision
current_state = "High"
selected_action = "Open Extra Counter"

print("\nSample Decision")
print("Current State:", current_state)
print("Selected Action:", selected_action)

print(
    "Next State Probabilities:",
    transitions[current_state][selected_action]
)

# Calculate expected immediate reward
expected_reward = 0

for next_state, probability in (
    transitions[current_state][selected_action].items()
):
    reward = (
        state_rewards[next_state]
        + action_rewards[selected_action]
    )
    expected_reward += probability * reward

print("Expected Immediate Reward:", round(expected_reward, 2))

```
---
## Output

```
Name: Mithun S
Register Number: 212224240088

States: ['Low', 'Medium', 'High']
Actions: ['Close Extra Counter', 'Maintain',
          'Open Extra Counter']
Discount Factor: 0.9

Transition Probability Validation:
Low - Close Extra Counter = 1.0
Low - Maintain = 1.0
Low - Open Extra Counter = 1.0
Medium - Close Extra Counter = 1.0
Medium - Maintain = 1.0
Medium - Open Extra Counter = 1.0
High - Close Extra Counter = 1.0
High - Maintain = 1.0
High - Open Extra Counter = 1.0

All transition probabilities are valid.

Sample Decision
Current State: High
Selected Action: Open Extra Counter
Next State Probabilities: {'Low': 0.35, 'Medium': 0.55, 'High': 0.1}
Expected Immediate Reward: 2.15

```


---

## Result

The real-world problem of smart college canteen queue management was successfully represented as a Markov Decision Process using Python dictionaries. The state space, action space, transition probabilities, reward function, and discount factor were defined, and the program validated the transition probabilities.

The sample decision produced an expected immediate reward of 2.15 when the queue was high and the agent selected the Open Extra Counter action.

This project demonstrates how Reinforcement Learning can be applied to reduce queue lengths, improve student waiting times, and manage canteen resources efficiently. A further algorithm such as Q-learning can be implemented to train the agent and learn an optimal policy through repeated interactions with the environment.

---
