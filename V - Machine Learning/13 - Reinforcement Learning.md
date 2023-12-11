In reinforcement learning, an agent can actively learn from its own experience, by considering its ultimate success or failure.
# Learning from rewards
Reinforced learning algorithms are implemented with a wide range of learning methods. The basic idea of RL can be seen by giving a categorization of the approaches used.
- A **model-based RL** uses a transition model of the environment. The model may be initially unknown, in which the agent learns from its actions consequences. For each state, the sum of reward got so far is used to learn a utility function $U(s)$, which can provide a state estimation for the model.
- A **model-free RL** does not make use of a transition model of the environment, in fact the agent learns a more direct representation of how to behave. It comes in two varieties:
	- **action-utility learning**, or Q-learning, makes use of a quality function $Q(s, a)$ that maps each state $s$ with the rewards it can get by taking action $a$. Thus, choosing what to do is based on the action with the highest Q-value;
	- **policy learning** makes use of a policy function $\pi(s)$ to directly map states to actions. This is a reflex agent.

Furthermore, there can be a classification for whether the RL is passive or active:
- an **active RL** fixes the agent's policy and the task is to learn the utilities of the states (this can make use of a model);
- in a **passive RL** the agent must also figure out what to do, and thus the principal issue is exploration.

In the end, another alternative does exist, that is to use demonstration rather than reward signals. This is the case of an **apprenticeship learning**.